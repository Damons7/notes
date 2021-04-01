# koa

#### 一、起步

node需要7.6以上版本，下载koa：npm i koa 

快速搭起HTTP服务（http://localhost:3000）

```js
const Koa = require('koa');
const app = new Koa();
app.listen(3000);
```



#### HTTP Response 的类型

```js
const Koa = require('koa');
const app = new Koa();
/*Koa 默认的返回类型是text/plain，如果想返回其他类型的内容，可以先用ctx.request.accepts判断一下，客户端希望接受什么数据（根据 HTTP Request 的Accept字段），然后使用ctx.response.type指定返回类型。*/
const main = ctx => {
  if (ctx.request.accepts('xml')) {
    ctx.response.type = 'xml';
    ctx.response.body = '<data>Hello World</data>';
  } else if (ctx.request.accepts('json')) {
    ctx.response.type = 'json';
    ctx.response.body = { data: 'Hello World' };
  } else if (ctx.request.accepts('html')) {
    ctx.response.type = 'html';
    ctx.response.body = '<p>Hello World</p>';
  } else {
    ctx.response.type = 'text';
    ctx.response.body = 'Hello World';
  }
};

app.use(main);
app.listen(3000);
```



#### **运用模板**

```js
const fs = require('fs');
const Koa = require('koa');
const app = new Koa();

const main = ctx => {
  ctx.response.type = 'html';
  ctx.response.body = fs.createReadStream('./template.html'); //template模板html
};

app.use(main);
app.listen(3000);
```



#### 二、路由

- **原生路由**：网站一般都有多个页面。通过`ctx.request.path`可以获取用户请求的路径，由此实现简单的路由。

  ```js
  const Koa = require('koa');
  const app = new Koa();
  
  const main = ctx => {
    if (ctx.request.path !== '/') {
      ctx.response.type = 'html';
      ctx.response.body = '<a href="/">Index Page</a>';
    } else {
      ctx.response.body = 'Hello World';
    }
  };
  
  app.use(main);
  app.listen(3000);
  ```

- ### koa-router（封装）（npm i koa-router）

  ```js
  const Koa = require('koa');
  // 注意require('koa-router')返回的是函数:
  const router = require('koa-router')();
  const app = new Koa();
  
  // add url-route:
  router.get('/hello/:name', async (ctx, next) => {
      var name = ctx.params.name;
      ctx.response.body = `<h1>Hello, ${name}!</h1>`;
  });
  
  router.get('/', async (ctx, next) => {
      ctx.response.body = '<h1>Index</h1>';
  });
  
  // add router middleware:
  app.use(router.routes());
  
  app.listen(3000);
  console.log('app started at port 3000...');
  ```

- #### koa-static 处理静态资源（npm i koa-static）

  ```js
  const Koa = require('koa');
  const app = new Koa();
  const path = require('path');
  const serve = require('koa-static');
  
  const main = serve(path.join(__dirname));
  
  app.use(main);
  app.listen(3000);
  ```

- #### 重定向（基于上面koa-router继续示例）

```js
  
const Koa = require('koa');
// 注意require('koa-router')返回的是函数:
const router = require('koa-router')();
const app = new Koa();

// add url-route:
router.get('/hello/:name', async (ctx, next) => {
    var name = ctx.params.name;
    ctx.response.body = `<h1>Hello, ${name}!</h1>`;
});
router.get('/redirect', async (ctx, next) => {
    ctx.response.redirect('/');//重定向
    ctx.response.body = '<h1>redirect</h1>';
});

router.get('/', async (ctx, next) => {
    ctx.response.body = '<h1>Index</h1>';
});

// add router middleware:
app.use(router.routes());

app.listen(3000);
console.log('app started at port 3000...');
```

#### 三、middleware中间件

- 

```JS
const Koa = require('koa');
const app = new Koa();
const logger = (ctx, next) => {
  console.log(`${Date.now()} ${ctx.request.method} ${ctx.request.url}`);
  next();
}
const main = ctx => {
  ctx.response.body = 'Hello World';
};
app.use(logger);
app.use(main);
app.listen(3000);
```

