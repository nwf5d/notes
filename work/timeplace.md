# timeplace 地名识别

## timeplace

### 对外接口

#### 初始化timeplace

    timeplace_init()
调用老接口globalMap_T *InitTimePlace(char *mapPath, char *mapFile, char *propPath, char *propFile) 初始化：

* 加载地区树LoadMap(&pMap, mapFn, propFn)
递归读入结点信息ReadInfo()
处理别名DealAlias((*ppMap)->pPlace);
处理地区号DealAreaCode(*ppMap, (*ppMap)->pPlace);
建立索引：MakeMapIndex((*ppMap), (*ppMap)->pPlace);
加载属性文件：LoadMapProp(*ppMap, fnProp)!
* NumberMap(pMap);




#### 根据标题、内容判断文章的地区分类
	
	//Des@: 获取地区编码，包括"是否为地区新闻判定"  "隶属于哪个地区"两部分
	//title@in: 标题
	//cont@in: 正文
	//segTitle@in: 分词后的标题term1\tterm2\t
	//segCont@in: 分词后的正文 term1\tterm2\t
	//classID@in: 新闻类别编号  ( 国内 国际 等)
	//segFlag@in: 是否需要进行分词 0 不需要  1 需要
	//recogFlag@in: 是否需要地区新闻识别  0 不需要  1 需要                                                                                                                                       
	//ret@int: 0x7FFF 无法识别地区 other 地区编码
	unsigned int getPlaceID(const char * title, const char * cont, const char * segTitle,
		const char * segCont, int classID, int recogFlag, int segFlag,
		handle_t handle, token_t * tokens, int tokenNum)

1. segFlag判断是否需要分词，需要则使用seg_segment进行分词，否则直接使用输入的分词内容
* delNewsStart去除正文开头的“据新华网 *日 报导”之类的开头语
* isRegionNews判断是否为地区新闻`isRegionNews(char * title,char * cont,char * segTitle,char * segCont,int classID)`
	* 计算各类特征的得分，达到某阈值为地区新闻，否则为非地区新闻
* 若为地区新闻，SearchPlace_2nd(pageBuf)得到新闻的地区ID
	* searchInMap查找地区所属列表
		* `PretreatText(pMap, textCopy, &textMark)`：预处理，得到正文的起始和结束位置
		* SearchWithoutTreat
		* SearchAreaCode

#### 根据地区号获取地区名称
    
    //Des@:依据地区编号获取地区字串，如: 福建省|福州市
	//placeID@in: 地区编号
	//placeBuf@ou:地区字串
	//placeBufSize@in: 地区字串缓冲区大小
	//ret@: -1 失败 0 成功
	int getPlaceStr(unsigned int placeID,char * placeBuf,int placeBufSize);
	
### 主要的函数

#### 判断一篇新闻是否为地区新闻

	//Des@: 判定一篇新闻是否为地区新闻
	//title@in: 标题
	//cont@in: 正文
	//segTitle@in: 分词后的标题term1\tterm2\t
	//segCont@in: 分词后的正文 term1\tterm2\t
	//classID@in: 新闻类别编号  ( 国内 国际 等)
	//ret@: -1 失败 0 非地区  1 地区
	int isRegionNews(char * title,char * cont,char * segTitle,char * segCont,int classID);

 1. 获取特征，存入featureVec中
 2. 累积特征权值
 3. 根据阈值判断新闻类别

#### 判断一篇新闻隶属的地区

	//Des@: 判定一篇新闻隶属的地区
	//text@in: 待判定的新闻
	//ret@out: 0x7FFF 无法识别地区 other 地区编码
	unsigned int SearchPlace_2nd(char * text);

### 内部函数

#### 匹配地名SearchInMap(g_globalMap, title);
#### 标题特征getVTitle(pPlaceList)
#### 首地址位置getVFirstAd(pPlaceList);
#### 地址出现次数getVAdNum(pPlaceList);
#### 第一级地址出现次数getVAdNum_firstlevel(pPlaceList,g_globalMap);
#### 最深层级getVDeepestLevel(pPlaceList);
#### 最大权重getVLargestWeight(pPlaceList);
#### 标签特征getVTag(cont);
#### 内容权重getVContWeight(segTitle,  segCont);
#### 类别特征getVClassWeight(classID); 

### 数据结构
   
#### 文章正文的标识
    struct textMark_T
    {
        char *contBegin;  //文章起始位置
        char *contEnd;    //文章结束位置
        int badDistance;
        textMark_T()
        {
            contBegin=NULL;
            contEnd=NULL;
            badDistance=MAX_BAD_DISTANCE;
        }
    };

#### 地区信息
    //***********************************
    //  Type of place's property's value
    //***********************************
    struct placePropValue_T
    {
        char *value;//当placeProp_T为注释信息时，该成员记录"地名别名"
        placePropValue_T *nextValue;// 下一个"地名别名"
    };
    
    //***********************************
    //  Type of place's property
    //***********************************
    struct placeProp_T
    {
        char *propName;
        placePropValue_T *pPropValue;
        placeProp_T *nextProp;
    };
    //***********************************
    //  Type of placemap's node
    //***********************************
    struct placeMap_T
    {
        char *placeName;//地名字串
        int level;//地名层次
        int num;            //serial number；第几个地名
        placeProp_T *pProp;//注释信息列表
        placeMap_T *pFather;//父节点
        placeMap_T *pBrother;//兄弟节点
        placeMap_T *pSon;//子节点
        placeMap_T *pNext;  //pNext point to the next place by index  首字相同的下个节点指针
        placeMap_T *pTruePlace; //alias places will set this pointer to their original place, others set NULL 别名节点
    };

    //***********************************
    //  Type of global map
    //***********************************
    struct globalMap_T
    {
        placeMap_T *pPlace;//地名节点指针，指向第一个地名
        placeMap_T *index[256][256];//地名首字索引
        placeMap_T *areaCode[1000];//区号2地名索引
        placeMap_T *numPlace[PLACE_DEAL_LEVEL][PLACE_NUM_MAX];//地名编号2地名节点;地名别名使用真实地名的编号，不加入numPlace数组
        double *pProp;//阈值数组
    };
    
    //***********************************
    //  Type of placeNodeInfo
    //  this is used only for transfering 
    //  parameter of gloable variable.
    //  db's user need not know it.
    //***********************************
    struct placeNodeInfo_T
    {
        char buf[LINE_LEN_MAX];//地址文件的一行；不含空格；格式: 地名#注释信息
        char *propHead;//注释信息在buf中的偏移量
        int level;//地名的级数
    };

#### 搜索结果
    //***********************************
    //  Type of placeNameList
    //  this is used as the type of search result.
    //***********************************
    struct placeList_T
    {
        char *name;             //place name
        double times;           //appeared times
        double suffixTimes;     //last part's suffix appeared times
        int firstLevel;         //the level of name's first part
        int lastLevel;          //the level of name's last part
        placeMap_T *pLastPlace; //this is only used to dedup, no use to users  该地址字串最底层地名对应的地址树节点
        placeList_T *next;
        int firstDis;//该地址第一次出现，距离正文开头的距离
        int badDis;//该地址第一次出现，距离badword的距离    
    };

### 现有的词典
nerdict
posdict
rqdict
worddict
idf