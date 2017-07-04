---
layout: post
title:  iCloud自动备份导致被拒
date:   2014-12-18 20:53:33
author:  "Yvan"
header-img: "img/post-bg-04.jpg"
---

最近上线的一个项目被苹果拒了，原因是iCloud备份了不该备份的东西。
Document目录中是iCloud自动备份的，除了用户自己创建的文件，其他的都不能让iCloud备份。设置不备份：

	NSString *documentsDirectory = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) lastObject];
	NSURL * fileURL = [NSURL fileURLWithPath:documentsDirectory];
	[fileURL setResourceValue:[NSNumber numberWithBool:YES] forKey: NSURLIsExcludedFromBackupKey error: nil];
