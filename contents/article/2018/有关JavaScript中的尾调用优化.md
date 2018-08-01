# 有关JavaScript中的尾部调用优化

标签（空格分隔）： JavaScript 代码优化

---

在函数式编程中的一个重要概念就是，其含义为：我们在函数的最后一步操作中调用另一个函数

如下一个尾调用的实例

```
    function f(x) {
        return g(x);
    }
```

在函数`f(x)`中，我们在其最后一步的返回函数中调用了函数`g(x)`

而，如下的几种情况比较容易混淆，但是由于不是在函数的最后一步调用另一个函数，故都不属于尾调用
```
    function f(x) {
        let y = g(x);
        return y;
    }

    function f(x) {
        return g(x) + 1;
    }

    function f(x) {
        g(x);
        //最后还会返回一个undefined
    }
```

那么使用尾调用具体的优化有体现在哪里呢？

我们首先看如下的未进行优化的代码
```
    function f(x) {
        let y = x * x;
        let z = g(y);
        return z;
    }
```

在调用函数`g(y)`的时候，由于其不是最后的语句，所有我们不能确定变量`x`和`y`在函数后面时候还有用，故这个时候就会生成一个对象保存这些变量以及`g(x)`函数的位置指针，并将该对象放到一个栈中去。

那我们我们进行尾调用优化之后的代码如下
```
    function f(x) {
        let y = x * x;
        return g(y);
    }
```

由于`g(y)`是最后一步调用的，故我们就可以非常明确变量`x`和`y`在函数后面不会再被调用了，而`g(x)`的位置信息也不会再用到了，故就不需要生成一个对象保存在栈中了。

在一些调用量非常大的函数中，这种优化就会明显的提高程序的性能中。

## 尾递归

如果使用递归的方式求解斐波那契数列或者阶乘等问题的时候，这个时候递归调用函数的次数是非常多的，我们就考虑进行尾调用优化（又称尾递归）

如下一个求`n`阶乘的函数

```
    function factorial(n) {
        if (n === 1) return 1;
        return n * factorial(n - 1);
    }
```
很显然我们在函数执行完乘之后又对其进行了乘n的操作，这就会产生非常多的调用帧，很容易会产生"栈溢出"

执行如下代码`factorial(16000)`，就会产生栈溢出，如下错误`VM275:1 Uncaught RangeError: Maximum call stack size exceeded`

这个时候我们对其进行尾调用优化，优化后代码如下

```
    function factorial(n, sum = 1) {
        if (n === 1) return sum;
        return factorial(n - 1, n * sum);
    }
```

