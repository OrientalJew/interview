#### 主线程更新UI

主线程在oncreate中可以通过子线程快速更新UI，因为此时Activity还没绑定到ActivityThread，之后不行是考虑到线程安全，同步的问题；（[http://www.cnblogs.com/lao-liang/p/5108745.html）](http://www.cnblogs.com/lao-liang/p/5108745.html）)

> onstart\(\)、onPostCreate也是可以的，都是在Activity被绑定之前执行；

1，子线程理论上是能够更新**自己**的UI的，前提是拥有自己的ViewRootImpl；

2，每一个Activity都有自己的ViewRootImpl，在handleResumeActivity时，ActivityThread会将当前将要显示的Activity的DecorView

通过WindowManagerGlobal的addView方法添加到窗口中；

3，添加的同时，会为DecorView创建一个ViewRootImpl，而在ViewRootImpl的构造方法中，会将其线程绑定到UI线程中；

```
mThread = Thread.currentThread();
```

4，检查用户是否在非法线程更新，实际是通过调用ViewRootImpl：

```
    void checkThread() {
        if (mThread != Thread.currentThread()) {
            throw new CalledFromWrongThreadException(
                    "Only the original thread that created a view hierarchy can touch its views.");
        }
    }
```

可以知道，当在其他线程创建的View拥有自己的ViewRootImpl，即使在非UI线程，也是能够更新自己的UI的；

_因为ViewRootImpl实在Resume之后才会将创建，也就是说，在resume之前，我们在子线程更新UI，是没有ViewRootImpl来进行检查的，所以onResume之前，我们可以在子线程更新UI；_

