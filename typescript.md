# TypeScript

## 基础类型

### 布尔值
``` typescript
let isDone: boolean = false
```

### 数字
``` typescript
let decLiteral: number = 20 // 十进制
let hexLiteral: number = 0x14 // 十六进制
let bingryLiteral: number = 0b10100 // 二进制
let octalLiteral: number = 0o24 // 八进制
```

### 字符串
``` typescript
let name: string = 'bob'
let say = `hello + ${name}`
```

### 数组
``` typescript
let list: number[] = [1,2,3]
let list: Array<number> = [1,2,3]
```

### 元组
``` typescript
let x: [string, number]
x = ['hello', 10]
```

### 枚举
``` typescript
enum Color {
	Red,
	Green,
	Blue
}
let c: Color = Color.Green // 2
let colorName: string = Color[2] // green
```

### 任意类型
``` typescript
let notSure: any = 4
notSure = 'maybe a string'
```

### void (不返回任何值)
``` typescript
function fuc(): void {
	console.log('???')
}

let a: void = null
let a: void = undefined
```

### null undefined
``` typescript
let u: undefind = undefined
let n: null = null
```

### never (不存在的数据类型)
``` typescript
function error(message: string): never {
	throw new Error(message)
}

function fail() {
	return error('something faild')
}

function inifiniteLoop(): never {
	while (true) {
	
	}
}
```

### object
``` typescript
declare function create(o: object | null): void

create({prop: 0})
create(null)
create(undefind)
```

### 类型断言
``` typescript
let someValue: any = 'this is a string'
let strLength: number = (<string>someValue).length
let strLength: number = (someValue as string).length // 推荐
```



## 接口

### 可选属性

``` typescript
interface Square {
    color: string
    area: number
}

interface SquareConfig {
    color?: string
    width?: number
}

function createSquare (config: SquareConfig): Square {
    let newSquare = {color: 'white', area: 100}
    if (config.color) {
        newSuqare.color = config.color
    }
    if (config.width) {
        newSquare.area = config.width * config.width
    }
    return newSquare
}

let mySquare = createSquare({color: 'black'})
```



### 只读属性

```typescript
interface Point {
	readonly x: number
	readonly y: number
}
```



### 属性检查

```typescript
interface SquareConfig {
    color?: string
    width?: number
    [propName: string]: any // 除了color 和 width 还可能有其他类型的属性
}
```



### 函数类型

``` typescript
interface SearchFunc {
    (source: string, subString: string): boolean
}

let mySearch: SearchFunc
mySearch = function (src: string, sub: string): boolean {
    let result = src.search(sub)
    return result > -1
}

```



### 可索引的接口

``` typescript
interface StringArray {
    [index: number]: string // 数字为索引 返回为字符串
}

let myArray: stringArray

myArray = ['bob', 'Fred']

let myStr:string = myArray['0']
```



### 类类型

实例接口

```typescript
interface ClockInterface {
    currentTime: Date
    setTime(d: Date)
}

class Clock implements ClockINterface {
    currentTime: Date
    
    constructor(h: number, m: number) {
    
    }
    
    setTime(d: Date) {
        this.currentTime = d
    }
}
```

构造器接口

``` typescript
interface ClockInterface {
    tick() 
}

interface ClockConstructor {
    new(hour: number, minute: number): ClockInterface
}

function createClock(ctor: ClockConstructor, hour: numbre, minute: number): ClockInteface {
    return new ctor(hour, minute)
}

class DigitalClock implements ClockInterface {
    constructor(h: number, m: number) {
        
    }
    
    tick() {
        console.log('beep beep')
    }
}


class AnalogClock implements ClockInterface {
    constructor(h: number, m: number) {
        
    }
    
    tick() {
        console.log('tick toc')
    }
}

let digital = createClock(DigitalClock, 12, 17)

let abaloa = createClock(AnalogClock, 13, 14)
```



### 继承接口

```ts
interface Shape {
    color: string
}

interface penStroke {
    penWidth: number
}

interface Square extends Shape {
    sideLength: number
}

let square = {} as Square
square.color = 'blue'
square.sideLength = 10
square.penWidth = 5.0

```



### 混合类型

```ts
interface Counter {
    (start: number): string,
    interval: number
   	reset(): void
}
    
function getCounter(): Counter {
    let counter = (function (star: number) {
    	               
    }) as Counter
    
    counter.interval = 123
    
    counter.reset = function () {
        
    }
    
    return counter
}

let c = getCounter()
c(10)
c.reset()
c.interval = 5.0
```



### 接口继承类

```ts
class Control {
    private state: any
}

interface SelectableControl extends Control {
    select()
}

class Button extends Control implements SelectableControl {
    select() {
        
    }
}

class TextBox extends Control {
    select() {
        
    }
}

class ImageC implements SelectableControl { // 报错
    select() {
        
    }
}
```

