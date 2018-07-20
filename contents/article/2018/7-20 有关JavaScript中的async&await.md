# 有关JavaScript中的async&await

标签（空格分隔）： JavaScript 同步

---

经常会看到使用async和await来修饰函数，首先需要明确三点

 1. async修饰的对象的返回值是一个Promise对象
 2. await只能在async定义的函数内部使用
 3. 遇到await时，其修饰的函数会立即执行，然后跳出其处于的被async修饰的对象


 ```
 
      function normalFunc() {
        console.log("进入一个普通的函数");
        return "普通函数的返回值";
      }

      async function asyncFunc() {
        console.log("进入一个被async修饰的函数");
        return "被async修饰的函数的返回值";
      }

      async function test() {
        console.log("进入一个测试函数");

        let r1 = await normalFunc();
        console.log(r1);

        let r2 = await asyncFunc();
        console.log(r2);
      }

      test();

      let promise = new Promise((resolve, reject) => {
        console.log("进入外部的Promise");
        resolve("外部Promise成功回调的值");
      });
      promise.then((val) => {
        console.log(val);
      });

      console.log("最后一行代码");
 ```
