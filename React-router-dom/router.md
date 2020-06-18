# React-router-dom

# 特殊应用

## 主动跳转路由(带参数)

又是需要用到不需要点击`Linke`标签直接出发携带参数跳转路由

**Link跳转路由**

```jsx
<Link
  to={{
    pathname: "/courses",
    search: "?sort=name",
    hash: "#the-hash",
    state: { fromDashboard: true }
  }}
/>
```

**使用`js`主动跳转路由**

`history.push(pathName,state)`,其中pathNmae就是路由的路径;state就是想要传过去的参数.

其中参数可以在路由命中的组件中的`this.props.location.state`可以看到

```js
this.props.history.push('/search',{
    userDefinedKey: userDefinedKey,
    timer: this.state.timer
})
```

> 需要注意的是,只有当前组件中有React-router-dom产生的history属性时(目前根据自己测试,可以将该属性传给没有该属性的子组件并使用),调用该属性的push(pathName,state)方法