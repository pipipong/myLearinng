# Marker
## 事件
### 事件概括样例
格式
`所需参数[参数描述(附加描述)]`
例子
```js
let marker = L.marker(point[坐标]).addTo(mymap[地图实例])

marker.on('click',(ev[事件回调信息(包含坐标)]) => {
	...
})
```



### 常用事件

**继承的Mouse鼠标事件**

| 事件名      | 事件描述                                                     |
| ----------- | ------------------------------------------------------------ |
| click       | 不用多说最常用的事件,单击marker触发                          |
| dbclick     | 当用户双击marker触发                                         |
| mousedown   | 在marker上按下鼠标触发                                       |
| mouseover   | 当鼠标移入marker时触发                                       |
| mouseout    | 当鼠标移出marker时触发                                       |
| contextmenu | 当用户右键单击图层时触发，如果此事件上有侦听器，则会阻止默认的浏览器上下文菜单显示。当用户持有一次触摸（也称为长按）时，也会在手机上触发。 |

**继承来自Layer的事件**

| 事件   | 事件描述                 |
| ------ | ------------------------ |
| add    | 在marker添加到地图后触发 |
| remove | 在marker移除出地图后触发 |



### move事件

move事件比较特殊需要经过特殊处理后才能使用

```js
let marker = L.marker(point[坐标]).addTo(mymap[地图实例])
// marker 为上述生成的实例
marker.dragging.enable() //之后可以进行拖动操作
marker.dragging.disable() //之后不可以进行拖动操作
```

关于dragging --- Handler的描述

| 方法名    | 返回值  | 说明                          |
| --------- | ------- | ----------------------------- |
| enable()  | this    | 启用处理程序(如拖动这种动作)  |
| disable() | this    | 禁用处理程序                  |
| enabled() | Boolean | 如果启用了处理程序,则返回true |

**拖动事件描述**

| 事件      | 事件说明                           |
| --------- | ---------------------------------- |
| dragstrat | 当用户开始拖动标记时触发。         |
| movestart | 标记开始移动时发生（因为拖动）。   |
| drag      | 当用户拖动注记时不断触发.          |
| dragend   | 当用户停止拖动注记时触发           |
| moveend   | 当标记停止移动（由于拖动）时发出。 |











