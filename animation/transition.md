#### SharedElement、Content Transition

* 5.0之后提供的场景动画，能够同时作用于两个Activity或Fragment之间的View元素，协同进行动画；

* SharedElement和ContentTransition一般都是一起工作的，SharedElement作用于单个View，而ContentTransition作用于除那单个View之外的其他View元素；
* SharedElement动画实际是一种视觉的错觉动画。看上去，View从一个Activity跳转到另一个Activity，实际上，View从A进入到B时，在执行到enter动画时，Activity B已经被启动了，此时的B会记录View在A中的信息，复制到B中对应的SharedElement中，在exit动画之后，在B中继续执行enter动画，看上去像是连贯的过程；

* 默认情况下，SharedElement动画是开启的，而ContentTransition是关闭的，两者都可以在Activity的style中进行配置，也可以直接在onCreate时，通过window进行配置；

#### 实现微信相册的动画效果

类似于微信的相册动画效果，在展开预览的情况下，可以左右滑动图片进行切换，回退后又能够演变回对应的图片上。

这种效果涉及到两个Activity之间的SharedElement沟通的问题，在演示页面切换了图片，需要立即通知列表页面，对应的动画元素被切换了。

要实现这种效果，可以通过setExitSharedElementCallback和setEnterSharedElementCallback中实现共享元素的更换。



#### 动画场景

Activity A -&gt; Activity B

exit：exit动画从A开始执行，即将调起B协同执行动画；

enter：exit动画在A执行快结束，B衔接A，开始执行enter动画，之后exit动画结束，enter动画一直在B中执行到结束。

Activity B -&gt; Activity A

return：return动画从B开始执行，即将调起A协同执行动画；

reenter：return动画在B中执行快结束，A衔接B，开始执行reenter动画，之后return动画结束，reent动画一直在A中执行到结束。

