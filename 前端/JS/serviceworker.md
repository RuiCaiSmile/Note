# service workers 实践

## 什么是 service workers

> [A service worker is a type of web worker](https://w3c.github.io/ServiceWorker/#service-worker-concept)

service worker 是一个 WEB API,是 Web Workers 的一种实现，功能是可以拦截、处理请求，配合 CacheStorage 等 api，可以做到将资源存储到本地等，实现离线访问。

## 使用

### "须知"

- 使用网站必须是 https 协议，本地测试的时候支持 localhost 访问
- 不占用线程、异步设计等与 Web Workers 一样的特性
- IE11 与 Opera Mini 不支持
- 配合 cachestorage 使用

### 生命周期

service workers 的生命周期有：

- 安装 install
- 激活 activate

加载 service workers 需要注册(Registration)，更新 service workers 则会在 install 后先进入 waiting 状态，等到下次加载或使用`skipWaiting()`进入 activate 状态。

#### 注册 Registration

要使用 service workers，需要先注册，为避免缓存、浏览器不支持等问题，可以写成如下格式：

```
if('serviceWorker' in navigator){
    navigator.serviceWorker.register('serviceworker.js?V=0.0.1')
}
```

首次进入带有 service workers 的网站，会立即进行下载。
在旧版本浏览器上，如果设置的[Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)的更新时间大余 24 小时，则表现为至少 24 小时会更新一次。
在 chrome68 、firefox57 以及开始支持 service workers 的 edge、safari 的版本，会[忽略Cache-Control](https://stackoverflow.com/questions/38843970/service-worker-javascript-update-frequency-every-24-hours)。

#### 安装 install

当页面成功注册了 service workers 之后,就会发生 install 事件，使用`self.addEventListener`可以监听。此时已经可以将部分资源直接存储到 cachestroge 当中，这样下次加载时就可以直接从 cachestorge 获取，不必再次通过请求服务器下载资源。考虑到符合 PWA 的渐进原则，并保证 install 成功，此处仅加载必要且少量的资源即可，如果对离线访问有要求，可以优先存储离线需要而在线不需要的资源。
此时需要使用到[CacheStorage](https://developer.mozilla.org/zh-CN/docs/Web/API/CacheStorage)。

```
const filesToCache = [
  "/resources/js/main.js",
  "/resources/css/main.css",
];

//self 属性可返回对窗口自身的只读引用,在Workers中不能使用window对象
self.addEventListener("install", function(event) {
  event.waitUntil(
    //打开version的cache
    caches.open(version).then(function(cache) {
      //增加需要缓存的文件
      return cache.addAll(filesToCache);
    })
  );
});
```

#### 激活 activate && 更新 update

初次 install 成功后，会直接进入 activate，如果需要更新，install 后会先保持 waiting，此时会保持使用旧版本的 js，在下一次访问会使用新的 js 文件。
关于 service workers 的更新：

- 每当 serviceworker.js 发生了改动，便默认更新，访问时都会自动获取新的 js 文件（客户端自身缓存不考虑的情况），自然而然，给 js 加一个版本号便可实现更新。
  ```
  const version = "V0.0.1";
  ```
- 如果想跳过 waiting 阶段，可以在 install 之后使用`skipWaiting`方法
  ```
  self.addEventListener('install', function(event) {
         event.waitUntil(self.skipWaiting());
  });
  ```

当监听到 activate 时，可以对`CacheStorage`进行更新、清理等：

```
self.addEventListener("activate", event => {
  event.waitUntil(
    //caches.keys返回key的list
    caches.keys().then(keyNameList => {
      //删除旧版本缓存
      keyNameList.map(key => {
        if (key !== version) {
          return caches.delete(key)
        };
      });
    })
  );
});
```

#### 拦截与处理请求

进入到 activate 之后，我们便可以对浏览器发送的请求进行拦截与处理。
在拦截请求的时候，需要注意，[`cache.put`不支持"POST"方法](https://w3c.github.io/ServiceWorker/#cache-put),可以处理的策略有使用indexedb、localstorage，或者干脆只使"GET"请求；

```
self.addEventListener("fetch", event => {
   //Fetchevent.respondWith拦截request
   event.respondWith(
    //caches.match 返回第一个匹配的response对象
    caches.match(event.request).then(resp => {
      return resp ||  fetch(event.request).then(response => {
        const clonedResponse = response.clone();
         // 可以使用indexeddb存储post，也应该可以用localstorage
        if (response.ok && event.request.method != "POST") {
          caches.open(version).then(cache => {
            cache.put(event.request, clonedResponse);
          });
        }
        return response;
      });
    })
  );
  });
```

## 小结
service workers是谷歌推行的PWA 设计理念里重要的技术组成，除了上文介绍的以外，还有很多其他API，功能还是比较强大的。
不过如果想实现离线、安装桌面版本，还需要写好前进、后退、导航等功能，如果流畅性不高很影响体验。此外，[PushAPI](https://caniuse.com/#search=push) 的支持性还不是很好，通信应该比较受限制（未具体研究）。


参考资料：
1. [网站渐进式增强体验(PWA)改造](https://lzw.me/a/pwa-service-worker.html)
1. [SeriviceWorkers](https://w3c.github.io/ServiceWorker)
1. [前端应该了解的PWA](https://juejin.im/post/5af2fd776fb9a07a9c04372f?utm_medium=hao.caibaojian.com&utm_source=hao.caibaojian.com)