# 编译上下文

#### <u>*tsconfig.json*</u>

##### 编译选项

你可以通过 `compilerOptions` 来定制你的编译选项：

```js
{
  "compilerOptions": {

    /* 基本选项 */
    "target": "es5",                       // 指定 ECMAScript 目标版本: 'ES3' (default), 'ES5', 'ES6'/'ES2015', 'ES2016', 'ES2017', or 'ESNEXT'
    "module": "commonjs",                  // 指定使用模块: 'commonjs', 'amd', 'system', 'umd' or 'es2015'
    "lib": [],                             // 指定要包含在编译中的库文件
    "allowJs": true,                       // 允许编译 javascript 文件
    "checkJs": true,                       // 报告 javascript 文件中的错误
    "jsx": "preserve",                     // 指定 jsx 代码的生成: 'preserve', 'react-native', or 'react'
    "declaration": true,                   // 生成相应的 '.d.ts' 文件
    "sourceMap": true,                     // 生成相应的 '.map' 文件
    "outFile": "./",                       // 将输出文件合并为一个文件
    "outDir": "./",                        // 指定输出目录
    "rootDir": "./",                       // 用来控制输出目录结构 --outDir.
    "removeComments": true,                // 删除编译后的所有的注释
    "noEmit": true,                        // 不生成输出文件
    "importHelpers": true,                 // 从 tslib 导入辅助工具函数
    "isolatedModules": true,               // 将每个文件作为单独的模块 （与 'ts.transpileModule' 类似）.

    /* 严格的类型检查选项 */
    "strict": true,                        // 启用所有严格类型检查选项
    "noImplicitAny": true,                 // 在表达式和声明上有隐含的 any类型时报错
    "strictNullChecks": true,              // 启用严格的 null 检查
    "noImplicitThis": true,                // 当 this 表达式值为 any 类型的时候，生成一个错误
    "alwaysStrict": true,                  // 以严格模式检查每个模块，并在每个文件里加入 'use strict'

    /* 额外的检查 */
    "noUnusedLocals": true,                // 有未使用的变量时，抛出错误
    "noUnusedParameters": true,            // 有未使用的参数时，抛出错误
    "noImplicitReturns": true,             // 并不是所有函数里的代码都有返回值时，抛出错误
    "noFallthroughCasesInSwitch": true,    // 报告 switch 语句的 fallthrough 错误。（即，不允许 switch 的 case 语句贯穿）

    /* 模块解析选项 */
    "moduleResolution": "node",            // 选择模块解析策略： 'node' (Node.js) or 'classic' (TypeScript pre-1.6)
    "baseUrl": "./",                       // 用于解析非相对模块名称的基目录
    "paths": {},                           // 模块名到基于 baseUrl 的路径映射的列表
    "rootDirs": [],                        // 根文件夹列表，其组合内容表示项目运行时的结构内容
    "typeRoots": [],                       // 包含类型声明的文件列表
    "types": [],                           // 需要包含的类型声明文件名列表
    "allowSyntheticDefaultImports": true,  // 允许从没有设置默认导出的模块中默认导入。

    /* Source Map Options */
    "sourceRoot": "./",                    // 指定调试器应该找到 TypeScript 文件而不是源文件的位置
    "mapRoot": "./",                       // 指定调试器应该找到映射文件而不是生成文件的位置
    "inlineSourceMap": true,               // 生成单个 soucemaps 文件，而不是将 sourcemaps 生成不同的文件
    "inlineSources": true,                 // 将代码与 sourcemaps 生成到一个文件中，要求同时设置了 --inlineSourceMap 或 --sourceMap 属性

    /* 其他选项 */
    "experimentalDecorators": true,        // 启用装饰器
    "emitDecoratorMetadata": true          // 为装饰器提供元数据的支持
  }
}
```

#### <u>*TypeScript 编译*</u>

- 运行 tsc，它会在当前目录或者是父级目录寻找 `tsconfig.json` 文件。
- 运行 `tsc -p ./path-to-project-directory` 。当然，这个路径可以是绝对路径，也可以是相对于当前目录的相对路径。

  可以使用 `tsc -w` 来启用 TypeScript 编译器的观测模式，在检测到文件改动之后，它将重新编译。

## 

可以显式指定需要编译的文件：

```js
{
  "files": [
    "./some/file.ts"
  ]
}
```

可以使用 `include` 和 `exclude` 选项来指定需要包含的文件和排除的文件

```js
{
  "include": [
    "./folder"
  ],
  "exclude": [
    "./folder/**/*.spec.ts",
    "./folder/someSubFolder"
  ]
}
```

注意

