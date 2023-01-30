## 原文链接[https://zhuanlan.zhihu.com/p/40733281](https://zhuanlan.zhihu.com/p/40733281)
## 先说结论一：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/297368/1599660141441-fc985d64-f14d-43ec-be78-211492428e1b.png#align=left&display=inline&height=231&margin=%5Bobject%20Object%5D&name=image.png&originHeight=462&originWidth=1426&size=79607&status=done&style=none&width=713)
### 错误的写法
```javascript
// lib.js
export default { 
 a: 1,
 b: 2
}
// main.js
import { a,b } from './lib';
console.log('a:',a);
console.log('b:',b);
```
-Q：为什么禁止对复合对象字面量的导出操作？(如上)
-A：babel5会将错误的写法输出成正确的值（babel5的bug），其实按照正确的esm语法，使输出为undefined的。
-A：在esm和cjs互相转换的时候加大复杂性。如下图：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/297368/1599660867222-47c57e86-4a29-45e5-8f12-5f1f7dc0486c.png#align=left&display=inline&height=314&margin=%5Bobject%20Object%5D&name=image.png&originHeight=628&originWidth=1420&size=114520&status=done&style=none&width=710)


## 正确使用ESM
### 导出
```javascript
// lib.js
export function getName(){}
export const a = 1
export default 'name'

```
### 导入
```javascript
import * as all from './lib.js'
import lib from './lib.js'
import {a} from './lib.js'
```

## CJS的导入导出
### 导出
```javascript
module.export = {}
export.name = 'chenjian'
```
### 导入
```javascript
const name = require('./lib.js');
```

## 原文链接[https://zhuanlan.zhihu.com/p/97335917](https://zhuanlan.zhihu.com/p/97335917)
## 再说结论二：
彻底禁用**export default**


## 我个人的观点：
esm一把梭，兼容问题是暂时的，就像微软不支持ie一样。该进步还是要进步。
