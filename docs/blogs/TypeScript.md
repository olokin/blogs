---
title: typescript
date: 2023-03-23
categories:
  - 前端
tags:
  - ts
---

**const 和 readonly 的区别**
const 可以防止变量的值被修改，在运行时检查，声明的数组可以使用 push，pop 等方法
readonly 可以防止变量的属性被修改，在编译时检查，声明的数组不能使用 push，pop 等方法

**any、never、unknown、null & undefined 和 void 有什么区别**
any: 动态的变量类型（失去了类型检查的作用）。
never: 永不存在的值的类型。例如：never 类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型。
unknown: 任何类型的值都可以赋给 unknown 类型，但是 unknown 类型的值只能赋给 unknown 本身和 any 类型。
null & undefined: 默认情况下 null 和 undefined 是所有类型的子类型
void: 没有任何类型。例如：一个函数如果没有返回值，那么返回值可以定义为 void。

**常用用法**

```js
// 1. 属性名不确定的对象
// 等同于 type Paths = Record<string, string>
Reco
type Paths = {
  [x: string]: string,
}


// 2. 对象的 key 值
type ErrorMsg = {
  0: 'success'
}
type ErrorCode = keyof ErrorMsg


// 3. 部分对象 Partial
interface User {
  name: string
  age: number
}
// 等同于 type Criteria = Partial<User>
type Criteria = {
  [key in keyof User]?: User[key]
}


// 4. 从基础类型构造新类型
type CuOb<K extends string | number, T> = { [key in K]: T }
type User = {
  username: string
  age: number
}

// Obj1 对象的值只能为数字类型
type Obj1 = CuOb<string, number>
const obj1: Obj1 = {
  a: 100, // OK
  b: 100, // OK
}

// Obj2 对象的值只能为User类型
type Obj2 = CuOb<string, User>

const obj2: Obj2 = {
  u1: {
    username: 'xiaoming',
    age: 18,
  },
}


// 5.对象类型的继承
interface Res {
  data: any
  status: number
}

interface Cog extends Res {
  data: string // 改写 data 属性类型， any
  config: any // 添加 config 属性
}


// 6. 对象类型的修改，extends可以继承对象类型，但不可与原类型冲突，此时可以先使用 Omit 去除需要修改的属性
export interface TreeNode {
  id: number
  value: number
  children?: TreeNode[]
}

// 1. 去除 TreeNode 的 id 属性同时修改 children 属性的类型
export interface NodeWithoutId extends Omit<TreeNode, 'id' | 'children'> {
  children?: NodeWithoutId[]
}

const nodeWithoutId: NodeWithoutId = {
  value: 1,
  children: [
    {
      value: 2,
    },
  ],
}


// 7. 条件判断
export declare type Person<T extends 'User' | 'Admin'> = T extends 'User'
  ? {
      username: string
    }
  : {
      username: string
      role: string
    }

const user: Person<'User'> = { username: 'xiaoming' }

```

**常用配置**

```js
{
  /*
      tsconfig.json是ts编译器的配置文件，ts可以根据它的信息来对待吗进行编译 可以再tsconfig中写注释
      include : 用来指定哪些文件需要被编译
      exclude : 用来指定哪些文件不需要被编译 ：默认node_module
      extends : 用来指定继承的配置文件
      files   : 用来指定被编译的文件列表，只有编译少量文件才使用
      compilerOptions : 编译器的选项是配置文件中非常重要也是非常复杂的配置选项
  */
  "include":[
    // ** : 任意目录 ， * : 任意文件
    "./src/**/*"
  ],
  "exclude": [
    "./src/hello/**/*"
  ],
  // "extends": "./configs/base",
  "files": [
    "1.ts",
    // "2.ts"
  ],
  "compilerOptions": {
    // 用来指定 ES 版本 ESNext : 最新版。 'ES3', 'ES5', 'ES6'/'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020', 'ESNext'
    "target": "ES2020",
    // 指定要使用模块化的规范 : 'None', 'CommonJS', 'AMD', 'System', 'UMD', 'ES6'/'ES2015', 'ES2020' or 'ESNext'
    "module": "ESNext",
    // 用来指定项目中要使用的库 'ES5', 'ES6', 'ES2015', 'ES7', 'ES2016', 'ES2017', 'ES2018', 'ESNext', 'DOM', 'DOM.Iterable',
    //                          'WebWorker', 'ScriptHost', 'ES2015.Core', 'ES2015.Collection', 'ES2015.Generator', 'ES2015.Iterable',
    //                          'ES2015.Promise', 'ES2015.Proxy', 'ES2015.Reflect', 'ES2015.Symbol', 'ES2015.Symbol.WellKnown',
    //                          'ES2016.Array.Include', 'ES2017.object', 'ES2017.Intl', 'ES2017.SharedMemory', 'ES2017.String',
    //                          'ES2017.TypedArrays', 'ES2018.Intl', 'ES2018.Promise', 'ES2018.RegExp', 'ESNext.AsyncIterable',
    //                          'ESNext.Array', 'ESNext.Intl', 'ESNext.Symbol'
    // 运行在浏览器中不用设置，运行在node或其他中才需要设置
    // "lib":[]，
    // 用来指定编译后文件的存放位置
    "outDir":"./dist",
    // 将代码合并为一个文件,设置之后所有的全局作用域中的代码会合并到同一个文件中 但是只能在  'amd' and 'system' 中才能使用
    // "outFile": "./dist/app.js",
    // 是否对js文件进行编译，默认false
    "allowJs": false,
    // 是否检查js代码是否符合语法规范，默认false
    "checkJs": false,
    // 是否移除注释，默认false
    "removeComments":false,
    // 是否不生成编译后文件，默认false
    "noEmit": false,
    // 当有错误时是否生成文件，默认false
    "noEmitOnError": false,
    // 是否生成sourceMap，默认false  这个文件里保存的，是转换后代码的位置，和对应的转换前的位置。有了它，出错的时候，通过断点工具可以直接显示原始代码，而不是转换后的代码。
    "sourceMap":false,
    // 所有的严格检查的总开关，默认false
    "strict": false,
    // 编译后的文件是否开启严格模式，默认false
    "alwaysStrict": false,
    // 不允许隐式的any，默认false(允许)
    "noImplicitAny": false,
    // 不允许隐式的this，默认false(允许)
    "noImplicitThis": false,
    // 是否严格的检查空值，默认false 检查有可能为null的地方
    "strictNullChecks": true,
    // 是否严格检查bind、call和apply的参数列表，默认false  检查是否有多余参数
    "strictBindCallApply":false,
    // 是否严格检查函数的类型，
    "strictFunctionTypes":false,
    // 是否严格检查属性是否初始化，默认false
    "strictPropertyInitialization":false,
    // 是否检查switch语句包含正确的break，默认false
    "noFallthroughCasesInSwitch":false,
    // 检查函数没有隐式的返回值，默认false
    "noImplicitReturns":false,
    // 是否检查检查未使用的局部变量，默认false
    "noUnusedLocals":false,
    // 是否检查未使用的参数，默认false
    "noUnusedParameters":false,
    // 是否检查不可达代码报错，默认false   true，忽略不可达代码 false，不可达代码将引起错误
    "allowUnreachableCode":false
  }
}

```