使用 `globs`：`**/*` （一个示例用法：`some/folder/**/*`）意味着匹配所有的文件夹和所有文件（扩展名为 `.ts/.tsx`，当开启了 `allowJs: true` 选项时，扩展名可以是 `.js/.jsx`）。

# 声明空间

类型声明空间与变量声明空间

#### <u>*类型声明空间*</u>

类型声明空间包含用来当做类型注解的内容，例如下面的类型声明：

```ts
class Foo {}
interface Bar {}
type Bas = {};
```

你可以将 `Foo`, `Bar`, `Bas` 作为类型注解使用

```ts
let foo: Foo;
let bar: Bar;
let bas: Bas;
```

你定义了 `interface Bar`，却并不能够把它作为一个变量来使用，因为它没有定义在变量声明空间中。

```ts
interface Bar {}
const bar = Bar; // Error: "cannot find name 'Bar'"
```

##### <u>*变量声明空间*</u>

变量声明空间包含可用作变量的内容，在上文中 `Class Foo` 提供了一个类型 `Foo` 到类型声明空间，此外它同样提供了一个变量 `Foo` 到变量声明空间

```ts
class Foo {}
const someVar = Foo;
const someOtherVar = 123;
```

一些用 `var` 声明的变量，也只能在变量声明空间使用，不能用作类型注解。

```js
const foo = 123;
let bar: foo; // ERROR: "cannot find name 'foo'"
```

# 模块

#### <u>*全局模块*</u>

如在 foo.ts 里的以下代码

```ts
const foo = 123;
```

在相同的项目里创建了一个新的文件 `bar.ts`，TypeScript 类型系统将会允许你使用变量 `foo`，就好像它在全局可用一样：

```ts
const bar = foo; // allowed
```

使用全局变量空间是危险的，因为它会与文件内的代码命名冲突。推荐使用下文中将要提到的文件模块。

#### <u>文件模块</u>（外部模块）

ypeScript 文件的根级别位置含有 `import` 或者 `export`，那么它会在这个文件中创建一个本地的作用域。

```ts
export const foo = 123;
```

在全局命名空间里，我们不再有 `foo`，通过创建一个新文件 `bar.ts`测试：

```ts
const bar = foo; // ERROR: "cannot find name 'foo'"
```

如果你想在 `bar.ts` 里使用来自 `foo.ts` 的内容，你必须显式地导入它，更新后的 `bar.ts` 如下所示。

```ts
import { foo } from './foo';
const bar = foo; // allow
```

#### <u>*文件模块详情*</u>

##### commonjs, amd, es modules, others

你可以根据不同的 `module` 选项来把 TypeScript 编译成不同的 JavaScript 模块类型：

- AMD：不要使用它，它仅能在浏览器工作；
- SystemJS：这是一个好的实验，已经被 ES 模块替代；
- ES 模块：没有完善好。

综上，建议使用 `module: commonjs` 选项来替代这些模式。

**怎么书写 TypeScript 模块呢？**

- 放弃使用 `import/require` 语法即 `import foo = require('foo')` 写法
- 推荐使用 ES 模块语法



使用 `module: commonjs` 选项以及使用 ES 模块语法导入、导出、编写模块。

#### <u>*模块语法*</u>

- 使用 `export` 关键字导出一个变量或类型

```ts
// foo.ts
export const someVar = 123;
export type someType = {
  foo: string;
};
```

export` 的写法除了上面这种，还有另外一种：

```ts
// foo.ts
const someVar = 123;
type someType = {
  type: string;
};

export { someVar, someType };
```

也可以用重命名变量的方式导出

```ts
// foo.ts
const someVar = 123;
export { someVar as aDifferentName };//重命名为aDifferentName
```

- 使用 `import` 关键字导入一个变量或者是一个类型：

```ts
// bar.ts
import { someVar, someType } from './foo';
```

- 通过重命名的方式导入变量或者类型：

```ts
// bar.ts
import { someVar as aDifferentName } from './foo';
```

- 还可以使用整体加载，即用星号（*）指定一个对象：

```ts
// bar.ts
import * as foo from './foo';
// 你可以使用 `foo.someVar` 和 `foo.someType` 以及其他任何从 `foo` 导出的变量或者类型
```



- 从其他模块导入后整体导出：（import只导入文件）

```ts
export * from './foo';
```

- 从其他模块导入后，部分导出：

```ts
export { someVar } from './foo';
```

- 通过重命名，部分导出从另一个模块导入的项目：

```ts
export { someVar as aDifferentName } from './foo';
```

**<u>*默认导入／导出*</u>**

- 使用export default
  - 在一个变量之前（不需要使用 `let/const/var`）；
  - 在一个函数之前；
  - 在一个类之前。

```ts
// some var
export default (someVar = 123);

