# rest参数



```javascript
function sum(...values) {
  let total = 0;
  for (let v of values) {
    total += v;
  }
  return total;
}

let a = sum(1, 2, 3, 4, 5); // 15
```

ES6 引入 rest 参数（形式为`...变量名`），用于获取函数的多余参数，这样就不需要使用`arguments`对象了。rest 参数搭配的变量是一个数组，该变量将多余的参数放入数组中。











