#### 事件分发机制

> Activity和ViewGroup的onTouchEvent默认是不处理事件的，而View根据种类而定；

![](/assets/事件分发机制.png)

**一次触摸屏幕事件分发的过程：**

> 触摸事件是由InputManagerService分发给Activity的，不是WindowManagerService！

> 在ViewRootImpl的setView方法被调用时，WindowInputEventReceiver被创建，并绑定了InputChannel用来接收Touch事件，当事件到来时，通过InputManagerService将事件通知给WindowInputEventReceiver，再通过ViewRootImpl来分发给View的dispatchTouchEvent的。 [https://yq.aliyun.com/articles/41488](https://yq.aliyun.com/articles/41488%29%29\)

Touch事件传递给View：

![](/assets/1344733-9f7bba51649fd01e.png)

View传递事件给InputManagerService：

![](/assets/1344733-9f70457e2fa9b91b.png)

* ViewGroup需要消耗Touch事件

![](/assets/事件分发1.png)

* 子View需要消耗Touch事件

![](/assets/事件分发2.png)

> 可以看出，在子View需要消耗Touch事件的情况下，后续事件仍然会一直走ViewGroup的onInterceptTouchEvent。

* 子View不需要消耗Touch事件，ViewGroup需要消耗Touch事件\(不在OnInterceptTouchEvent中拦截\)

![](/assets/事件分发4.png)

> 子View不需要消耗Touch事件时，事件会交回给ViewGroup进行处理，如果此时ViewGroup需要处理Touch事件，则后续事件直接交给ViewGroup的onTouchEvent，不会再走onInterceptTouchEvent流程。
>
> 可见，如果ViewGroup需要消耗事件但是不在onInterceptTouchEvent中进行拦截，则只有在子View不消耗事件的情况下，其才能有机会消耗事件。

* 子View和ViewGroup都不需要消耗Touch事件

![](/assets/事件分发5.png)

> 在子View和ViewGroup都不需要事件的情况下，事件最终会回到Activity的dispatchTouchEvent中，并交给onTouchEvent处理；
>
> 同一次操作的后续事件只会传递到Activity这一层上，不会继续往下传。

#### 正确拦截Touch事件

* 不应该在onInterceptTouchEvent中做任何Touch事件处理，onInterceptTouchEvent应该只作为View是否拦截获取当前事件的依据；

onInterceptTouchEvent方法中无法获取到完整的Touch事件流，并且在某些场景下，子View可以通过调用  
parent.requestDisallowInterceptTouchEvent\(true\)，绕过父View的onInterceptTouchEvent方法，来请求事件不被父View拦截\(内部拦截法\)；

* 在多点触摸的场景下，如果需要获取多点的数据，则在判断事件时，需要增加事件掩码：event.getAction\(\)&MotionEvent.ACTION\_MASK ，这样才能够接收到ActionIndex &gt; 0 的其他点的触摸事件，否则我们只能够接收到ActionIndex = 0的点的事件；



