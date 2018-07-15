#### ViewRootImpl

一个Activity对应一个ViewRootImpl；

在Activity Resume时，系统将DecorView添加到了window中，此时会为Activity创建一个ViewRootImpl，同时在ViewRootImpl中为每一个View初始化mAttachInfo信息，此时才是真正的进行View的布局和绘制操作；

ViewRootImpl的performTraversals方法，每次执行invalidate方法，都会被执行一遍，进行View的测量、布局和绘制；

ViewRootImpl控制着Activity中包括了DecorView在内的所有的View；





