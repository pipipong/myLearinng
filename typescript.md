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

