### 循环类型
在ECMA262标准第三版中定义了四种类型的循环
#### for
#### while
#### do...while...
#### for...in...

for in 循环比其他几种要慢，这是因为每次迭代时都会搜索实例和原型对象

```javascript
for (const key in object) {
  if (object.hasOwnProperty(key)) {
    const element = object[key];

  }
}
```
上面代码是vscode快速补全插件带出来的forin循环，加了hasOwnProperty判断

```javascript
Object.prototype.objCustom = function() {}; 
Array.prototype.arrCustom = function() {};

let iterable = [3, 5, 7];
iterable.foo = 'hello';

for (let i in iterable) {
  console.log(i); // logs 0, 1, 2, "foo", "arrCustom", "objCustom"
}

for (let i in iterable) {
  if (iterable.hasOwnProperty(i)) {
    console.log(i); // logs 0, 1, 2, "foo"
  }
}
```
上面代码就直观了。
所以除非明确需要迭代一个属性数量未知的对象，否则避免使用forin，用它来遍历数组也是得不偿失

至于 `for...of` 
> [`for...in`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in) 语句以原始插入顺序迭代对象的[可枚举属性](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Enumerability_and_ownership_of_properties)。
> `for...of` 语句遍历[可迭代对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Iterators_and_Generators#Iterables)定义要迭代的数据。
> 

```javascript
for (let i of iterable) {
  console.log(i); // logs 3, 5, 7
}
```

该循环迭代并记录`iterable`作为[可迭代对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Iterators_and_Generators#Iterables)定义的迭代值，这些是数组元素 `3`, `5`, `7`，而不是任何对象的**属性**。