// some function
export default function someFunction() {}

// some class
export default class someClass {}
```

- 导入使用 `import someName from 'someModule'` 语法（你可以根据需要为导入命名）：

```ts
import someLocalNameForThisFile from './foo';
```

#### <u>*模块路径*</u>

place` -- 将在提及查找模式后解释它。

**<u>*相对模块路径*</u>**

略

**<u>*动态查找*</u>**

当导入路径不是相对路径时，模块解析将会模仿 [Node 模块解析策略](https://nodejs.org/api/modules.html#modules_all_together)，例：

- 当你使用 import * as  foo  from  'foo'，将会按如下顺序查找模块：
  - `./node_modules/foo`
  - `../node_modules/foo`
  - `../../node_modules/foo`
  - 直到系统的根目录
- 当使用import * as  foo  from  'something/foo'，将会按照如下顺序查找
  - `./node_modules/something/foo`
  - `../node_modules/something/foo`
  - `../../node_modules/something/foo`
  - 直到系统的根目录

### 什么是 `place`

？

#### <u>*重写类型的动态查找*</u>

可以通过 `declare module 'somePath'` 声明一个全局模块的方式，解决查找模块路径的问题。

```ts
// global.d.ts
declare module 'foo' {
  // some variable declarations
  export var bar: number;  //这里不能进行赋值操作
}
```

接着 ：

```ts
// anyOtherTsFileInYourProject.ts
import * as foo from 'foo';
// TypeScript 将假设（在没有做其他查找的情况下）  foo 是 { bar: number }

```

#### <u>*`import/require` 仅仅是导入类型*</u>

以下导入语法：

```ts
import foo = require('foo');
```

它实际上只做了两件事：

- 导入 foo 模块的所有类型信息；
- 确定 foo 模块运行时的依赖关系。

**例子1：懒加载**

类型推断需要提前完成，这意味着，如果你想在 `bar` 文件里，使用从其他文件 `foo` 导出的类型，将不得不这么做：

```ts
import foo = require('foo');
let bar: foo.SomeType;
```

假如只想在需要时加载模块 `foo`，此时需要仅在类型注解中使用导入的模块名称，而**不**是在变量中使用。在编译成 JavaScript 时，这些将会被移除。接着，你可以手动导入你需要的模块。

作为一个例子，考虑以下基于 `commonjs` 的代码，我们仅在一个函数内导入 `foo` 模块：

```ts
import foo = require('foo');
export function loadFoo() {
  // 这是懒加载 foo，原始的加载仅仅用来做类型注解
  const _foo: typeof foo = require('foo');
  // 现在，你可以使用 `_foo` 替代 `foo` 来作为一个变量使用
}
```

一个同样简单的 `amd` 模块（使用 requirejs）：

```ts
import foo = require('foo');
export function loadFoo() {
  // 这是懒加载 foo，原始的加载仅仅用来做类型注解
  require(['foo'], (_foo: typeof foo) => {
    // 现在，你可以使用 `_foo` 替代 `foo` 来作为一个变量使用
  });
}
```

这些通常在以下情景使用：

- 在 web app 里， 当你在特定路由上加载 JavaScript 时；
- 在 node 应用里，当你只想加载特定模块，用来加快启动速度时。

**例子2：打破循环依赖**

类似于懒加载的使用用例，某些模块加载器（commonjs/node 和 amd/requirejs）不能很好的处理循环依赖。在这种情况下，一方面我们使用延迟加载代码，并在另一方面预先加载模块是很实用的。

**例子3：确保导入**

当你加载一个模块，只是想引入其附加的作用（如：模块可能会注册一些像 [CodeMirror addons](https://codemirror.net/doc/manual.html#addons)）时，然而，如果你仅仅是 `import/require` （导入）一些并没有与你的模块或者模块加载器有任何依赖的 JavaScript 代码，（如：webpack），经过 TypeScript 编译后，这些将会被完全忽视。在这种情况下，你可以使用一个 `ensureImport` 变量，来确保编译的 JavaScript 依赖与模块。如：

```ts
import foo = require('./foo');
import bar = require('./bar');
import bas = require('./bas');
const ensureImport: any = foo || bar || bas;
```

# 命名空间(支持嵌套)

#### <u>*namespace关键字*</u>

```typescript
namespace Utility {
  export function log(msg) {
    console.log(msg);
  }
  export function error(msg) {
    console.log(msg);
  }
}
// usage
Utility.log('Call me');
Utility.error('maybe');
```

这种写法类似于

```js
(function(something) {
  something.foo = 123;
})(something || (something = {}));
console.log(something);
// { foo: 123 }
(function(something) {
  something.bar = 456;
})(something || (something = {}));
console.log(something); // { foo: 123, bar: 456 }
```

#### <u>*动态导入表达式*</u>

```typescript
import(/* webpackChunkName: "momentjs" */ 'moment')
  .then(moment => {
    // 懒加载的模块拥有所有的类型，并且能够按期工作
    // 类型检查会工作，代码引用也会工作  :100:
    const time = moment().format();
    console.log('TypeScript >= 2.4.0 Dynamic Import Expression:');
    console.log(time);
  })
  .catch(err => {
    console.log('Failed to load moment', err);
  });
```

# TypeScript类型系统

#### <u>*泛型*</u>

一个颠倒数据的例子

```typescript
function reverse<T>(items: T[]): T[] {
  const toreturn = [];
  for (let i = items.length - 1; i >= 0; i--) {
    toreturn.push(items[i]);
  }
  return toreturn;
}
const sample = [1, 2, 3];
let reversed = reverse(sample);
console.log(reversed); // 3, 2, 1
// Safety
reversed[0] = '1'; // Error
reversed = ['1', '2']; // Error
reversed[0] = 1; // ok
reversed = [1, 2]; // ok
```

 实际上，Js数组已经拥有了 `reverse` 的方法，于是可以

```typescript
interface Array<T> {
  reverse(): T[];
}
let numArr = [1, 2];
let reversedNums = numArr.reverse();
reversedNums = ['1', '2']; // Error
```

**<u>*交叉类型*</u>**

```typescript
function extend<T extends object, U extends object>(first: T, second: U): T & U {
  const result = <T & U>{};
  for (let id in first) {
    (<T>result)[id] = first[id];
  }
  for (let id in second) {
    if (!result.hasOwnProperty(id)) {
      (<U>result)[id] = second[id];
    }
  }
  return result;
}
const x = extend({ a: 'hello' }, { b: 42 });
// 现在 x 拥有了 a 属性与 b 属性
const a = x.a;
const b = x.b;
```

#### <u>*类型别名*</u>

可以为任意的类型注解提供类型别名

```typescript
type StrOrNum = string | number;// 使用
let sample: StrOrNum;
sample = 123;
sample = '123';
sample = true; // Error
```

#### **<u>*枚举型*</u>**

```typescript
enum CardSuit {
  Clubs,
  Diamonds,
  Hearts = 10,
  Spades
    //不赋值默认为0开始递增，赋值后，赋值的一下一位开始基于赋值的值进行递增,当然也可以是字符串类型
}
// 简单的使用枚举类型
let Card = CardSuit.Clubs;//0
let Card2 = CardSuit.Hearts;//10
let Card3 = CardSuit.Hearts;//11
// 类型安全
Card = 'not a member of card suit'; // Error: string 不能赋值给 `CardSuit` 类型
```

编译成js

```js
var Tristate;
(function(Tristate) {
  Tristate[(Tristate['False'] = 0)] = 'False';
  Tristate[(Tristate['True'] = 1)] = 'True';
  Tristate[(Tristate['Unknown'] = 2)] = 'Unknown';
})(Tristate || (Tristate = {}));
```

注意Tristate['False'] = 0是会自动返回等号右边值的，如console.log(Tristate['Unknown'] = 2),会输出2

#### **<u>*lib.d.ts*</u>**

安装typescript时顺带安装的，可以通过指定 `--noLib` 的编译器命令行标志（或者在 `tsconfig.json` 中指定选项 `noLib: true`）从上下文中排除此文件。            看下面的例子

```typescript
const foo = 123;
const bar = foo.toString();//通过，原因是因为lib.d.ts 为所有 JavaScript 对象定义了 toString 方法。如果你在 noLib 选项下将会出现类型检查错误： toString 不存在类型 number上
```

#### **<u>*`--lib` 选项*</u>**

```json
"compilerOptions": {
  "target": "es5",
  "lib": ["es6", "dom"，"scripthost", "es2015.symbol"]
}
```

# 函数

#### <u>*重载*</u>

```ts
// 重载
function padding(all: number);
function padding(topAndBottom: number, leftAndRight: number);
function padding(top: number, right: number, bottom: number, left: number);
function padding(a: number, b?: number, c?: number, d?: number) {
  if (b === undefined && c === undefined && d === undefined) {
    b = c = d = a;
  } else if (c === undefined && d === undefined) {
    c = a;
    d = b
  }
  return {
    top: a,
    right: b,
    bottom: c,
    left: d
  };
}
```

#### <u>*可实例化*</u>

```ts
interface CallMeWithNewToGetString {
  new (): string;
}
declare const Foo: CallMeWithNewToGetString;// 使用
const bar = new Foo(); // bar 被推断为 string 类型
```