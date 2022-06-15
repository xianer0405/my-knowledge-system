### 1. 混合类型实现

```typescript
interface PersonInstance {
  name: string
}
class PersonConstructor {
    name: string;

    constructor(name: string) {
        this.name = name
    }
    getName() {
        return this.name;
    }
}

// 用到了声明合并
interface PersonConstructor extends PersonInstance {
    age: number
}

function createPerson(name: string): PersonConstructor {
    return new PersonConstructor(name);
}

type createPerson = typeof createPerson

interface PersonFactory extends createPerson {
  new (name: string): PersonConstructor
}

const PersonMix = (createPerson as unknown) as PersonFactory
let p2 = new PersonMix('Jackie');
let p3 = PersonMix('Jackie');
console.log(p2.getName() === p3.getName())
```



2. 逆变和协变的理解

```typescript
// 开启strictFuntionTypes时
function fnWithChildParam(x: string) {
    console.log("Hello, " + x.toLowerCase());
}
type FnWithChildParam = typeof fnWithChildParam

type FnWithParentParam = (ns: string | number) => void;

const funcWithParentParam = (ns: string | number): void => {
    if (typeof ns === 'number') {
        console.log(ns.toFixed(2))
    } else {
        console.log(ns)
    }
}

// 正常子类可以赋值给父类， 但函数参数在逆变位置，所以不能进行赋值。
// 因为具有调用包含父类参数的函数传递多参数类型更多， 包含子类参数的函数无法处理其他类型
// let funcWithParentParam: FnWithParentParam = fnWithChildParam;
// funcWithParentParam(10);

// 如果交换位置，则没有问题， 因为传入包含父类参数的函数能够处理所有类型
let func2WithiChildParam: FnWithChildParam = funcWithParentParam;
func2WithiChildParam('sdf');
```

