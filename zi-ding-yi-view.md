#### View绘制流程

1、ViewRootImpl中调用ViewGroup.draw\(1\)；

2、如果

3、ViewGroup中判断是否绘制自己\(WILL\_NOT\_DRAW是否设置，背景是否透明\)；

* 如果WILL\_NOT\_DRAW设置或者背景为透明，则不绘制自己，不执行onDraw\(\)
* 如果WILL\_NOT\_DRAW设置为false或者背景不透明，则绘制自己，执行onDraw\(\)

4、调用dispatchDraw\(这个方法只在ViewGroup中才有实现\)方法，绘制childView；

5、如果是View则是空实现；如果是ViewGroup，dispatchDraw中会调用drawChild\(\)，实际上最终还是回到调用View或者ViewGroup的draw\(1\)方法中，继续重复；



https://www.jianshu.com/p/ad63cba2a3c0



