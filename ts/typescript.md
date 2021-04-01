## 一、基础类型

#### *<u>**元组Tuple**</u>*    允许表示一个已知元素数量和类型在数组，各个元素类型不必相同

```typescript
	let x:[string,number];
	x=['hello',10]//ok
	x=[10,"hello"]//error
```

当访问一直索引的元素时，可以得到正确的类型：

```typescript
	console.log(x[0].substr(1)); // OK,因为值为字符串
	console.log(x[1].substr(1)); // Error, 'number' does not have 'substr'
```
当访问越界元素，会使用联合类型替代：

```typescript
	x[3] = 'world'; // OK, 字符串可以赋值给(string | number)类型 (ts会报错，但是导出js仍可运行)
	console.log(x[1].toString()); // OK, 'string' 和 'number' 都有 toString
	x[6] = true; // Error, 布尔不是(string | number)类型
```


*<u>**Never**</u>*    表示的是那些永不存在的值的类型。是任何类型的子类型，也可以赋值给任何类型；但是，没有类型是never的子类型或可以赋值给never类型（除了never本身），即使any也不行

```typescript
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}
// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}
// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}
```


#### *<u>**类型断言**</u>* 

#####  告诉编译器，我比你懂！我知道他要什么类型.（只是在编译阶段作用，运行时没有影响）

两种形式：

***<u>“尖括号”</u>***

```typescript
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length; //告诉编译器someValue类型为string
```
***<u>“as语法”</u>***

```typescript
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;//告诉编译器someValue类型为string
```

两者几乎等价，唯一区别在于，当你在TypeScript里使用JSX时，只有 `as`语法断言是被允许的。



## 二、变量声明

#### *<u>**var,let,const**</u>*    : 假如后面没有做变化，应该优先使用const，其次let ，最后var

#### <u>**解构**</u>

**1.数组解构：**

这创建了2个命名变量 `first` 和 `second`。 相当于使用了索引，但更为方便：

```typescript
let input = [1, 2];
let [first, second] = input;
console.log(first); //  1
console.log(second); //  2
```

​	作用于函数参数：

```typescript
function f([first, second]: [number, number]) {
    console.log(first);
    console.log(second);
}
f(input); 
```

​	可以在数组里使用`...`语法创建剩余变量：

```typescript
let [first, ...rest] = [1, 2, 3, 4];
console.log(first); //  1
console.log(rest); //  [ 2, 3, 4 ]
```

​	可以忽略尾部元素：

```typescript
let [first] = [1, 2, 3, 4];
console.log(first); //  1
```

​	或者忽略其他元素：

```typescript
let [, second, , fourth] = [1, 2, 3, 4];
console.log(second,fourth); //  2  4
```

**2.对象解构：**

```typescript
let o = {
    a: "foo",
    b: 12,
    c: "bar"
};
let { a, b } = o;
console.log(a,b); //  foo  12
```

​	可以用没有声明的赋值：

