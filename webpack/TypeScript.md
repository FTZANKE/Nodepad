# TypeScript

## 1.TypeScript 安装

1. NPM 安装 TypeScript

```bash
npm install -g typescript
```

2. 安装完成后使用 **tsc** 命令来执行 TypeScript 的相关代码，以下是查看版本号：

```bash
$ tsc -v
Version 3.2.2
```

## 2.TypeScript 基础语法

TypeScript 程序由模块，函数，变量，语句和表达式，注释五部分组成。

第一个 TypeScript 程序：

1. File Name：Hello.ts

```ts
const hello : string = "Hello World!"
console.log(hello)
```

2. 首先通过 **tsc** 命令编译：

```bash
tsc Hello.ts
```

3. 使用 node 命令来执行该 js 代码

```bash
$ node Runoob.js
Hello World!
```

## 3.TypeScript 基础类型

1. **`任意类型(any)`**：声明为 any 的变量可以赋予任意类型的值。
   任意值是 TypeScript `针对编程时类型不明确的变量`使用的一种数据类型，它常用于以下三种情况。

   1. 变量的值会动态改变时，任意值类型可以让这些变量跳过编译阶段的类型检查

      ```ts
      let x: any = 1;    // 数字类型
      x = 'I am who I am';    // 字符串类型
      x = false;    // 布尔类型
      ```

   2. 改写现有代码时，任意值允许在编译时可选择地包含或移除类型检查

      ```ts
      let x: any = 4;
      x.ifItExists();    // 正确，ifItExists方法在运行时可能存在，但这里并不会检查
      x.toFixed();    // 正确
      ```

   3. 定义存储各种类型数据的数组时

      ```ts
      let arrayList: any[] = [1, false, 'fine'];
      arrayList[1] = 100;
      ```

      

2. **`数字类型(number)`**：双精度 64 位浮点值。它可以用来表示整数和分数。

   ```ts
   let binaryLiteral: number = 0b1010; // 二进制
   let octalLiteral: number = 0o744;    // 八进制
   let decLiteral: number = 6;    // 十进制
   let hexLiteral: number = 0xf00d;    // 十六进制
   ```

3. **`字符串类型(string)`**：使用单引号（**'**）或双引号（**"**）来表示字符串类型。反引号（**`**）来定义多行文本和内嵌表达式。

   ```ts
   let name: string = "Runoob";
   let years: number = 5;
   let words: string = `您好，今年是 ${ name } 发布 ${ years + 1} 周年`;
   ```

4. **`布尔类型(boolean	)`**：表示逻辑值：true 和 false。

   ```ts
   let flag: boolean = true;
   ```

5. **`数组类型`**：声明变量为数组。

   ```ts
   // 在元素类型后面加上[]
   let arr: number[] = [1, 2];
   // 或者使用数组泛型
   let arr: Array<number> = [1, 2];
   ```

6. **`元组`**：元组类型用来表示已知元素数量和类型的数组，各元素的类型不必相同，对应位置的类型需要相同。

   ```ts
   let x: [string, number];
   x = ['Hello', 1];    // 运行正常
   x = [1, 'Hello'];    // 报错
   console.log(x[0]);    // 输出 Runoob
   ```

7. **`枚举(enum)`**：枚举类型用于定义数值集合。

   ```ts
   enum Color {Red, Green, Blue};
   let c: Color = Color.Blue;
   console.log(c);    // 输出 2
   ```

8. **`void(void)`**：用于标识方法返回值的类型，表示该方法没有返回值。

   ```ts
   function hello(): void {
       alert("Hello Runoob");
       // return会报错，因为void无返回值 
   }
   ```

9. **`null(null)`**：表示对象值缺失。

10. **`undefined(undefined)`**：用于初始化变量为一个未定义的值。

11. **`never(never)`**： never 是其它类型（包括 null 和 undefined）的子类型，代表从不会出现的值。声明为 never 类型的变量只能被 never 类型所赋值，在函数中它通常表现为`抛出异常`或`死循环`

    ```ts
    let x: never;
    let y: number;
    
    // 运行错误，数字类型不能转为 never 类型
    x = 123;
    
    // 运行正确，never 类型可以赋值给 never类型
    x = (()=>{ throw new Error('exception')})();
    
    // 运行正确，never 类型可以赋值给 数字类型
    y = (()=>{ throw new Error('exception')})();
    
    // 返回值为 never 的函数可以是抛出异常的情况
    function error(message: string): never {
        throw new Error(message);
    }
    
    // 返回值为 never 的函数可以是无法被执行到的终止点的情况
    function loop(): never {
        while (true) {}
    }
    ```

    

## 4. Null 和 Undefined

1. null

在 JavaScript 中 null 表示 "什么都没有"。

null是一个只有一个值的特殊类型。表示一个空对象引用。

用 typeof 检测 null 返回是 object。

2. undefined

在 JavaScript 中, undefined 是一个没有设置值的变量。

typeof 一个没有值的变量会返回 undefined。

---

Null 和 Undefined 是其他任何类型（包括 void）的子类型，可以赋值给其它类型，如数字类型，此时，赋值后的类型会变成 null 或 undefined。

在TypeScript中启用严格的空校验（--strictNullChecks）特性，就可以使得null 和 undefined 只能被赋值给 void 或本身对应的类型

```ts
// 启用 --strictNullChecks

// 1.变量 x 只能是数字类型
let x: number;
x = 1; // 运行正确
x = undefined;    // 运行错误
x = null;    // 运行错误
// 2.如果一个类型可能出现 null 或 undefined， 可以用 | 来支持多种类型
let x: number | null | undefined;
x = 1; // 运行正确
x = undefined;    // 运行正确
x = null;    // 运行正确
```

## 5. TypeScript 变量声明

TypeScript 变量的命名规则：

- 变量名称：包含`数字`，`字母`，`_`，`$`
- 不能包含其他符号(除_,$)，不能以数字开头

声明变量

```ts
// 声明变量的类型及初始值：
var uname:string = "Typescript";
// 声明变量的类型，但没有初始值，变量值会设置为 undefined：
var uname:string;
// 声明变量并初始值，但不设置类型，该变量可以是任意类型(any)：
var uname = "Typescript";
// 声明变量没有设置类型和初始值，类型可以是任意类型(any)，默认初始值为 undefined：
var uname;
```

## 6. TypeScript 运算符

TypeScript 主要包含以下几种运算：

- 算术运算符：+，-，*，/，%，++，--
- 逻辑运算符：&&，||，!
- 关系运算符：==，!=，>，<，>=，<=
- 按位运算符：&，|，~，^，<<，>>，>>>
- 赋值运算符：=，+=，-=，*=，/=
- 三元/条件运算符：条件? : true : false
- 其他运算符：字符串运算符（连接运算符`+`），(负号运算符`-`)
- 类型运算符：typeof ，instanceof

## 7. TypeScript 条件语句

- **if 语句** - 只有当指定条件为 true 时，使用该语句来执行代码
- **if...else 语句** - 当条件为 true 时执行代码，当条件为 false 时执行其他代码
- **if...else if....else 语句**- 使用该语句来选择多个代码块之一来执行
- **switch 语句** - 使用该语句来选择多个代码块之一来执行

## 8. TypeScript 循环

- ### for 

- ### for...in

  ```ts
  for (var val in list) { 
      //语句 
      // val 需要为 string 或 any 类型。
  }
  ```

- ### for…of  允许遍历 Arrays（数组）, Strings（字符串）, Maps（映射）, Sets（集合）等可迭代的数据结构等

  

- ### forEach

- ### every 和 some

- ### while

- ### do...while

## 9. TypeScript 函数

