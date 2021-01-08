---
title: Koa
date: 2021-01-08 14:51:16
tags:
- node
- koa
categories:
- node
- koa
---

## Koa 是一个简单好用的Web框架，属于node服务器的层面语言技术。

> 此文默认当前已有node环境，如果你没有node环境，请自行安装

```js
$ node -v
v14.15.0
```

> 本地新建一个空目录，进行依赖的安装

```js
npm init -y
npm i koa2 koa-router koa-static -S
```

1. 搭建http服务

   ```js
   // base.js
   const Koa = require('koa2');
   const app = new Koa();
   
   app.listen(3000);
   ```

   运行这个脚本，`node base.js`

   然后在浏览器中输入`localhost:3000`， 你就会看到页面提示"Not Found"， 这就已经启动起来了。

2. 建立静态页面，并使用koa-static来加载

   ```js
   // index.html
   <html>
       <head>
           <meta charset="UTF-8" />
           <meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=0" />
           <script src="https://cdn.bootcdn.net/ajax/libs/vue/2.6.11/vue.js"></script>
           <script src="https://cdn.bootcdn.net/ajax/libs/axios/0.20.0-0/axios.js"></script>
           <script src="https://cdn.bootcdn.net/ajax/libs/qs/6.9.4/qs.js"></script>
           <style>
               /* .cube-btn {
               margin: 10px 0;
           } */
           </style>
       </head>
   
       <body>
           <div id="app">
               <div>{{name}} : {{age}}</div>
               <button @click="getTokens">getTokens</button>
               <button @click="setTokens">setTokens</button>
               <button @click="noAPI">noAPI</button>
               <button @click="redirect">redirect</button>
           </div>
           <script>
               var app = new Vue({
                   el: '#app',
                   data() {
                       return {
                           name: 'dangchaofeng',
                           age: '28'
                       };
                   },
                   methods: {
                       async getTokens() {
                           const res = await axios.get(`/getTokens`);
                           console.log('res', res);
                       },
                       async setTokens() {
                           const res = await axios.post(`/setTokens`);
                           console.log('res', res);
                       },
                       async redirect() {
                           const res = await axios.get(`/redirect`);
                           console.log('res', res);
                       },
                       async noAPI() {
                           const res = await axios.delete(`/noAPI`, {
                               params: {
                                   id: ' 1'
                               }
                           });
                           console.log('res', res);
                       }
                   },
                   mounted: function () {}
               });
           </script>
       </body>
   </html>
   
   // base-static.js
   
   const Koa = require('koa2');
   const app = new Koa();
   const static = require('koa-static');
   const path = require('path');
   
   const main = static(path.join(__dirname));
   
   app.use(main);
   app.listen(3000);
   ```

   运行这个脚本，`node base-static.js`

   然后在浏览器中输入`localhost:3000`， 你就会看到自己的页面。

3. 加载koa-router路由功能

   ```js
   // base-router.js
   
   const Koa = require('koa2');
   const Router = require('koa-router');
   const static = require('koa-static');
   const path = require('path');
   const app = new Koa();
   const router = new Router();
   
   router.get('/getTokens', ctx => {
       ctx.body = {
           token: '123'
       };
   });
   
   router.post('/setTokens', ctx => {
       ctx.body = {
           success: true
       };
   });
   
   router.post('/noAPI', ctx => {
       ctx.body = {
           success: true
       };
   });
   
   app.use(static(path.join(__dirname)));
   
   // 启用路由
   app.use(router.routes());
   // * 自动识别405等返回。
   // * /noAPI , 只有post ，没有 delete方法，页面触发delete，就会直接405
   app.use(router.allowedMethods());
   
   app.listen(3000);
   
   ```

   >
   >
   >此处强调说明三点：
   >
   >router是用的restful api
   >
   >app.use(router.routes());  此句的作用就是启用路由，将路由这个中间件加载给koa
   >
   >app.use(router.allowedMethods()); 此句的作用，是让路由自动识别方法，例如你的接口只实现了get方法，没有对应的post,put,delete方法，那么当你从页面上访问并没有定义的方法时，正常情况会识别到404返回，如果加载这个中间件，那么它会识别为405，表示你不允许这个方法操作

4. 重定向 `// ctx.redirect('getTokens'); ` 会产生302 的状态码

   ```js
   // base-redirect.js
   
   /*
    * @Author: Adam Dang
    * @Description: 重定向路由
    * @Date: 2021-01-08 14:25:38
    * @LastEditors: Adam Dang
    * @LastEditTime: 2021-01-08 14:25:38
    */
   
   const Koa = require('koa2');
   const Router = require('koa-router');
   const static = require('koa-static');
   const path = require('path');
   const app = new Koa();
   const router = new Router();
   
   router.get('/getTokens', ctx => {
       ctx.body = {
           token: '123'
       };
   });
   
   router.post('/setTokens', ctx => {
       ctx.body = {
           success: true
       };
   });
   
   // 重定向到其他url上
   router.get('/redirect', ctx => {
       ctx.redirect('/getTokens');
   });
   
   app.use(static(path.join(__dirname)));
   
   // 启用路由
   app.use(router.routes());
   // * 自动识别405等返回。
   // * /noAPI , 只有post ，没有 delete方法，页面触发delete，就会直接405
   app.use(router.allowedMethods());
   
   app.listen(3000);
   
   ```

5.  通过监听error 事件处理错误 `ctx.throw(500);`   `app.on('error',(err, ctx)=>{ // handle })`

   ```js
   // base-handleError.js
   
   const Koa = require('koa2');
   const Router = require('koa-router');
   const static = require('koa-static');
   const path = require('path');
   const app = new Koa();
   const router = new Router();
   
   router.get('/getTokens', ctx => {
       ctx.throw(500);
       // ctx.body = {
       //     token: '123'
       // };
   });
   
   router.post('/setTokens', ctx => {
       ctx.body = {
           success: true
       };
   });
   
   app.use(static(path.join(__dirname)));
   
   // 启用路由
   app.use(router.routes());
   app.use(router.allowedMethods());
   
   // 错误处理
   app.on('error', (err, ctx) => {
       console.error('server error', err);
   });
   
   app.listen(3000);
   
   ```

   

