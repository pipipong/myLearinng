# 前言

## jQuery与js转换问题

- jquery---->javascript: var jsObject=$("#id").get(0); //这样jsObject就可以调用javascript的方法就没问题了. 
- javascript---->jquery: var \$object=\$(document.getElementById("id"));//$object这个就可以调用jquery的方法了 

## 浏览器兼容问题

**1.常遇到的关于浏览器的宽高问题：**

​	document代表的是整个文档(对于一个网页来说包括整个网页结构)；

​	document.documentElement是整个文档节点树的根节点，在网页中即html标签；

​	document.body是整个文档DOM节点树里的body节点，网页中即为body标签元素。

```js
//以下均可console.log()实验
var winW=document.body.clientWidth||document.documentElement.clientWidth;//网页可见区域宽
var winH=document.body.clientHeight||document.documentElement.clientHeight;//网页可见区域宽
//以上为不包括边框的宽高，如果是offsetWidth或者offsetHeight的话包括边框

var winWW=document.body.scrollWidth||document.documentElement.scrollWidth;//整个网页的宽
var winHH=document.body.scrollHeight||document.documentElement.scrollHeight;//整个网页的高

var scrollHeight=document.body.scrollTop||document.documentElement.scrollTop;//网页被卷去的高
var scrollLeft=document.body.scrollLeft||document.documentElement.scrollLeft;//网页左卷的距离

var screenH=window.screen.height;//屏幕分辨率的高
var screenW=window.screen.width;//屏幕分辨率的宽
var screenX=window.screenLeft;//浏览器窗口相对于屏幕的x坐标（除了FireFox）
var screenXX=window.screenX;//FireFox相对于屏幕的X坐标
var screenY=window.screenTop;//浏览器窗口相对于屏幕的y坐标（除了FireFox）
var screenYY=window.screenY;//FireFox相对于屏幕的y坐标
```

**2.event事件问题：**

```js
//event事件问题
document.onclick=function(ev){//谷歌火狐的写法，IE9以上支持，往下不支持；
    var e=ev;
    console.log(e);
}
document.onclick=function(){//谷歌和IE支持，火狐不支持；
    var e=event;
    console.log(e);
}
document.onclick=function(ev){//兼容写法；
    var e=ev||window.event;
    var mouseX=e.clientX;//鼠标X轴的坐标
    var mouseY=e.clientY;//鼠标Y轴的坐标
}
```

**3.节点相关的问题，我直接封装了函数，以便随时可以拿来使用：**

 	1）兄弟节点，所有浏览器都支持

​     	   	 ①nextSibling 下一个兄弟节点，可能是非元素节点；会获取到文本节点

​      		   ②previousSibling  上一个兄弟节点，可能是非元素节点；会获取到文本节点

​	 2）兄弟元素，IE8以前不支持

​       		 ①previousElementSibling 获取上一个紧邻的兄弟元素，会忽略空白 

​       		 ②nextElementSibling  获取下一个紧邻的兄弟元素，会忽略空白  

```js
//兼容浏览器
// 获取下一个紧邻的兄弟元素
function getNextElement(element) {
   // 能力检测
  if(element.nextElementSibling) {
      return element.nextElementSibling;
   } else {
      var node = element.nextSibling;
      while(node && node.nodeType !== 1) {  //Element nodeType：1
          node = node.nextibling;
      }
      return node;
    }
 }

/**
* 返回上一个元素
* @param element
* @returns {*}
*/
function getPreviousElement(element) {
    if(element.previousElementSibling) {
        return element.previousElementSibling;
    }else {
        var node = element.previousSibling;
        while(node && node.nodeType !== 1) {
            node = node.previousSibling;
            }
        return node;
    }
}

/**
* 返回第一个元素firstElementChild的浏览器兼容
* @param  parent
* @returns {*}
*/
function getFirstElement(parent) {
    if(parent.firstElementChild) {
        return parent.firstElementChild;
    }else {
        var node = parent.firstChild;
        while(node && node.nodeType !== 1) {
            node = node.nextSibling;
            }
        return node;
    }
}

/**
* 返回最后一个元素
* @param  parent
* @returns {*}
*/
function getLastElement(parent) {
    if(parent.lastElementChild) {
        return parent.lastElementChild;
    }else {
        var node = parent.lastChild;
        while(node && node.nodeType !== 1) {
            node = node.previousSibling;
            }
        return node;
    }
}

/**
*获取当前元素的所有兄弟元素
* @param  element
* @returns {Array}
*/
function sibling(element) {
    if(!element) return null;//如果元素不存在，返回空

    var elements = [ ];
    //previousSibling：上一个兄弟节点，可能是非元素节点；会获取到文本节点【所有浏览器都支持】
    var node = element.previousSibling;
    while(node) {
        if(node.nodeType === 1) {
            elements.push(node);
        }
        node = node.previousSibling;
    }
    node = element.previousSibling;
    while(node ) {
        if(node.nodeType === 1) {
            elements.push(node);
        }
        //nextSibling：下一个兄弟节点，可能是非元素节点；会获取到文本节点【所有浏览器都支持】
        node = node.nextSibling;
    }
    return elements;
}
```

**4. 设置监听事件：**

```js
//设置监听事件
//添加事件监听，三个参数分别为 对象、事件类型、事件处理函数，默认为false
function addEvent(obj,type,fn){
    //若支持则返回ƒ addEventListener() { [native code] }  若不支持则返回undefined
    if (obj.addEventListener) {  //addEventListener无括号
        obj.addEventListener(type,fn,false);//非IE
    } else{
        obj.attachEvent("on"+type,fn);//ie,这里已经加上on，传参的时候注意不要重复加了
    };
}
//删除事件监听
function removeEvent(obj,type,fn){
    //若支持则返回ƒ removeEventListener() { [native code] }  若不支持则返回undefined
    if (obj.removeEventListener) {   //removeEventListener无括号
        obj.removeEventListener(type,fn,false);//非IE
    } else{
        obj.detachEvent("on"+type,fn);//ie，这里已经加上on，传参的时候注意不要重复加了
    };
    }
```

**5.元素到浏览器边缘的距离：**

![](img\3.gif)

```js
//在这里加个元素到浏览器边缘的距离，很实用
function offsetTL(obj){//获取元素内容距离浏览器边框的距离（含边框）
    var ofL=0,ofT=0;
    while(obj){
        //offsetLeft：如父元素没有position时：距离浏览器左边的距离
        //offsetLeft：如父元素有position时且不是static时：距离父元素左边框的距离			
        //clientLeft：元素本身左边框宽度
        ofL+=obj.offsetLeft+obj.clientLeft; 
        ofT+=obj.offsetTop+obj.clientTop;
        obj=obj.offsetParent;
    }
    return{left:ofL,top:ofT};
}
```

**6.关于event事件中的target：**

​	event.target指向的是触发该事件的节点，监听click的话，就是被点击的那个元素  。

```js
//关于event事件中的target
document.onclick=function(e){
    var e=e||window.event;
    var Target=e.target||e.srcElement;//获取target的兼容写法，后面的为IE
}
```

**7.获取元素的非行间样式值：**

```js
//获取元素的非行间样式值
function getStyle(object,oCss) {
	if (object.currentStyle) {
		return object.currentStyle[oCss];//IE
	}else{
		return getComputedStyle(object,null)[oCss];//除了IE
	}
}
```

https://www.cnblogs.com/duenyang/p/6066737.html

https://www.jb51.net/article/82676.htm

https://www.cnblogs.com/DF-fzh/p/5408241.html

https://www.jianshu.com/p/419436afc845

# 一、JavaScript基本概念

## 数据类型

变量本身没有类型，其类型取决于赋予变量的值

使用typeof可以检测数据的类型

```js
var value = "lbc";
alert (typeof value);
alert (typeof (value));  //()加不加都可以
```

| 数据类型  |           说明           |
| :-------: | :----------------------: |
|   null    | 空值，赋予对象的值为null |
| undefined | 未声明；已声明但未初始化 |
|  number   |           数字           |
|  string   |          字符串          |
|  boolean  |          布尔值          |
|  object   |           对象           |

## for/in语句

key存储的是数组元素的下标，或者对象的属性名或者方法名

```js
for(var index in arr){
    alert(arr[index]);
}
```

# 二、JavaScript引用类型

## Number类型

**转换为数值的2种方法：**

1.使用parseInt()，可用于向下取整

依次查看，直到遇到非数字字符为止，将前面分析的合法数字转义为数值

```js
alert(parseInt("123lbc"));    //返回值123
alert(parseInt("123.456"));   //返回值123
alert(parseInt("lbc"));       //返回值NaN  not a number   用来判断是否为数字  isNaN(value)
```

2.使用parseFloat()

用法与parseInt基本相同，但他能识别第一个出现的小数点号

```js
alert(parseFloat("123lbc"));    //返回值123  如果可解析为整数，parseFloat()会返回一个整数
alert(parseFloat("123.456lbc"));   //返回值123.456
alert(parseFloat("123.456.789"));      //返回值123.4563
alert(parseFloat("lbc"));       //返回值NaN  not a number   用来判断是否为数字  isNaN(value)
```

## String类型

**转换成字符串的2种方法：**

1.使用加号运算符

当值与空字符串相加时，JavaScript会自动把值转换为字符串

```js
var value = 12;
value = value+"";
alert(typeof value);   //string
```

2.使用toString()方法

```js
var value = 12；
var string  = value.toString();
alert(typeof string);  //string
```

**String的对象方法及属性：**

