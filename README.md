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



#### 2.用UIView的block回调实现动画的代码封装　

```Objective-C
#pragma UIView实现动画
- (void) animationWithView : (UIView *)view WithAnimationTransition : (UIViewAnimationTransition) transition
{
    [UIView animateWithDuration:DURATION animations:^{
        [UIView setAnimationCurve:UIViewAnimationCurveEaseInOut];
        [UIView setAnimationTransition:transition forView:view cache:YES];
    }];
}
```

#### 3.改变View的背景图，便于切换时观察
```Objective-C
#pragma 给View添加背景图
-(void)addBgImageWithImageName:(NSString *) imageName
{
     self.view.backgroundColor = [UIColor colorWithPatternImage:[UIImage imageNamed:imageName]];
}
```

## 二.调用上面的方法实现我们想要的动画

#### 1.我们在View上添加多个Button,给不同的Button设置不同的Tag值，然后再ViewController中绑定同一个方法，点击不同的button实现不同的页面切换效果。storyBoard上的控件效果如下图所示：
![](http://images.cnitblog.com/blog/545446/201412/121710078375833.png)

#### 2.下面我们就开始编写点击button要回调的方法

(1).定义枚举来标示按钮所对应的动画类型，代码如下：
```Objective-C
typedef enum : NSUInteger {
    Fade = 1,                   //淡入淡出
    Push,                       //推挤
    Reveal,                     //揭开
    MoveIn,                     //覆盖
    Cube,                       //立方体
    SuckEffect,                 //吮吸
    OglFlip,                    //翻转
    RippleEffect,               //波纹
    PageCurl,                   //翻页
    PageUnCurl,                 //反翻页
    CameraIrisHollowOpen,       //开镜头
    CameraIrisHollowClose,      //关镜头
    CurlDown,                   //下翻页
    CurlUp,                     //上翻页
    FlipFromLeft,               //左翻转
    FlipFromRight,              //右翻转
    
} AnimationType;
```
(2),获取Button的Tag值：
```Objective-C
     UIButton *button = sender;
     AnimationType animationType = button.tag;
```
 

(3).每次点击button都改变subtype的值，包括上,左,下,右
```Objective-C
NSString *subtypeString;
    
    switch (_subtype) {
        case 0:
            subtypeString = kCATransitionFromLeft;
            break;
        case 1:
            subtypeString = kCATransitionFromBottom;
            break;
        case 2:
            subtypeString = kCATransitionFromRight;
            break;
        case 3:
            subtypeString = kCATransitionFromTop;
            break;
        default:
            break;
    }
    _subtype += 1;
    if (_subtype > 3) {
        _subtype = 0;
    }
```
(4),通过switch结合上边的枚举来判断是那个按钮点击的
```Objective-C
 switch (animationType)
{
     //各种Case,此处代码下面会给出  
}
```
#### 3.调用我们封装的运动方法，来实现动画效果

(1),淡化效果
```Objective-C
 case Fade:
     [self transitionWithType:kCATransitionFade WithSubtype:subtypeString ForView:self.view];
     break;
```

(2) PUSH效果
```Objective-C
 case Push:
     [self transitionWithType:kCATransitionPush WithSubtype:subtypeString ForView:self.view];
     break;
```
![](http://images.cnitblog.com/blog/545446/201412/121723047906597.png)

(3)、揭开效果
```Objective-C
 case Reveal:
     [self transitionWithType:kCATransitionReveal WithSubtype:subtypeString ForView:self.view];
     break;
```
![](http://images.cnitblog.com/blog/545446/201412/121728297283768.png)

(4)、覆盖效果
```Objective-C
case MoveIn:
    [self transitionWithType:kCATransitionMoveIn WithSubtype:subtypeString ForView:self.view];
    break;
```
![](http://images.cnitblog.com/blog/545446/201412/121730580565348.png)

(5)、立方体切换
```Objective-C
case Cube:
    [self transitionWithType:@"cube" WithSubtype:subtypeString ForView:self.view];
    break;
```
![](http://images.cnitblog.com/blog/545446/201412/121732513841605.png)

(6)、吮吸效果
```Objective-C
case SuckEffect:
    [self transitionWithType:@"suckEffect" WithSubtype:subtypeString ForView:self.view];
    break;
```
![](http://images.cnitblog.com/blog/545446/201412/121734290719149.png)

(7)、翻转效果
```Objective-C
case OglFlip:
    [self transitionWithType:@"oglFlip" WithSubtype:subtypeString ForView:self.view];
    break;
```
![](http://images.cnitblog.com/blog/545446/201412/121742192432356.png)

(8)、波纹效果
```Objective-C
case RippleEffect:
    [self transitionWithType:@"rippleEffect" WithSubtype:subtypeString ForView:self.view];
    break;
```
![](http://images.cnitblog.com/blog/545446/201412/121741041184172.png)

(9)、翻页效果
```Objective-C
case PageCurl:
    [self transitionWithType:@"pageCurl" WithSubtype:subtypeString ForView:self.view];
    break;

case PageUnCurl:
    [self transitionWithType:@"pageUnCurl" WithSubtype:subtypeString ForView:self.view];
    break;
```
![](http://images.cnitblog.com/blog/545446/201412/121745555255331.png)

(10)、相机打开效果
```Objective-C
case CameraIrisHollowOpen:
    [self transitionWithType:@"cameraIrisHollowOpen" WithSubtype:subtypeString ForView:self.view];
    break;

case CameraIrisHollowClose:
    [self transitionWithType:@"cameraIrisHollowClose" WithSubtype:subtypeString ForView:self.view];
    break;
```
![](http://images.cnitblog.com/blog/545446/201412/121748313686675.png)

(11),调用上面封装的第二个动画方法
```Objective-C
case CurlDown:
    [self animationWithView:self.view WithAnimationTransition:UIViewAnimationTransitionCurlDown];
    break;

case CurlUp:
    [self animationWithView:self.view WithAnimationTransition:UIViewAnimationTransitionCurlUp];
    break;

case FlipFromLeft:
    [self animationWithView:self.view WithAnimationTransition:UIViewAnimationTransitionFlipFromLeft];
    break;

case FlipFromRight:
    [self animationWithView:self.view WithAnimationTransition:UIViewAnimationTransitionFlipFromRight];
    break;
```
