# AIArtist
AIArtist technique sum up



## lib sum 
* [KLCPopup](https://github.com/jmascia/KLCPopup)   @pop up a view with animation

* [PXAlertView](https://github.com/alexanderjarvis/PXAlertView) alert view that can customs replace systom alertvc






## knowledge sum 
1. 多个地方会调用(公用)的类或者控制器, 比如分享弹窗控制器, 登录控制器, 应在init方法传入来源控制器的信息(source), 可能用于统计.
2. 协议, 可以弥补无法多继承的缺点.  协议即是增加方法, 面向协议(面向接口)开发更加规范.
3. block用于回调, 完成某些代码之后开始执行block, 或者完成某些代码之后回调返回结果(有参或有返回值).
4. 善于利用通知, 一对多的作用. 比如账户信息变化影响很多界面使用通知, 比如很多地方做分享, 分享成功都发送通知(分散功能用通知聚合, 统一处理).
5. 线程安全, 关于数据处理逻辑的代码, 应该新建一个类来管理, 对可能造成线程冲突的属性加锁.
