title: Nodejs 异步回调之异常
date: 2017-07-03 10:47:09
tags: [NodeJs]
categories: frontend
---

目前我们项目的Nodejs异常是通过express next 到 errorhandler 中间件去处理的，
原本以为此方法可以捕获到所有的异常，但事实发现并非如此。

下面以一个异常举例子：

```js 
req.get('',function(req, res, next){
    var a = undefined.b; // 产生了一个exception
})

req.use(function(){req, res, next}{
    next();  //最终到 errorhandler中间件中处理
})
```

上面这个例子中，我们认为的制造了一个excepteion，同时我们期望的结果是异常能进入到我们写好的handler中去做处理。

从上面代码的运行结果来看，也符合我们的预期。

<!-- more -->

如果换个地方抛出异常，结果就不是我们想要的了。

```js 
req.get('',function(req, res, next){
    redis.get('key', function(){
        var a = undefined.b; // 产生了一个exception
    })
})

req.use(function(){req, res, next}{
    next();  //最终到 errorhandler中间件中处理
})
```

上面的代码抛出的异常并不会被express捕获，也不会被next到我们的错误处理器中，而是会下面的代码捕获

```js
process.on('uncaughtException', uncaughtExceptionHandler);
```

所以，nodejs中，异步回调中的异常是无法被外围的try catch捕获的。

```js
req.get('',function(req, res, next){
    try{
        redis.get('key', function(){
            var a = undefined.b; // 产生了一个exception
        })
    }catch(e){
        //并不会进到这里来
    }
})

```

解决方案：

1.Promise

```js
function promiseFun() {
    return new Promise(function (resolve, reject) {
        redis.get('key', function(){
            resolve("Hello");
            // reject();
        })
    })
}

promiseFun().then().catch();
```


2.Async await

```js
var getAsync1 = await async1();

async function async1() {
return new Promise(function (resolve, reject) {
        redis.get('key', function(){
            resolve("Hello");
            // reject();
        })
    });
}

console.log(getAsync1);
```

But ...

如果是下面这样写

```js
function promiseFun() {
    return new Promise(function (resolve, reject) {
        redis.get('key', function(){
            throw Error();  //依然捕获不到
        })
    })
}

promiseFun().then().catch();
```

是依然捕获不到的。