| **length**                                     | str.length                                                   |
| ---------------------------------------------- | ------------------------------------------------------------ |
| **charAt( *index* )**                          | **返回指定索引位置的字符**                                   |
| concat( *string1*, *string2*, ..., *stringX* ) | 连接两个或多个字符串，返回连接后的字符串                     |
| indexOf( *searchvalue* )                       | 返回字符串中检索指定字符第一次出现的位置，如果没有找到则返回-1 |
| lastIndexOf(searchvalue )                      | 从后向前检索字符串 ，返回字符串中检索指定字符最后一次出现的位置， 如果没有找到匹配字符串则返回 -1 |
| match()                                        | 找到一个或多个正则表达式的匹配                               |
| replace()                                      | 替换与正则表达式匹配的子串                                   |
| **search()**                                   | 返回字符在字符串中的位置，检索与正则表达式相匹配的值str.search('a') |
| **split( *separator* )**                       | str.split(“ ”)把字符串分割为子字符串数组                     |
| **substr(start,length)**                       | 在字符串中抽取从 *开始* 下标开始的指定数目的字符。 substr() 的参数指定的是子串的开始位置和长度，因此它可以替代 substring() 和 slice() 来使用 |
| slice(start，stop)                             | slice(start, end) 方法可提取字符串的某个部分，并以新的字符串返回被提取的部分。  使用 start（包含） 和 end（不包含） 参数来指定字符串提取的部分 |
| substring(start,stop)                          | substring(start，stop)提取字符串中两个指定的索引号之间的字符， 其内容是从 *start* 处到 *stop*-1 处的所有字符 ， 如果省略stop参数，那么返回的子串会一直到字符串的结尾。 |
| **toLocaleLowerCase()**                        | 把字符串转换为小写                                           |
| **toLocaleUpperCase()**                        | 把字符串转换为大写                                           |
| **toString()**                                 | 返回字符串对象值                                             |
| **trim()**                                     | 移除字符串首尾空白                                           |

1.length

```js
var txt="Hello World!";
alert(txt.length);    // 12
```

2.charAt(*index* )

```js
var str = "abcdefg";
alert(str.charAt(0));;    // a
```

3.concat(*string1*, *string2*, ..., *stringX* )

```js
var str1="Hello ";
var str2="world!";
var str3=" Have a nice day!";
var res1 = str1.concat(str2);   //Hello world!
var res2 = str1.concat(str2,str3);  //Hello world! Have a nice day!
```

4.indexOf(*searchvalue* )

```js
var str="Hello world, welcome to the universe.";
var res=str.indexOf("welcome"); //13
```

5.lastIndexOf(*searchvalue* )

```js
var str="I am from runoob，welcome to runoob site.";
var res=str.lastIndexOf("runoob"); //28
```

6.match()

```js
var text = "cat,bat,dot,ext";
var result = text.match(/.at/g);
alert(result);  //cat,bat
```

7.replace()

```js
var text = "cat,bat,dat,eat";
var result = text.replace(/at/,"ond");  //正则表达式不加引号
alert(result);   //cond,bat,dat,eat
result = text.replace(/at/g,"ond");    //正则表达式不加引号
alert(result);  //cond,bond,dond,eond
```

8.search()

```js
var text = "cat,bat,dat,eat";
var postion = text.search(/at/);    //正则表达式不加引号
alert(postion);  //1
```

9.split(*separator* )

```js
var str="How are you doing today?";
var res=str.split(" "); //How,are,you,doing,today?
```

10.substr(*start*,*length* )

```js
var str="Hello world!";
var res=str.substr(2,3);  //llo
```

11.slice(start，stop)

```js
var str="Hello world!";
var res=str.slice(1,5);  //ell0
```

12.substring(start，stop)

```js
var str="Hello world!";
var res=str.substring(1,5);  //ell0
```

13.toLocaleLowerCase()

```js
var str="Runoob";
var res = str.toLocaleLowerCase(); //runoob
```

14.toLocaleUpperCase()

```js
var str="Runoob";
var res = str.toLocaleUpperCase();//RUNOOB
```

15.toString()

```js
var str = 123;
var res = str.toString(); //123 类型为string
```

16.trim()

```js
var str = "       Runoob        ";
var res = str.trim(); //Runoob
```

## Array类型

**改变原数组的：**

shift：将第一个元素删除并且返回删除元素，空即为undefined
unshift：向数组开头添加元素，并返回新的长度
pop：删除最后一个并返回删除的元素
push：向数组末尾添加元素，并返回新的长度
reverse：颠倒数组顺序
sort：对数组排序
splice:splice(start,length,item)删，增，替换数组元素，返回被删除数组，无删除则不返回

**不改变原数组的：**

concat：连接多个数组，返回新的数组
join：将数组中所有元素以参数作为分隔符放入一个字符
slice：slice(start,end)，返回选定元素
map,filter,some,every等不改变原数组 值类型，但改变原数组 引用类型  **当数组中元素是值类型，绝对不会改变数组；当是引用类型，则可以改变数组**

### 1.定义数组

```js
var a = [];    //定义一个空数组
var arr = [1, 2 ,3 ,4];  //直接声明数组元素
alert (arr.length);    //返回数组元素的个数
```

### 2.引用数组

```js
var arr1 = [1,2,3];
var arr2 = arr1;  //引用，arr2和arr1都指向同一块空间
arr2.push(4);
alert(arr1);   //1,2,3,4
alert(arr2);   //1,2,3,4

```

消除引用

```js
var arr1 = [1,2,3];
var arr2 = [];  
for(var i=0; i<arr1.length; i++){
    arr2.push(arr1[i]);
}
alert(arr1);   //1,2,3
alert(arr2);   //1,2,3

```

### 3.栈/队列方法 影响原数组

|  push()   | 在数组尾部添加任意数量元素，返回修改后数组的长度, 影响原数组 |
| :-------: | :----------------------------------------------------------: |
| **pop()** |     **删除数组最后一个元素；返回被删除元素**, 影响原数组     |
| unshift() | 在数组头部添加任意数量元素，返回修改后数组的长度, 影响原数组 |
|  shift()  |        删除数组第一个元素；返回被删除元素,影响原数组         |

1.push()/pop()

```js
var arr = ["burc"];
var count = arr.push("love","vinx");
alert(count);  //3
count = arr.push("forever");
alert(count);  //4
var item = arr.pop();
alert(item);  //forever
```

### 4.重排序方法 影响原数组

| sort() | 返回值是经过排序后的数组,影响原数组 |
| ------ | ----------------------------------- |
|        |                                     |

1.sort()默认按字符串处理

```js
var arr = ["a", "b", "e", "y",1,2,3,10];
arr.sort();
alert(arr);          //1,10,2,3,a,b,e,y
```

2.对数字进行排序

```js
var arr = [1,2,15,6,4,3];
arr.sort(function compare(x,y){
	return x-y;            //从小到大：x-y   从大到小：y-x
});
alert(arr);

-----------------------第二种写法-----------------------------------------------------
function compare(x,y){
	return x-y;            //从小到大：x-y   从大到小：y-x
}
var arr = [1,2,15,6,4,3];
arr.sort(compare);
alert(arr);
```

### 5.迭代方法  不影响原数组

| map(function(item,index,array){})         | map() 方法按照原始数组元素顺序依次处理元素。 map() 方法返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值。 |
| ----------------------------------------- | ------------------------------------------------------------ |
| **forEach(function(item,index,array){})** | **forEach()方法按照原始数组元素顺序依次处理元素。无返回值。** |

1.map(function(item,index,array){

​    })

```js
var number = [1,2,3,4,5,6];
var mapResult = number.map(function(item,index,array){
	return item*2;
});
alert(mapResult); //2,4,6,8,10,12
```

2.forEach(function(item,index,array){

​    })

```js
var number = [1,2,3,4,5,6];
number.forEach(function(item,index,array){
    /*
    *执行某些操作
    */
	alert(item*3);
});
```

### 6.转换方法 不影响原数组

| toString() |              将数组转化字符串，不改变原数组              |
| :--------: | :------------------------------------------------------: |
| **join()** | **将数组转化字符串，不过他可以指定分隔符，不改变原数组** |

1.toString() 	

```js
var arr = [1,2,"burc","vinx"];
var brr = arr.toString();
alert(brr);   //1,2,burc,vinx    显式调用toString()方法 
alert(arr);   //1,2,burc,vinx    alert()要接受字符串参数，所以它会在后台调用toString()方法，
```

2.join()

```js
var arr = [1,2,"burc","vinx"];
var brr = arr.join("-");
alert(brr);        //1-2-burc-vinx
```

### 7.操作方法

| **concat()**         | 用于连接两个或多个数组 ，不会影响原始数组                    |
| -------------------- | ------------------------------------------------------------ |
| **slice(start,end)** | **使用 start（包含） 和 end（不包含） 参数来指定数组提取的部分，不会影响原始数组** |
| **splice() **        | **从数组中删除、修改、插入元素。 返回值是被删除元素，影响原数组** |

1.concat()

```js
var arr = [1,2,3];
var brr = arr.concat(6,7,8);
alert(arr);     // 1,2,3,6,7,8
```

2.slice()

```js
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
var citrus = fruits.slice(1,3);
alert(citrus);     // Orange,Lemon
```

3.splice() 

```js
var arr = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
-------------一个参数------------------
arr.splice(3);      //从位置三开始全部删除   
alert(arr);			//Banana,Orange,Lemon
-------------两个参数------------------
arr.splice(1,1);   //从位置一开始删除一个元素   
alert(arr);			//Banana,Lemon
-------------多个参数------------------
arr.splice(1,0,"A","B","C");  //从位置一开始，删除零个，插入三个元素
alert(arr);        //Banana,A,B,C,Lemon
-------------多个个参数------------------
arr.splice(2,1,"burc");  //从位置二开始，删除一个，插入一个元素
alert(arr); //Banana,A,burc,C,Lemon
```

