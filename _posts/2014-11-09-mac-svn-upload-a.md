---
layout: post
title:  mac下SVN上传.a静态库文件
date:   2014-11-09 22:20:09
category: "技术"
header-img: "img/post-bg-03.jpg"
---
突然发现公司SVN库里没保存.a文件，都是在各自的电脑上，问了同事，原因是传不上，果断解决一下mac下SVN上传.a静态库文件的问题。

用命令行可以解决此问题。
打开终端，cd 进入到需要上传的.a文件所在的文件夹。 确保 ls能看到.a文件
然后使用命令，如：svn add libzbar.a
使用完成后出现
 A  (bin)  libzbar.a
表示添加成功，用svn图形管理工具就可以看到，刚才添加的.a文件，此时就可以手动上传了。

1.打开终端，输入cd，空格，然后将需要上传的.a文件所在的文件夹（不是.a文件）拖拽到终端（此办法无需输入繁琐的路径，快捷方便） ，回车；
2.之后再输入如下命令：svn add libGoogleAnalytics.a，回车；
3.之后会出现：A (bin) libGoogleAnalytics.a
表示添加成功，打开Versions就可以看到，刚才添加的.a文件，此时就可以手动上传了。

Mac Xcode/Subversion：vi ~/.subversion/config，修改global-ignores的值。

 以上也是修改svn默认排除文件的方法。
