#### RecyclerView二级缓存

* 一级缓存：detach操作后的缓存mAttachedScrap、mCachedViews

> detach操作让ChildView脱离RecyclerView，但并不会将其从RecyclerView中移除。比如，在初始时，在RecyclerView执行测量之前，onLayoutChildren会被先执行，用于辅助测量，之后就会调用detach，将ChildView暂时的回收。紧接着，在进入布局阶段是，就会马上重用被detach掉的ChildView。\(只是没有被layout，还是在子View集合中的\)

一级缓存中存在两个缓存池，一个为mAttachedScrap，用来缓存当前屏幕上被detach掉的ViewHolder，这里的ViewHolder很快又会被重用。

另一个为mCachedViews，这个缓存池的默认大小为2,用来保存屏幕上下方两个没有显示，但即将被显示的ViewHolder。这里的ViewHolder都是有绑定数据。

两者区别在于，mAttachedScrap是attch在RecyclerView中，即算在RecyclerView的childCount中，而mCachedViews则不是，只是绑定了特定位置的数据。

注意：mCachedViews中的ViewHolder虽然已经被remove出RecyclerView的child View集合中，但是其绑定的数据还是有效的，在被重新绑定回RecyclerView时，无需进行onBindViewHolder绑定数据的操作，直接layout到recyclerView中即可使用。

> 回收时机：
>
> detachAndScrapAttachedViews\(recycler\);//detach轻量回收所有View
>
> detachAndScrapView\(view, recycler\);//detach轻量回收指定View

* 二级缓存：detach后没有重用，或已经划出屏幕的ChildView，按ItemType将ViewHolder保存到RecyclerViewPool中；

每种ViewType会相应缓存5个ViewHolder；

这些ViewHolder不再与RecyclerView绑定，而且其中的数据也是无效的，处于半废弃状态，当要被重用时，其数据和状态都需要被重置；

> 回收时机：
>
> removeAndRecycleView\(View child, Recycler recycler\)
>
> removeAndRecycleAllViews\(Recycler recycler\);
>
> removeAndRecycleScrapInt\(mRecycler\);
>
> recycleAndClearCachedViews\(\);



