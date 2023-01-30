类似于数组，但是值都是唯一的。
`const a = new Set([1,2,3,3,4])`
##### Set 内部判断两个值是否不同，使用的算法叫做“Same-value-zero equality” 
了解下各比较算法

1.比较熟悉的 == 和 ===，前者会在类型不相等时，依据规则进行类型转换。而后者则是不允许类型转化来比较。
2. `Object.is()` 方法也是比较两者是否相等，它使用的是**SameValue算法。**与 == 和 ===的区别如下

```javascript
Object.is(NaN, NaN) // true
Object.is(0,-0) // false
Object.is(null, null) // true
NaN === NaN // false
null === null // true
0 === -0 // true
```
3.我们Set内部使用的是**SameValueZero算法，**而它的不同之处

```javascript
const a = new Set([0, -0, NaN, null])
a.has(NaN)
// true
a.has(NaN)
//  true
a.add(NaN)
Set(3) {0, NaN, null}
a.has(NaN)
// true
// 可以看到NaN在使用SameValueZero算法的时候NaN是相等的，这和SameValue表现一样，
// 0和-0也认为他们是相等,这就和Object.is()的结论不一致，也就是和SameValue表现不一致
```

总结：Set数据类型保证其值唯一不重复是依据SameValueZero算法实现


