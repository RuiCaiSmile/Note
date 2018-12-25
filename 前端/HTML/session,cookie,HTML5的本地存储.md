# sesscion,cookie及HTML5本地存储对比

1. session 在服务器端，cookie 在客户端（浏览器）
2. session 默认被存在在服务器的一个文件里（不是内存）
3. session 的运行依赖 session id，而 session id 是存在 cookie 中的，也就是说，如果浏览器禁用了 cookie ，同时 session 也会失效（但是可以通过其它方式实现，比如在 url 中传递 session_id）
4. session 可以放在 文件、数据库、或内存中都可以。
5. 用户验证这种场合一般会用 session 因此，维持一个会话的核心就是客户端的唯一标识，即 session id


|cookie|sessionStorage和localStorage|
|:---|:----|
| 数据始终在同源的http请求中携带，在B/S间来回传递。  | 不会自动把数据发给服务器，仅在本地保存。  |
| 数据有路径的概念，可以限制cookie只属于某个路径下。  |无路径概念  |
|  小于等于4KB | 大于等于5MB，不同浏览器大小会有区别  |
|  在所有同源窗口中共享 | sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；localStorage在所有同源窗口中都共享  |
| 过期时间之前有效，即使窗口关闭后也有效  |  sessionStorage窗口关闭前有效，localStorage永久有效 |
