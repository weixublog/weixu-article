# JavaScript中的Promise对象

标签（空格分隔）： JavaScript Promise

---

在ES6中，Promise对象成为了一个原生对象，有关其基本的用法如下

我们经常会看到格式如下的代码

```
  Promise.resolve("success").then(result => {
    console.log(result);
  });
```

控制台打出结果为：success

---

Promise是运用在异步编程中的，比如说加载图片的时候我们就希望图片不阻塞主线程等等。
Promise对象有三种状态：进行中，成功，失败

我们再看如下代码
```
      let p = new Promise(function (resolve, reject) {
        if (STATUS === 'SUCCESS') {
          resolve("success");
        } else {
          reject("fail");
        }
      });

      p.then(
        function (val) { console.log(val); },
        function (err) { console.log(err); }
      );
```

在实例化Promise的时候传入一个方法，该方法的两个参数分别是两个回调函数，其中`resolve`为成功时的回调，`reject`为失败时的回调

在看第二段代码，`p.then()`，接受两个方法，其中`function (val) { console.log(val); }`为成功时回调执行的方法，`function (err) { console.log(err); }`为失败时回调执行的方法

这样也就是说，当`STATUS`的值为`SUCCESS`时，控制台打出`succcess`，反之，控制台打印`fail`。

上面的写法还有另外一种形式，如下代码
```
      Promise.resolve("success").then((val) => {
        console.log(val)
      });

      Promise.reject("fail").then(null,(err) => {
        console.log(err)
      });
```

---

