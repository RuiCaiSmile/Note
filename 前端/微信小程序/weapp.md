# weapp

使用基本 API，没有什么特别，照着轮子用就行了，引入 webview 的时候注意部分属性可能无法解析，需要重新适配。

## 一些细节

1.  事件监听，在事件标签上使用`bindtap/catchtap`等绑定
2.  页面跳转，使用`wx.navigateTo`, 底部Tab跳转，使用`wx.switchTab`
3.  使用`event.target.dataset.name` 或 `event.currentTarget.dataset.name` 获取自定义的 view 层的 `data-name`的值，前者指事件绑定位置，后者指事件触发位置，按结构配合事件冒泡或直接使用
4.  通过app.js的`globalData`全局传值
4.  通过小程序接触到数据驱动，通过绑定数据到DOM，实现数据的更新直接更新DOM