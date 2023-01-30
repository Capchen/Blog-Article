## 新建一个Promise实例的时候，发生了什么

```javascript
const chenjian = new Promise(function(resolved, rejected) {
    setTimeout( () => {
      resolved()
    },1000)
  })
chenjian.then(function(){})
```

new一个实例的时候，Promise构造函数使用内部的doResolve方法运行了它的入参，所以为啥里面的代码会同步执行。代码如下

```javascript
function Promise(fn) {
  if (typeof this !== 'object') {
    throw new TypeError('Promises must be constructed via new');
  }
  if (typeof fn !== 'function') {
    throw new TypeError('Promise constructor\'s argument is not a function');
  }
  this._deferredState = 0;
  this._state = 0;
  this._value = null;
  this._deferreds = null;
  if (fn === noop) return;
  doResolve(fn, this);
}
function doResolve(fn, promise) {
  var done = false;
  // 这就是为啥生成实例时会同步运行，因为这里运行了fn
  var res = tryCallTwo(fn, function (value) {
    if (done) return;
    done = true;
    resolve(promise, value);
  }, function (reason) {
    if (done) return;
    done = true;
    reject(promise, reason);
  });
  if (!done && res === IS_ERROR) {
    done = true;
    reject(promise, LAST_ERROR);
  }
}
```

这里我们使用的是setTimeout来模拟一个异步，过1秒之后会运行resolved方法，resolved和rejected方法，在Promise函数内部都有定义，他们的作用是改变函数的状态。因为是异步，所以并不会立刻改变状态。
根据事件循环机制，计时器的回调会在计时结束之后被推入消息队列。
在等待1s的时候，程序还是会继续运行。运行了then方法，那么来看看运行then方法时发生了什么：
then方法会新建一个Promise实例,然后判断状态变成了成功还是失败，成功就执行then里面的成功回调，并且判断成功回调是否有返回值，没有返回值执行结束，有返回值那么就去重新从resolve方法来一遍，但是注意的是promise是新的实例。
