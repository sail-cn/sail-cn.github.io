---
layout: post
title: 利用runtime优化NSCoding协议
date:   2015-04-23 22:23:11
author:  "Yvan"
header-img: "img/post-bg-05.jpg"
---
NSKeyedArchiver 和 NSKeyedUnarchiver 提供了很方便的API把对象读取/写入磁盘。

	[NSKeyedArchiver archiveRootObject:object toFile:@"/path/to/archive"];

	object = [NSKeyedUnarchiver unarchiveObjectWithFile:@"/path/to/archive"];

	1.序列化:编码方法，归档时触发。
	-(void)encodeWithCoder:(NSCoder *)encoder 
	{
    unsigned int count = 0;
    Ivar *ivars = class_copyIvarList([XHAccount class], &count);
    for (int i = 0; i<count; i++) {
        Ivar ivar = ivars[i];
        const char *name = ivar_getName(ivar);
        NSString *key = [NSString stringWithUTF8String:name];
        id value = [self valueForKey:key];
        [encoder encodeObject:value forKey:key];
        
  	 }
   	free(ivars);
	}

	2.反序列化:解码方法，反归档时触发。
	-(id)initWithCoder:(NSCoder *)decoder
	{
    if (self = [super init]) {
        unsigned int count = 0;
        Ivar *ivars = class_copyIvarList([XHAccount class], &count);
        for (int i = 0; i<count; i++) {
            Ivar ivar = ivars[i];
            const char *name = ivar_getName(ivar);
            NSString *key = [NSString stringWithUTF8String:name];
            id value = [decoder decodeObjectForKey:key];
            [self setValue:value forKey:key];
        }
        free(ivars);
    }
    return self;
	}