### 8.位置方法

| indexOf()          | 从前到后检索数组 ，返回数组中某个指定的元素的位置。 不改变原数组 |
| ------------------ | ------------------------------------------------------------ |
| **lastIndexOf() ** | **从后向前检索数组，返回一个指定的元素在数组中最后出现的位置 ** |

1.indexOf()

```js
var arr = ["Banana", "Orange", "Apple", "Mango"];
var brr = arr.indexOf("Apple");
alert(brr);  //2
```

2.lastIndexOf()

```js
var arr = ["Banana", "Orange", "Mango", "Apple", "Apple", "Apple"];
var brr = arr.lastIndexOf("Apple");
alert(brr);  //5
```



## Date类型

```js
Date.now();  //获取当前时间的毫秒数
var myDate = new Date();//获取系统当前时间
myDate.getTime(); //获取当前时间(从1970.1.1开始的毫秒数)
myDate.getYear(); //获取当前年份(2位)
myDate.getFullYear(); //获取完整的年份(4位,1970-????)
myDate.getMonth(); //获取当前月份(0-11,0代表1月)
myDate.getDate(); //获取当前日(1-31)
myDate.getDay(); //获取当前星期X(0-6,0代表星期天)
myDate.getHours(); //获取当前小时数(0-23)
myDate.getMinutes(); //获取当前分钟数(0-59)
myDate.getSeconds(); //获取当前秒数(0-59)
myDate.getMilliseconds(); //获取当前毫秒数(0-999)

myDate.setDate(myDate.getDate()+1000);   //1000天以后的日期

alert(myDate.getFullYear()+"-"+(myDate.getMonth()+1)+"-"+myDate.getDate());
```

## RegExp类型

