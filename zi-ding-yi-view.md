#### View绘制流程

1、ViewRootImpl中调用ViewGroup.draw\(1\)；

2、ViewGroup中判断是否绘制自己\(WILL\_NOT\_DRAW是否设置，背景是否设置\)；

* 如果WILL\_NOT\_DRAW设置或者背景为透明，则不绘制自己，调用dispatchDraw-&gt;drawChild-&gt;view.draw\(3\)



