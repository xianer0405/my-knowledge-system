### 1. 什么是 module-augmentation

1. BetterScroll是如何扩展插件属性的

   基于模块扩充的能力 [module-augmentation](https://link.juejin.cn/?target=https%3A%2F%2Fwww.typescriptlang.org%2Fdocs%2Fhandbook%2Fdeclaration-merging.html%23module-augmentation)

   ```typescript
   // observable.ts
   export class Observable<T> {
     //...
   }
   
   // map.ts, 扩展一个接口，并实现
   import { Observable } from './oobservable'
   declare module './observable' {
     interface Observable<T> {
       map<U>(f: (x: T) => U): Observable<U>;
     }
   }
   Observable.prototype.map = function (f) {
     // ... another exercise for the reader
     
   };
   
   // consumer.ts
   import { Observable } from "./observable";
   import "./map";
   let o: Observable<number>;
   o.map((x) => x.toFixed());
   
   ```

   > 说明： 有两个限制
   >
   > 1. You can’t declare new top-level declarations in the augmentation — just patches to existing declarations.
   > 2. Default exports also cannot be augmented, only named exports (since you need to augment an export by its exported name, and `default` is a reserved word

   

2. 插件属性如何在输入时提示
3. 实例如何智能提示引入的插件属性

> 知识点: 什么是协变和逆变

- “型变”分为两种，一种是子类型可以赋值给父类型，叫做协变（covariant），一种是父类型可以赋值给子类型，叫做逆变（contravariant）
- 函数的参数是一个逆变位置
- 变量，对象属性， 函数返回值都是协变位置
- 配置 `strictFunctionTypes: true`  仅支持函数参数的逆变， 设置为false表示双向协变

