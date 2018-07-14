#### 判断Fragment可见性

* 如果Fragment是通过FragmentManager进行show和hide操作导致的显隐的，可以通过onHiddenChanged和其他生命周期方法进行判断；
* 如果Fragment是配合ViewPager进行显隐的，则通过Fragment的setUserVisibleHint进行判断；



#### FragmentPagerAdapter与FragmentStatePagerAdapter的区别

* 应用场景

一般在Fragment较少的情况下，适合使用FragmentPagerAdapter；

相对的，在Fragment较多的情况下，适合使用FragmentStatePagerAdapter；

* 主要区别

FragmentPagerAdapter不会销毁Fragment实例，只是销毁Fragment的视图，在Fragment被销毁时，只会执行到onDestoryView，Fragment中的成员数据不会被销毁；

FragmentStatePagerAdapter会完全销毁Fragment实例，在Fragment被销毁时，会执行到onDestory方法，如果要恢复成员变量，只能通过onSaveInstanceState来进行；





