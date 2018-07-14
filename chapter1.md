#### Activity、DecorView、PhoneWindow、WindowManagerService、ViewRootImpl关系

每个Activity启动时都会创建一个PhoneWindow实例；

每一个PhoneWindow实例中都包含一个DecorView实例；

DecorView就是Activity根ViewGroup，是其中所有View和ViewGroup的parent；

Activity中接收到系统的消息事件是通过WindowManagerService转接的，在系统消息事件发生时\(比如Touch事件\)，WindowManagerService将回调Activity中对应的方法\(dispatchTouchEvent\)。

ViewRootImpl保存当前View在window中的绘制、布局和动画数据，相当于View与WindowManager之间的协议类；





