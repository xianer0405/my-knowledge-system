### RESTful API 

- Representational State Trasfer

1. Representational: 数据的表现形式（JSON/ XML）
2. State: 当前状态或数据
3. Transfer： 数据传输

特点：

-  无状态Stateless

   所有用户会话信息都保存在客户端

   每次请求包含所有信息，不依赖上下文

  服务端不保存会话信息

- 缓存

- 统一接口 Uniform Interface  最突出的特点

- 分层系统， 安全层，负载均衡层， 缓存层

统一接口的限制

1. 资源的标识， 每个资源可以通过URI被唯一标识
2.  通过表述来操作资源 表述就是Representation, 比如JSON或XML
3. 自描述信息， 媒体类型，HTTP方法，是否缓存Cache-Control



GitHub的API是教科书级别的RESTful API

RESTful的请求设计规范 / 响应设计规范

1. URI使用名词, 尽量使用复数 
2. 使用嵌套关系表示关联关系 - 
3.  



状态码： 2XX 成功  3XX 重定向 4xx:客户端错误 5xx: 服务端错误



开发者友好： 文档 |  超媒体



0. 熟悉MongoDB的概念， Schema的设计思路，编程语法，重要特性。



1. npm包 ： 

   ```
   koa-body 支持文件上传， koa-bodyparser  不支持
   koa-static 构建静态资源服务器
   ```

   

2. Node API 

   Path 相关

   

3. 原生form表单， input的accept： 支持image/png, image/jpeg格式， 也支持.png, .jpeg, jpg

   ```
   Unique file type specifiers
   A unique file type specifier is a string that describes a type of file that may be selected by the user in an <input> element of type file. Each unique file type specifier may take one of the following forms:
   
   A valid case-insensitive filename extension, starting with a period (".") character. For example: .jpg, .pdf, or .doc.
   A valid MIME type string, with no extensions.
   The string audio/* meaning "any audio file".
   The string video/* meaning "any video file".
   The string image/* meaning "any image file".
   ```

   

4. Koa开发web服务的思路， 中间件思路， 洋葱模型的pre和post



5. 善用Math.max(), Math.min()

   分页接口参数的处理场景的使用

   

6. Schema的如何引用，

   1. 存在关联关系的Module，数据如何更新。

   2. 引用字段如何去掉默认查询， 数据如何关联查询populate()

   3. 如何指定查询字段（默认不支持查询的字段）

   4. 如何模糊搜索，单字段模糊搜索：find({ fieldName: regex })  

      ​                          多字段模糊搜索： find({ $or: [{ fieldName1: regex}, { fieldName1: regex}] })

   5. Module的常用api:  find, findById , save, findAndUpdate, findAndRemove,

7. 知乎用户与话题的多对多关系

8. 问题与用户，问题与话题的关系,

   