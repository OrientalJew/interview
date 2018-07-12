#### 实现思路

* 方案1：纯事件分发机制

Touch事件由父View进行拦截，当父View消耗够Touch事件后，再主动分发一次down事件\(通过dispatchTouchEvent\(\)\)，子View在消耗了down事件后，开始接收和消耗后续的Touch事件；

> 在传统的事件分发机制下，父View一旦拦截并消耗了down事件，则子View无法再接收到Touch事件，除非我们主动再分发一次down事件，将Touch事件的消耗权移交出去；

* 方案2：NestedScrolling机制

NestedScrolling机制下，Touch事件的接收对象和分配权都交给子View。子View在接收到Move事件后，并不会立即滑动自身，而是先检查父View是否需要消耗Move事件，需要则将消耗的权利先交给父View\(此时，Touch事件仍然是由子View进行接收\)，直到父View消耗完毕，才由自身进行消耗；





