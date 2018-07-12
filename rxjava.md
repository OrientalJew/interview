#### RxJava

* RxJava是一个基于观察者模式的异步操作库，通过它能够方便快捷的实现线程的切换操作；

* RxJava是一种思想，它让我们以工作流的方式来处理任务。通过RxJava，我们能以流水线的形式操作任务，每过一个环节，相应就是一种处理\(一个操作符处理\)。

* RxJava是一种函数式编程的模式，每一个操作符实际上对应一种函数。对应有一致的输入和一致的输出。

#### 线程切换

* subscribeOn：用于切换Observable的线程，在第一次调用时有效，后面每次调用并不能切换Observable的线程，只能影响前面调用doOnSubscribe执行代码所在的线程；
* ObserveOn：每次调用都能够影响其后除了doOnSubscribe之外，其余流程运行的线程；
* doOnSubscribe：在流程中，越后调用，则越先被执行；其执行的线程由其后流程的第一个subscribeOn\(\)设置的线程决定；



#### Subject

Subject既可以作为Observable，也可以作为Observer；

Subject在RxJava中起到一种中转的作用，通过它中转的数据，我们能让原本普通的数据，汇聚成数据流，变得适合使用Rx操作符进行操作。

Subject本身就是热操作符，通过它订阅中转来发送数据，能让原本冷操作符也变为热操作符；

#### 冷、热操作符

* 冷操作符

冷操作符只有被订阅时，才会开始发送数据，并且对于每一个订阅者，虽然他们订阅的是相同的对象，但接收的却是单独的数据流。即同一时刻，不同的订阅对象受到的数据是不同的；

一般的Rx操作符都是冷操作符；

* 热操作符

热操作符可以认为是一种单例的操作符\(一般较耗资源，常驻型的使用场景\)；

多个订阅者共享一个数据流，同一时间收到的是相同的事件。并且当其中订阅者把收到的数据修改了，后面的接受者接收到的数据也会被修改；

热操作符一旦开始发送数据，不管有没有订阅者，数据都会一直发送下去；

#### 常见的热操作符

* public:将coldObservable转为connectableObservable\(hot Observable\)

* connect：让connectableObservable开始发送数据，没有订阅者也会继续发射数据，除非手动停止订阅；

* refCount：让connectableObservable在第一次订阅时就开始发送数据，无需等到调用connect，最后一个订阅者取消订阅时停止；

* reply：同样将cold Observable转为connectable Observable，也需要调用connect来启动，不同的是会缓存，确保所有订阅者不管先后，收到的数据流相同；

* share：等于先通过public转为connectableObservable，再通过refCount让connectableObservable在第一次订阅就开始发送数据；

除此之外，各种subject也是热操作符的一种形式。

