# React-Router

## web 版本

### 组件

- `<BrowserRouter>`
- `<HashRouter>`
- `<Link>`
- `<NavLink>`
- `<Redirect>`
- `<Route>`
- `<Router>`
- `<Switch>`
  使用 switch 会匹配第一个合适的 path 并结束匹配，不使用 switch 包裹，可能会出现渲染多个匹配的情况

```
<Switch>
  <Route exact path="/" component={Home} />
  <Route path="/about" component={About} />
  <Route path="/contact" component={Contact} />
  {/* when none of the above match, <NoMatch> will be rendered */}
  <Route component={NoMatch} />
</Switch>
```

### 参数
使用 exact 关键字，保证只有“完全一致”的情况会匹配
component 接收一个组件作为参数，如果传递给组件的 props 改变，会重写渲染整个页面（采用 createElement 方式构建，如果 props
render 接收一个组件作为参数，直接调用组件的 render 方法，不会重写渲染

### 属性

- history
- location
- match
- matchPath
- withRouter
