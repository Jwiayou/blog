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

`undefined` 类型的变量只能被赋值为 `undefined`, `null` 类型的变量智能被赋值为 `null`。与 `void` 的区别是，`undefined` 和 `null` 是所有类型的子类型。也就是说 `undefined` 类型的变量，可以赋值给 `number` 类型的变量：
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
​```

也允许条用任何方法：

​```TypeScript
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

## 类型推轮
如果没有明确的制定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断一个新的类型

### 什么是类型推论
以下代码虽然没有指定类型，但是会在编译的时候报错：
```TypeScript
let myFavoriteNumber = 'serven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```

事实上，它等价于：
```TypeScript
let myFavoriteNumber: string = 'seven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```

## 联合类型
联合类型（Union Types) 表示取值可以为多种类型中的一种。

### 简单的例子
```TypeScript
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```
联合类型使用 `|` 分隔每个类型。
这里的 `let myFavoriteNumber: string | number` 的含义是，允许 `myFavoriteNumber` 的类型是 `string` 或者 `number`, 但是不能是其他类型。

### 访问联合类型的属性的方法
当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们**只能访问此联合类型的所有类型里共有的属性或方法:**
```TypeScript
function getLength() {
   return something.length
}

// index.ts(2,22): error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.
```
上例中， `length` 不是 `string` 和 `number` 的共有属性，所以会报错。

访问 `string` 和 `number` 的共有属性是没有问题的：
```TypeScript
function getString(something: string | number): string {
    return something.toString();
}
```

联合类型的变量在被赋值的时候，会根据类型推论的规则出一个类型：
```TypeScript
let myFavoriteNumber: string | number
myFavoriteNumber = 'serven'
console.log(myFavoriteNumber.length); //5
myFavoriteNumber = 7;
console.log(myFavoriteNumber.length);

// index.ts(5,30): error TS2339: Property 'length' does not exist on type 'number'.
```
上例中，第二行的 `myFavoriteNumber` 被推断成了 `string`， 访问它的 `length` 属性不报错。
而第四行的 `myFavoriteNumber` 被推断成了 `number`, 访问它的 `length` 属性就报错了。

## 对象的类型--接口
在 TypeScript 中，我们使用接口（interfaces）来定义对象的类型。

### 什么是接口
在面向对象语言中，接口（Interfaces)是一个很重要的概念，它是对行为的抽象，而具体如何行动需要由类（classes）去实现（implements）。
TypesScript 中的接口是一个非常灵活的概念，除了可用于对想类的一部分行为进行抽象以外，也常用于对 「对象的形状（Shape）」进行描述。

### 简单的例子
```TypeScript
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
};
```
上面例子中，我们定义了一个接口 `Person`，接着定义了一个变量 `tom`，它的类型是 `person`。这样，我们就约束了 `tom` 的形状必须和接口 `person` 一致。
定义的变量比接口少了一些属性是不允许的, 多一些属性也是不允许的：
```TypeScript
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tome',
}

// index.ts(6,5): error TS2322: Type '{ name: string; }' is not assignable to type 'Person'.
//   Property 'age' is missing in type '{ name: string; }'.

interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
}

// index.ts(9,5): error TS2322: Type '{ name: string; age: number; gender: string; }' is not assignable to type 'Person'.
//   Object literal may only specify known properties, and 'gender' does not exist in type 'Person'.
```

### 可选属性
有时我们希望不要完全匹配一个形状，那么可以用可选属性：
```TypeScript
interface Person {
    name: string;
    age?: number;
}

let tom: Person = {
    name: 'Tom'
};
```
```TypeScript
interface Person {
    name: string;
    age?: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
}
```
 
可选属性的含义是该属性可以不存在。
这时**仍然不允许添加未定义的属性**

## 任意属性
有时候我们希望一个接口允许有任意的属性，可以使用如下方式
```TypeScript
interface Person {
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    name: 'Tom',
    gender: 'male'
};
```

