#### Activity、DecorView、PhoneWindow、WindowManagerService、ViewRootImpl关系

每个Activity启动时都会创建一个PhoneWindow实例；

每一个PhoneWindow实例中都包含一个DecorView实例；

DecorView就是Activity根ViewGroup，是其中所有View和ViewGroup的parent；

Activity中接收到系统的消息事件是通过WindowManagerService转接的，在系统消息事件发生时\(比如Touch事件\)，WindowManagerService将回调Activity中对应的方法\(dispatchTouchEvent\)。

ViewRootImpl保存当前View在window中的绘制、布局和动画数据，相当于View与WindowManager之间的协议类；



#### Activity的启动模式

* Standard

默认的Activity启动模式，每次都会创建一个新的Activity；

* SingleTop

如果目标Activity位于栈顶，则不会创建新的Activity，而是调用目标Activity的onNewIntent\(\)方法；

如果不在栈顶，则创建一个新的Activity；

* SingleTask

如果当前栈中存在目标Activity，则清空目标Activity之上的所有Activity，使其位于栈顶，然后回调onNewIntent\(\)方法；

否则创建新的Activity，并检查当前的栈标志于目标栈标识是否相同，不是，则为其创建新的栈；

* SingleInstance

目标Activity占用单独的一个栈，与默认的程序栈是分离的；如果Activity是从后台启动的，则不会回到主程序的其他页面\(大概是因为单独一个栈的原因吧\)；相当于singleTask设置了taskAffinity；

#### Activity的生命周期

onCreate-&gt;onPostCreate-&gt;\(onRestart\)onStart-&gt;onResume\(\)-&gt;onPostResume\(\)-&gt;onPause\(\)-&gt;onStop\(\)-&gt;onDestory\(\)

#### Fragment的生命周期

onAttach\(\)-&gt;onCreate\(\)-&gt;onCreateView\(\)-&gt;onViewCreated\(\)-&gt;onActivityCreate-&gt;onStart\(\)-&gt;onResume\(\)-&gt;onPause\(\)-&gt;onStop\(\)-&gt;onDestoryView\(\)-&gt;onDestory\(\)-&gt;onDetach\(\)

#### 面向对象三大特性：封装、继承、多态

* 封装

将一个事务的所有特性，功能等使用一个类进行抽象描述。并且，对于其中需要进行隐藏的成员进行访问权限的设置，防止外界对其进行修改；

* 继承

一个类可以通过继承的方式来拥有另一个类的所有开放的特性和功能；

* 多态

我们可以创建一个接口或抽象类的变量来接收其子类的实例；

对于接口或抽象类中被子类实现的方法，我们通过接口或抽象类变量进行调用，其呈现的结果将根据赋予的子类的实例的不同，而不同；





























