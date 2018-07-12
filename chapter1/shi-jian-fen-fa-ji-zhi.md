#### 正确拦截Touch事件

* 不应该在onInterceptTouchEvent中做任何Touch事件处理，onInterceptTouchEvent应该只作为View是否拦截获取当前事件的依据；

onInterceptTouchEvent方法中无法获取到完整的Touch事件流，并且在某些场景下，子View可以通过调用parent.requestDisallowInterceptTouchEvent\(true\)，绕过父View的onInterceptTouchEvent方法，来请求事件不被父View拦截\(内部拦截法\)；

* 在多点触摸的场景下，如果需要获取多点的数据，则在判断事件时，需要增加事件掩码：event.getAction\(\)&MotionEvent.ACTION\_MASK ，这样才能够接收到ActionIndex &gt; 0 的其他点的触摸事件，否则我们只能够接收到ActionIndex = 0的点的事件；



