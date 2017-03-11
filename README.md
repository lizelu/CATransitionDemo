CATransitionDemo
================

##iOS开发常用页面切换效果
因工作原因，有段时间没发表博客了，今天就发表篇博客给大家带来一些干货，切勿错过哦。今天所介绍的主题是关于动画的，在之前的博客中也有用到动画的地方，今天就好好的总结一下iOS开发中常用的动画。说道动画其中有一个是仿射变换的概念，至于怎么仿射的怎么变换的，原理如何等在本篇博客中不做赘述。今天要分享的是如和用动画做出我们要做的效果。

今天主要用到的动画类是CALayer下的CATransition至于各种动画类中如何继承的在这也不做赘述，网上的资料是一抓一大把。好废话少说切入今天的正题。
###一.封装动画方法

####1.用CATransition实现动画的封装方法如下，每句代码是何意思，请看注释之。
```Objective-C
#pragma CATransition动画实现
- (void) transitionWithType:(NSString *) type WithSubtype:(NSString *) subtype ForView : (UIView *) view
{
    //创建CATransition对象
    CATransition *animation = [CATransition animation];
    
    //设置运动时间
    animation.duration = DURATION;
    
    //设置运动type
    animation.type = type;
    if (subtype != nil) {
        
        //设置子类
        animation.subtype = subtype;
    }
    
    //设置运动速度
    animation.timingFunction = UIViewAnimationOptionCurveEaseInOut;
    
    [view.layer addAnimation:animation forKey:@"animation"];
}
```

代码说明：

* CATransition常用的属性如下：

* duration:设置动画时间

* type:稍后下面会详细的介绍运动类型

* subtype:和type匹配使用，指定运动的方向，下面也会详细介绍

* timingFunction ：动画的运动轨迹，用于变化起点和终点之间的插值计算,形象点说它决定了动画运行的节奏,比如是

* 均匀变化(相同时间变化量相同)还是先快后慢,先慢后快还是先慢再快再慢.　　　　

* *  动画的开始与结束的快慢,有五个预置分别为(下同):

* *  kCAMediaTimingFunctionLinear            线性,即匀速

* *  kCAMediaTimingFunctionEaseIn            先慢后快

* *  kCAMediaTimingFunctionEaseOut           先快后慢

* * kCAMediaTimingFunctionEaseInEaseOut     先慢后快再慢

* * kCAMediaTimingFunctionDefault           实际效果是动画中间比较快.



2.用UIView的block回调实现动画的代码封装　

```Objective-C
```
```Objective-C
```
###PUSH效果
![](http://images.cnitblog.com/blog/545446/201412/121723047906597.png)

###揭开效果
![](http://images.cnitblog.com/blog/545446/201412/121728297283768.png)

###覆盖效果
![](http://images.cnitblog.com/blog/545446/201412/121730580565348.png)

###立方体切换
![](http://images.cnitblog.com/blog/545446/201412/121732513841605.png)

###吮吸效果
![](http://images.cnitblog.com/blog/545446/201412/121734290719149.png)

###翻转效果
![](http://images.cnitblog.com/blog/545446/201412/121742192432356.png)

###波纹效果
![](http://images.cnitblog.com/blog/545446/201412/121741041184172.png)

###翻页效果
![](http://images.cnitblog.com/blog/545446/201412/121745555255331.png)

###相机打开效果
![](http://images.cnitblog.com/blog/545446/201412/121748313686675.png)
