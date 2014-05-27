Linux 系统
---
# Ubuntu
## 中文乱码
### GBK显示乱码
#### 添加系统支持
ubuntu默认不支持gbk，修改/var/lib/locales/supported.d/local文件,在文件中添

    zh_CN.GBK GBK
    zh_CN.GB2312 GB2312
    sudo dpkg-reconfigure --force locales
然后在输出的结果中会出现

    zh_CN.GB2312 done
    zh_CN.GBK done
这样, Ubuntu就支持GBK编码了
#### 显示配置
打开vim的配置文件，位置在/etc/vim/vimrc，增加以下三行

    set fileencodings=utf-8,gb2312,gbk,gb18030
    set termencoding=utf-8
    set encoding=prc
保存退出，此时vim就能正确显示中文了。