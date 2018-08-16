## 布尔值

注意，使用构造函数 `Boolean` 创造的对象不是布尔值：
```TypeScript
let createByNewBoolean: boolean = new Boolean(1);
// index.ts(1,5): error TS2322: Type 'Boolean' is not assignable to type 'boolean'.
```

事实上 `new Boolean（）` 返回的是一个 `Boolean` 对象：
```TypeScript
let createdByNewBoolean: Boolean = new Boolean(1);
```

直接调用 `Boolean` 也可以返回一个 `boolean` 类型：
```TypeScript
let createdByBoolean: boolean = Boolean(1);
```

> 在 TypeScript 中， `boolean` 是 JavaScript 中的基本类型，而 `Boolean` 是 JavaScript 中的构造函数。其他基本类型（除了 `null` 和 `undefined`) 一样，不再赘述。

## 数值

使用 `number` 定义数值类型:
```TypeScript
// ES6 中的二进制表示法
let binaryLiteral: number = 0b1010;
// ES6 中的八进制表示法
let octalLiteral: number = 0o744;
```
> 其中 `0b1010` 和 `0o744` 是 ES6 中的二进制和八进制表示法，它们会被编译为十进制数字。

## 字符串

使用 `string` 定义字符串类型：
```TypeScript
let myName: string = 'Tom';
let myAge: number = 25;
// 模板字符串
let sentence: string = `Hello, my name is ${myName}.
I'll be ${myAge + 1} years old next month.`;
```
编译结果：
```
var myName = 'Tom';
var myAge = 25;
// 模板字符串
var sentence = "Hello, my name is " + myName + ".\nI'll be " + (myAge + 1) + " years old next month.";
```

## 空值

JavaScript 没有空值 （void）的概念，在TypeScirpt中，可以用 `void` 表示没有任何返回值的函数：
```Typescript
function alertName(): void {
    alert('My name is Tom')
}
```
声明一个 `void` 类型的变量没有声明用，因为你只能将它赋值为 `undefined` 和 `null`:
```TypeScript
let unusable: void = undefined
```

## Null 和 Undefined

在TypeScript中，可以使用 `null` 和 `undefined` 来定义这两个原始数据类型：
```TypeScript
let u: undefined = undefined;
let n: null = null;
```

`undefined` 类型的变量只能被赋值为 `undefined`, `null` 类型的变量智能被赋值为 `null`。
与 `void` 的区别是，`undefined` 和 `null` 是所有类型的子类型。也就是说 `undefined` 类型的变量，可以赋值给 `number` 类型的变量：
```TypeScript
// 这样不会报错
let num: number = undefined;
```

```
let u: undefined:
let num: number = u;
```

而 `void` 类型的变量不能赋值给 `number` 类型的变量:
```
let u: void;
let num: number = u;

// index.ts(2,5): error TS2322: TYpe 'void' is not assignable to type 'number'.
```

## 任意值

任意值（Any) 用来表示允许赋值为任意类型。

### 什么是任意值类型

如果是一个普通类型，在赋值过程中改变类型是不允许的：
```TypeScript
let myFavoriteNumber: string = 'seven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```

但如果是 `any` 类型，则允许被赋值为任意类型。
```TypeScript
let myFavoriteNumber: any = 'sevena';
myFavoriteNumber = 7;
```

### 任意值的属性和方法
在任意值上访问任何属性都是允许的：
```TypeScript
let anyThing: any = 'hello';
console.log(anyThing.myName);
console.log(anyThing.myName.firstName)
````
也允许条用任何方法：
```TypeScript
let anyThing: any = 'hello';
anyThing.setName('Jerry');
anyThing.setname('Jerry').sayHello();
anyThing.myName.setFirstName('Cat');
```
可以认为，**声明一个变量为任意值之后，对它的任何操作，返回的内容的类型都是任意值**

### 未声明类型的变量
**变量如果在声明的时候，未指定其类型，那么它会被识别为任意值类型：**
```TypeScript
let something;
something = 'seven';
something = 7;

something.setName('Tom');
```
等价于
```TypeScript
let something: any;
something = 'seven';
something = 7;

something.setName('Tom');
```

## 未完待续。。。。。