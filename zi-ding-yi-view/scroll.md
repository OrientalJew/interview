#### Scroll效果

* 无论是scrollBy还是scrollTo，滑动的实际是View的内容，而不是View本身相对于窗口的滑动，如果View自身需要实现滑动，应该通过父布局调用scroll方法来实现；
* scrollBy和scrollTo在传入参数为正值时，View 的内容是向左或上滑动的。从视觉上看，相当于当前View窗口向右或向下移动；

#### Scroller

* Scroller本身并不能让View滑动，其作用是用来计算滑动的距离。通过Scroller计算滑动距离，实际的滑动需要调用scrollTo和scrollBy来实现；
* Scroller通过invalidate\(\)来触发滑动计算，实际的滚动效果在computeScroll\(在该方法中通过Scroller拿到当前滑动的偏移量，在调用scrollTo或scrollBy进行位移\)方法中进行；



