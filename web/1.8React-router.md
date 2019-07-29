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
    history 对象通常具有以下属性和方法：
    - length - （number）历史堆栈中的条目数
    - action- （字符串）当前动作（PUSH，REPLACE，或 POP）
    - location - （对象）当前位置。可能具有以下属性：
      -  pathname - （字符串）URL 的路径
      -  search - （字符串）URL 查询字符串
      -  hash - （字符串）URL 哈希片段
      -  state- （对象）特定于位置的状态，例如 push(path, state)当该位置被推入堆栈时。仅适用于浏览器和内存历史记录。
    - push(path, [state]) - （function）将新条目推送到历史堆栈
    - replace(path, [state]) - （function）替换历史堆栈中的当前条目
    - go(n)- （function）按 n 条目移动历史堆栈中的指针
    - goBack() - （功能）相当于 go(-1)
    - goForward() - （功能）相当于 go(1)
    - block(prompt)- （功能）防止导航（参见历史文档
- location
- match
- matchPath
- withRouter