像上面代码中的`logger`函数就叫做"中间件"（middleware），因为它处在 HTTP Request 和 HTTP Response 中间，用来实现某种中间功能。`app.use()`用来加载中间件。基本上，Koa 所有的功能都是通过中间件实现的，前面例子里面的`main`也是中间件。每个中间件默认接受两个参数，第一个参数是 Context 对象，第二个参数是`next`函数。只要调用`next`函数，就可以把执行权转交给下一个中间件。

- ### 中间件栈,(多个中间件会形成一个栈结构（middle stack），以"先进后出"的顺序执行)

  - 最外层的中间件首先执行。
  - 调用`next`函数，把执行权交给下一个中间件。
  - ...
  - 最内层的中间件最后执行。
  - 执行结束后，把执行权交回上一层的中间件。
  - ...
  - 最外层的中间件收回执行权之后，执行`next`函数后面的代码。

```JS
const Koa = require('koa');
const app = new Koa();

const one = (ctx, next) => {
  console.log('>> one');
  next();
  console.log('<< one');
}
const two = (ctx, next) => {
  console.log('>> two');
  next();
  console.log('<< two');
}
const three = (ctx, next) => {
  console.log('>> three');
  next();
  console.log('<< three');
}

app.use(one);
app.use(two);
app.use(three);
app.listen(3000);
/*
	执行顺序为：	>> one
				>> two
				>> three
				<< three
				<< two
				<< one
*/
```

- ### 异步中间件（需要用async函数）

- ### 中间件的合成([`koa-compose`](https://www.npmjs.com/package/koa-compose)模块可以将多个中件合成为一个)

```js
const Koa = require('koa');
const compose = require('koa-compose');
const app = new Koa();

const logger = (ctx, next) => {
  console.log(`${Date.now()} ${ctx.request.method} ${ctx.request.url}`);
  next();
}
const main = ctx => {
  ctx.response.body = 'Hello World';
};
const middlewares = compose([logger, main]); //合成

app.use(middlewares);
app.listen(3000);
```

#### 四、错误处理

- ### 500，404 错误

`使用ctx.throw()`方法，用来抛出错误，如`ctx.throw(500)`就是抛出500错误。

```js
const Koa = require('koa');
const app = new Koa();
const main = ctx => {
  ctx.throw(500);
};
app.use(main);
app.listen(3000);
```

- ###  处理错误的中间件

为方便处理错误，最好使用`try...catch`将其捕获。但为每个中间件都写`try...catch`太麻烦，我们可以让最外层的中间件，负责所有中间件的错误处理。

- ### error 事件的监听

```js
const Koa = require('koa');
const app = new Koa();
const main = ctx => {
  ctx.throw(500);
};
app.on('error', (err, ctx) => {
  console.error('server error', err);
});
app.use(main);
app.listen(3000);
```

- ### 释放 error 事件

注意，如果错误被try...catch捕获，就不会触发error事件。这时，必须调用ctx.app.emit()，手动释放error事件，才能让监听函数生效。

```js
const Koa = require('koa');
const app = new Koa();
const handler = async (ctx, next) => {
  try {
    await next();
  } catch (err) {
    ctx.response.status = err.statusCode || err.status || 500;
    ctx.response.type = 'html';
    ctx.response.body = '<p>Something wrong, please contact administrator.</p>';
    ctx.app.emit('error', err, ctx);
  }
};
const main = ctx => {
  ctx.throw(500);
};
app.on('error', function(err) {
  console.log('logging error ', err.message);
  console.log(err);
});
app.use(handler);
app.use(main);
app.listen(3000);
```

#### 五、cookies

```js
const Koa = require('koa');
const app = new Koa();
const main = function(ctx) {
  const n = Number(ctx.cookies.get('view') || 0) + 1;
  ctx.cookies.set('view', n);  //存入view=1至cookies
  ctx.response.body = n + ' views';
}
app.use(main);
app.listen(3000);
```

### 