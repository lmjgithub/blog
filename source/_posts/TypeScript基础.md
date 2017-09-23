---
title: TypeScript基础
date: 2017-09-07 19:24:07
categories:
- TypeScript
tags: TypeScript
---

_void_ : 空值。在 TypeScirpt 中，可以用 void 表示没有任何返回值的函数。声明一个 void 类型的变量没有什么用，因为你只能将它赋值为 undefined 和 null。
_null_: 定义数据类型为null。null 类型的变量只能被赋值为 null。
_undefined_: 定义数据类型为undefined。undefined 类型的变量只能被赋值为 undefined。
_undefined 和 null 是所有类型的子类型。也就是说 undefined 类型的变量，可以赋值给 number 类型的变量,而void 类型的变量不能赋值给 number 类型的变量_
_any_ : 允许赋值为任意类型。访问任何属性都是允许。允许调用任何方法。声明一个变量为任意值之后，对它的任何操作，返回的内容的类型都是任意值。
_interface_ : 接口对行为的抽象，而具体如何行动需要由类实现。定义的变量比接口少了或多了一些属性是不允许的。当属性定义了可选属性时仍然不允许添加未定义的属性。定义了任意属性，那么确定属性和可选属性都必须是它的子属性。

```javascript
// interface任意属性。
interface Person {
  name: string;
  age?: number;
  [propName: string]: string;
}

let xcatliu: Person = {
  name: 'Xcat Liu',
  age: 25,
  website: 'http://xcatliu.com',
};
// 任意属性的值允许是 string，但是可选属性 age 的值却是 number，number 不是 string 的子属性，所以报错了。
```

_readonly_ : 定义只读属性.只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候.

_函数声明_:一个函数有输入和输出，要在 TypeScript 中对其进行约束，需要把输入和输出都考虑到。

```javascript
// 声明函数
function sum(x: number, y: number): number {
  return x + y;
}
// 输入多余的（或者少于要求的）参数，是不被允许的
sum(1, 2, 3); // error
```

```javascript
// 函数表达式
let sum: (x: number, y: number) => number = function (x: number, y: number): number {
  return x + y;
};
```

```javascript
// 通过接口定义函数符合的形状
interface Sum {
  (x: number, y: number): number;
}

let sum: Sum;
sum = function(x: number, y: number) {
  return x + y;
}
```

```javascript
// 可选参数
function sum(x: number, y: number, z?: number): number {
  if (z) {
    return x + y + z;
  } else {
    return x + y;
  }
}
// 可选参数后面不允许再出现必须参数
function errorSum(x: number, z?: number, y: number): number {
  if (z) {
    return x + y + z;
  } else {
    return x + y;
  }
}
```

```javascript
// 默认参数
// TypeScript 会将添加了默认值的参数识别为可选参数.此时就不受「可选参数必须接在必需参数后面」的限制
function sum(x: number,y?: number,z: number = 0) {
    return x + y + z;
}
```

```javascript
// 剩余参数
// rest 是一个数组
// rest 参数只能是最后一个参数
function push(array, ...rest: any[]) {
  items.forEach(function(item) {
    array.push(item);
  });
}
```

```javascript
// 重载
// 重载允许一个函数接受不同数量或类型的参数时，作出不同的处理。
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
  if (typeof x === 'number') {
    return Number(x.toString().split('').reverse().join(''));
  } else if (typeof x === 'string') {
    return x.split('').reverse().join('');
  }
}
// 我们重复定义了多次函数 reverse，前几次都是函数定义，最后一次是函数实现.
// TypeScript 会优先从最前面的函数定义开始匹配，所以多个函数定义如果有包含关系，需要优先把精确的定义写在前面
```

_联合类型_ : 取值可以为多种类型中的一种.当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型里共有的属性或方法：
_类型断言_ : 绕过编译器的类型推断，手动指定一个值的类型.通过类型断言可以访问其中的属性或方法。类型断言不是类型转换，断言成一个联合类型中不存在的类型是不允许的：

```javascript
// 类型断言
function getLength(something: string | number): number {
  if ((<string>something).length) {
    return (<string>something).length;
  } else {
    return something.toString().length;
  }
}
function getLength(something: string | number): number {
  if ((something as string).length) {
    return (<string>something).length;
  } else {
    return something.toString().length;
  }
}
```

_declare_ : 类型声明只会用于编译时的检查，编译结果中会被删除。
_内置对象_ : 参考<https://github.com/Microsoft/TypeScript/tree/master/src/lib>
_type_ : 类型别名用来给一个类型起个新名字；

```javascript
type Direction = 'ltr' | 'rtl';
let direction: Direction = 'rtl';
```

_元组_ : 数组合并了相同类型的对象，而元组（Tuple）合并了不同类型的对象。

```javascript
// 定义元祖
let xcatliu: [string, number] = ['Xcat Liu', 25];
// 当直接对元组类型的变量进行初始化或者赋值的时候，需要提供所有元组类型中指定的项。
let xcatliu: [string, number] = ['Xcat Liu'];  // ERROR
// 当赋值给越界的元素时，它类型会被限制为元组中每个类型的联合类型：
let xcatliu: [string, number];
xcatliu = ['Xcat Liu', 25, 'http://xcatliu.com/']; // SUCCESS
xcatliu = ['Xcat Liu', 25, trye]; // ERROR
```

_枚举_ : 枚举（Enum）类型用于取值被限定在一定范围内的场景.

```javascript
// 定义枚举
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
// 编译出来
var Days;
(function (Days) {
    Days[Days["Sun"] = 0] = "Sun";
    Days[Days["Mon"] = 1] = "Mon";
    Days[Days["Tue"] = 2] = "Tue";
    Days[Days["Wed"] = 3] = "Wed";
    Days[Days["Thu"] = 4] = "Thu";
    Days[Days["Fri"] = 5] = "Fri";
    Days[Days["Sat"] = 6] = "Sat";
})(Days || (Days = {}));
// 手动赋值
// 未手动赋值的枚举项会接着上一个枚举项递增。
enum Days {Sun = 3, Mon = 1, Tue, Wed, Thu, Fri, Sat};
var Days;
(function (Days) {
    Days[Days["Sun"] = 3] = "Sun";
    Days[Days["Mon"] = 1] = "Mon";
    Days[Days["Tue"] = 2] = "Tue";
    Days[Days["Wed"] = 3] = "Wed";
    Days[Days["Thu"] = 4] = "Thu";
    Days[Days["Fri"] = 5] = "Fri";
    Days[Days["Sat"] = 6] = "Sat";
})(Days || (Days = {}));

// 手动赋值的枚举项可以不是数字，此时需要使用类型断言来让tsc无视类型检查
enum Days {Sun = 7, Mon, Tue, Wed, Thu, Fri, Sat = <any>"S"};
enum Days {Sun = 7, Mon, Tue, Wed, Thu, Fri, Sat = ("S" as any)};
```