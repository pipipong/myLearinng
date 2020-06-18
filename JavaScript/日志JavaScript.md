# JavaScript基本概念

## 基本数据类型
### Srting

#### 不改变原字符串

##### String.prototype.concat()
concat()将多个字符串拼接在一起,不改变原字符串
```js
const a = 'aaa'
const b = 'bbb'
const c = 'ccc'.concat(a, b, 'ddd')
console.log(c) // 'cccaaabbbddd' 
```

##### String.prototype.includes()
includes()判断一个字符串是否包含在另一个字符串中
```js
const name = 'my name is amz' 
name.includes('amz') //true
name.includes('s amz') //true
name.includes('you') // false
```

##### String.prototype.indexOf()
indexOf()方法返回给定字符串在原字符串中首次出现的索引
```js
const name = 'my name is amz'
name.indexOf('my n') //0   字符串可以给字符串
name.indexOf('y') // 1
name.indexOf('m', 6) //12     //第二个参数是从第几位开始找
name.indexOf('l') // -1 没找到返回-1
```

##### String.prototype.search()
search()返回字符串在指定字符串首次出现的位置，如果没找到就返回-1
```js
'my name is amz'.search('amz')  // 11
'my name is amz'.search('my') // 0
'my name is amz'.search(/amz/) // 11 也可以传正则表达式
```

##### String.prototype.slice() 
slice()截取字符串的一部分，并返回这个新字符串
```js
'my name is amz'.slice(11) // "amz" 传递2个参数，第一个参数是从什么位置开始裁剪，第二个参数是 截取到什么地方，如果没传递第二个参数，就默认裁剪到最后一位
'my name is amz'.slice(0,2) // 'my' 从第1位裁剪到第三位
'my name is amz'.slice(0,-1) // "my name is am"  两个参数都可以是负数， 负数参数相加原字符串的长度  也就是上面的意思是说 从第1位裁剪到'my name is amz'.length + -1的位置
```

##### String.prototype.split() 
```js
split()方法把字符串分割成数组
const amz = 'my name is amz'
amz.split()  //  ['my name is amz']
amz.split(' ', 2)  // ['m', ''y']  第二个参数是获取字符串的几位，分割成数组
amz.split('name')  // ["my ", " is amz"]   第一个参数是 拿掉字符串匹配的字符段 然后分割数组
amz.split('m')  // ["", "y na", "e is a", "z"]   第一个参数可以是正则表达式
```

##### 转换大小写 
```js
// 针对少部分地区的语言设计,大部分时候返回的结果一样
String.prototype.toLocaleLowerCase()
String.prototype.toLocaleUpperCase()
String.prototype.toUpperCase()
String.prototype.toLowerCase()

const amz = 'my NAME is amz'
amz.toLocaleLowerCase()  //'my name is amz'

const amz = 'my name IS amz'
amz.toLocaleUpperCase() // 'MY NAME IS AMZ'

const amz = 'my name IS amz’
amz.toUpperCase() // ''MY NAME IS AMZ
```

##### String.prototype.trim() 
trim()清除字符串两端的空格
```js
const amz = '  my name is amz  ‘
amz.trim()  // 'my name is amz'
```

##### String.prototype.replace() 
replace()方法返回一个由替换值 替换一些匹配到的新字符串，
```js
const amz = 'my name is amz‘
amz.replace(/amz/, '123') // 'my name is 123'
amz.replace('m', '123') //  '123y name is amz'
```

##### String.prototype.substr()
substr()方法返回从指定位置开始 到指定数量的字符串
```js
const amz = 'my name is amz'
amz.substr(3) // 'name is amz'
amz.substr(-3) // 字符串长度+ -3    ‘amz’
amz.substr(1, 2) // 'y '  第二个参数是几位
```
如果开始位置也就是第一个字符串大于字符串长度，则返回一个空字符串 第二个位置超出了字符串剩余长度，则默认为字符串剩余长度。为负数则是字符串长度加负数，也就是说比如我第一个参数为-1 那么我剩余字符串长度是1了，最多只能复制一个长度为1的字符串，第二个值大于1都默认转化为1

##### String.prototype.substring()
substring()返回字符串从开始索引到结束索引之间的一个子集

也就是提取从substring()第一个参数到第二个参数的 子字符串，参数均为整数，小于0都会被转化为0 ，如果大于字符串长度都会被转化为字符串长度 如果第二个参数大于第一个参数，则会默认吧两个参数位置调换

```js
const amz = 'my name is amz'
amz.substring(1,4) // 'y n'  从第一位截取第四位
amz.substring(4,1) // 'y n'  因为第二个参数大于第一个参数，则默认调换她们的位置    所以还是从第一位截取第四位
```

### Number
### Boolean
### Undefined
### Null
### Symbol
### BigInt(新)

## 引用数据类型
### Object

