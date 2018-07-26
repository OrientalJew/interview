#### 主线程更新UI

主线程在oncreate中可以通过子线程快速更新UI，因为此时Activity还没绑定到ActivityThread，之后不行是考虑到线程安全，同步的问题；（[http://www.cnblogs.com/lao-liang/p/5108745.html）](http://www.cnblogs.com/lao-liang/p/5108745.html）)

> onstart\(\)、onPostCreate也是可以的，都是在Activity被绑定之前执行；



