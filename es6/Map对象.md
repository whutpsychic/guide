# Map数据结构



## 简介

ES6 提供了新的数据结构 Map，它是一种有对应 键/值 对的对象。

其基本用法如下：

```javascript
const map = new Map();
```



## Map实例的方法和属性

`Map`提供的方法和属性列举如下：

`set`：设置键名`key`对应的键值为`value`，然后返回整个 Map 结构。如果`key`已经有值，则键值会被更新，否则就新生成该键。

`get`：读取`key`对应的键值，如果找不到`key`，返回`undefined`。

`has`：返回一个布尔值，表示某个键是否在当前 Map 对象之中。

`delete`：删除某个键，返回`true`。如果删除失败，返回`false`。

`clear`：清除所有成员，没有返回值。

`size属性`：返回 Map 结构的成员总数。



代码实例如下：

```javascript
const map = new Map();

console.log(map.size); // 0

map.set('x', 5);
map.set('y', 10);

console.log(map.size); // 2

console.log(map.has('x')); // true
console.log(map.has('z')); // false

console.log(map.get('x')); // 5
console.log(map.get('z')); // undefined

map.delete('x');
console.log(map.size); // 1

map.clear();
console.log(map.size); // 0

```



##遍历Map

遍历`Map`的方式如下：

```javascript
var map = new Map();

map.set('x', 5);
map.set('y', 10);
 
for (let [key, value] of map) {
  console.log(key, value); 
}

// 输入如下
// x 5
// y 10
```