[参考链接](https://segmentfault.com/a/1190000010753942#item-2-5)
#### Object.prototype.hasOwnProperty(prop)
该方法仅在目标属性为对象自身属性时返回true,而当该属性是从原型链中继承而来或根本不存在时，返回`false`。
```js
var o = {prop:1};
o.hasOwnProperty('prop'); // true
o.hasOwnProperty('toString'); // false
o.hasOwnProperty('formString'); // false
```
#### Object.prototype.isPropertyOf(obj)
如果目标对象是当前对象的原型，该方法就会返回true，而且，当前对象所在原型上的所有对象都能通过该测试，并不局限与它的直系关系。
```js
var s = new String('');
Object.prototype.isPrototypeOf(s); // true
String.prototype.isPrototypeOf(s); // true
Array.prototype.isPrototypeOf(s); // false
```

#### Object.prototype.propertyIsEnumerable(prop)
如果目标属性能在for in循环中被显示出来，该方法就返回true
```js
var a = [1,2,3];
a.propertyIsEnumerable('length'); // false
a.propertyIsEnumerable(0); // true
```

#### Object.create(obj, descr) (ES5)
该方法主要用于创建一个新对象，并为其设置原型，用（上述）属性描述符来定义对象的原型属性。
```js
var parent = {hi: 'Hello'};
var o = Object.create(parent, {
    prop: {
        value: 1
    }
});
o.hi; // 'Hello'
// 获得它的原型
Object.getPrototypeOf(parent) === Object.prototype; // true 说明parent的原型是Object.prototype
Object.getPrototypeOf(o); // {hi: "Hello"} // 说明o的原型是{hi: "Hello"}
o.hasOwnProperty('hi'); // false 说明hi是原型上的
o.hasOwnProperty('prop'); // true 说明prop是原型上的自身上的属性。
```
现在，我们甚至可以用它来创建一个完全空白的对象，这样的事情在ES3中可是做不到的。
```js
var o = Object.create(null);
typeof o.toString(); // 'undefined'
```

#### Object.keys(obj) (ES5)
该方法是一种特殊的for-in循环。它只返回当前对象的属性（不像for-in），而且这些属性也必须是可枚举的（这点和Object.getOwnPropertyNames()不同，不论是否可以枚举）。返回值是一个字符串数组。
```js
Object.prototype.customProto = 101;
Object.getOwnPropertyNames(Object.prototype);
// [..., "constructor", "toLocaleString", "isPrototypeOf", "customProto"]
Object.keys(Object.prototype); // ['customProto']
var o = {own: 202};
o.customProto; // 101
Object.keys(o); // ['own']
```

#### Object.values(obj) (ES8)

`Object.values()` 方法与`Object.keys`类似。返回一个给定对象自己的所有可枚举属性值的数组，值的顺序与使用`for...in`循环的顺序相同 ( 区别在于`for-in`循环枚举原型链中的属性 )。

```js
var obj = {a:1,b:2,c:3};
Object.keys(obj); // ['a','b','c']
Object.values(obj); // [1,2,3]
```

##### Object.entries(obj) (ES8)

`Object.entries()` 方法返回一个给定对象自己的可枚举属性`[key，value]`对的数组，数组中键值对的排列顺序和使用 `for...in` 循环遍历该对象时返回的顺序一致（区别在于一个`for-in`循环也枚举原型链中的属性）。

```js
var obj = {a:1,b:2,c:3};
Object.keys(obj); // ['a','b','c']
Object.values(obj); // [1,2,3]
Object.entries(obj); // [['a',1],['b',2],['c',3]]
```

##### Object.is(value1, value2) (ES6)

该方法用来比较两个值是否严格相等。它与严格比较运算符（===）的行为基本一致。
不同之处只有两个：一是`+0`不等于`-0`，而是`NaN`等于自身。

```js
Object.is('若川', '若川'); // true
Object.is({},{}); // false
Object.is(+0, -0); // false
+0 === -0; // true
Object.is(NaN, NaN); // true
NaN === NaN; // false
```
ES5`可以通过以下代码部署`Object.is
```js
Object.defineProperty(Object, 'is', {
    value: function() {x, y} {
        if (x === y) {
           // 针对+0不等于-0的情况
           return x !== 0 || 1 / x === 1 / y;
        }
        // 针对 NaN的情况
        return x !== x && y !== y;
    },
    configurable: true,
    enumerable: false,
    writable: true
});
```

#### Object.assign(target, ...sources) (ES6) (为浅拷贝)

该方法用来源对象（`source`）的所有可枚举的属性复制到目标对象（`target`）。它至少需要两个对象作为参数，第一个参数是目标对象`target`，后面的参数都是源对象（`source`）。只有一个参数不是对象，就会抛出`TypeError`错误。

```js
var target = {a: 1};
var source1 = {b: 2};
var source2 = {c: 3};
obj = Object.assign(target, source1, source2);
target; // {a:1,b:2,c:3}
obj; // {a:1,b:2,c:3}
target === obj; // true
// 如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。
var source3 = {a:2,b:3,c:4};
Object.assign(target, source3);
target; // {a:2,b:3,c:4}
```

`Object.assign`只复制自身属性，不可枚举的属性（`enumerable`为`false`）和继承的属性不会被复制。

```js
Object.assign({b: 'c'}, 
    Object.defineProperty({}, 'invisible', {
        enumerable: false,
        value: 'hello'
    })
);
// {b: 'c'}
```

属性名为`Symbol`值的属性，也会被`Object.assign()`复制。

```js
Object.assign({a: 'b'}, {[Symbol('c')]: 'd'});
// {a: 'b', Symbol(c): 'd'}
```

对于嵌套的对象，`Object.assign()`的处理方法是替换，而不是添加。

```js
Object.assign({a: {b:'c',d:'e'}}, {a:{b:'hello'}});
// {a: {b:'hello'}}
```

对于数组，`Object.assign()`把数组视为属性名为0、1、2的对象。

```js
Object.assign([1,2,3], [4,5]);
// [4,5,3]
```



### Array
[参考链接](https://juejin.im/post/5b0903b26fb9a07a9d70c7e0)
#### 改变原数组的方法(9个)
```js
let a = [1,2,3];
    // ES5:
     a.splice()/ a.sort() / a.pop()/ a.shift()/  a.push()/ a.unshift()/ a.reverse()
    // ES6:
    a.copyWithin() / a.fill
```
##### splice() 添加/删除数组元素
定义： splice() 方法向/从数组中添加/删除项目，然后返回被删除的项目
语法： array.splice(index,howmany,item1,.....,itemX)
参数:

1. index：必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置。
2. howmany：可选。要删除的项目数量。如果设置为 0，则不会删除项目。
3. item1, ..., itemX： 可选。向数组添加的新项目。

eg1:删除元素

```js
    let a = [1, 2, 3, 4, 5, 6, 7];
    let item = a.splice(0, 3); // [1,2,3]
    console.log(a); // [4,5,6,7]
    // 从数组下标0开始，删除3个元素
    let item = a.splice(-1, 3); // [7]
    // 从最后一个元素开始删除3个元素，因为最后一个元素，所以只删除了7
```

eg2: 删除并添加

```js
     let a = [1, 2, 3, 4, 5, 6, 7];
    let item = a.splice(0,3,'添加'); // [1,2,3]
    console.log(a); // ['添加',4,5,6,7]
    // 从数组下标0开始，删除3个元素，并添加元素'添加'
     let b = [1, 2, 3, 4, 5, 6, 7];
    let item = b.splice(-2,3,'添加1','添加2'); // [6,7]
    console.log(b); // [1,2,3,4,5,'添加1','添加2']
    // 从数组最后第二个元素开始，删除3个元素，并添加两个元素'添加1'、'添加2'
```

eg3: 不删除只添加

```js
    let a = [1, 2, 3, 4, 5, 6, 7];
    let item = a.splice(0,0,'添加1','添加2'); // [] 没有删除元素，返回空数组
    console.log(a); // ['添加1','添加2',1,2,3,4,5,6,7]
    let b = [1, 2, 3, 4, 5, 6, 7];
    let item = b.splice(-1,0,'添加1','添加2'); // [] 没有删除元素，返回空数组
    console.log(b); // [1,2,3,4,5,6,'添加1','添加2',7] 在最后一个元素的前面添加两个元素
```

从上述三个栗子可以得出:

1. 数组如果元素不够，会删除到最后一个元素为止
2. 操作的元素，包括开始的那个元素
3. 可以添加很多个元素
4. 添加是在开始的元素前面添加的

##### sort() 数组排序
定义: sort()方法对数组元素进行排序，并返回这个数组。
参数可选: 规定排序顺序的比较函数。
默认情况下sort()方法没有传比较函数的话，默认按字母升序，如果不是元素不是字符串的话，会调用toString()方法将元素转化为字符串的Unicode(万国码)位点，然后再比较字符。

```js
// 字符串排列 看起来很正常
    var a = ["Banana", "Orange", "Apple", "Mango"];
    a.sort(); // ["Apple","Banana","Mango","Orange"]
    // 数字排序的时候 因为转换成Unicode字符串之后，有些数字会比较大会排在后面 这显然不是我们想要的
    var	a = [10, 1, 3, 20,25,8];
    console.log(a.sort()) // [1,10,20,25,3,8];
```
**比较函数的两个参数：**
sort的比较函数有两个默认参数，要在函数中接收这两个参数，这两个参数是数组中两个要比较的元素，通常我们用 a 和 b 接收两个将要比较的元素：

若比较函数返回值<0，那么a将排到b的前面;
若比较函数返回值=0，那么a 和 b 相对位置不变；
若比较函数返回值>0，那么b 排在a 将的前面；

对于sort()方法更深层级的内部实现以及处理机制可以看一下这篇文章[深入了解javascript的sort方法](https://juejin.im/entry/59f7f3346fb9a04514635552)

##### pop() 删除一个数组中的最后的一个元素
定义: pop() 方法删除一个数组中的最后的一个元素，并且返回这个元素。

##### shift() 删除数组的第一个元素
定义: shift()方法删除数组的第一个元素，并返回这个元素。

##### push() 向数组的末尾添加元素
定义：push() 方法可向数组的末尾添加一个或多个元素，并返回新的长度。

##### unshift()
定义：unshift() 方法可向数组的开头添加一个或更多元素，并返回新的长度。

##### reverse() 颠倒数组中元素的顺序
定义: reverse() 方法用于颠倒数组中元素的顺序。
无参数
```js
	let  a =  [1,2,3];
    a.reverse();  
    console.log(a); // [3,2,1]
```

##### ES6: copyWithin() 指定位置的成员复制到其他位置

定义: 在当前数组内部，将指定位置的成员复制到其他位置,并返回这个数组。

语法:

```js
    array.copyWithin(target, start = 0, end = this.length)
```

参数:

三个参数都是数值，如果不是，会自动转为数值.

1. target（必需）：从该位置开始替换数据。如果为负值，表示倒数。
2. start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示倒数。
3. end（可选）：到该位置前停止读取数据，默认等于数组长度。使用负数可从数组结尾处规定位置。

浏览器兼容(MDN): chrome 45,Edge 12,Firefox32,Opera 32,Safari 9, IE 不支持

eg:

```js
    // -2相当于3号位，-1相当于4号位
    [1, 2, 3, 4, 5].copyWithin(0, -2, -1)
    // [4, 2, 3, 4, 5]
    var a=['OB1','Koro1','OB2','Koro2','OB3','Koro3','OB4','Koro4','OB5','Koro5']
    // 2位置开始被替换,3位置开始读取要替换的 5位置前面停止替换
    a.copyWithin(2,3,5)
    // ["OB1","Koro1","Koro2","OB3","OB3","Koro3","OB4","Koro4","OB5","Koro5"] 
```

从上述栗子:

1. 第一个参数是开始被替换的元素位置
2. 要替换数据的位置范围:从第二个参数是开始读取的元素，在第三个参数前面一个元素停止读取
3. 数组的长度不会改变
4. **读了几个元素就从开始被替换的地方替换几个元素**


##### ES6: fill() 填充数组

定义:  使用给定值，填充一个数组。

参数:

第一个元素(必须): 要填充数组的值

第二个元素(可选): 填充的开始位置,默认值为0

第三个元素(可选)：填充的结束位置，默认是为`this.length`

[MDN浏览器兼容](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/fill#浏览器兼容性)

```js
    ['a', 'b', 'c'].fill(7)
    // [7, 7, 7]
    ['a', 'b', 'c'].fill(7, 1, 2)
    // ['a', 7, 'c']
```

#### 不改变原数组的方法(8个):
```js
	// ES5：
    slice、join、toLocateString、toStrigin、cancat、indexOf、lastIndexOf、
    // ES7：
    includes
```



##### slice() 浅拷贝数组的元素

定义： 方法返回一个从开始到结束（不包括结束）选择的数组的一部分浅拷贝到一个新数组对象，且原数组不会被修改。

**注意**：字符串也有一个slice() 方法是用来提取字符串的，不要弄混了。

语法:

```js
    array.slice(begin, end);
```

参数:

begin(可选): 索引数值,接受负值，从该索引处开始提取原数组中的元素,默认值为0。

end(可选):索引数值(不包括),接受负值，在该索引处前结束提取原数组元素，默认值为数组末尾(包括最后一个元素)。

```js
    let a= ['hello','world'];
    let b=a.slice(0,1); // ['hello']
    a[0]='改变原数组';
    console.log(a,b); // ['改变原数组','world'] ['hello']
    b[0]='改变拷贝的数组';
     console.log(a,b); // ['改变原数组','world'] ['改变拷贝的数组']
```

如上：新数组是浅拷贝的，**元素是简单数据类型，改变之后不会互相干扰**。

如果是**复杂数据类型(对象,数组)的话，改变其中一个，另外一个也会改变**。

```js
    let a= [{name:'OBKoro1'}];
    let b=a.slice();
    console.log(b,a); // [{"name":"OBKoro1"}]  [{"name":"OBKoro1"}]
    // a[0].name='改变原数组';
    // console.log(b,a); // [{"name":"改变原数组"}] [{"name":"改变原数组"}]
    // b[0].name='改变拷贝数组',b[0].koro='改变拷贝数组';
    //  [{"name":"改变拷贝数组","koro":"改变拷贝数组"}] [{"name":"改变拷贝数组","koro":"改变拷贝数组"}]
```

原因在定义上面说过了的：slice()是浅拷贝，对于复杂的数据类型浅拷贝，拷贝的只是指向原数组的指针，所以无论改变原数组，还是浅拷贝的数组，都是改变原数组的数据。



##### join()  数组转字符串

定义:  join() 方法用于把数组中的所有元素通过指定的分隔符进行分隔放入一个字符串，返回生成的字符串。

语法:

```js
    array.join(str)
```

参数:

str(可选): 指定要使用的分隔符，默认使用逗号作为分隔符。

```js
    let a= ['hello','world'];
    let str=a.join(); // 'hello,world'
    let str2=a.join('+'); // 'hello+world'
```

使用join方法或者下文说到的toString方法时，当数组中的元素也是数组或者是对象时会出现什么情况？

```js
    let a= [['OBKoro1','23'],'test'];
    let str1=a.join(); // OBKoro1,23,test
    let b= [{name:'OBKoro1',age:'23'},'test'];
    let str2 = b.join(); // [object Object],test
    // 对象转字符串推荐JSON.stringify(obj);
```

所以，`join()/toString()`方法在数组元素是数组的时候，会将里面的数组也调用`join()/toString()`,如果是对象的话，对象会被转为`[object Object]`字符串。



##### cancat

定义： 方法用于合并两个或多个数组，返回一个新数组。

语法：

```js
    var newArr =oldArray.concat(arrayX,arrayX,......,arrayX)
```

参数：

arrayX（必须）：该参数可以是具体的值，也可以是数组对象。可以是任意多个。

eg1:

```js
    let a = [1, 2, 3];
    let b = [4, 5, 6];
    //连接两个数组
    let newVal=a.concat(b); // [1,2,3,4,5,6]
    // 连接三个数组
    let c = [7, 8, 9]
    let newVal2 = a.concat(b, c); // [1,2,3,4,5,6,7,8,9]
    // 添加元素
    let newVal3 = a.concat('添加元素',b, c,'再加一个'); 
    // [1,2,3,"添加元素",4,5,6,7,8,9,"再加一个"]
   // 合并嵌套数组  会浅拷贝嵌套数组
   let d = [1,2 ];
   let f = [3,[4]];
   let newVal4 = d.concat(f); // [1,2,3,[4]]
```



##### indexOf() 查找数组是否存在某个元素，返回下标

定义: 返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回-1。

语法:

```js
    array.indexOf(searchElement,fromIndex)
```

参数:

searchElement(必须):被查找的元素

fromIndex(可选):开始查找的位置(不能大于等于数组的长度，返回-1)，接受负值，默认值为0。

严格相等的搜索:

数组的indexOf搜索跟字符串的indexOf不一样,数组的indexOf使用严格相等`===`搜索元素，即**数组元素要完全匹配**才能搜索成功。

**注意**：indexOf()不能识别`NaN`

eg:

```js
    let a=['啦啦',2,4,24,NaN]
    console.log(a.indexOf('啦'));  // -1 
    console.log(a.indexOf('NaN'));  // -1 
    console.log(a.indexOf('啦啦')); // 0
```

使用场景：

1. [数组去重](https://juejin.im/post/5aad40e4f265da237f1e12ed#heading-10)
2. 根据获取的数组下标执行操作，改变数组中的值等。
3. 判断是否存在，执行操作。



##### ES7 includes() 查找数组是否包含某个元素 返回布尔

定义： 返回一个布尔值，表示某个数组是否包含给定的值

语法：

```js
    array.includes(searchElement,fromIndex=0)
```

参数：

searchElement(必须):被查找的元素

fromIndex(可选):默认值为0，参数表示搜索的起始位置，接受负值。正值超过数组长度，数组不会被搜索，返回false。负值绝对值超过长数组度，重置从0开始搜索。

**includes方法是为了弥补indexOf方法的缺陷而出现的:**

1. indexOf方法不能识别`NaN`
2. indexOf方法检查是否包含某个值不够语义化，需要判断是否不等于`-1`，表达不够直观

eg:

```js
    let a=['OB','Koro1',1,NaN];
    // let b=a.includes(NaN); // true 识别NaN
    // let b=a.includes('Koro1',100); // false 超过数组长度 不搜索
    // let b=a.includes('Koro1',-3);  // true 从倒数第三个元素开始搜索 
    // let b=a.includes('Koro1',-100);  // true 负值绝对值超过数组长度，搜索整个数组
```

兼容性(MDN): chrome47, Firefox 43,Edge 14,Opera 34, Safari 9,IE 未实现。



### 遍历方法(12个):

js中遍历数组并不会改变原始数组的方法总共有12个:

```js
    ES5：
    forEach、every 、some、 filter、map、reduce、reduceRight、
    ES6：
    find、findIndex、keys、values、entries
```

#### 关于遍历：

- 关于遍历的效率，可以看一下这篇[详解JS遍历](http://louiszhai.github.io/2015/12/18/traverse/#测试各方法效率)
- 尽量不要在遍历的时候，修改后面要遍历的值
- 尽量不要在遍历的时候修改数组的长度（删除/添加）

#### forEach

定义: 按升序为数组中含有效值的每一项执行一次回调函数。

语法：

```js
    array.forEach(function(currentValue, index, arr), thisValue)
```

参数:

function(必须): 数组中每个元素需要调用的函数。

```js
    // 回调函数的参数
    1. currentValue(必须),数组当前元素的值
    2. index(可选), 当前元素的索引值
    3. arr(可选),数组对象本身
```

thisValue(可选):  当执行回调函数时this绑定对象的值，默认值为`undefined`

**关于forEach()你要知道**：

- 无法中途退出循环，只能用`return`退出本次回调，进行下一次回调。
- 它总是返回 undefined值,即使你return了一个值。

#### 下面类似语法同样适用这些规则

```js
    1. 对于空数组是不会执行回调函数的
    2. 对于已在迭代过程中删除的元素，或者空元素会跳过回调函数
    3. 遍历次数再第一次循环前就会确定，再添加到数组中的元素不会被遍历。
    4. 如果已经存在的值被改变，则传递给 callback 的值是遍历到他们那一刻的值。
```

eg:

```js
    let a = [1, 2, ,3]; // 最后第二个元素是空的，不会遍历(undefined、null会遍历)
    let obj = { name: 'OBKoro1' };
    let result = a.forEach(function (value, index, array) { 
      a[3] = '改变元素';
      a.push('添加到尾端，不会被遍历')
      console.log(value, 'forEach传递的第一个参数'); // 分别打印 1 ,2 ,改变元素
      console.log(this.name); // OBKoro1 打印三次 this绑定在obj对象上
      // break; // break会报错
      return value; // return只能结束本次回调 会执行下次回调
      console.log('不会执行，因为return 会执行下一次循环回调')
    }, obj);
    console.log(result); // 即使return了一个值,也还是返回undefined
    // 回调函数也接受接头函数写法
```

#### every 检测数组所有元素是否都符合判断条件

定义: 方法用于检测数组所有元素是否都符合函数定义的条件

语法：

```js
    array.every(function(currentValue, index, arr), thisValue)
```

参数:(这几个方法的参数，语法都类似)

function(必须): 数组中每个元素需要调用的函数。

```js
    // 回调函数的参数
    1. currentValue(必须),数组当前元素的值
    2. index(可选), 当前元素的索引值
    3. arr(可选),数组对象本身
```

thisValue(可选):  当执行回调函数时this绑定对象的值，默认值为`undefined`

方法返回值规则:

1. 如果数组中检测到**有一个元素不满足，则整个表达式返回 false**，且剩余的元素不会再进行检测。
2. 如果所有元素**都满足条件，则返回 true**。=

eg:

```js
    function isBigEnough(element, index, array) { 
      return element >= 10; // 判断数组中的所有元素是否都大于10
    }
    let result = [12, 5, 8, 130, 44].every(isBigEnough);   // false
    let result = [12, 54, 18, 130, 44].every(isBigEnough); // true
    // 接受箭头函数写法 
    [12, 5, 8, 130, 44].every(x => x >= 10); // false
    [12, 54, 18, 130, 44].every(x => x >= 10); // true
```

#### some 数组中的是否有满足判断条件的元素

定义：数组中的是否有满足判断条件的元素

语法：

```js
    array.some(function(currentValue, index, arr), thisValue)
```

参数:(这几个方法的参数，语法都类似)

function(必须): 数组中每个元素需要调用的函数。

```js
    // 回调函数的参数
    1. currentValue(必须),数组当前元素的值
    2. index(可选), 当前元素的索引值
    3. arr(可选),数组对象本身
```

thisValue(可选):  当执行回调函数时this绑定对象的值，默认值为`undefined`

方法返回值规则：

1. 如果**有一个元素满足条件，则表达式返回true**, 剩余的元素不会再执行检测。

2. 如果**没有满足条件的元素，则返回false**。

```js
function isBigEnough(element, index, array) {
return (element >= 10); //数组中是否有一个元素大于 10
}
let result = [2, 5, 8, 1, 4].some(isBigEnough); // false
let result = [12, 5, 8, 1, 4].some(isBigEnough); // true
```

#### filter 过滤原始数组，返回新数组

定义: 返回一个新数组, 其包含通过所提供函数实现的测试的所有元素。

语法：

```js
    let new_array = arr.filter(function(currentValue, index, arr), thisArg)
```

参数:(这几个方法的参数，语法都类似)

function(必须): 数组中每个元素需要调用的函数。

```js
    // 回调函数的参数
    1. currentValue(必须),数组当前元素的值
    2. index(可选), 当前元素的索引值
    3. arr(可选),数组对象本身
```

thisValue(可选):  当执行回调函数时this绑定对象的值，默认值为`undefined`

eg:

```js
     let a = [32, 33, 16, 40];
    let result = a.filter(function (value, index, array) {
      return value >= 18; // 返回a数组中所有大于18的元素
    });
    console.log(result,a);// [32,33,40] [32,33,16,40]
```

#### map 对数组中的每个元素进行处理，返回新的数组

定义：创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。

语法：

```js
    let new_array = arr.map(function(currentValue, index, arr), thisArg)
```

参数:(这几个方法的参数，语法都类似)

function(必须): 数组中每个元素需要调用的函数。

```js
    // 回调函数的参数
    1. currentValue(必须),数组当前元素的值
    2. index(可选), 当前元素的索引值
    3. arr(可选),数组对象本身
```

thisValue(可选):  当执行回调函数时this绑定对象的值，默认值为`undefined`

eg:

```js
let a = ['1','2','3','4'];
let result = a.map(function (value, index, array) {
  return value + '新数组的新元素'
});
console.log(result, a); 
// ["1新数组的新元素","2新数组的新元素","3新数组的新元素","4新数组的新元素"] ["1","2","3","4"]
```

#### reduce 为数组提供累加器，合并为一个值

定义：reduce() 方法对累加器和数组中的每个元素（从左到右）应用一个函数，最终合并为一个值。

语法：

```js
    array.reduce(function(total, currentValue, currentIndex, arr), initialValue)
```

参数：

function(必须): 数组中每个元素需要调用的函数。

```js
    // 回调函数的参数
    1. total(必须)，初始值, 或者上一次调用回调返回的值
    2. currentValue(必须),数组当前元素的值
    3. index(可选), 当前元素的索引值
    4. arr(可选),数组对象本身
```

initialValue(可选): 指定第一次回调 的第一个参数。

回调第一次执行时:

- 如果 initialValue 在调用 reduce 时被提供，那么第一个 total 将等于 initialValue，此时 currentValue 等于数组中的第一个值；
- 如果 initialValue 未被提供，那么 total 等于数组中的第一个值，currentValue 等于数组中的第二个值。此时如果数组为空，那么将抛出 TypeError。
- 如果数组仅有一个元素，并且没有提供 initialValue，或提供了 initialValue 但数组为空，那么回调不会被执行，数组的唯一值将被返回。

eg:

```js
    // 数组求和 
    let sum = [0, 1, 2, 3].reduce(function (a, b) {
      return a + b;
    }, 0);
    // 6
    // 将二维数组转化为一维 将数组元素展开
    let flattened = [[0, 1], [2, 3], [4, 5]].reduce(
      (a, b) => a.concat(b),
      []
    );
    // [0, 1, 2, 3, 4, 5]
```

#### reduceRight  从右至左累加

这个方法除了与reduce执行方向相反外，其他完全与其一致，请参考上述 reduce 方法介绍。

#### ES6：find()& findIndex() 根据条件找到数组成员

find()定义：用于找出第一个符合条件的数组成员，并返回该成员，如果没有符合条件的成员，则返回undefined。

findIndex()定义：返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1。

这两个方法

语法：

```js
    let new_array = arr.find(function(currentValue, index, arr), thisArg)
    let new_array = arr.findIndex(function(currentValue, index, arr), thisArg)
```

参数:(这几个方法的参数，语法都类似)

function(必须): 数组中每个元素需要调用的函数。

```
    // 回调函数的参数
    1. currentValue(必须),数组当前元素的值
    2. index(可选), 当前元素的索引值
    3. arr(可选),数组对象本身

```

thisValue(可选):  当执行回调函数时this绑定对象的值，默认值为`undefined`

这两个方法都可以识别`NaN`,弥补了`indexOf`的不足.

eg:

```js
    // find
    let a = [1, 4, -5, 10].find((n) => n < 0); // 返回元素-5
    let b = [1, 4, -5, 10,NaN].find((n) => Object.is(NaN, n));  // 返回元素NaN
    // findIndex
    let a = [1, 4, -5, 10].findIndex((n) => n < 0); // 返回索引2
    let b = [1, 4, -5, 10,NaN].findIndex((n) => Object.is(NaN, n));  // 返回索引4
```

浏览器兼容(MDN):Chrome 45,Firefox 25,Opera 32, Safari 8, Edge yes,

#### ES6 keys()&values()&entries() 遍历键名、遍历键值、遍历键名+键值

定义：三个方法都返回一个新的 Array Iterator 对象，对象根据方法不同包含不同的值。

语法：

```js
    array.keys()
    array.values()
    array.entries()
```

参数：无。

遍历栗子(摘自[ECMAScript 6 入门](http://es6.ruanyifeng.com/#docs/array#数组实例的-entries，keys-和-values))：

```js
    for (let index of ['a', 'b'].keys()) {
      console.log(index);
    }
    // 0
    // 1
    
    for (let elem of ['a', 'b'].values()) {
      console.log(elem);
    }
    // 'a'
    // 'b'
    
    for (let [index, elem] of ['a', 'b'].entries()) {
      console.log(index, elem);
    }
    // 0 "a"
    // 1 "b"
```

在`for..of`中如果遍历中途要退出，可以使用`break`退出循环。

如果不使用`for...of`循环，可以手动调用遍历器对象的next方法，进行遍历:

```js
    let letter = ['a', 'b', 'c'];
    let entries = letter.entries();
    console.log(entries.next().value); // [0, 'a']
    console.log(entries.next().value); // [1, 'b']
    console.log(entries.next().value); // [2, 'c']
```

entries()浏览器兼容性(MDN):Chrome 38, Firefox 28,Opera 25,Safari 7.1

keys()浏览器兼容性(MDN):Chrome 38, Firefox 28,Opera 25,Safari 8,

**注意**:目前只有Safari 9支持,，其他浏览器未实现，babel转码器也还未实现

### Date

### function

## 作用域

### 暂时性死区
```js
var a = 300
if(true){
	a = 10
	// 当前块级作用域中存在a使用let / const声明的情况下,给a赋值时,
	// 只会在当前作用域找变量a, 而这时还未到声明时候,所以会报错
	let a = 1
}
```

## 继承
[参考链接](https://zhuanlan.zhihu.com/p/25578222)


### 构造函数继承
```js
function A_one(){
    this.name = 'this is A_one'
}
A_one.prototype.say = function(){
    console.log('my name is A_one');
}
function B_one(){
    A_one.call(this)
    this.height = '180cm'
}
B_one.prototype.see = function(){
    console.log('B_one see something');
}

// 构造函数实例
let test_one = new B_one()
console.log(test_one);
// test_one.say() //无法继承A_one的原型
test_one.see()
console.log(test_one.name);
```

### 原型链继承
```js
//原型链继承
function A_two(){
    this.name = 'this is A_two'
}
A_two.prototype.say = function(){
    console.log('my name is A_two');
}
function B_two(){
    this.height = '180cm'
}
B_two.prototype.see = function(){
    console.log('B_two see something');
}

B_two.prototype = new A_two()
B_two.prototype.constructor = B_two

let test_two = new B_two()
test_two.say()
// test_two.see() //B_two的原型被覆盖
console.log(test_two);
console.log(test_two.__proto__.name);          //this is A_two
console.log(test_two.__proto__.constructor);   //[Function: B_two]
```

### 组合继承
```js
//组合继承
function Parent(name) { 
    this.name = name;
}

Parent.prototype.sayName = function() {
    console.log('parent name:', this.name);
}
Parent.prototype.doSomething = function() {
    console.log('parent do something!');
}
function Child(name, parentName) {
    Parent.call(this, parentName);
    this.name = name;
}

Child.prototype = new Parent();      
Child.prototype.constructor = Child;
Child.prototype.sayName = function() {
    console.log('child name:', this.name);
}

var child = new Child('son');
child.sayName();       // child name: son
child.doSomething();   // parent do something!

```

### 寄生继承
```js
function Parent(name) {
    this.name = name;
}
Parent.prototype.sayName = function() {
    console.log('parent name:', this.name);
}

function Child(name, parentName) {
    Parent.call(this, parentName);  
    this.name = name;    
}

function create(proto) {
    function F(){}
    F.prototype = proto;
    return new F();
}

Child.prototype = Object.create(Parent.prototype);
Child.prototype.sayName = function() {
    console.log('child name:', this.name);
}
Child.prototype.constructor = Child;

var parent = new Parent('father');
parent.sayName();    // parent name: father


var child = new Child('son', 'father');
child.sayName();     // child name: son
```

### ES6继承
```js
class Parent {
    constructor(name) {
	this.name = name;
    }
    doSomething() {
	console.log('parent do something!');
    }
    sayName() {
	console.log('parent name:', this.name);
    }
}

class Child extends Parent {
    constructor(name, parentName) {
	super(parentName);
	this.name = name;
    }
    sayName() {
 	console.log('child name:', this.name);
    }
}

const child = new Child('son', 'father');
child.sayName();            // child name: son
child.doSomething();        // parent do something!

const parent = new Parent('father');
parent.sayName();           // parent name: father
```

## Set
Set 对象存储的值总是唯一的，所以需要判断两个值是否恒等。有几个特殊值需要特殊对待：

- +0 与 -0 在存储判断唯一性的时候是恒等的，所以不重复
- undefined 与 undefined 是恒等的，所以不重复
- NaN 与 NaN 是不恒等的，但是在 Set 中认为NaN与NaN相等，所有只能存在一个，不重复。

### Set的属性
- size：返回Set实例的成员总数。

### Set实例对象的方法
- `add(value)`：添加某个值，返回 Set 结构本身(可以链式调用)。
- `delete(value)`：删除某个值，删除成功返回`true`，否则返回`false`。
- `has(value)`：返回一个布尔值，表示该值是否为Set的成员。
- `clear()`：清除所有成员，没有返回值。

```js
let a = new Set()
a.add('1').add(undefined).add(null).add(NaN)

a.size          // 4
a.delete('1')   // true
a.has('1')      // true
a.clear()       // Set(0) {}

```
### 遍历方法
- `keys()`：返回键名的遍历器。
- `values()`：返回键值的遍历器。
- `entries()`：返回键值对的遍历器。
- `forEach()`：使用回调函数遍历每个成员。

由于Set结构没有键名，只有键值（或者说键名和键值是同一个值），所以`keys`方法和`values`方法的行为完全一致。

```js
const set = new Set(['a', 'b', 'c'])

for (let item of set.keys()) {
  console.log(item)
}
// a
// b
// c

for (let item of set.values()) {
  console.log(item)
}
// a
// b
// c

for (let item of set.entries()) {
  console.log(item)
}
// ["a", "a"]
// ["b", "b"]
// ["c", "c"]

// 直接遍历set实例，等同于遍历set实例的values方法
for (let i of set) {
  console.log(i)
}
// a
// b
// c

set.forEach((value, key) => console.log(key + ' : ' + value))

// a: a
// b: b
// c: c
```

### 应用
- 数组去重
```js
arr = [1,1,5,5,6,7]
unquine = [... new Set(arr)]
//[1,5,6,7]
```
- 合并两个set对象
```js
let a = new Set([1, 2, 3])
let b = new Set([4, 3, 2])
let union = new Set([...a, ...b]) // {1, 2, 3, 4}
```
- 交集
```js
let a = new Set([1, 2, 3])
let b = new Set([4, 3, 2])
let intersect = new Set([...a].filter(x => b.has(x)))  // {2, 3} 利用数组的filter方法
```
- 差集
```js
let a = new Set([1, 2, 3])
let b = new Set([4, 3, 2])
let difference = new Set([...a].filter(x => !b.has(x))) //  {1} 
```

## Map
Map对象保存键值对。任何值(对象或者原始值) 都可以作为一个键或一个值。构造函数Map可以接受一个数组作为参数。

### Map对象的属性
- size：返回Map对象中所包含的键值对个数

### Map对象的方法
- set(key, val): 向Map中添加新元素, 如果key已存在则覆盖value
- get(key): 通过键值查找特定的数值并返回
- has(key): 判断Map对象中是否有Key所对应的值，有返回true,否则返回false
- delete(key): 通过键值从Map中移除对应的数据，删除成功返回true,否则返回false
- clear(): 将这个Map中的所有元素删除

```js
const m1 = new Map([['a', 111], ['b', 222]])
console.log(m1) // {"a" => 111, "b" => 222}
m1.get('a')  // 111

const m2 = new Map([['c', 3]])
const m3 = new Map(m2)
m3.get('c') // 3
m3.has('c') // true
m3.set('d', 555)
m3.get('d') // 555
m3.size    // 2
```

### 遍历方法
- `keys()`：返回键名的遍历器
- `values()`：返回键值的遍历器
- `entries()`：返回键值对的遍历器
- `forEach()`：使用回调函数遍历每个成员

```js
const map = new Map([['a', 1], ['b',  2]])

for (let key of map.keys()) {
  console.log(key)
}
// "a"
// "b"

for (let value of map.values()) {
  console.log(value)
}
// 1
// 2

for (let item of map.entries()) {
  console.log(item)
}
// ["a", 1]
// ["b", 2]

// 或者
for (let [key, value] of map.entries()) {
  console.log(key, value)
}
// "a" 1
// "b" 2

// for...of...遍历map等同于使用map.entries()

for (let [key, value] of map) {
  console.log(key, value)
}
// "a" 1
// "b" 2
```

### 应用
- Map与对象的互换
```js
const obj = {}
const map = new Map(['a', 111], ['b', 222])
for(let [key,value] of map) {
  obj[key] = value
}
console.log(obj) // {a:111, b: 222}
```
- 数组去重
```js
arr = [1,1,2,2,3,3,4,4,4,4]
arr.forEach((item) => {
    map.set(item,item)
})
res = [...map.values()] // values(),keys() 均可
res = [...map.keys()]
```

## 事件

### currentTarget与taget的关系
currentTarget始终是监听事件者，而target是事件的真正发出者。

```html
<body>
    <ul>
        <li>hello 1</li>
        <li>hello 2</li>
        <li>hello 3</li>
        <li>hello 4</li>
    </ul>
    <script>
        let ul = document.querySelectorAll('ul')[0]
        let aLi = document.querySelectorAll('li')
        ul.addEventListener('click', function (e) {
            let oLi1 = e.target
            let oLi2 = e.currentTarget
            console.log(oLi1)   //  被点击的li
            console.log(oLi2)   // ul
            console.log(oLi1 === oLi2)  // false
        })
    </script>
</body>
```

### 事件

`element.addEventListener(type, listener, useCaputre)`

- type: 事件类型
- listener: 事件回调函数
- userCaputre: true捕获阶段触发, false 冒泡阶段触发 ; 默认值为false

`event.preventDefault()`

位于回调函数中,可以阻止类似于a标签跳转页面的默认事件

`event.stopPropagation()`

位于回调函数中,用于阻止捕获和冒泡阶段中当前事件的进一步传播


## Math
### Math.abs(number) 获取参数的绝对值
```js
Math.abs(10)//10
Math.abs(-10)//10
Math.abs("10")//10
Math.abs("10abc")//NaN
window.parseFloat("10abc")//10
Number("10abc")//NaN
```

### Math.ceil(number) 向上取整
```js
Math.ceil(10.0001)//11
Math.ceil("10.99abc")//NaN
```

### Math.floor(number) 向下取整
```js
Math.floor(10.2) //10
Math.floor(10.99)//10
Math.floor("10.99")//10 字符串参数
Math.floor("10.99abc")//NaN
```

### Math.round(number) 四舍五入保留整数
```js
Math.round(10.499)//10
Math.round(10.500)//11
```

### Math.max(numberX,numberY......)
```js
arr = [1,2,3,5,4,8,9,5,45]
Math.max(10,200,400)// 400
Math.max("1077",200,400)// 1077 注意返回值是Number类型,而不是String类型
Math.max([1,2,3],[4,5,6]) // NaN
Math.max.apply(arr,arr) // 45
```

### Math.min(numberX,numberY,...)获取输入参数中最小的一个
```js
arr = [1,2,3,5,4,8,9,5,45]
Math.min(10,200,400)//10
Math.min(1077,"200",400)//200 注意返回值是Number类型,而不是String类型
Math.min([1,2,3],[4,5,6]) //NaN
Math.max.apply(arr,arr) // 1
```

### Math.random() 返回0-1的随机数
```js
Math.random()//0.636911032255739
```

### Math.toFixed 保留几位小数是Number的方法
toFIxed 采用 四舍五入的法则
```js
123.43323.toFixed() //"123",当参数为空时默认保留到整数,注意返回的是String而不是Number
123.43323.toFixed(2)//"123.43"
123.toFixed(2)//Uncaught SyntaxError: Unexpected token ILLEGAL 这里报错了是因为系统把123后面的点当成了是小数点而非调用方法点
(123).toFixed(2)//"123.00" ,当小数点后不足两位时,该方法会自动不足,这也是为什么返回值为字符串而非数值的原因
```

# JavaScript技巧

## 数组去重
### set去重
```js
arr = [1,1,5,5,6,7]
// Set 为可迭代对象
unquine = [... new Set(arr)]
//[1,5,6,7]
```

### map
```js
arr = [1,1,2,2,3,3,4,4,4,4]
arr.forEach((item) => {
    map.set(item,item)
})
res = [...map.values()] // values(),keys() 均可
res = [...map.keys()]
```

### object
```js
arr = [1,1,2,2,3,3,4,4,4,4]
obj = {}
res = arr.filter(item => {
    if(!obj.hasOwnProperty(item)){
        obj[item] = item
        return true
    }else{
        return false
    }
})
```

## 防抖
```html
<body>
    <button id="debounce">点我防抖</button>
</body>
<script>
    window.onload = function(){
        // 1,获取这个按钮,并绑定事件
        let myBtn = document.getElementById('debounce')
        myBtn.addEventListener('click',debounce(sayDebounce))
    }    

    // 2,防抖功能函数,接受传参
    function debounce(fn){
        // 4,创建一个标记用来存放定时器的返回值
        let timeout = null
        return function(){
            // 5,每次当用户点击/输入的时候,把前一个定时器清除
            clearTimeout(timeout)
            // 6,然后创建一个新的setTimeout
            // 这样就能保证点击按钮后的interval 间隔内
            // 如果用户还点击了的话,就不会执行fn函数
            timeout = setTimeout(() => {
                console.log(arguments);
                fn.call(this, arguments)
            },1000)
        }
    }

    // 3,需要进行防抖的事件处理
    function sayDebounce(){
        // ...有些需要防抖的工作,在这里执行
        console.log('防抖成功');
    }
</script>
```

## 节流
```html
<body>
    <button id="throttle">点我节流!</button>
</body>
<script>
    window.onload = function(){
        // 1、获取按钮，绑定点击事件
        var myBtn = document.getElementById('throttle')
        myBtn.addEventListener('click',throttle(sayThrottle))
    }
    // 2、节流函数体
    function throttle(fn){
        // 4、通过闭包保存一个标记
        let canRun = true
        console.log(canRun);
        return function() {
            // 5、在函数开头判断标志是否为 true，不为 true 则中断函数
            if(!canRun){
                return
            }
            // 6、将 canRun 设置为 false，防止执行之前再被执行
            canRun = false
            // 7、定时器
            setTimeout(() => {
                fn.call(this,arguments)
                // 8、执行完事件（比如调用完接口）之后，重新将这个标志设置为 true
                canRun = true
            },1000)
        }
    }

    // 3、需要节流的事件
    function sayThrottle(){
        console.log("节流成功!")
    }
</script>
```


## 深拷贝
```js
function deepCopy(obj){
    if(typeof obj !== 'object'){
        return obj
    }

    let copyResult = obj instanceof Array ? [] : {}
    for(const key in obj){
        if(obj.hasOwnProperty(key)){
            if(typeof obj[key] === 'object'){
                copyResult[key] = deepCopy(obj[key])
            }else{
                copyResult[key] = obj[key]
            }
        }
    }
    return copyResult
}
```

## JSONP
```html
<div id="wrap">
		<input type="text" id="text" placeholder="请输入搜索关键字">
		<ul id="list"></ul>
	</div>
	<script type="text/javascript">
		//wd  查询关键字
		//cd 返回函数 回调函数
		let oInput=document.getElementById("text"),
			oList=document.getElementById("list");
			oInput.oninput=function(){
				var _val=this.value;//获取当前input框里的内容
				if(_val){
					let oS=document.createElement("script"); //创造script标签
					oS.src=`https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd=${_val}&cb=getDate`;
					document.body.appendChild(oS);
					oS.onload=function(){ //在script标签加载完后删除标签
						document.body.removeChild(oS);
					}
				}
			};
		function getDate(data){
			//data是后台发送过来的函数调用里的参数
			console.log(data);
			let arr=data.s, //获取联想词
				str='',
				len=arr.length;
			for(let i=0;i<len;i++){
				str+=`<li><a href="">${arr[i]}</a></li>`
			}
			oList.innerHTML=str;
		}
	</script>
```

## 判断数组

```js
// 第一种 Array.isArray()
Array.isArray([]) //true
Array.isArray("") //false
```
```js
//第二种 Object.prototype.toString.call()
Object.prototype.toString.call([]) // "[object Array]"
Object.prototype.toString.call({}); // "[object Object]"
Object.prototype.toString.call(null); // "[object Null]"
Object.prototype.toString.call(document); // "[object HTMLDocument]"
Object.prototype.toString.call(function f(){}); // "[object Function]"
Object.prototype.toString.call(undefined) // "[object Undefined]"
Object.prototype.toString.call(NaN) // "[object Number]"
Object.prototype.toString.call(new Map([{1:2}])) // "[object Map]"
```
```js
// instanceof运算符方案(存在缺陷)
['a'] instanceof Array; // true
```

## 快速强制转换为布尔值
```js
a = 1
b = 2 
!!a  // true
!!b  // false
// 其中要注意的是
!!{} // 结果为true
!![] // 结果为true
!!new Map //true
!!new Set //true
```

## new 操作符

```js
function Test(name, age) {
    this.name = name
    this.age = age
}
Test.prototype.sayName = function () {
    console.log(this.name)
}
  

function objectFactory(){
    let obj = new Object() //用new Object() 的方式新建了一个对象 obj
    // console.log([].shift.call(arguments));
    console.log(arguments);
    //取出第一个参数，就是我们要传入的构造函数。
    //此外因为 shift 会修改原数组，所以 arguments 会被去除第一个参数
    let Constructor = [].shift.call(arguments) 
    //将 obj 的原型指向构造函数，这样 obj 就可以访问到构造函数原型中的属性
    obj.__proto__ = Constructor.prototype
    //使用 apply，改变构造函数 this 的指向到新建的对象，这样 obj 就可以访问到构造函数中的属性
    Constructor.apply(obj,arguments)
    //返回obj
    return obj
}

const a = objectFactory(Test,'123',321)
console.log(a);
a.sayName()

```



# JavaScript API


# DOM

## 获取常用元素
- html标签元素
`document.documentElement`
-  body标签元素
`document.body`
- 获取网页文档标题(也可设置)
`document.title`
`document.title = 'xxxx'`

## 查找元素
```js
document.getElementById()     //通过id
document.getElementByTagNme() //通过标签名

document.querySelector()      //通过css选择符来查找 只会获取一个
//例如
document.querySelector('body')   //获取body元素
document.querySelector('#mydiv') //获取id为mydiv元素
document.querySelector('.btn')   //获取类为 btn 的第一个元素
document.querySelector('img.btn')//获取类为 btn 的第一个图像元素 

document.querySelectorAll()   //获取所有
```

## 获取修改元素特性
`<div id='hahaha' data-test='test'></div>`
其中 id 与 data-test 为特性
```js
element = document.getElementById('hahaha')
element.getAttribute('id') // hahaha
element.getAttribute('data-test') // test
element.getAttribute('data') // null
element.setAttribute('class','btn') //设置标签元素class为btn
element.removeAttribute()
```

## 自定义数据属性
给标签添加非标准属性,但要添加 data- 前缀
```html
<div id='hahaha' data-test='test'></div>
<script>
	element = document.getElementById('hahaha')
	
	// 取得自定义属性的值
	element.dataset.test
	
	// 设置值
	element.dataset.what = '???'
    
    // 设置类似与 data-v-btn 有多层后缀的
    element.dataset.vBtn = 'hhh'
</script>
```



# BOM

## location对象
location 对象提供了与当前窗口中加载的文档有关的信息，还提供了一些导航功能。事实上，location 对象是很特别的一个对象，因为它既是 window 对象的属性，也是 document 对象的属性；换句话说，window.location 和 document.location 引用的是同一个对象。location 对象的用处不只表现在它保存着当前文档的信息，还表现在它将 URL 解析为独立的片段，让开发人员可以通过不同的属性访问这些片段。下表列出了 location 对象的所有属性。

### hash
返回 URL 中的 `hash`（#号后跟零或多个字符），如果 URL 中不包含散列，则返回空字符串

### host
返回服务器名称和端口号（如果有）

### hostname
返回不带端口号的服务器名称

### href
返回当前加载页面的完整URL。而` location` 对象的` toString()`方法也返回这个值

### pathname
返回URL中的目录和（或）文件名

### port
返回 URL 中指定的端口号。如果 URL 中不包含端口号，则这个属性返回空字符串

### protocol
返回页面使用的协议。通常是 http: 或 https:

### search
返回URL的查询字符串。这个字符串以问号开头

### 解析参数
```js
function urlArgs() {
    var args = {};                                  // 定义一个空对象
    var query = location.search.substring(1);       // 查找到查询串，并去掉'? '
    var pairs = query.split("&");                   // 根据"&"符号将查询字符串分隔开
    for (var i = 0; i < pairs.length; i++) {        // 对于每个片段
        var pos = pairs[i].indexOf('=');            // 查找"name=value"
        if (pos == -1) continue;                    // 如果没有找到的话，就跳过
        var name = pairs[i].substring(0, pos);      // 提取name
        var value = pairs[i].substring(pos + 1);    // 提取value
        value = decodeURIComponent(value);          // 对value进行解码
        args[name] = value;                         // 存储为属性
    }
    return args;                                    // 返回解析后的参数
}
```
当通过上述任何一种方式修改URL之后，浏览器的历史记录中就会生成一条新记录，因此用户通过单击“后退”按钮都会导航到前一个页面。要禁用这种行为，可以使用 replace() 方法。这个方法只接受一个参数，即要导航到的 URL；结果虽然会导致浏览器位置改变，但不会在历史记录中生成新记录。在调用 replace() 方法之后，用户不能回到前一个页面，来看下面的例子：

```html
<!DOCTYPE html>
<html>
<head>
    <title>You won't be able to get back here</title>
</head>
    <body>
    <p>Enjoy this page for a second, because you won't be coming back here.</p>
    <script type="text/javascript">
        setTimeout(function () {
            location.replace("http://shijiajie.com/");
        }, 1000);
    </script>
</body>
</html>
```

如果将这个页面加载到浏览器中，浏览器就会在1秒钟后重新定向到 `shijiajie.com`。然后，“后退”按钮将处于禁用状态，如果不重新输入完整的 URL，则无法返回示例页面。

与位置有关的最后一个方法是 `reload()`，作用是重新加载当前显示的页面。如果调用 `reload()` 时不传递任何参数，页面就会以最有效的方式重新加载。也就是说，如果页面自上次请求以来并没有改变过，页面就会从浏览器缓存中重新加载。如果要强制从服务器重新加载，则需要像下面这样为该方法传递参数 `true`。

```js
location.reload();        // 重新加载（有可能从缓存中加载）
location.reload(true);    // 重新加载（从服务器重新加载）
```

## history 对象

`history` 对象保存着用户上网的历史记录，从窗口被打开的那一刻算起。因为 `history` 是 `window` 对象的属性，因此每个浏览器窗口、每个标签页乃至每个框架，都有自己的 `history` 对象与特定的 `window` 对象关联。出于安全方面的考虑，开发人员无法得知用户浏览过的 URL。不过，借由用户访问过的页面列表，同样可以在不知道实际 URL 的情况下实现后退和前进。

使用 `go()` 方法可以在用户的历史记录中任意跳转，可以向后也可以向前。这个方法接受一个参数，表示向后或向前跳转的页面数的一个整数值。负数表示向后跳转（类似于单击浏览器的“后退”按钮），正数表示向前跳转（类似于单击浏览器的“前进”按钮）。来看下面的例子。

```js
// 后退一页
history.go(-1);

// 前进一页
history.go(1);

// 前进两页
history.go(2);
```

 也可以给 `go()` 方法传递一个字符串参数，此时浏览器会跳转到历史记录中包含该字符串的第一个位置——可能后退，也可能前进，具体要看哪个位置最近。如果历史记录中不包含该字符串，那么这个方法什么也不做，例如： 

```js
// 跳转到最近的 shijiajie.com 页面
history.go("shijiajie.com");
```

 另外，还可以使用两个简写方法 `back()` 和 `forward()` 来代替 `go()`。顾名思义，这两个方法可以模仿浏览器的“后退”和“前进”按钮。 

除了上述几个方法外，`history` 对象还有一个 `length` 属性，保存着历史记录的数量。这个数量包括所有历史记录，即所有向后和向前的记录。对于加载到窗口、标签页或框架中的第一个页面而言，`history.length` 等于0。通过像下面这样测试该属性的值，可以确定用户是否一开始就打开了你的页面。

```js
if (history.length == 0){
    //这应该是用户打开窗口后的第一个页面
}
```

 虽然 `history` 并不常用，但在创建自定义的“后退”和“前进”按钮，以及检测当前页面是不是用户历史记录中的第一个页面时，还是必须使用它。 

### 判断浏览器

```js
const ua = navigator.userAgent.toLowerCase(); // 获取浏览器信息
```

# HTTP
[参考文章](https://juejin.im/post/5cd0438c6fb9a031ec6d3ab2)
http:请求发送
- 应用层
	应用层规定了向用户提供应用服务时通信的协议，如：
    TCP/IP 协议族内预存了各类通用的应用服务协议。比如，FTP（File Transfer Protocol，文件传输协议）和DNS（Domain Name System，域名系统）服务就是其中的两类以及HTTP协议。
    DNS域名系统提供域名（如：www.baidu.com）到IP地址（如：119.75.217.109）之间的解析服务。
- 传输层
	传输层对接上层应用层，提供处于网络连接中两台计算机之间的数据传输所使用的协议。
	在传输层有两个性质不同的协议：TCP（Transmission Control Protocol，传输控制协议）和UDP（User Data Protocol，用户数据报协议）。
- 网络层
	网络层规定了数据通过怎样的传输路线到达对方计算机传送给对方（IP协议等）。
- 链路层
	用来处理连接网络的硬件部分，包括控制操作系统、硬件的设备驱动、NIC（Network Interface Card，网络适配器，即网卡），及光纤等物理可见部分（还包括连接器等一切传输媒介）。硬件上的范畴均在链路层的作用范围之内。

## 请求方法
- GET：get方法一般用于获取服务器资源
- POST：post方法一般用于传输实体主体
- PUT：put方法一般用于传输文件
- DELETE：delete方法用于删除文件
- HEAD：head方法用于获取报文首部，不返回报文主体
- OPTIONS：options方法用于询问请求URI资源支持的方法



# 浏览器页面生命周期

[参考链接]( https://juejin.im/post/59f47e815188251364040e00#heading-0 )

## window.onload

`window`对象上的`onload`事件在所有文件包括样式表，图片和其他资源下载完毕后触发。

下面的例子正确检测了图片的大小，因为`window.onload`会等待所有图片的加载。

```js
<script>
  window.onload = function() {
    alert('Page loaded');

    // image is loaded at this time
    alert(`Image size: ${img.offsetWidth}x${img.offsetHeight}`);
  };
</script>

<img id="img" src="https://en.js.cx/clipart/train.gif?speed=1&cache=0">
```

## window.onunload

用户离开页面的时候，`window`对象上的`unload`事件会被触发，我们可以做一些不存在延迟的事情，比如关闭弹出的窗口，可是我们无法阻止用户转移到另一个页面上。

所以我们需要使用另一个事件 —  `onbeforeunload`。

## window.onbeforeunload

如果用户即将离开页面或者关闭窗口时，`beforeunload`事件将会被触发以进行额外的确认。

浏览器将显示返回的字符串，举个例子：

```js
window.onbeforeunload = function() {
  return "There are unsaved changes. Leave now?";
};
```



# CSS

## 布局

### 圣杯布局

```html
<style>
        * {
            margin: 0px;
        }
        .container{
            padding: 0 200px;
            height: 20px;
        }
        .center {
            float: left;
            width: 100%;
            background-color: red;
        }
        .left {
            float: left;
            width: 200px;
            background-color: green;
            margin-left: -100%;
            position: relative;
            left: -200px;
        }
        .right {
            float: left;
            width: 200px;
            background-color: blue;
            margin-left: -200px;
            position: relative;
            right: -200px;
        }
    </style>
    <section class='container'>
        <div class='center'>center</div>
        <div class="left">left</div>
        <div class="right">right</div>
    </section>
```

缺点

- center部分的最小宽度不能小于left部分的宽度，否则会left部分掉到下一行
- 如果其中一列内容高度拉长(如下图)，其他两列的背景并不会自动填充。(借助等高布局正padding+负margin可解决，下文会介绍)

### 双飞翼布局

```html
<style>
        * {
            margin: 0px;
        }
        .container{
            min-width: 600px;
            overflow: hidden;
        }
        .center {
            float: left;
            width: 100%;
            background-color: red;
        }
        .inner {
            margin: 0 200px;
            height: 500px;
        }
        .left {
            float: left;
            width: 200px;
            background-color: green;
            margin-left: -100%;
        }
        .right {
            float: left;
            width: 200px;
            background-color: blue;
            margin-left: -200px;
        }
    </style>
    <section class='container'>
        <div class='center'>
            <div class='inner'>hh</div>
        </div>
        <div class="left">left</div>
        <div class="right">right</div>
    </section>
```

缺点

 **多加一层 dom 树节点，增加渲染树生成的计算量**。 



## flex

 [参考链接](https://juejin.im/post/58e3a5a0a0bb9f0069fc16bb#heading-8 )



# EventLoop

# cookie,localStorage,sessionStorage

## cookie

### 添加

```js
document.cookie = "name=oeschger";
document.cookie = "favorite_food=tripe";
alert(document.cookie);
// 显示: name=oeschger;favorite_food=tripe
```

## loaclStorage

```js
// 添加 或 修改(覆盖原键值)
localStorage.setItem('myCat', 'Tom');
// 读取
let cat = localStorage.getItem('myCat');
// 移除
localStorage.removeItem('myCat');
// 移除所有
localStorage.clear();
```



