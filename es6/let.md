# let



```javascript
 let x = 5;
```

`let`和`var`类似，可以用来声明变量。



```javascript
{
  var x = 1;
  let y = 2;
}

console.log(typeof x); // number
console.log(typeof y); // undefined
```

使用`let`声明的变量，仅在块级作用域内有效，这是`let`和`var`的最大区别。









