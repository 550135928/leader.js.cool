# Koa

## 前置条件

koa 2.0以上版本

```
npm install koa
```

(更新本文时的最新版本为2.0 alpha [^1])

[^1]: 最新版本: <https://github.com/koajs/koa>

## 带async的示例

app.js:

```js
const Koa = require('koa');
const app = new Koa();

// logger

app.use(async (ctx, next) => {
  const start = new Date;
  await next();
  const ms = new Date - start;
  console.log(`${ctx.method} ${ctx.url} - ${ms}ms`);
});

// response

app.use(ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);
```