```typescript
({ a, b } = { a: "baz", b: 101 });//注意，我们需要用括号将它括起来，因为Javascript通常会将以 `{` 起始的语句解析为一个块。
console.log(a,b); //  baz  101
```

​	可以在对象里使用`...`语法创建剩余变量：

```typescript
let { a, ...passthrough } = o;
let total = passthrough.b + passthrough.c.length;//15 此时的passthrough相当于对象{b: 12,c: "bar"}
```

​	可以给属性重命名：

```typescript
let { a: newName1, b: newName2 } = o;
console.log(newName1,newName2);//foo 12
```

​	设置默认值：

```typescript
let wholeObject={a:"aa"}
function keepWholeObject(wholeObject: { a: string, b?: number }) {
	let { a, b = 1001 } = wholeObject;
	console.log(a,b);
}
keepWholeObject(wholeObject); //aa 1001
```

​	展开操作：（目前版本TypeScript编译器不允许展开泛型函数上的类型参数。）

```typescript
let defaults = { food: "spicy", price: "$$", ambiance: "noisy" };
let search = { ...defaults, food: "rich" };
console.log(search); //{ food: 'rich', price: '$$', ambiance: 'noisy' }
// 对象的展开像数组展开一样，它是从左至右进行处理，但结果仍为对象。 这就意味着出现在展开对象后面的属性会覆盖前面的属性	
```

```typescript
class C {
  p = 12;
  m() {
  }
}
let c = new C();
let clone = { ...c };
clone.p; // ok
clone.m(); // error!  对象展开仅包含对象 自身的可枚举属性。 就是是说展开一个对象实例时，会丢失其方法
```

## 三、接口

简单示例：(类型检查器不会去检查属性的顺序，只要相应的属性存在并且类型也是对的就可以。)

```typescript
interface LabelledValue {
  label: string;
}
function printLabel(labelledObj: LabelledValue) {
  console.log(labelledObj.label);
}
let myObj = {size: 10, label: "Size 10 Object"};
pri

```

​	***<u>可选属性</u>***：(可选属性在应用“option bags”模式时很常用，即给函数传入的参数对象中只有部分属性赋值了。)

```typescript
interface SquareConfig {
	color?: string;
	width?: number;
  }
  function createSquare(config: SquareConfig): {color: string; area: number} {
	let newSquare = {color: "white", area: 100};
	if (config.color) {
	  newSquare.color = config.color;
	}
	if (config.width) {
	  newSquare.area = config.width * config.width;
	}
	return newSquare;
  }
  
  let mySquare = createSquare({color: "black"});
  console.log(mySquare);//{ color: 'black', area: 100 }
  
```

​	***<u>只读属性</u>***（在属性名前用 `readonly`来指定）

```typescript
interface Point {
    readonly x: number;
    readonly y: number;
}
let p1: Point = { x: 10, y: 20 };
p1.x = 5; // error! 上面用对象赋值后xy不能再次改变了
```

ReadonlyArray<T>，它与`Array<T>`相似，只是保数组创建后再也不能被修改

```typescript
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
ro[0] = 12; // error!
ro.push(5); // error!
ro.length = 100; // error!
a = ro; // error!
```

#### <u>***函数类型***</u>   

​	定义后，我们可以像使用其它接口一样使用这个函数类型的接口。 下例展示了如何创建一个函数类型的变量，并将一个同类型的函数赋值给这个变量。(对于函数类型的类型检查来说，函数的参数名不需要与接口里定义的名字相匹配。)

```typescript
interface SearchFunc {
  (source: string, subString: string): boolean;
}
let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
  let result = source.search(subString);
  return result > -1;
}
```

#### *<u>**可索引的类型**</u>*    

```typescript
interface StringArray {
  [index: number]: string;
}
let myArray: StringArray;
myArray = ["Bob", "Fred"];
let myStr: string = myArray[0];
```

​	字符串索引签名能够很好的描述`dictionary`模式，并且它们也会确保所有属性与其返回值类型相匹配。  下面的例子里， `name`的类型与字符串索引类型不匹配，所以给出错误提示

```typescript
interface NumberDictionary {
  [index: string]: number;
  length: number;    // 可以，length是number类型
  name: string       // 错误，`name`的类型与索引类型返回值的类型不匹配
}
```

​	可以将索引签名设置为只读，这样就防止了给索引赋值

```typescript
interface ReadonlyStringArray {
    readonly [index: number]: string;
}
let myArray: ReadonlyStringArray = ["Alice", "Bob"];
myArray[1] = "Mallory"; // error!
```

#### *<u>**类类型**</u>*    

实现接口

```typescript
interface ClockInterface {
    currentTime: Date;
    setTime(d: Date);
}
class Clock implements ClockInterface {
    currentTime: Date;
    setTime(d: Date) {
        this.currentTime = d;
    }
    constructor(h: number, m: number) { }
}
```

#### 类静态部分与实例部分的区别？？？（不会）

####  

#### 继承接口（和类一样，接口也可以相互继承）

```typescript
interface Shape {
    color: string;
}

interface Square extends Shape {
    sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
```

一个接口可以继承多个接口，创建出多个接口的合成接口。

```typescript
interface Shape {
    color: string;
}
interface PenStroke {
    penWidth: number;
}
interface Square extends Shape, PenStroke {
    sideLength: number;
}
let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;
```

##### <u>*混合类型*</u>

```typescript
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}
function getCounter(): Counter {
    let counter = <Counter>function (start: number) { };
    counter.interval = 123;
    counter.reset = function () { };
    return counter;
}
let c = getCounter();
c(10);
c.reset();
c.interval = 5.0;
```

