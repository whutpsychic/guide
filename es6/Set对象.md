# Set数据结构



## 简介

ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。

其基本用法如下：

```javascript
const set = new Set();
```

Set 函数可以接受一个数组作为参数，用来初始化。

```javascript
let array = [1, 2, 3];
const set = new Set(array);
```



##Set实例的方法和属性

`Set`提供的方法和属性列举如下：

`add`：添加某个值，返回Set结构本身。

`delete`：删除某个值，返回一个布尔值，表示删除是否成功。

`has`：返回一个布尔值，表示该值是否为`Set`的成员。

`clear`：清除所有成员，没有返回值。

`size属性`：返回`Set`实例的成员总数。



代码实例如下：

```javascript
const set = new Set();

console.log(set.size); // 0

set.add(1);        
set.add(2);

console.log(set.size); // 2

console.log(set.has(1)); // true
console.log(set.has(5)); // false

set.delete(1);
console.log(set.size); // 1

set.clear();
console.log(set.size); // 0
```



##遍历Set

遍历`Set`的方式如下：

```javascript
const set = new Set();

set.add(1);        
set.add(2);

for (let x of set) {
  console.log(x);
}

// 返回如下
// 1
// 2
```