[[js中的正则表达式入门]](https://www.cnblogs.com/chenmeng0818/p/6370819.html)

| match()       | 找到一个或多个正则表达式的匹配str.match(re)                  |
| ------------- | ------------------------------------------------------------ |
| **search()**  | **返回字符在字符串中的位置，检索与正则表达式相匹配的值  str.search(re)** |
| **replace()** | **替换与正则表达式匹配的子串str.replace(re,’--’）**          |
| **test()**    | **校验字符串是否满足正则的要求 返回true 、false      re.test(srt);** |

语法

```
/正则表达式主体/修饰符(可选)

```

| 修饰符 | 描述                                                     |
| ------ | -------------------------------------------------------- |
| i      | 执行对大小写不敏感的匹配。                               |
| g      | 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。 |
| m      | 执行多行匹配。                                           |

| 表达式 | 描述                                     |
| ------ | ---------------------------------------- |
| [abc]  | 查找方括号之间的任何字符。任意一个就可以 |
| [0-9]  | 查找任何从 0 至 9 的数字。范围           |
| (x\|y) | 查找任何以 \| 分隔的选项。或             |

| 元字符  | 描述                                              |
| ------- | ------------------------------------------------- |
| \d      | 数字。 [0-9]                                      |
| \s      | 空白字符。 包括\n,\r,\f,\t,\v等                   |
| \w      | 数字、英文、 下划线  [0-9a-zA-Z_]                 |
| \D      | 非数字 [^0- 9】                                   |
| \S      | 非空白字符                                        |
| \W      | 非数字英文 [^0-9a-zA-Z_】                         |
| .       | 代表任意字符                                      |
| [^a-z】 | 非字母 []中^代表除了                              |
| \|      | 或者                                              |
| \       | 转义字符                                          |
| ^       | 限定开始位置 => 本身不占位置， 表示必须以什么开头 |
| $       | 限定结束位置 => 本身不占位置， 表示必须以什么结尾 |

| 量词  | 描述                                     |
| ----- | ---------------------------------------- |
| n+    | 匹配任何包含一次或多次 的字符串。        |
| n*    | 匹配任何包含零次或多次 的字符串。        |
| n?    | 匹配任何包含零次或一次 的字符串。        |
| {n}   | 精确匹配n次                          n   |
| {n,}  | 匹配至少n次                          >=n |
| {n,m} | 匹配至少n次，至多m次       >=n&&<=m      |

**一般来说，正则中的^表示开头，\$表示结束**

**比如 ^\d+\$ 匹配的整个字符串【从头到尾】只能是数字，因为他开头结尾都是数字，那么他就只能匹配： 1 、 12、 123....等等 **

**\d+$ 这个就匹配结尾是数字：比如 ：abc123**

**^\d+ 就匹配开头是数字，比如：123abc **

```js
var str = '1223334444';
var reg = /\d{2}/g;
var res = str.match(reg);
console.log(res)  //["12", "23", "33", "44", "44"]

var str ='  我是空格君  ';
var reg = /^\s+|\s+$/g; //匹配开头结尾空格
var reg = /\s+/g; //匹配空格  两种方法都可以
var res = str.replace(reg,'');
console.log('('+res+')')  //(我是空格君)

/*校验邮件地址是否合法 */
function IsEmail(str) {
    var reg=/^\w+@[a-zA-Z0-9]+\.[a-z]+$/i;
    return reg.test(str);
}

/*校验是否全由8位数字组成 */
function isStudentNo(str) {
    var reg=/^[0-9]{8}$/;   /*定义验证表达式*/
    return reg.test(str);     /*进行验证*/
}

/*是否带有小数*/
function isDecimal(strValue )  {  
   var  objRegExp= /^\d+\.\d+$/;
   return  objRegExp.test(strValue);  
}  

/*校验是否中文名称组成 */
function ischina(str) {
    var reg=/^[\u4E00-\u9FA5]{2,4}$/;   /*定义验证表达式*/
    return reg.test(str);     /*进行验证*/
}

/*js中正则表达式使数字、中文或指定字符高亮或变颜色*/
 var preCount= $(".myessay .panel-body pre");
 preCount.each(function(){
    var codestr = $(this).html().replace(/var|new|this|function|console.log|alert/img,"<span            style='color:red;'>$&</span>");  //目前在正则表达式匹配里可用$&代替被匹配到的内容。
    $(this).html(codestr);
 });
```

**[]中，不会出现两位数**

```js
//[12]表示1或者2 不过[0-9]这样的表示0到9 [a-z]表示a到z
//例如:匹配从18到65年龄段所有的人
var reg = /[18-65]/; // 这样写对么
reg.test('50')
 //Uncaught SyntaxError: Invalid regular expression: /[18-65]/: Range out of order in character class
//聪明的你想可能是8-6这里不对，于是改成[16-85]似乎可以匹配16到85的年龄段的，但实际上发现这也是不靠谱的

//实际上我们匹配这个18-65年龄段的正则我们要拆开来匹配
//我们拆成3部分来匹配 18-19  20-59 60-65 
reg = /(18|19)|([2-5]\d)|(6[0-5])/;
```

## Function类型

**函数定义**

```js
//方法一  推荐
function addNumber(x,y){
	return x+y;
}
alert(addNumber(1,6));
//方法二
var addNumber = function(x,y){  //function关键字后面没有函数名，使用变量addNumber即可引用函数
	return x+y;
};
alert(addNumber(1,6));
```

### 1.**作为值的函数**

因为函数名本身就是一个指针，所以函数也可以作为值来使用。

要访问函数的指针而不执行函数的话，必须去掉函数名后面那对大括号。

```js
function compare(x,y){
	return x-y;            //从小到大：x-y   从大到小：y-x
}
var arr = [1,2,15,6,4,3];
arr.sort(compare);  //访问函数的指针
alert(arr);
```

### 2.**arguments对象**

本函数没有定义形参，但是在函数体内通过arguments对象（伪数组）可以获取传递给该函数的每个实参值，arguments对象仅能够在函数体内使用，作为函数的属性而存在。

arguments有一个属性callee，该属性是一个指针，指向拥有这个arguments对象的函数。

```js
function outNumber(){
    for(var i = 0; i < arguments.length; i++)
    	alert(arguments[i]);
}
outNumber(2,3,4,6,9);

//阶乘函数
function factorial(num){
	if(num<=1){
		return 1;
	}else{
		return num * arguments.callee(num-1); //arguments.callee()指向factorial()函数
	}
}
alert(factorial(3));
```

### 3.call()方法

1.call()可以扩充函数赖以运行的作用域。

2.call：调用一个对象的一个方法，用另一个对象替换当前对象。例如：B.call(A, args1,args2);即A对象调用B对象的方法。 【继承时有用】

```js
window.color = "red";
var obj = {"color":"blue"};
function sayColor(){
	alert (this.color);
}
sayColor();  //red
sayColor.call(window);   //red
sayColor.call(obj);   //blue
```

## 单体内置对象

### 1.URL编码方法

**encodeURI()是Javascript中真正用来对URL编码的函数。**

它着眼于对整个URL进行编码，不会对本身属于URL的特殊字符进行编码，“; / ? : @ & = + $ , #”，也不进行编码

它对应的解码函数是decodeURI()。

```js
alert(encodeURI("http://www.w3school.com.cn"));  
//  http://www.w3school.com.cn
alert(encodeURI("http://www.w3school.com.cn/成Burc first/"));
//  http://www.w3school.com.cn/%E6%88%90Burc%20first/
alert(encodeURI(",/?:@&=+$#"));
//  ,/?:@&=+$#
```

encodeURIComponent()

与encodeURI()的区别是，它用于对URL的组成部分进行个别编码，而不用于对整个URL进行编码。**因此，“; / ? : @ & = + $ , #”，这些在encodeURI()中不被编码的符号，在encodeURIComponent()中统统会被编码 。**

它对应的解码函数是decodeURIComponent()。 

```js
alert(encodeURIComponent("http://www.w3school.com.cn"));
//  http%3A%2F%2Fwww.w3school.com.cn
alert(encodeURIComponent("http://www.w3school.com.cn/成Burc first/"));
//http%3A%2F%2Fwww.w3school.com.cn%2F%E6%88%90Burc%20first%2F
alert(encodeURIComponent(",/?:@&=+$#"));
//%2C%2F%3F%3A%40%26%3D%2B%24%23
```

### 2.eval()方法

用于Ajax方法中：非标准的json字符串、标准的json字符串转化为 json对象

```js
//PHP传入
echo json_encode($list);
//Ajax获取
success: function(xmlhttp){
    //非标准的json字符串、标准的json字符串转化为 json对象
    var obj  = eval("("+xmlhttp.responseText+")");
    ...
    ...
},
```

当解析器发现代码中调用eval()方法，它会将传入的参数当做实际的ECMAScript语句来解析，然后把解析结果插入到原位置。

```js
eval("alert('hi')");  //hi
```

### 3.Math对象

**舍入方法**

| ceil(num)      | 对num进行上舍入。       |
| -------------- | ----------------------- |
| **floor(num)** | **对 num进行下舍入。 ** |
| **round(num)** | **四舍五入。 **         |

```js
alert(Math.ceil(25.9));  //26
alert(Math.ceil(25.5));  //26
alert(Math.ceil(25.1));  //26
alert(Math.floor(25.9));  //25
alert(Math.floor(25.5));  //25
alert(Math.floor(25.1));  //25
alert(Math.round(25.9));  //26
alert(Math.round(25.5));  //26
alert(Math.round(25.1));  //25
```

**min()和max()**

| max(x,y)     | 返回 x 和 y 中的最高值。 |
| ------------ | ------------------------ |
| **min(x,y)** | 返回 x 和 y 中的最小值。 |

```js
var max = Math.max(3,2,88,41,5);
var min = Math.min(1,8,55,44,76);
alert(max);  //88
alert(min);  //1
```

**random()**

返回[0,1)之间的随机数。包含0，不包含1。

```js
Math.floor((Math.random()*10)+1);  //取得介于 1 到 10 之间的一个随机数
Math.floor((Math.random()*100)+1);  //取得介于 1 到 100 之间的一个随机数
```

**其他方法**

| abs()                                                        | 返回数的绝对值。                                             |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **sqrt()**                                                   | **返回数的平方根。 **                                        |
| **pow(x,y)**                                                 | **返回 x 的 y 次幂。**                                       |
| [acos(x)](http://www.w3school.com.cn/jsref/jsref_acos.asp)   | 返回数的反余弦值。                                           |
| [asin(x)](http://www.w3school.com.cn/jsref/jsref_asin.asp)   | 返回数的反正弦值。                                           |
| [atan(x)](http://www.w3school.com.cn/jsref/jsref_atan.asp)   | 以介于 -PI/2 与 PI/2 弧度之间的数值来返回 x 的反正切值。     |
| [atan2(y,x)](http://www.w3school.com.cn/jsref/jsref_atan2.asp) | 返回从 x 轴到点 (x,y) 的角度（介于 -PI/2 与 PI/2 弧度之间）。 |
| [cos(x)](http://www.w3school.com.cn/jsref/jsref_cos.asp)     | 返回数的余弦。                                               |
| [exp(x)](http://www.w3school.com.cn/jsref/jsref_exp.asp)     | 返回 e 的指数。                                              |
| [log(x)](http://www.w3school.com.cn/jsref/jsref_log.asp)     | 返回数的自然对数（底为e）。                                  |
| [sin(x)](http://www.w3school.com.cn/jsref/jsref_sin.asp)     | 返回数的正弦。                                               |
| [tan(x)](http://www.w3school.com.cn/jsref/jsref_tan.asp)     | 返回角的正切。                                               |
| [toSource()](http://www.w3school.com.cn/jsref/jsref_tosource_math.asp) | 返回该对象的源代码。                                         |
| [valueOf()](http://www.w3school.com.cn/jsref/jsref_valueof_math.asp) | 返回 Math 对象的原始值。                                     |

# 三、面向对象

类：模子

对象：产品

**原则**

1.用构造函数模式加属性【构造函数首字母大写】

2.用原型模式加方法 【节省资源，原型是类似class ，修改他可以影响一类元素】

```js
function CreateGirlFriend(name,age,appearance){
 	// var this = new Object(); 系统偷偷替咱做
 	this.name = name;
 	this.age = age;
    this.appearance = appearance;
    // return this;  系统偷偷替咱做
 }
 CreateGirlFriend.prototype.showName = function(){ //原型
 	console.log("女朋友的名字:"+this.name);
 };
  CreateGirlFriend.prototype.showAge = function(){ //原型
 	console.log("女朋友的年龄:"+this.age);
 };
  CreateGirlFriend.prototype.showAppearance = function(){ //原型
 	console.log("女朋友的外貌:"+this.appearance);
 };
 var girlFriend = new CreateGirlFriend("Gal Gadot","20","beautiful");
 girlFriend.showName();
```

## 面向对象的形式

**把面向过程的程序，改写成面向对象的形式**(面向对象高级)

**原则：**

不能有嵌套函数，可以有全局变量

**过程：**

window.onload   -->  构造函数

变量                     --->   属性

函数                    ---->  方法

**改错：**

this/事件/闭包/传参

```js
<script type="text/javascript">
	window.onload = function(){
		new TabSwitch("myDiv");
	}

    function TabSwitch(id){
    	var _this =this;
		var myDiv = document.getElementById(id);
		this.myBtn = myDiv.getElementsByTagName("input");   //属性
		this.myContent = myDiv.getElementsByTagName("div");  //属性
		for(var i=0;i<this.myBtn.length;i++){
			this.myBtn[i].index = i;          //给myBtn[i]添加一个属性index
			this.myBtn[i].onclick = function(){
				_this.btnClick(this);   //this即：this.myBtn[i]点击的按钮
			};
		}
	}

	TabSwitch.prototype.btnClick = function(clickBtn){
		for(var i=0;i<this.myBtn.length;i++){
			this.myBtn[i].className="";
			this.myContent[i].className="";
		}
		clickBtn.className="active";       //this即：myBtn[i]
		this.myContent[clickBtn.index].className="active";
	}
</script>

```

## 继承

**组合继承最常用**

**核心：**通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用 

```js
window.onload = function(){
	function Friend(){  //构造函数
		this.name = "Gal Gadot";
		this.age = "20";
	}
	Friend.prototype.showAppearence=function(){
		alert("beautiful");
	}
	//继承 如果我们想把被传入的函数对象里this的指针指向外部字面量定义的对象，那么我们就是用apply和call
	function GirlFriend(){
		Friend.call(this);
	}
	GirlFriend.prototype = new Friend();////此时 GirlFriend.prototype 中的 constructor 被重写了，会导致 GirlFriend.prototype.constructor === Friend
	GirlFriend.prototype.constructor = GirlFriend; //需要修复构造函数指向的。

	var friend = new Friend();
	var girlFriend = new GirlFriend();
	friend.showAppearence();
	girlFriend.showAppearence();
	alert(girlFriend.name);
}

```

## 原型链

http://www.cnblogs.com/shuiyi/p/5305435.html

所有引用类型（函数，数组，对象）都拥有\_\_proto\_\_属性（隐式原型）

所有函数拥有prototype属性（显式原型）（仅限函数）

原型对象：拥有prototype属性的对象，在定义函数时就被创建

我们所说的“找原型链”其实不是从prototype属性中找，而是从\_\_proto\_\_属性中找。

**构造函数 **

我们先复习一下构造函数 

```js
function Person(name, age) {
      this.name = name;
      this.age = age;
      this.sayName = function() { alert(this.name) };
  }
  var person1 = new Person("BurC", 21);
  var person2 = new Person("VinX", 22);
  console.log(person1.constructor == Person); //true
  console.log(person2.constructor == Person); //true
```

上面的例子中 person1 和 person2 都是 Person 的实例。这两个实例都有一个 constructor （构造函数）属性，该属性（是一个指针）指向 Person。 即：

我们要记住两个概念（构造函数，实例）： **person1 和 person2 都是 构造函数 Person 的实例**

一个公式： **实例的构造函数属性（constructor）指向构造函数。**

**原型对象prototype**

在 JavaScript 中，每当定义一个对象（函数也是对象）时候，对象中都会包含一些预定义的属性。其中每个函数对象都有一个prototype 属性，这个属性指向函数的原型对象。

**每个对象都有 \_\_proto\_\_ 属性，但只有函数对象才有 prototype 属性**

**原型对象，顾名思义，它就是一个普通对象（废话 = =!）。从现在开始你要牢牢记住原型对象就是 Person.prototype**

![](img\A001.jpg)

**prototype和\_\_proto\_\_的区别**

![](img\A002.png)

```js
var a = {};
console.log(a.prototype);  //undefined
console.log(a.__proto__);  //Object {}

var b = function(){}
console.log(b.prototype);  //b {}
console.log(b.__proto__);  //function() {}
```

![](img\A003.png)

```js
/*1、字面量方式*/
var a = {};
console.log(a.__proto__);  //Object {}
console.log(a.__proto__ === a.constructor.prototype); //true

/*2、构造器方式*/
var A = function(){};  //或者function A(){};
var a = new A();
console.log(a.__proto__); //A {}
console.log(a.__proto__ === A.prototype); //true
console.log(a.__proto__ === a.constructor.prototype); //true
console.log(A === a.constructor); //true

/*3、Object.create()方式*/
var a1 = {a:1}
var a2 = Object.create(a1);
console.log(a2.__proto__); //Object {a: 1}
console.log(a2.__proto__ === a2.constructor.prototype); //false（此处即为图1中的例外情况）
```

![](img\A004.png)

```js
var A = function(){};
var a = new A();
console.log(a.__proto__); //A {}（即构造器function A 的原型对象）
console.log(a.__proto__.__proto__); //Object {}（即构造器function Object 的原型对象）
console.log(a.__proto__.__proto__.__proto__); //null
```

# 四、闭包

让你分分钟理解 JavaScript 闭包：https://www.cnblogs.com/onepixel/p/5062456.html

JS-原生/一个例子讲清楚什么是闭包，什么是内存销毁：https://blog.csdn.net/coder_vader/article/details/78839686

## 经典的闭包问题

当然，这里也不仅仅局限于点击事件，也可以换成setTimeout等也有相同的问题，可以用这两种方法来解决； 

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <!-- 在页面添加三个按钮 -->
    <input type="button" value="1">
    <input type="button" value="2">
    <input type="button" value="3">


    <script>
        // 获取页面所有的input
        var aBtn = document.getElementsByTagName('input');

        // 循环绑定点击事件,当然毫无疑问这里点击之后会弹出3，
        //因为我们知道js是单线程的，当基本逻辑语句执行完之后，才会执行点击事件
        //而当你触发点击事件的时候，for循环都已经跑完了，所以i已经变成了3；
        // for(var i = 0; i < aBtn.length; i++){
        //     aBtn[i].onclick = function(){ 
        //         console.log(i);
        //     }
        // }

        // 解决方法1:也是最简单的方法，就是将for循环的var变成let
        //这样当点击每个按钮的时候，就会依次弹出0,1,2；
        //let是ES6新增的一个变量声明方式，拥有块级作用域；
        for(let i = 0; i < aBtn.length; i++){
            aBtn[i].onclick = function(){
                console.log(i);
            }
        }

        //解决方法2：利用闭包(说是闭包，貌似也不完全是)，也就是函数作用域访问原则的特性
        //函数内部可以访问外部的变量，外部却访问不了里边的；
        // for(var i = 0; i < aBtn.length; i++){
        //     (function(i){
        //         aBtn[i].onclick = function(){
        //             console.log(i);
        //         }
        //     })(i);
        // }
    </script>
</body>
</html>
```



# 五、定时器的使用

## setInterval()  循环执行

第一次执行时 浏览器打开并不会立即执行，等待一个循环时间后才执行

```js
window.onload=function(){
//循环执行，每隔1秒钟执行一次 1000 
	var timer = setInterval(function(){
		console.log("李百成");
	},1000);
//去掉定时器的方法 
    var close = document.getElementById("lbc");
	close.onclick = function(){
		clearInterval(timer);
	}
};
```

## setTimeout()  延时执行

```js
//延时一秒执行
var timer = setTimeout(function(){
	console.log("李百成");
},1000);
//去掉定时器的方法
clearTimeout(timer);
```

# 六、DOM操作

## 选择器

- `document.getElementById()`-- 通过 id 找到 HTML 元素；**单个元素**
- `document.getElementsByTagName()`-- 通过标签名找到 HTML 元素；**NodeList集合**
- `document.getElementsByName()`-- 通过name找到 HTML 元素；**NodeList集合**
- `document.querySelector()`-- 返回文档中匹配指定的CSS选择器的第一元素 ；**单个元素**
-  `document.querySelectorAll()` -- 返回文档中匹配指定 CSS 选择器的所有元素；**NodeList集合**

```js
var mydiv = document.getElementById("myDiv");

//通过标签名来获得当前网页中的元素对象的，而且它返回的是一个数组，因为tag相同的元素可能不止一个这个时候就需要用getElementsByTagName("a")[0](返回第一个元素)来获得对象的引用
var myul = mydiv.getElementsByTagName("a");
var myul = mydiv.getElementsByTagName("a")[0];

//常使用getElementsByName()取得单选按钮
var myInput = document.getElementsByName("color");

var myP = document.querySelector("p.example");
var myP = document.querySelector("#example");

// 获取文档中所有 class="example" 的 <p> 元素
var x = document.querySelectorAll("p.example"); 
// 设置 class="example" 的第一个 <p> 元素的背景颜色
x[0].style.backgroundColor = "red";
//设置文档中所有 class="example" 元素的背景颜色:
var x = document.querySelectorAll(".example");
var i;
for (i = 0; i < x.length; i++) {
    x[i].style.backgroundColor = "red";
}
```



## 属性操作

**attribute特性**

- `element.getAttribute()` -- 通过名称获取属性的值。
- `element.setAttribute()` -- 创建或改变某个新属性。如果指定属性已经存在,则只设置该值。
- `element.removeAttribute()` -- 从元素中删除指定的属性 。

**property属性**

- `element.className` -- 设置或返回元素的class属性 
- `element.id`-- 设置或者返回元素的 id。 

```js
document.getElementsByTagName("a")[0].getAttribute("target");
document.getElementsByTagName("INPUT")[0].setAttribute("type","button");
document.getElementsByTagName("H1")[0].removeAttribute("style");

//element.className
//获取属性值：HTMLElementObject.className
//设置属性值：HTMLElementObject.className = "classname"

//element.id
//获取属性值: HTMLElementObject.id
//设置属性值：HTMLElementObject.id = "id"
```



## 操作文本/值

- `element.innerHTML` -- 设置或者返回元素的内容。 （包括 HTML 标签） 类似jQuery的HTML()

```js
//设置元素内容
document.getElementById('myAnchor').innerHTML="RUNOOB";
document.getElementById('myAnchor').innerHTML="<h1>RUNOOB</h1>";
//返回元素内容
var a = document.getElementById('myAnchor').innerHTML;
```



## CSS相关

- `document.getElementById(id).style.property` -- 获取、修改CSS样式

```js
//获取 ： document.getElementById(id).style.property
//修改 ： document.getElementById(id).style.property=新样式
document.getElementById("p2").style.color
document.getElementById("p2").style.color="blue";
```



## 元素遍历

### 1.节点类型

| 节点类型            | 属性`nodeType`返回值 |
| ------------------- | -------------------- |
| 元素节点            | 1                    |
| 属性节点            | 2                    |
| 文本节点            | 3                    |
| 注释节点（comment） | 8                    |
| document            | 9                    |
| DocumentFragment    | 11                   |

### 2.新增节点

- `document.createElement()` -- 创建元素节点。 

```js
var div = document.getElementById('div'); 
//创建链接 
function createA(url,text) 
{ 
    var a = document.createElement("a"); 
    a.href = url; 
    a.innerHTML = text; 
    a.style.color = "red"; 
    div.appendChild(a); 
} 
createA("http://www.ffasp.com/","网页教学网"); 
```

### 3.插入节点

- `appendChild()` -- 在调用元素的最后一个子节点（节点包括文本）后添加节点，该方法返回新的节点。
- `insertBefore()` -- 在调用元素的指定已有的子节点之前插入新节点。
- `removeChild()` -- 删除某个节点中指定的子节点，一定要有参数 

### 4..遍历节点树

在DOM中实际上有一个叫做textNode的元素，相应的还有document.createTextNode的JS方法，而在IE和Chrome浏览器中会将源代码中的换行符渲染成一个textNode，只是视觉上不可见。 然而，通过childNodes来获取子元素的时候，结果会包含这些textNode，所以会得到题主所见的情况。 而解决方法很简单，主要有两种： 第一，使用children代替childNodes 第二，遍历childNodes，根据nodeType过滤掉textNode 

https://blog.csdn.net/qq_41713692/article/details/80648906

- parentNode ——>父节点（最顶端的parentNode为#document）（所有浏览器都支持）
- `childNodes` ——> 子节点们（所有浏览器都支持）
- firstChild ——>第一个子节点（所有浏览器都支持）
- lastChild ——> 最后一个子节点（所有浏览器都支持）
- nextSibling ————>后一个兄弟节点（所有浏览器都支持）
- previousSibling ——>前一个兄弟节点 （所有浏览器都支持）
- parentElement ——> 返回当前元素的父元素节点（IE6、7、8不支持 ）
- children ——> 只返回当前元素的元素子节点（所有浏览器都支持）
- firstElementChild ——> 返回的是第一个元素节点（IE6、7、8不支持）
- lastELementChild ————>返回的是最后一个元素节点（IE6、7、8不支持）
- nextElementSibling / previousElementSibling ——> 返回后一个/前一个兄弟元素节点（IE6、7、8不支持）
- Node.hasChildNodes();   是否有孩子节点。有：true；没有：fals
- node.childElementCount === node.children.length 当前元素节点的子元素节点个数（IE6、7、8不支持）

```html
<!DOCTYPE html>
<html>
<head>
	<title></title>
</head>
<Body>
    <div >
        <strong></strong>
        <span></span>
        <em></em>
    </div>
</Body>
</html>
```

js代码

```js
//parentNode ——>父节点（最顶端的parentNode为#document）
var strong = document.getElementsByTagName('strong')[0];  
console.log(strong.parentNode);
//strong.parentNodes ————div
//strong.parentNode.parentNode ————body
//strong.parentNode.parentNode.parentNode ————html
//strong.parentNode.parentNode.parentNode .parentNode————document

//childNodes ——> 子节点们
var div = document.getElementsByTagName('div')[0];
console.log(div.childNodes);//NodeList(7){text,strong,text,span,text,em,text}

//firstChild ——>第一个子节点
var div = document.getElementsByTagName('div')[0];
console.log(div.firstChild); //#text

//lastChild ——> 最后一个子节点
var div = document.getElementsByTagName('div')[0];
console.log(div.lastChild); //#text

//nextSibling ————>后一个兄弟节点
var strong  = document.getElementsByTagName('strong')[0];
console.log(strong.nextSibling);//#text

//previousSibling ——>前一个兄弟节点 
var strong  = document.getElementsByTagName('strong')[0];
console.log(strong.previousSibling);//#text

//parentElement ——> 返回当前元素的父元素节点（IE不兼容）
var strong = document.getElementsByTagName('strong')[0];  
console.log(strong.parentElement); //<div>...</div>

//children ——> 只返回当前元素的元素子节点
var div = document.getElementsByTagName('div')[0];
console.log(div.children);//HTMLCollection(3) [strong, span, em]

//firstElementChild ——> 返回的是第一个节点（IE不兼容）
var div = document.getElementsByTagName('div')[0];
console.log(div.firstElementChild); //<span></span>

//lastELementChild ————>返回的是最后一个元素节点（IE不兼容）
var div = document.getElementsByTagName('div')[0];
console.log(div.lastElementChild);//<em></em>

//nextElementSibling / previousElementSibling ——> 返回后一个/前一个兄弟元素节点（IE不兼容）
var strong = document.getElementsByTagName('strong')[0];  
console.log(strong.nextElementSibling); //<span></span>
console.log(strong.previousElementSibling); //null

//var div = document.getElementsByTagName('div')[0];
console.log(div.hasChildNodes()); //true

//node.childElementCount === node.children.length 当前元素节点的子元素节点个数（IE不兼容）
var div = document.getElementsByTagName('div')[0];
console.log(div.children.length); //3
```



## 元素大小

### 1.offset 

**P321**

1. 如果当前元素的父级元素没有进行css定位（position为static或null），offsetParent为body，即距离浏览器的距离
2. 如果当前元素的父级元素中有css定位(position为absolute或relative或fixed)，offsetParent取最近的那个父级元素
3. style.left 返回的是字符串，如28px，offsetLeft返回的是数值28，如果需要对取得的值进行计算，还用offsetLeft比较方便。

```js
 //offsetWidth:获取元素的宽    width + border*2 + padding*2
 document.getElementById("p2").offsetWidth;
 //offsetHeight:获取元素的高   height + border*2 + padding*2
 document.getElementById("p2").offsetHeight;
 //offsetLeft:获取自己左外边框与offsetParent的左内边框的距离  
 document.getElementById("p2").offsetLeft;
// offsetTop:获取自己上外边框与offsetParent的上内边框的距离  
 document.getElementById("p2").offsetTop;
```

![](img\A005.png)

### 2.client 

**P322**

1. 假如元素无padding无滚动
   `clientWidth = style.width`
2. 假如元素有padding无滚动
   `clientWidth = style.width + style.padding*2`
3. 假如元素有padding有滚动，且滚动是显示的
   `clientWidth = style.width + style.padding*2 - 滚动轴宽度`
   **clientHeight同理**

```js
//element.clientWidth  在页面上返回内容的可视宽度（不包括边框或滚动条）
//element.clientHeight 在页面上返回内容的可视高度（不包括边框或滚动条）
//element.clientLeft 元素本身左边框的宽度
//element.clientTop 元素本身上边框的宽度
```

![](img\A006.png)

### 3.scroll 

**P324**

1. **无滚动轴时：**
   `scrollWidth = clientWhidth = style.width + style.padding*2`
2. **有滚动轴时：**
   `scrollWidth = 实际内容的宽度 + padding*2`
   **scrollHeight同理**

IE、Opera 认为 scrollHeight 是网页内容实际高度，可以小于 clientHeight。FF 认为 scrollHeight 是网页内容高度，不过最小值是 clientHeight。 

```js
//element.scrollWidth   返回整个元素的高度（包括带滚动条的隐蔽的地方）
//element.scrollHeight  返回元素的整个宽度（包括带滚动条的隐蔽的地方）
//element.scrollLeft  	返回当前视图中的实际元素的左边缘和左边缘之间的距离
//element.scrollTop     返回当前视图中的实际元素的顶部边缘和顶部边缘之间的距离
```

​	**3.返回顶部**

```js
window.scrollTo(x-coord, y-coord);  
window.scrollTo(0,0);  
```

# 七、事件

## 事件类型

### 1.UI事件

- onload --会在页面或图像加载完成后立即发生，在页面完全载入后(包括图片、css文件等等。)执行脚本代码

- onresize--当浏览器窗口大小改变的时候将会触发onresize事件。 

- onscroll-- 事件在元素滚动条在滚动时触发。 

```js
//方法一：
document.getElementById("myImg").addEventListener("load", myFunction,false);
function myFunction() {
    document.getElementById("demo").innerHTML = "图片已加载完成";
}
//方法二：推荐
window.onload=function(){SomeJavaScriptCode};
---------------------------------------------------------------------------------------
//方法一：

//方法二：推荐
window.onresize(function(){
    //code
});
---------------------------------------------------------------------------------------
//方法一：
document.getElementById("myDIV").addEventListener("scroll", myFunction,false);
function myFunction() {
    document.getElementById("demo").innerHTML = "你滚动了 div。";
}
//方法二：
document.getElementById("myDIV").onscroll = function() {myFunction()};
function myFunction() {
    document.getElementById("demo").innerHTML = "你滚动了 div。";
}
```

### 2.表单事件

- onblur--事件会在对象失去焦点时发生 。

- onfocus-- 事件在对象获得焦点时发生。 
- onchange-- 事件会在域的内容改变时发生 。

```js
object.onblur=function(){SomeJavaScriptCode};
object.onfocus=function(){SomeJavaScriptCode};
object.onchange=function(){SomeJavaScriptCode};
```

### 3.鼠标事件

- onclick 事件会在元素被点击时发生。 
- ondblclick 事件会在对象被双击时发生。 
- onmouseover 事件会在鼠标指针移动到指定的元素上时发生。 **其子孙后代都可以响应改事件 **
- onmouseout 事件会在鼠标指针移出指定的对象时发生。 **其子孙后代都可以响应改事件 **
- onmouseenter 事件在鼠标指针移动到元素上时触发。 只作用于目标元素 
- onmouseleave 事件在鼠标移除元素时触发。 只作用于目标元素 
- onmousedown 事件会在鼠标按键被按下时发生。 
- onmouseup 事件会在鼠标按键被松开时发生。 
- onmousemove 事件在鼠标移动时发生。

```js
object.onclick=function(){SomeJavaScriptCode};
object.ondblclick=function(){SomeJavaScriptCode};
object.onmouseenter=function(){myScript};
object.onmouseleave=function(){myScript};
object.onmousedown=function(){SomeJavaScriptCode};
object.onmouseup=function(){SomeJavaScriptCode};
```

### 4.键盘事件

- onkeydown 事件会在用户按下一个键盘按键时发生。 若按住不放，会重复触发此事件。
- onkeypress 事件会在键盘按键被按下并释放一个键时发生。若按住不放，会重复触发此事件。 
- onkeyup 事件会在键盘按键被松开时发生。
- event.keyCode 数字：48-57 

```js
object.onkeydown=function(){SomeJavaScriptCode};
object.onkeypress =function(){SomeJavaScriptCode};
object.onkeyup=function(){SomeJavaScriptCode};

//匹配左右键
document.addEventListener("keydown",keydown);
//键盘监听，注意：在非ie浏览器和非ie内核的浏览器
//参数1：表示事件，keydown:键盘向下按；参数2：表示要触发的事件
function keydown(event){
    //表示键盘监听所触发的事件，同时传递参数event
    switch(event.keyCode){
        case 37:
            alert("左键");
            break;
        case 39:
            alert("右键");
            break;
    }
}

//每个键盘按键都有编码，按键测试。
document.addEventListener("keydown",keydown);
//键盘监听，注意：在非ie浏览器和非ie内核的浏览器
//参数1：表示事件，keydown:键盘向下按；参数2：表示要触发的事件
function keydown(event){
//表示键盘监听所触发的事件，同时传递传递参数event
    document.write(event.keyCode);//keyCode表示键盘编码
}
```

### 5.触摸事件

![](img\A007.png)

**客户区坐标位置 P370**

clientX、clientY  ：点击位置距离当前body可视区域的x，y坐标 

**页面坐标位置 P370**

pageX、pageY  ：对于整个页面来说，包括了被卷去的body部分的长度 

**屏幕坐标位置 P371**

screenX、screenY  ：点击位置距离当前电脑屏幕的x，y坐标 

**触摸事件类型**： 

touchstart : 触摸开始（手指放在触摸屏上）event.touches[0].clientX或 event.targetTouches[0].clientX

touchmove : 拖动（手指在触摸屏上移动）event.touches[0].clientX或event.targetTouches[0].clientX

touchend : 触摸结束（手指从触摸屏上移开）event.changedTouches[0].clientX

**每个触摸事件都包括了三个触摸属性列表：**

1.touches：当前位于屏幕上的所有手指触摸点的一个列表。

2.targetTouches：当前元素对象上所有触摸点的列表。

3.changedTouches：涉及当前事件的触摸点的列表。

它们都是一个数组，每个元素代表一个触摸点。

每个触摸点对应的Touch都有三对重要的属性，clientX/clientY、pageX/pageY、screenX/screenY。

其中screenX/screenY代表事件发生的位置对于屏幕的偏移量，clientX/clienYt和pageX/pageY都代表事件发生位置对应对象的偏移量，不过区别是clientX/clientY不包括对象滚动而隐藏的偏移量，而pageX/pageY包括对象滚动而隐藏的偏移量。**移开屏幕的那个触摸点，只会包含在changedTouches列表中，而不会包含在touches 和targetTouches 列表中， 所以changedTouches在项目当中会比较常用。**

```js
//兼容移动触摸的事件写法
//如果在移动手机端，hastouch为 true
//如果在电脑端，hastouch 为false
var hastouch = "ontouchstart" in window ? true : false,
    tapstart = hastouch ? "touchstart" : "mousedown",
    tapmove = hastouch ? "touchmove" : "mousemove",
    tapend = hastouch ? "touchend" : "mouseup";

//添加事件监听器  tapstart，移动端添加touchstart；电脑端添加mousedown
this.mycanvas.addEventListener(tapstart, function (e) {
    //判断坐标兼容移动端、电脑端
     var x = hastouch ? e.targetTouches[0].clientX - that.mycanvas.offsetLeft : e.clientX  - that.mycanvas.offsetLeft;
     var y = hastouch ? e.targetTouches[0].clientY - that.mycanvas.offsetTop : e.clientY - that.mycanvas.offsetTop;
     // console.log('touchstart:X='+x+'Y='+y);
     that.touchType(x,y,'touchstart');
 }, { passive: true });
this.mycanvas.addEventListener(tapmove, function (e) {
    var x = hastouch ? e.targetTouches[0].clientX - that.mycanvas.offsetLeft : e.clientX  - that.mycanvas.offsetLeft;
    var y = hastouch ? e.targetTouches[0].clientY - that.mycanvas.offsetTop : e.clientY - that.mycanvas.offsetTop;
    // console.log('touchmove:X='+x+'Y='+y);
    that.touchType(x,y,'touchmove');
}, { passive: true });
this.mycanvas.addEventListener(tapend, function (e) {
    that.touchType(0,0,'touchend');
}, { passive: true });
```

## 事件委托

/*这里用到事件源：event对象， 事件源，不管在哪个事件中，只要你操作的那个元素就是事件源*

*标准下： event.target*      ie： window.event.srcElement

*nodeName: 找到元素的标签名；*

target 事件属性可返回事件的目标节点（触发该事件的节点） / 

```js
oUl.onmouseout = function(ev) {
    var ev = ev || window.event;
    var target = ev.target|| ev.srcElement;
   
    if(target.nodeName.toLowerCase() == 'li'){
       target.style.background = ' ';
    }
}
```

## 阻止默认行为

return false；

# 八、BOM

```js
window.location("http://www.baidu.com")；  //js页面跳转
window.scroll=function(){}    //页面滚动触发事件
window.onresize=function(){}   //窗口大小发生改变触发事件

document.body.scrollTop   //滚动距离  谷歌
document.documentElement.scrollTop   //滚动距离 IE 火狐
var scrollTop = document.body.scrollTop||document.documentElement.scrollTop ；//处理兼容
```

# 九、Cookie

**cookie测试环境**

部署在wamp上运行，地址栏是file://开头无法测试，换http://开头就可以了。 

**cookie生命周期：**

默认情况下是一次会话（浏览器被关闭）

如果通过expires设置了过期时间，并且过期时间没有过期，那么下次打开浏览器数据还是存在

如果通过expires设置了过期时间，并且过期时间已经过期，那么会立刻删除保存的数据

**cookie注意点：**

cookie不能一次性设置多条数据，只能一条一条的设置

个数限制：20 -50

大小限制：4KB

**cookie作用范围：**

如果在同一个浏览器中，默认同级、下一级路径下有效，上级目录无效

如果想让上级目录有效，添加path属性

```
document.cookie="name=lbc;path=/;";    //最后一个；加不加无所谓
```

```
 	<script type="text/javascript">
	window.onload=function(){
		function creatCookie(key,value,day,path){
	        var myDate = new Date();
	        myDate.setDate(myDate.getDate()+day);  //设置day时间后的天数 day int类型
	        document.cookie = key+"="+value+";expires="+myDate.toGMTString()+";path="+path+";";  //expires设置过期时间
	    }
	    function getCookie(key){
	        var result = document.cookie.split(';');
	        for(var i=0;i<result.length;i++){
	            var temp = result[i].split('=');
	            if(temp[0].trim()==key){
	                return temp[1];
	            }
	        }
	        return '';
	    }
	    function delCookie(key,path){
	        creatCookie(key,'',-1,path);
	    }

	    creatCookie('userkey','bc',8,'/');  //创建Cookie day int类型
	    alert(getCookie('userkey'));   //获取Cookie
	    delCookie('userkey','/');   //删除Cookie
	};
	</script>
```

# 十、Ajax

Ajax W3C官方文档：http://www.w3school.com.cn/ajax/ajax_xmlhttprequest_create.asp

## **javascript 的 Ajax**基本使用

**onreadystatechange 事件**

当请求被发送到服务器时，我们需要执行一些基于响应的任务。

每当 readyState 改变时，就会触发 onreadystatechange 事件。

readyState 属性存有 XMLHttpRequest 的状态信息。

下面是 XMLHttpRequest 对象的三个重要的属性：

| 属性               | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| onreadystatechange | 存储函数（或函数名），每当 readyState 属性改变时，就会调用该函数。 |
| readyState         | 存有 XMLHttpRequest 的状态。从 0 到 4 发生变化。                                                  0: 请求未初始化                                                                                                               1: 服务器连接已建立                                                                                                       2: 请求已接收                                                                                                                   3: 请求处理中                                                                                                                  4: 请求已完成，且响应已就绪 |
| status             | 200: "OK"                                                                                                                        404: 未找到页面 |

如需获得来自服务器的响应，请使用 XMLHttpRequest 对象的 responseText 或 responseXML 属性。

| 属性         | 描述                       |
| ------------ | -------------------------- |
| responseText | 获得字符串形式的响应数据。 |
| responseXML  | 获得 XML 形式的响应数据。  |

```
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>Ajax基本实例</title>
</head>
<body>
<button id="mybtn">Ajax</button>

<script type="text/javascript" src="https://cdn.bootcss.com/jquery/1.12.4/jquery.js"></script>
<script type="text/javascript">
	window.onload = function(){
		var mybtn = document.getElementById('mybtn');
		mybtn.onclick = function(){
			//1.创建AJax对象
			if(window.XMLHttpRequest){
				var xmlhttp = new XMLHttpRequest();
			}else{
				var xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
			}

			//2.建立连接
			xmlhttp.open("GET", "text.txt?time="+(new Date().getTime()),true);
			//3.发送请求
			xmlhttp.send();
			//4.接受数据
			xmlhttp.onreadystatechange=function(){
				if (xmlhttp.readyState==4){
					if(xmlhttp.status==200){
					  alert(xmlhttp.responseText);
					}
				}
			}
		}
	}
</script>
</body>
</html>

```

## Ajax-GET封装

```
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>Ajax基本实例</title>
</head>
<body>
<button id="mybtn">Ajax</button>

<script type="text/javascript" src="https://cdn.bootcss.com/jquery/1.12.4/jquery.js"></script>
<script type="text/javascript">
	window.onload = function(){
		var mybtn = document.getElementById('mybtn');
		mybtn.onclick = function(){
			//地址
			//请求成功回调函数
			//请求失败回调函数
			Ajax("test.php",function(xmlhttp){
				alert(xmlhttp.responseText);
			},function(xmlhttp){
				alert(xmlhttp.readyState);
			});
		}
	}
	//封装Ajax-GET函数
	function Ajax(url,success,error){
		//1.创建AJax对象
			if(window.XMLHttpRequest){
				var xmlhttp = new XMLHttpRequest();
			}else{
				var xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
			}

			//2.建立连接
			//请求方式POST GET
			//请求地址
				/**
				**IE缓存问题
				**在IE浏览器中，如果通过Ajax发送GET请求，那么IE浏览器认为同一个URL地址只有一个结果
				**通过添加new Data().getTime()解决缓存问题
				**/
			//异步请求true  同步请求false
			xmlhttp.open("GET", url+"?time="+(new Date().getTime()),true);
			//3.发送请求
			xmlhttp.send();
			//4.接受数据
			xmlhttp.onreadystatechange=function(){
				if (xmlhttp.readyState==4){
					if(xmlhttp.status==200){
						//调用请求成功的函数
						success(xmlhttp);
					}else{
						//调用请求失败的函数
					    error(xmlhttp);
				    }
				}
			}
	}
</script>
</body>
</html>

```

## Ajax-GET高级封装

添加了请求超时时间

将URL地址中的参数模块化

URL中不可以出现中文，如果出现，需要转码encodeURIComponent()

```
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>Ajax基本实例</title>
</head>
<body>
<button id="mybtn">Ajax</button>

<script type="text/javascript" src="https://cdn.bootcss.com/jquery/1.12.4/jquery.js"></script>
<script type="text/javascript">
	window.onload = function(){
		var mybtn = document.getElementById('mybtn');
		mybtn.onclick = function(){
			//地址
			//超时时间
			//URL地址传递的参数
			//请求成功回调函数
			//请求失败回调函数
			Ajax("test.php",3000,{
				"name":"李百成",
				"age":"18"
			},function(xmlhttp){
				alert(xmlhttp.responseText);
			},function(xmlhttp){
				alert(xmlhttp.readyState);
			});
		}
	}

	//封装Ajax-GET函数
	function Ajax(url,timeout,obj,success,error){
		//0.将对象转换成字符串
			var str = objStr(obj);
		//1.创建AJax对象
			if(window.XMLHttpRequest){
				var xmlhttp = new XMLHttpRequest();
			}else{
				var xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
			}

			//2.建立连接
			//请求方式POST GET
			//请求地址
				/**
				**IE缓存问题
				**在IE浏览器中，如果通过Ajax发送GET请求，那么IE浏览器认为同一个URL地址只有一个结果
				**通过添加new Data().getTime()解决缓存问题
				**/
			//异步请求true  同步请求false
			xmlhttp.open("GET", url+"?"+str,true);
			//3.发送请求
			xmlhttp.send();
			//4.接受数据
			xmlhttp.onreadystatechange=function(){
				//清除定时器
				clearInterval(timer);
				if (xmlhttp.readyState==4){
					if(xmlhttp.status==200){
						success(xmlhttp);
					}else{
					    error(xmlhttp);
				    }
				}
			}

			//判断外界是否传入了超时时间
			if(timeout){
				var timer = setInterval( function (){
					console.log("超时中断");
					//中断请求
					xmlhttp.abort();
					//清除定时器
					clearInterval(timer);
			   },timeout);
			}
	}


	//URL地址传参模块化
	function objStr(obj){
		var res = [];
		//给obj对象添加一个时间元素 ，目的：是每次访问URL地址都不同，解决缓存问题
		obj.time = new Date().getTime();
		for(var key in obj){
			res.push(encodeURIComponent(key)+"="+encodeURIComponent(obj[key]));
		}
		//将数组转换成字符串 中间用&连接
		return res.join("&");
	}
</script>
</body>
</html>

```

## Ajax高级封装

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>Ajax基本实例</title>
</head>
<body>
<button id="mybtn">Ajax</button>
<script type="text/javascript" src="ajax.js"></script>
<script type="text/javascript">
	window.onload = function(){
		var mybtn = document.getElementById('mybtn');
		mybtn.onclick = function(){
			//类型
			//地址
			//超时时间
			//URL地址传递的参数
			//请求成功回调函数
			//请求失败回调函数

			Ajax({
				type: "post",
				url: "test.php",
				timeout: 3000,
				data:{
				  "name":"李百成",
				  "age":"18"
			    },
			    success: function(xmlhttp){
					alert(xmlhttp.responseText);
				},
				error: function(xmlhttp){
				  alert(xmlhttp.readyState);
			    }
			});
		}
	}
</script>
</body>
</html>
```

ajax.js封装函数

```js
	//封装Ajax函数
	function Ajax(option){
		//0.将对象转换成字符串
			if(option.data)
				var str = dataStr(option.data);
			else
				var str = null;
		//1.创建AJax对象
			if(window.XMLHttpRequest){
				var xmlhttp = new XMLHttpRequest();
			}else{
				var xmlhttp = new ActiveXdataect("Microsoft.XMLHTTP");
			}

			//2.建立连接
			//请求方式POST GET
			//请求地址
				/**
				**IE缓存问题
				**在IE浏览器中，如果通过Ajax发送GET请求，那么IE浏览器认为同一个URL地址只有一个结果
				**通过添加new Data().getTime()解决缓存问题
				**/

			if(option.type.toLowerCase()=="get"){
				xmlhttp.open("GET", option.url+"?"+str,true);
				//3.发送请求
				xmlhttp.send();
			}else{
				xmlhttp.open("POST", option.url,true);
				// 2.5如果需要像 HTML 表单那样 POST 数据，请使用 setRequestHeader() 来添加 HTTP 头。然后在 send() 方法中规定您希望发送的数据：
				xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
				//3.发送请求
				xmlhttp.send(str);
			}
			//4.接受数据
			xmlhttp.onreadystatechange=function(){
				//清除定时器
				clearInterval(timer);
				if (xmlhttp.readyState==4){
					if(xmlhttp.status>=200 && xmlhttp.status<=200||xmlhttp.status==304){
						option.success(xmlhttp);
					}else{
					    option.error(xmlhttp);
				    }
				}
			}

			//判断外界是否传入了超时时间
			if(option.timeout){
				var timer = setInterval( function (){
					console.log("超时中断");
					//中断请求
					xmlhttp.abort();
					//清除定时器
					clearInterval(timer);
			   },option.timeout);
			}
	}


	//URL地址传参模块化
	function dataStr(data){
		var res = [];
		//给data对象添加一个时间元素 ，目的：是每次访问URL地址都不同，解决缓存问题
		data.time = new Date().getTime();
		for(var key in data){
			res.push(encodeURIComponent(key)+"="+encodeURIComponent(data[key]));
		}
		//将数组转换成字符串 中间用&连接
		return res.join("&");
	}

```

## json字符串转化

```js
//JSON对象装成JSON字符串经常用于前后台传输数据！
//从ajax的服务器发过的，一定是字符串，你想要把它解析，很简单，把它先变成JSON对象才行。
//JSON对象可以通过jsonObj.name来调用，如果是字符串，那么就是字符串了
//JSON字符串:
var JSONStr = '{ "name": "cxh", "sex": "man" }'; 
//JSON对象:"
var JSONObj = { "name": "cxh", "sex": "man" };
-------------------------------------------------------------------------------
//eval("("+str+")")  非标准的JSON字符串、标准的JSON字符串转化为JSON对象
var str = '{name:"lbc",age:20,sex:"man"}' //非标准的JSON字符串
var str = '{"name":"lbc","age":20,"sex":"man"}'//标准的JSON字符串
var obj  = eval("("+str+")"); //非标准的JSON字符串、标准的JSON字符串转化为JSON对象
console.log(obj); //{ "name":"lbc","age":20,"sex":"man" }

//JSON.parse(str) 标准的JSON字符串转化为JSON对象
const str= '{"b":12,"a":12}';
console.log(JSON.parse(str));//{b: 12, a: 12}
-------------------------------------------------------------------------------
//JSON.stringify(json);将JSON对象转化为JSON字符串
var str = {"name":"lbc","age":20,"sex":"man"}
console.log(JSON.stringify(str));//'{"name":"lbc","age":20,"sex":"man"}'
```



# 小技术

## 无缝滚动

```
<!DOCTYPE html>
<html>
<head>
	<title>无缝滚动</title>
	<style type="text/css">
		*{
			padding: 0;
			margin:0;
		}
		.go-out{
			width: 1000px;
			height: 120px;
			margin:200px auto;
			position: relative;
			background: #f00;
			overflow: hidden;
		}
		.direction{
			width: 1000px;
			margin:-100px auto;
			text-align: center;
		}
		.myul{
			position: absolute;
			left: 0;
			right: 0;
		}
		.myul li{
			float: left;
			list-style: none;
		}
		.myul li img{
			width: 200px;
			height: 120px;
		}
	</style>
</head>
<body>
<div class="go-out" id="mydiv">
	<ul class="myul">
		<li><img src="images/1.jpg"></li>
		<li><img src="images/2.jpg"></li>
		<li><img src="images/3.jpg"></li>
		<li><img src="images/4.jpg"></li>
		<li><img src="images/5.jpg"></li>
	</ul>
</div>
<div class="direction">
    <a href="#">向左</a>
	<a href="#">向右</a>
</div>
<script type="text/javascript">
// offsetLeft  offsetWidth  offsetTop....
// offsetLeft 获取的是相对于父对象的左边距
//  style.left 返回的是字符串，如28px，offsetLeft返回的是数值28，如果需要对取得的值进行计算，
// 还用offsetLeft比较方便。
	window.onload = function(){
		var mydiv = document.getElementById("mydiv");
		var myul = mydiv.getElementsByTagName("ul")[0];
		var myli = myul.getElementsByTagName("li");
		var speed = 2;
		//ul里面内容复制一份
		myul.innerHTML += myul.innerHTML;
		//ul 增大宽度，以容下所有li
		myul.style.width = myli[0].offsetWidth *  10 +"px";

		//移动函数
		function move(){
			//如果ul向左移动超过一半，立马将其向右拖拽ul总宽度的一半即：左边距为0
			if(-myul.offsetLeft > myul.offsetWidth / 2)
				myul.style.left = 0;
			//如果ul向左移动超过0，立马将其向左拖拽ul总宽度的一半
			if(myul.offsetLeft>0)
				myul.style.left = -myul.offsetWidth /2+"px";

			myul.style.left = myul.offsetLeft+speed+'px';
		}

		var timer = setInterval(move,30);

		//鼠标移入div去掉定时器
		mydiv.onmouseover = function(){
			clearInterval(timer);
		};
		//鼠标移出div开启定时器
		mydiv.onmouseout = function(){
			timer = setInterval(move,30);
		};

		//修改滚动方向
		document.getElementsByTagName("a")[0].onclick = function(){
			speed = -2;
		};
		document.getElementsByTagName("a")[1].onclick = function(){
			speed = 2;
		};
	};
</script>
</body>
</html>
```

## 选项卡

```
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>选项卡</title>
	<style type="text/css">
		#myDiv input.active {
			background-color: #696969;
		}
		#myDiv div{
			width: 200px;
			height: 200px;
			background: #696969;
			display: none;
		}
		#myDiv div.active{
			display: block;
		}
	</style>
</head>
<body>
<div id="myDiv">
	<input class="active" type="button" name="" value="one">
	<input type="button" name="" value="two">
	<input type="button" name="" value="there">
	<div class="active">One巴拉巴拉</div>
	<div>Two巴拉巴拉</div>
	<div>There巴拉巴拉</div>
</div>
<script type="text/javascript">
window.onload = function(){
	var myDiv = document.getElementById("myDiv");
	var myBtn = myDiv.getElementsByTagName("input");
	var myContent = myDiv.getElementsByTagName("div");
	for(var i=0;i<myBtn.length;i++){
		myBtn[i].index = i;          //给myBtn[i]添加一个属性index  
		myBtn[i].onclick = function(){
			for(var k=0;k<myBtn.length;k++){
				myBtn[k].className="";
				myContent[k].className="";
			}
			this.className="active";       //this即：myBtn[i]
			myContent[this.index].className="active";
		}
	}
}
</script>
</body>
</html>
```
