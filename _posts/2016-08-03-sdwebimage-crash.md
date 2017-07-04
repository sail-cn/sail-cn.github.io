---
layout: post
title:  SDWebImage设置大尺寸图片导致崩溃
date:   2016-08-03 17:30:20
author:  "Yvan"
header-img: "img/post-bg-06.jpg"
---
今天产品跟我说了一个bug:进入“最美摄影”列表页，图片一刷出来就崩溃。
我发现进入这个页面后，内存变化剧烈，突然升高50M，然后回落正常。(Tips:Xcode关闭僵尸对象才显示内存。)
但是我找了很久没也发现什么问题，就是用sdwebimage加载图片，关闭加载图片后就不崩溃了，那么问题肯定出在sdwebimage上。用Instruments的Allocations查看，内存会跳到800M（不知道这跟Xcode显示的内存使用情况有什么区别?）。于是我尝试在每张图片下载后释放sdwebimage内存，但无济于事。后来在网上找了下，把图片压缩一下，完美解决。

	[[SDImageCache sharedImageCache] queryDiskCacheForKey:url2 done:^(UIImage *image, EMSDImageCacheType cacheType) {
	                        if (image) {
	                            iv.image = [UIImage compressImageWith:image];
	                        }else{
	                            //download image
	                            [[SDWebImageDownloader sharedDownloader] downloadImageWithURL:[NSURL URLWithString:url2] options:EMSDWebImageDownloaderLowPriority progress:^(NSInteger receivedSize, NSInteger expectedSize) {} completed:^(UIImage *image, NSData *data, NSError *error, BOOL finished) {
                                iv.image = [UIImage compressImageWith:image];
                            }];
                        }
                    }];
                    
 压缩图片分类：
 
	 /** 图片压缩 */
	+ (UIImage *)compressImageWith:(UIImage *)image
	{
	    float imageWidth = image.size.width;
	    float imageHeight = image.size.height;
	    float width = 640;
	    float height = image.size.height/(image.size.width/width);
	    
	    float widthScale = imageWidth /width;
	    float heightScale = imageHeight /height;
	    
	    // 创建一个bitmap的context
	    // 并把它设置成为当前正在使用的context
	    UIGraphicsBeginImageContext(CGSizeMake(width, height));
	    
	    if (widthScale > heightScale) {
	        [image drawInRect:CGRectMake(0, 0, imageWidth /heightScale , height)];
	    }
	    else {
	        [image drawInRect:CGRectMake(0, 0, width , imageHeight /widthScale)];
	    }
	    
	    // 从当前context中创建一个改变大小后的图片
	    UIImage *newImage = UIGraphicsGetImageFromCurrentImageContext();
	    // 使当前的context出堆栈
	    UIGraphicsEndImageContext();
	    
	    return newImage;
    
	}
