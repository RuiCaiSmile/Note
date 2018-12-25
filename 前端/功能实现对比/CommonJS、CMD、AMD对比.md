# CommonJS，AMD，CMD区别
AMD/CMD/CommonJs是JS模块化开发的标准。
commonjs是用在服务器端的，同步的，如nodejs
amd, cmd是用在浏览器端的，异步的，如requirejs和seajs

Requirejs遵循的时AMD规范（异步模块定义规范）是requirejs自己定义的规范。
Asynchronous Module Definition 异步模块加载机制
Seajs遵循的时CMD规范（通用模块加载规范）是seajs自己定义的一个规范。

CMD推崇依赖就近，AMD规范推崇依赖前置。
