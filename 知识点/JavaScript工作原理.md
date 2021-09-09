# engine, the runtime, and the call stack（JS引擎，运行时和调用栈）
原文： https://blog.sessionstack.com/how-does-javascript-actually-work-part-1-b0bacc073cf

### JS引擎

JS引擎就是处理js代码的，不同的浏览器有自己的js引擎。

比较著名的就是谷歌的V8引擎，谷歌浏览器和node.js都用了V8引擎。下面就是JS引擎简单的示例图：

![image](https://user-images.githubusercontent.com/69185043/132645740-02558aea-7a33-4b44-b556-06f66615a38d.png)


js引擎包含两个重要的组成部分：
1. 内存堆 —— 分配内存的地方
2. 调用栈 —— 代码执行时栈帧所在的位置

### 运行时

我们经常用到的浏览器的一些API像 setTimeout 并不是由js引擎所提供的。可以将JS运行时看作以一个大容器，如下图。

![image](https://user-images.githubusercontent.com/69185043/132647900-efd71869-2f55-4474-895f-8d12ddb45a0b.png)

除了js引擎，还有浏览器（或者node.js）提供的叫做 Web APIs的东西。比如 DOM, AJAX, setTimeout 等等 。之后就有了大家经常看的事件循环(event loop) 和回调队列（callback queue）

### 调用栈

JS是单线程的编程语言，这意味着只有一个调用栈，因此js一次只能做一件事。
调用栈是一种数据结构，记录了我们在程序中的位置。比如我们进入了一个方法，这个方法就会放在栈顶。如果我们从栈顶返回，这个方法将会从栈顶弹出。

举个例子：

```
function multiply(x, y) {
    return x * y;
}
function printSquare(x) {
    var s = multiply(x, x);
    console.log(s);
}
printSquare(5);
```

一开始调用栈是空的，执行此段代码的过程将会是这样子的：
![image](https://user-images.githubusercontent.com/69185043/132652848-300d6dcf-3dd6-488a-bf80-6ec1256d14d1.png)

调用栈里的每一条叫做栈帧 Stack Frame.

这就是当有异常抛出时，栈轨迹stack traces如何构建的，基本上就是异常发生时调用栈的情况。
举个例子：

```
function foo() {
    throw new Error('SessionStack will help you resolve crashes :)');
}
function bar() {
    foo();
}
function start() {
    bar();
}
start();
```
在谷歌浏览器中执行时的栈轨迹：

![image](https://user-images.githubusercontent.com/69185043/132654246-37e0e382-5525-4e48-a8bf-d6e6007e1c07.png)

如果我们在一个方法中递归的调用自己会发生什么呢？像这样子：

```
function foo() {
    foo();
}
foo();
```

js引擎会将foo()一遍又一遍地加到调用栈中，直到超过最大调用栈的大小 maximum Call Stack size

![image](https://user-images.githubusercontent.com/69185043/132657080-753d9c99-6ef9-49ff-9250-71c191f281de.png)

当超出这个大小时，浏览器就会抛出错误。

![image](https://user-images.githubusercontent.com/69185043/132657537-a3e91be9-d5ed-4de2-9a8e-d8a35e779333.png)


