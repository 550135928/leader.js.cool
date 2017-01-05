# ES Next

## Async (ES 7)

```js
async function fn(args){
  // ...
}

// 等同于

function fn(args){
  return spawn(function*() {
    // ...
  });
}
```

多个`await`命令后面的异步操作，如果不存在继发关系，最好让它们同时触发。

```javascript
let foo = await getFoo();
let bar = await getBar();
```

上面代码中，`getFoo`和`getBar`是两个独立的异步操作（即互不依赖），被写成继发关系。这样比较耗时，因为只有`getFoo`完成以后，才会执行`getBar`，完全可以让它们同时触发。

```javascript
// 写法一
let [foo, bar] = await Promise.all([getFoo(), getBar()]);

// 写法二
let fooPromise = getFoo();
let barPromise = getBar();
let foo = await fooPromise;
let bar = await barPromise;
```

上面两种写法，`getFoo`和`getBar`都是同时触发，这样就会缩短程序的执行时间。

## Proxy (ES 6)

经测试Node v6.1.0之后版本已集成。

示例代码：


```js
const proxy = new Proxy({}, {
  get: (target, property) => [target, property]
});

console.log(proxy.func);          // [ {}, 'func' ]
console.log(proxy.func('123'));   // TypeError: proxy.func is not a function
```

```js
const proxy = new Proxy({}, {
  get: (target, property) =>
    (test) => [target, property, test]
});

console.log(proxy.func);          // [Function]
console.log(proxy.func('123'));   // [ {}, 'func', '123' ]
```
