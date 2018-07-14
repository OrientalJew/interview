#### Handler消息机制

* Handler在创建时，就会在构造方法中拿到当前线程的Looper，而Looper保存有当前线程的消息队列，也会和Handler绑定到一起；

* Handler在哪个线程被创建，就会绑定到哪个线程中；
* 主线程默认会创建一个MainLooper，而如果在子线程中创建Handler，就需要我们手动为其创建一个Looper，并绑定到Handler中；
* 使用Looper.prepare\(\)即可为当前线程创建一个Looper，每一个Looper中都自带一个消息队列；



#### Handler消息机制相关的内存泄露

> 任何超过Activity生命周期的成员，都有可能造成内存泄露；

Handler作为Activity成员变量时，会持有Activity的引用，而Message又持有了Handler的引用，形成了引用链。当我们通过Handler发送延时消息时，由于消息所在的队列位于UI线程，是全局性的，当Activity销毁时，消息可能还存在于消息队列中，这将导致Activity的引用无法被及时释放。

解决方案：

Handler和Runnable都使用静态内部类的形式进行创建\(或者干脆创建成单独的实现类\)，因为静态内部类不属于任何类实例，所以也不会持有任何类的实例，在Activity中使用，如果需要持有Activity的实例，可以使用弱引用进行包裹。

