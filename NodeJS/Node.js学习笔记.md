### API

process,

process.argv(命令行输入的参数)，前两个参数是， 执行器命令和执行文件， 用户输入参数



webpack打包Node模块文件的原理

```__webpack_require__```函数

moduleId: 模块路径

modules:{ 

​	moudleId1: (function(module, exports, ```__webpack_require__```) {

​	}),

​	moudleId2: (function(module, exports, ```__webpack_require__```) {

​	})

}

var module = installedModules[moduleId] = {

​	i: moduleId, l: false, exports: {}

}

modules[moudleId].call(module.exports, module, module.exports, webpack_require)



Npm event-stream 事件



Node.js的事件模型

Node.js内置的EventEmiter

Node.js的异步

​	Error-first Callback

​	Node-style Callback

函数调用栈

每一个事件循环都是一个全新的调用栈， 所以在setTimeout的回调中的异常，在其外部函数无法被trycatch捕获到



Node.js的vm的使用场景



--------------------

Node.js的性能分析

工具

自带的profile

Chrome devtool

Clinic.js npm包



Node.js性能优化准则

尽量减少计算（将运行阶段的计算提前到启动阶段）， 空间换时间

如何检测内存泄漏， devtool中的memery， 内存快照

Node.js Buffer的内存分配策略， 做的很好



Node.js的子进程与线程。 child_process  work_thread

使用子进程或线程利用更多的cpu资源



多进程优化： Node.js cluster模块 

cluster的实现原理， 多个子进程如何监听同一个端口？

childProcess.on('uncaughtException', (err) => {

​	// 1. 错误上报 2. 退出进程

})

子进程中监听内存使用量，超过阈值就自动上报并退出



parentProcess.on('exit', (err) => {

​	// 监听子进程的退出， 延时5000ms再创建一个子进程。

})

子进程心跳检测



---------

#### 架构优化

1. 动静分离

  静态资源：  Cdn 分发

动态资源： nginx反向代理， 负载均衡，代理层缓存， redis缓存



2. nginx的基本概念和配置要了解

   linux基本命令要熟悉， 文件查找，日志分析等等...

3. 压测工具 ab，基本的压测指标

   前后端基本的压测指标的优化方案

---------

框架设计和工程化

架构设计： 

底层更稳固--》 程序不容易崩溃

更容易往上搭--》扩展新功能更方便

工程工具：

更加易于施工--》学习上手成本低

保证施工安全--》不会因为操作失误搞挂程序

**软件质量体系**

KISS原则 ， 渐进式



设计模式6大法则





