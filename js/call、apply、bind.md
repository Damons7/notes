#### apply

```js
Function.prototype.apply = function (thisArg) 
{ 
    thisArg = thisArg == null ? window : Object(thisArg); //获取函数执行需要的参数
    const exeArgs = arguments[1]; //如果exeArgs为原始类型，抛出错误 
    if(exeArgs !== Object(exeArgs)) 
    { 
        throw new TypeError(`'CreateListFromArrayLike' is called on non-object`); 
    } 
    /* 这里对ExeArgs再次判断是否为Array类型，因为ECMAScript官方标准指出还可以传入类数组。如： NodeList、argument和HTMLCollection，以及像{0:1,'1':2,4:'sss',length:4}这种模样的，都算是类数组。 */ 
    let filterExeArgs = []; 
    if(Array.isArray(exeArgs)) 
    { 
        filterExeArgs = exeArgs; 
    }else { 
        const len = Number(exeArgs.length); 
        for(let i = 0 ; i < len ; i++) 
        { 
            filterExeArgs.push(exeArgs[i]); 
        } 
    } 
    const fn = Symbol('fn');
    thisArg[fn] = this; 
    const res = thisArgs[fn](...filterExeArgs); 
    delete thisArg[fn];
    return res; 
}

```

#### bind

```js
Function.prototype.bind = function (thisArg ,...args) 
{ 
    return (...curArgs) => this.apply(thisArg, [...curArgs, ...args]);
}

```

