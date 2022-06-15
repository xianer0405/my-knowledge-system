# 1. TypeScript类型体操掘金小册学习笔记



### 2022.05.23 

1. 静态类型系统的目的是把类型检查从运行时提前到编译时
2. 







## 1. TS的三种类型系统

1. 简单类型系统：基础类型，复杂类型(联合类型，交叉类型，接口,  类)
2. 支持泛型的类型系统： 将变化的类型使用泛型定制，实际使用时传入实际的类型
3. 支持类型编程的类型系统： 对传入的类型参数(泛型）做逻辑运算，产生新的类型

### 类型套路总结

1. 模式匹配做提取

   extends结合infer的使用， 提取类型中局部类型

   示例一： 提取数组提一个元素

   ```typescript
   type GetLast<Arr extends unknown[]> = 
       Arr extends [...unknown[], infer Last] ? Last : never;
   ```

   示例二： 提取函数的参数或返回值类型

   ```typescript
   type GetParameters<Func extends Function> = 
       Func extends (...args: infer Args) => unknown ? Args : never;
   
   type GetReturnType<Func extends Function> = 
       Func extends (...args: any[]) => infer ReturnType 
           ? ReturnType : never;
   ```

   示例三： 利用模板字符串类型，做字符串的匹配，替换，转换逻辑

   *** 模板字符串特性是TypeScript4.1 新增的特性 ***

   ```typescript
   type Whitespace = ' ' | '\n' | '\r' | '\t'
   // 条件类型， 匹配， 推导，递归
   type TrimLeft<
     S extends string,
     P extends string = Whitespace
   > = S extends  `${P}${infer R}` ? TrimLeft<R, P> : S
   
   type Str = TrimLeft<' \r    \t12123'>;
   
   // 剔除尾部空白字符
   type TrimRight<Str extends string, P extends string = Whitespace> =
       Str extends `${infer Rest}${P}` ? TrimRight<Rest, P> : Str
   
   type Str2 = TrimRight<'12123 \r    \t', Whitespace>
   
   
   type IsStartWith<T extends string, Prefix extends string> = T extends `${Prefix}${string}` ?  true : false;
   type isStartWith_Shanghai = IsStartWith<'Shanghai is a very old city', 'Shanghai'>;
   
   // 匹配一个模式， 提取想要的部分， 也可以用这些再构造一个新的类型
   type ReplaceStr<Str extends string, From extends string, To extends string> =
       Str extends `${infer Prefix}${From}${infer Suffix}` ? `${Prefix}${To}${Suffix}` : Str
   
   type ReplaceStrRes = ReplaceStr<'Shanghai is old city', 'old', 'modern'>
   ```

   

   示例四： 提取函数的参数、返回值的类型

   ```typescript
   type GetReturnType<F extends Function> = F extends (...args: any[]) => infer R ? R : never;
   
   // 参数类型可以是任意类型，也就是 any[]
   //（注意，这里不能用 unknown，因为参数类型是要赋值给别的类型的，而 unknown 只能用来接收类型，所以用 any）。
   // unknown是Top type 不能赋值给别的类型， any即是Top type又是Bottom type所以可以赋值给别的类型
   type GetReturnType2<F extends Function> = F extends (...args: unknown[]) => infer R ? R : never;
   
   type ReturnRes = GetReturnType<Add<number>>
   type ReturnRes2 = GetReturnType2<Add<number>>
   ```
   
   示例五： 提取构造函数的返回值类型
   
   ```typescript
   // 通过匹配模式，提取构造器函数的参数和返回值类型
   class Person {
       name: string;
       constructor(name: string) {
           this.name = name;
       }
   }
   interface PersonConstructor {
       new (name: string): Person
   }
   type GetInstanceType<ConstructorType extends new (...args: any) => any> =
       ConstructorType extends new (...args: any) => infer InstanceType ? InstanceType : any
   
   type PersonConstr = GetInstanceType<PersonConstructor>
   type isPersion = PersonConstr extends Person ? true : false
   
   
   type GetInstanceParameterType<ConstructorType extends new (...args: any) => any> =
       ConstructorType extends new (...args: infer ParameterType) => any ? ParameterType : any
   
   type PersionParamerType = GetInstanceParameterType<PersonConstructor>
   ```
   
   



1. 重新构造做变换

   *** TS类型系统支持3种可以声明任意类型的变量： type infer 类型参数 ***

   *** TypeScript 的 type、infer、类型参数声明的变量都不能修改，想对类型做各种变换产生新的类型就需要重新构造 ***

   - 数组、字符串、函数等类型的重新构造比较简单。

   - 索引类型，也就是多个元素的聚合类型的重新构造复杂一些，涉及到了映射类型的语法

   

   对数组的重新构造

   示例一 

   ```typescript
   type Zip<One extends unknown[], Another extends unknown[]> =
       One extends [infer First1, infer Second1]
           ? Another extends [infer First2, infer Second2]
               ? [[First1, First2], [Second1, Second2]]
               : []
           : []
   
   type _Keys = ['name', 'age']
   type _Values = ['jackie', 30]
   type ZipRes = Zip<_Keys, _Values>
   
   type ZipEnhance<One extends unknown[], Another extends unknown[]> =
       One extends [infer First1, ...infer Rest1]
           ? Another extends [infer First2, ...infer Rest2]
               ? [[First1, First2], ...ZipEnhance<Rest1, Rest2>]
               : []
           : []
   
   type ZipRes1 = ZipEnhance<[1,2,3,4,5], ['a', 'b', 'c', 'd', 'e']>
   ```

   

   对字符串的重新构造

   示例一

   ```html
   type CamelCase<Str extends string> =
       Str extends `${infer Left}_${infer Right}${infer Rest}`
           ? `${Left}${Uppercase<Right>}${CamelCase<Rest>}`
           : Str
   type _T3 = 'dong_dong_dong_dong_dong';
   
   // 错误的推断
   // 第一次：  Left _ Right Rest
   //          dong _ dong  _dong_dong_dong  结果  dongDong_dong_dong_dong
   
   // 第二次：  Left _ Right Rest
   //               _ dong  _dong_dong       结果  dongDongDong_dong_dong
     
     
   // 正确的推断, 证明是采用最短匹配模式
   // 第一次：  Left _ Right Rest
   //          dong _ d  ong_dong_dong_dong  结果  dongDong_dong_dong_dong
     
   // 第一次：  Left     _  Right Rest
   //          dongDong _  d     ong_dong_dong  结果  dongDongDong_dong_dong
     
   type CamelCaseRes = CamelCase<_T3>
     
     
   type D1<Str extends string> =
       Str extends `${infer Left}_${infer Right}${infer Rest}`
           ? `${Left}${Uppercase<Right>}`
           : Str
   type D1Res = D1<_T3> //  type D1Res = "dongD"
   ```

   

   对索引类型的重新构造， 分为对值的重新构造， 对key的***重映射***

   ```typescript
   type UppercaseKey<Obj extends object> = { 
       [Key in keyof Obj as Uppercase<Key & string>]: Obj[Key]
   }
   // 使用as对Key进行重映射，达到修改Key的目的
   
   
   type FilterByValueType<
       Obj extends Record<string, any>,
       ValueType
   > = {
       [Key in keyof Obj as ValueType extends Obj[Key] ? Key : never] : Obj[Key]
   }
   
   interface Person {
       name: string;
       age: number;
       hobby: string []
   }
   
   type FilterRes = FilterByValueType<Person, string | number>
   
   // *** [Key in keyof Obj as ValueType extends Obj[Key] ? Key : never] : Obj[Key] ***
   // 这行类型运算如何理解ValueType extends Obj[Key]
   // TODO 
   // 
   ```

   

   对索引类型的重新构造， 增加索引修饰符， 减少修饰符，提取部分key， 等等

   

   

2. 递归复用做循环

   示例一

   ***如何计算数组类型的长度***

   ```typescript
type BuildArray<Length extends number, Ele = unknown, Arr extends unknown[] = []> =
       Arr['length'] extends Length
           ? Arr
           : BuildArray<Length, Ele, [...Arr, Ele]>
   
   // 这里的5需要理解为数字字面量类型
   type numArr = BuildArray<5, number>
   
   // 访问数组类型的长度属性
   ```
   

   

   
3. 数组长度做计数

   

4. 联合分散可简化

   ***数组转联合类型的方法***

   type UnionFromArr<T extends unknown[]> = T[number]

   

   **联合类型中的每个类型都是相互独立的，TypeScript 对它做了特殊处理，也就是遇到字符串类型、条件类型的时候会把每个类型单独传入做计算，最后把每个类型的计算结果合并成联合类型。**

   示例一

    // NOTE:  ***很重要***

   - A extends A 不是没意义，意义是取出联合类型中的单个类型放入 A
   - A extends A 才是分布式条件类型， [A] extends [A] 就不是了，只有左边是单独的类型参数才可以。

   ````typescript
   type Combination<A extends string, B extends string> =
       A | B | `${A}${B}` | `${B}${A}`
   type Result = Combination<'A', 'B' | 'C'>
   
   
   ````

   <img src="/Users/jackie/Library/Application Support/typora-user-images/image-20220424170231664.png" alt="image-20220424170231664" style="zoom:20%;" />

5. 特殊特性要记清

   - isAny

     **any 类型与任何类型的交叉都是 any，也就是 1 & any 结果是 any。**

     

   - 

6. TS内置的类型

   示例一

   ```typescript
   type Record<K extends keyof any, T> = {
       [P in K]: T;
   };
   
   type IndexTypes = keyof any
   // 可以得到能够作为映射类型key的所有类型
   ```

   <img src="/Users/jackie/Library/Application Support/typora-user-images/image-20220425124101465.png" alt="image-20220425124101465" style="zoom:50%;" />

   示例二 

   ```typescript
   // Awaited， 内置的提取的Promise类型的高级类型
   type _Awaited<T> =
       T extends null | undefined
           ? T
           : T extends object & { then(onfulfilled: infer F): any }
               ? F extends ((value: infer V, ...args: any) => any)
                   ? _Awaited<V>
                   : never
               : T
   // 自己实现的， 与内置的有什么区别
   type DeepPromiseValueType2<T> =
           T extends Promise<infer ValueType>
               ? DeepPromiseValueType2<ValueType>
               : T;
   ```



## 2. 函数重载

三种函数重载的方式

**取重载函数的 ReturnType 返回的是最后一个重载的返回值类型**

- 第一种方式

  

  ```typescript
  function func(name: number): number;
  function func(name: string): string;
  function func(name: any): any {
      if (typeof name === 'string') {
          return `name is ${name}`;
      } else {
          return name
      }
  }
  
  type ReturnType1 = ReturnType<typeof func> // string
  ```

  

- 第二种方式

  

  ```typescript
  // 使用接口来定义函数， 该接口有多个函数定义，来实现重载
  interface Func {
      (name: string): string;
      (name: number): number;
  }
  
  const funcA: Func = (name: any): any => {
      if (typeof name === 'string') {
          return `name is ${name}`;
      } else {
          return name
      }
  }
  
  type ReturnType2 = ReturnType<Func> // number
  ```

  

- 第三种方式

  

  ```typescript
  // 第三种重载的实现方式,  使用函数的交叉类型
  type Func2 = ((name: string) => string) & ((name: number) => number);
  
  type ReturnType3 = ReturnType<Func2> // number
  
  // 返回最后一个重载的返回值的特性，实际应用场景
  
  // 联合转交叉
  // U extends U 是触发分布式条件类型，构造一个函数类型，通过模式匹配提取参数的类型，利用函数参数的逆变的性质，就能实现联合转交叉。
  // 函数参数是一个逆变的位置，触发联合转交叉的特性
  // 因为函数参数的类型要能接收多个类型，那肯定要定义成这些类型的交集，所以会发生逆变，转成交叉类型。
  type UnionToIntersection<U> =
      (U extends U ? (x: U) => unknown : never) extends (x: infer R) => unknown
          ? R
          : never;
  
  type UnionToFuncIntersection<T> = UnionToIntersection<T extends any ? () => T : never>;
  
  type UnionToFuncIntersectionRes = UnionToFuncIntersection<string|number>
  
  
  // 对联合类型的各种基本操作特性， 要熟悉其场景
  // 1. 联合类型 A extends A的分布式类型判断的特性
  // 2. Exclude对联合类型进行删除的功能，在递归逻辑中的应用
  // 3. XXX  需要整理一下。
  type UnionToTuple<T> = UnionToIntersection<T extends any ? () => T : never> extends () => infer ReturnType
          ? [...UnionToTuple<Exclude<T, ReturnType>>, ReturnType]
          : [];
  
  type UnionToTupleRes = UnionToTuple<string|number>
```
  
  

## 3. 结构化类型 VS 名义上的类型

1. Structure typing , ts是结构化类型， Java等是Nominal Typing 名义类型

2. Top Type  /  Bottom Type

3. 类型系统层级关系，  联合类型， 派生类型 -> 基类， 字符字面量类型 -> 原始类型

   父类和子类可以相互断言

   两个子类可以通过 先断言到TopType或基类做中转，再断言到对应的兄弟类型

4. 索引签名 + 映射类型 + 索引类型访问

5. 协变与逆变

6. 分布式条件类型

7. 交集，并集，补集，差集

<img src="/Users/jackie/Library/Application Support/typora-user-images/image-20220423134001959.png" alt="image-20220423134001959" style="zoom: 15%;" />

### 4. 声明文件

#### 1. 举例

示例jQuery的全局变量声明

```ts
declare var jQuery: (selector: string) => any;

jQuery('#foo');
```

`declare var` 并没有真的定义一个变量，只是定义了全局变量 `jQuery` 的类型，仅仅会用于编译时的检查，在编译结果中会被删除。

#### 2. 什么是声明文件

通常我们会把声明语句放到一个单独的文件（`jQuery.d.ts`）中，这就是声明文件

声明文件必需以 `.d.ts` 为后缀。

```typescript
// src/jQuery.d.ts
declare var jQuery: (selector: string) => any;

// src/index.ts
jQuery('#foo');
```



#### 3. 书写声明文件

在不同的场景下，声明文件的内容和使用方式会有所区别。

库的使用场景主要有以下几种：

<img src="/Users/jackie/Library/Application Support/typora-user-images/image-20220429114818876.png" alt="image-20220429114818876" style="zoom:25%;" />

##### 3.1 全局变量

全局变量是最简单的一种场景，之前举的例子就是通过 `<script>` 标签引入 jQuery，注入全局变量 `$` 和 `jQuery`。