使用 `[propName: string]` 定义任意属性取 `string` 类型的值
需要注意的是， **一旦定义了任意属性，那么确定属性和可选属性都必须是他的子属性：**
```TypeScript
interface Person {
    name: string;
    age: number;
    [propName: string]: string;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
}

// index.ts(3,5): error TS2411: Property 'age' of type 'number' is not assignable to string index type 'string'.
// index.ts(7,5): error TS2322: Type '{ [x: string]: string | number; name: string; age: number; gender: string; }' is not assignable to type 'Person'.
//   Index signatures are incompatible.
//     Type 'string | number' is not assignable to type 'string'.
//       Type 'number' is not assignable to type 'string'.
```
 上例中，任意属性的值允许是 `string`，但是可选属性 `age` 的值却是 `number`, `number` 不是 `string` 的子属性，所以报错了。
 另外，在报错信息中可以看出，此时 `{name: 'Tom', age: 25, gender: 'male' }` 的类型被推断成了 `{ [x: string]: string | number; name: string; age: number; gender: string; }`, 这是联合类型的接口结合。

## 只读属性
有时候我们希望对象中的一些字段只能在创建的时候被赋值，那么可以用 `readonly` 定义制度属性：
```TypeScript
interface Person {
    readonly id: number;
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    id: 89757,
    name: 'Tom',
    gender: 'made'
};

tom.id = 9527;

// index.ts(14,5): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
```
上例中，使用 `readonly` 定义的属性 `id` 初始化后，又被赋值了，所以报错了。
**注意，只读的约束在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候：
```TypeScript
interface Person {
    readonly id: number
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    name: 'Tom',
    gender: 'male'
};

tom.id = 89757;

// index.ts(8,5): error TS2322: Type '{ name: string; gender: string; }' is not assignable to type 'Person'.
//   Property 'id' is missing in type '{ name: string; gender: string; }'.
// index.ts(13,5): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
```
上例中，报错信息有两处，第一处是在对 `tom` 进行赋值的时候，没有给 `id` 赋值。
第二处是给 `tom.id` 赋值的时候，由于它是只读属性，所以报错了。

## 数组的类型
在 TypeScript 中， 数组类型有多种定义方式，比较灵活。

###「类型 + 方括号」来表示数组：
最简单的方法是使用「类型 + 方括号」来表示数组：
```TypeScript
let fibonacci: number[] = [1, 1, 2, 3, 5];
```
数组的项中`不允许`出现其他的类型：
```TypeScript
let fibonacci: number[] = [1, '1', 2, 3, 5];

// index.ts(1,5): error TS2322: Type '(number | string)[]' is not assignable to type 'number[]'.
//   Type 'number | string' is not assignable to type 'number'.
//     Type 'string' is not assignable to type 'number'.
```
上例中，`[1, '1', 2, 3, 5]` 的类型被推断为 `（number | string）[]`,这是联合类型和数组的结合。
数组中的一些方法的参数也会根据数组在定义时约定的类型进行限制：
```TypeScript
let fibonacci: number[] = [1, 1, 2, 3, 5];
fibonacci.push('8');

// index.ts(2,16): error TS2345: Argument of type 'string' is not assignable to parameter of type 'number'.
```
上例中，`push`方法只允许传入 `number` 类型参数，但是却传入一个  `string` 类型的参数，所以报错了

### 数组泛型
也可以使用数组泛型（Array Generic） `Array<elemType>` 来表示数组：
```TypeScript
let fibonacci: Array<number> = [1, 1, 2, 3, 5];
```

关于泛型，可以参考泛型一章。

###  用接口表示数组
接口也可用来比描述数组
```TypeScript
interface NumberArray {
    [index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5];
```

any在数组中的应用

一个比较常见的做法是，用 `any` 表示数组中允许出现任意类型：
```TypeScript
let list: any[] = ['Xcat Liu', 25, {website: 'http://xcatliu.com' }];
```

