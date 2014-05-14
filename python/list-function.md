list的函数
--
功能：将字符创转化为列表，例：

    >>> word = 'hello'
    >>> name = list(word)
    >>> name
    ['h', 'e', 'l', 'l', 'o']
    
### 列表基本函数：

#### 1.元素赋值，例：

    >>> list1 = [1, '234', 2]
    >>> list1
    [1, '234', 2]
    >>> list1[0] = 'hel'
    >>> list1
    ['hel', '234', 2]
    
注意：通过list[0]= 'hel',如果原来位置上有值，会覆盖掉原来的。

#### 2.分片操作  
1）显示序列，例：

    >>> list1
    ['hel', '234', 2]
    >>> list1[1:2]
    ['234', 2]
    >>> list1[1:]
    ['234', 2]
    >>> list1[:]
    ['hel', '234', 2]
注意：（1）list1[beg:end]将显示列表的从list1[beg]到list1[end-1]的元素，list1[end]不会显示
（2）list1[beg:end]省略beg，默认beg= 0; 省略end默认end = len(list1)。因此list1[:]显示整个列表。

2）修改序列，例：

    >>> list1
    ['hel', '234', 2]
    >>> list1[1:] = list('hello')
    >>> list1
    ['hel', 'h', 'e', 'l', 'l', 'o']
    
3）插入序列，例：

    >>> list1
    ['hel', 'h', 'e', 'l', 'l', 'o']
    >>> list1[2:2] = [1, 2, 3]
    >>> list1
    ['hel', 'h', 1, 2, 3, 'e', 'l', 'l', 'o']
    >>> list1[2:2] = 'love'
    >>> list1
    ['hel', 'h', 'l', 'o', 'v', 'e', 1, 2, 3, 'e', 'l', 'l', 'o']
**注意：**往list1的某个位置插入列表或字串时，列表的每项、字串的每个字符都会作为list1的一个元素，而不会整体插入。

**思考：**那作为整体插入咋办？
    
    >>> list6
    [3, 'a']
    >>> list6.insert(1, [2, 3, 4]
    >>> list6
    [3, [2, 3, 4], 'a']
    >>> list6.insert(0, 'hello')
    ['hello', 3, [2, 3, 4], 'a']
    
4）删除序列，例：

    >>> list1
    ['hel', 'h', 'l', 'o', 'v', 'e', 1, 2, 3, 'e', 'l', 'l', 'o']
    >>> list1[2:len(list1)] = []
    >>> list1
    ['hel', 'h']
    
#### 3.count函数
功能：统计列表中某元素出现的次数。例：

    >>> list2
    [1, 2, 3, 3]
    >>> list2.count(3)
    2
    
#### 4.len函数
功能：统计列表中元素的个数。例：

    >>> list2
    [1, 2, 3, 3]
    >>> len(list2)
    4
    
#### 5.append函数
功能:往列表的最后一个位置插入（入栈）操作。例：

    >>> list2
    [1, 2, 3, 3]
    >>> list2.append('hello')
    >>> list2
    [1, 2, 3, 3, 'hello']

扩展：可以”+“号 来实现列表的相加。例：

    >>> a = ['a', 'b', 'c']
    >>> b = ['d', 'e', 'f']
    >>> c = ['g', 'h', 'i']
    >>> a + b + c
    ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i']
         
#### 6.extend函数
功能：修改原序列，链接两个序列产生新的序列。例：

    >>> a = [1, 2, 3]
    >>> b = ['a', 'b', 'c']
    >>> a.extend(b)
    >>> a
    [1, 2, 3, 'a', 'b', 'c']
    >>> b
    ['a', 'b', 'c']

#### 7.insert函数
功能：将元素插入到列表的指定位置。例：

    >>> list2.insert(1, 'hello')
    >>> list2
    [2, 'hello', 3]
    
#### 8.pop函数
功能：删除指定位置元素。例：

    >>> list2
    [1, 2, 3, 3, 'hello']
    >>> list2.pop()
    'hello'
    >>> list2
    [1, 2, 3, 3]
    >>> list2.pop(0)
    [2, 3, 3]
 **注意：**pop(n)，n指明在列表中的位置，如果pop(),默认弹出最后一个元素(出栈操作）。
        
#### 9.remove函数
功能：删除第一个指定元素。例：

    >>> list2.remove(3)
    >>> list2
    [2, 3]

**思考：**怎样删除所有的指定元素？

    >>> list6 = [2, 2, 3, 'a', 2]
    >>> while 2 in list6:
            list6.remove(2)
    ...
    >>> list6
    [3, 'a']
    
#### 10.index函数
功能：从列表中找出与某个元素匹配的第一个匹配项的位置

    >>> list3 = [1, 2, 3, 4, 4]
    >>> list3.index(4)
    3
    >>> list3.index(5)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    ValueError: 4 is not in list

#### 11.reverse函数
功能：翻转列表。例：

    >>> list3
    [1, 2, 3, 4, 4]
    >>> list3.reverse()
    >>> list3
    [4, 4, 3, 2, 1]
   
#### 12.sort函数
功能：队员列表进行排序

    >>> list4 = [2, 1, 44, 0]
    >>> list4.sort()
    >>> list4
    [0, 1, 2, 44]
注意：sort函数修改了原序列,这里如果是采用b = a的方式，那么b和a指向同一个列表。例：

    >>> a = [1, 13, 4, 2]
    >>> b = a
    >>> b.sort()
    >>> a
    [1, 2, 4, 13]
    >>> b
    [1, 2, 4, 13]

**思考：**那么如何不改变原序列呢？
方法一：可以利用sorted()函数。例：

    >>> a = [1, 13, 4, 2]
    >>> b = sorted(a)
    >>> a
    [1, 13, 4, 2]
    >>> b
    [1, 2, 4, 13]

方法二：创建副本。例：

    >>> a = [1, 13, 4, 2]
    >>> b = a[:]
    >>> b.sort()
    >>> a
    [1, 13, 4, 2]
    >>> b
    [1, 2, 4, 13]
    
注意： 对于列表a:
    b = a   那么b和a都指向同一个列表
    b = a[:] 那么吧创建了一个列表副本

##### 关键字排序：key
长度(len)排序：  

    >>> list4 = ['aaa', 'cccc', 'bb']
    >>> list4.sort(key=len)
    >>> list4
    ['bb', 'aaa', 'cccc']

**关键字排序：reverse()**

    >>> list4 = [3, 1, 13]
    >>> list4.sort(reverse = True)
    >>> list4
    [13, 3, 1]
    >>> list4.sort(reverse = False)
    >>> list4
    [1, 3, 13]
注意：reverse = True降序，reverse = False升序
      
#### 13.cmp函数
功能：比较两个元素的大小。例：

    >>> cmp(10,10)
    0
    >>> cmp(10,1)
    1
    >>> cmp(1,10)
    -1

**注意：**
（1）两个元素相同返回0，前大后小返回1，前小后大返回-1
（2）比较的对象是元素首个字符的ascii值，例：

    >>> cmp('a', 'b')
    -1
    >>> cmp('aa', 'cc')
    -1
    >>> cmp('aaa', 3333)
    1

#### 14. set函数
功能：列出列表中不重复的元素（去重）集合。例：

    >>> list1 = [1, 1, 2, 2, 2, 3]
    >>> list2 = set(list1)
    >>> list2
    set([1, 2, 3])
    >>> print list2
    set([1, 2, 3])
    >>> list2[2]
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: 'set' object does not support indexing

**注意：**利用set() 函数后就变成了集合，集合例元素无序，再利用list2[2]就出错了。