#### 事件分发机制

> Activity和ViewGroup的onTouchEvent默认是不处理事件的，而View根据种类而定；

![](/assets/事件分发机制.png)

事件到来时，WindowManagerService回调Activity对应方法：Activity的dispatchTouchEvent最先拿到事件，此时Activity不会自己用，而是先调用子ViewGroup的dispatchTouchEvent，子ViewGroup并不是马上再调用子View的dispatchTouchEvent，而是先调用自己的onInterceptTouchEvent，判断自己是否需要事件：

#### 正确拦截Touch事件

* 不应该在onInterceptTouchEvent中做任何Touch事件处理，onInterceptTouchEvent应该只作为View是否拦截获取当前事件的依据；

onInterceptTouchEvent方法中无法获取到完整的Touch事件流，并且在某些场景下，子View可以通过调用  
parent.requestDisallowInterceptTouchEvent\(true\)，绕过父View的onInterceptTouchEvent方法，来请求事件不被父View拦截\(内部拦截法\)；

* 在多点触摸的场景下，如果需要获取多点的数据，则在判断事件时，需要增加事件掩码：event.getAction\(\)&MotionEvent.ACTION\_MASK ，这样才能够接收到ActionIndex &gt; 0 的其他点的触摸事件，否则我们只能够接收到ActionIndex = 0的点的事件；



