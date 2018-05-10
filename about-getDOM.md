# differences between methods to access DOM node
被腾讯老爹坑过的一道题。

1。 querySelector() querySelectorAll()
返回类型分别是node类型的object 和 非活动 的nodelist

2。 getElementById() 
返回类型是Element类型的Object

3。 getElementsByClassName() getElementByTagName()
返回类型是 活动 的HTMLCollection

4。 getElementsByName()
返回类型是 活动 的nodelist

5. getElementsByTagNameNS(Namespace, name)
返回类型是 活动 的nodelist。但是没怎么用过。 element.namespace似乎已经过时。

nodelist 和 HTMLCollection 都是类似数组，可以直接用list[index]的格式对其操作。
HTMLCollection，从定义上是一直活动的。对DOM的更新操作会对其造成影响。

（待测试：活动与非活动，定位到某一元素之后能否对其修改特性）