### 类数组
类数组（Array-like Object） 不是数组类型，比如 `arguments`:
```TypeScript
function sum() {
    let args: number[] = arguments;
}

// index.ts(2,7): error TS2322: Type 'IArguments' is not assignable to type 'number[]'.
//   Property 'push' is missing in type 'IArguments'.
```

事实上常见的类数组都有自己的接口定义， 如 `IArguments`,`NodeList`,`HTMLCollection`等：
```TypeScript
function sum() {
    let args: IArguments = arguments;
}

// index.ts(2,7): error TS2322: Type 'IArguments' is not assignable to type 'number[]'.
//   Property 'push' is missing in type 'IArguments'.
```

## 函数的类型

### 函数声明
再 JavaScript 中， 有两种常见的定义函数的方式 -- 函数声明（Function Declaration）和函数表达式（Function Expression）:
```TypeScript
// 函数声明（Function Declaration）
function sum(x, y) {
    return x + y;
}

// 函数表达式（Funtion Expression)
let mySun = function (x, y) {
    return x + y;
}
```

一个函数的输入和输出，要在TypeScript中对其进行约束，需要把输入和输出都考虑到，其中函数声明的类型定义较简单：
```TypeScript
function sum(x: number, y: number): number } {
    return x + y;
}
```

注意，**输入多余的（或者少于要求的）参数，是不被允许的：
```TypeScript
function sum(x: number, y: number): number {
    return x + y;
}

sum(1, 2, 3);

// index.ts(4,1): error TS2346: Supplied parameters do not match any signature of call target.
```
```TypeScript
function sum(x: number, y: number): number {
    return x + y;
}

sum(1);

// index.ts(4,1): error TS2346: Supplied parameters do not match any signature of call target.
```

### 函数表达式
如果要我们现在写一个对函数表达式（Funtion Expression）的定义，可能会写成这样：
```TypeScript
let mySun = function (x: number, y: number) {
    return x + y;
}; 
```
 这是可以通过编译的，不过事实上，上面的代码只是对等号右侧的匿名函数进行了类型定义，而等号左边的 `mySum`，是通过赋值操作进行类型推论而推断出来的。如果需要我们手动给 `mySun` 添加类型，则应该是这样：
 ```TypeScript
 let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
     return a + b;
 }
 ```
 注意不要混淆了 TypeScript 中的 `=>` 和 ES6 中的 `=>`。
 在 TypeScript 的类型定义中，`=>` 用来表示函数的定义，左边的输入类型，需要用括号括起来，右便是输出类型。

### 用接口定义函数的形状
我们也可以用接口的方式来顶一个函数需要符合的形状：
```TypeScript
interface SearchFunc {
    (sounrce: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}
```

### 可选参数
前面提到，输入多余的（或者少于要求的）参数，是不允许的。那么如何定义可选的参数呢？
与接口中的可选属性类似，我们用 `?` 表示可选的参数：
```TypeScript
function buildName(firstName: string, lastName?: string) {
    if(lastName) {
        return firstName + ' ' + lastName;
    } else {
        return firstName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```
需要注意的是，可选参数必须接再必需参数后面。换句话说，**可选参数后面不允许再出现必须参数了：**
```TypeScript
function buildName(firstName?: string, lastName: string) {
    if(firstName) {
        return firstName + ' ' + lastName;
    } else {
        return lastName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom')

// index.ts(1,40): error TS1016: A required parameter cannot follow an optional parameter.
```

### 参数默认值

在 ES6 中， 我们允许给函数的参数缇娜家默认值，**TypeScript会将加了默认值的参数识别为可选参数：**
```TypeScript
function buildName(firstName: string, lastName: string = 'Cat') {
    return firstName + ' ' + lastName;
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```

此时就不受「可选参数必须在必需参数后面」的限制了：
```TypeScript
function buildName(firstName: string = 'Tom', lastName: string) {
    return firstName + ' ' + lastName;
}
let tomcat = buildName('Tom', 'Cat');
let cat = buildName(undefined, 'Cat');
```
