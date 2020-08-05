# ES6

## 1、let 和const 命令

### 1、let 命令

>  用法类似`var`，但是let声明的变量只在`let`所在的代码块生效
>
> 只要块级作用域内存在`let`命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。
>
> `let`不允许在相同作用域内，重复声明同一个变量。

**区别：**

`var`命令会发生“变量提升”现象，即变量可以在声明之前使用，值为`undefined`。

```javascript
// var 的情况
console.log(foo); // 输出undefined
var foo = 2;

// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;
```

### 2、const 命令

> `const`声明一个**只读**的常量。一旦声明，常量的值就不能改变。所以`const`声明时必须赋初值，后面不能赋值
>
> `const`的作用域与`let`命令相同：只在声明所在的块级作用域内有效。
>
> `const`命令声明的常量也是不提升，同样存在暂时性死区，只能在声明的位置后面使用。