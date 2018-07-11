#### 实现RecyclerView顶部Header

* 方案1：

外部包裹FrameLayout，添加一个HeaderView布局，通过ScrollListener监听滑动，实行顶部联动效果；

* 方案2：

重写dispathDraw方法，直接将headerView绘制进RecyclerView的Canvas中\(直接调用headerView的draw\(canvas\)进行绘制\)，并配合scrollListener进行控制。

* 方案3：

使用ItemDecoration进行定制，在onDrawOver中绘制联动效果的Decoration；



