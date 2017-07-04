---
layout: post
title:  类似豆瓣主页滑动即隐藏导航栏
date:   2016-07-12 13:11:23
author:  "Yvan"
header-img: "img/post-bg-03.jpg"
---
今天实现了类似豆瓣主页的一个效果－－向上滑动隐藏导航栏，这样能阅读更多的内容，向下滑动显示导航栏，导航栏上的内容可以操作。把代码贴出来，以供参考。

首先加上一个状态栏同样大小的图片。在导航栏隐藏时，这个图片就贴在状态栏的位置。

	_statusbarView = [[UIImageView alloc] initWithFrame:CGRectMake(0, 0, kScreenWidth, 20)];
	_statusbarView.image = [UIImage imageNamed:@"navigation-bar_bg"];
	_statusbarView.hidden = YES;
	[self.view addSubview:_statusbarView];
	
接下来，实现scrollView的代理方法。

	- (void)scrollViewDidScroll:(UIScrollView *)scrollView
	{
	    int currentPostion = scrollView.contentOffset.y;
	    if (currentPostion - _lastPosition > 20  && currentPostion > 0) {
	        _lastPosition = currentPostion;
	        [self hideNaviBar];
	    }else
	        if ((_lastPosition - currentPostion > 20) && (currentPostion  <= scrollView.contentSize.height-scrollView.bounds.size.height-20) )
	    {
	        _lastPosition = currentPostion;
	        [self showNaviBar];
	    }
	}
	
隐藏导航栏的方法。

	- (void)hideNaviBar
	{
	    [self.navigationController setNavigationBarHidden:YES animated:YES];
	    [UIView animateWithDuration:.3 animations:^{
	        webView.frame = CGRectMake(0, 20, kScreenWidth, kScreenHeight-20-44);
	        self.bottomToolView.frame = CGRectMake(0, kScreenHeight-44, kScreenWidth, 44);//底部工具条
	        _statusbarView.hidden = NO;
	    }];
	}
	
显示导航栏的方法。

	- (void)showNaviBar
	{
	    [self.navigationController setNavigationBarHidden:NO animated:YES];
	    [UIView animateWithDuration:.3 animations:^{
	        webView.frame = self.webViewOriginalRect;
	        self.bottomToolView.frame = CGRectMake(0, kScreenHeight-64-44, kScreenWidth, 44);
	        _statusbarView.hidden = YES;
	    }];
	}
	
记得在viewWillDisappear时调用显示导航栏，这样就不影响跳转页面的导航栏。

<a href="#">
	<img src="http://7xqhpt.com1.z0.glb.clouddn.com/1.pic.jpg" alt="Post Sample Image">
</a>
<span class="caption text-muted">有导航栏效果图.</span>

<a href="#">
	<img src="http://7xqhpt.com1.z0.glb.clouddn.com/2.pic.jpg" alt="Post Sample Image">
</a>
<span class="caption text-muted">无导航栏效果图.</span>

