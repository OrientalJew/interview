#### 扩展方法不存在方法覆盖的问题

> 子类和父类定义了相同的扩展方法，当用父类类型接收子类实例进行赋值后，通过该父类变量调用的扩展方法仍然是父类扩展方法中的实现。即扩展方法并不满足Lisk替换原则；
>
> 对于Kotlin的扩展方法，并不存在子类重写父类的扩展方法的情况，扩展方法依赖的是调用者的静态类型，也就是其声明的数据类型，而不是调用者的动态类型，也即实际类型；

```
fun View.showOff() = println("I'm a view!")
fun Button.showOff() = println("I'm a button!")

//按照里斯替换原则，此处的view实际上是Button的实例（动态类型是Button，静态类型是View）
val view: View = Button()

//扩展方法依赖的是静态类型，此处调用的是View的扩展函数
>>> view.showOff()
I'm a view!
```



