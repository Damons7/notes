# 搭建webpack

#### 默认搭建完webpack

**在终端：**

- cnpm i react react-dom -S   (即--save（保存）
  包名会被注册在package.json的dependencies里面，在生产环境下这个包的依赖依然存在)

- 在index.js直接打出如下：

  ```js
  import React from 'react';
  import ReactDom from 'react-dom';
  const h1 = React.createElement('h1',null,'这是简单的react示例');
  ReactDom.render(h1,document.querySelector("#app"));
  ```

  

- 同时在index.html创建一个id为app的div即可