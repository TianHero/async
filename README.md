# async
异步操作解决方法

## 1 ES6 promise 异步函数顺序执行
```js
// promise 异步函数顺序执行
const a = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log('第一个异步函数');
      return resolve('第一个异步函数完成')
    }, 1000);
  })
}

const b = (data) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log('第二个异步函数');
      return resolve(data + '第二个异步函数完成')
    }, 1000);
  })
}

const c = (data) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log('第三个异步函数');
      return resolve(data + '第三个异步函数完成')
    }, 1000);
  })
}

// 组织函数队列
function reduce(arr) {
  let sequence = Promise.resolve()
  arr.forEach(item => {
    sequence = sequence.then(item)
  });
  return sequence
}

//执行函数队列
reduce([a, b, c]).then(function (data) {
  console.log(data);
}).catch(function (err) {
  console.log(err);
})
```

## 2 ES7 async await (终极解决方案)
```js
/*
    async 表示这是一个async函数，await只能用在这个函数里面。

    await 表示在这里等待promise返回结果了，再继续执行。

    await 后面跟着的应该是一个promise对象（当然，其他返回值也没关系，只是会立即执行，不过那样就没有意义了…）

*/
var sleep = function (time) {
  return new Promise(function (resolve, reject) {
    setTimeout(function () {
      resolve();
    }, time);
  })
};

//值得注意的是，await必须在async函数的上下文中的。
var start = async function () {
  for (var i = 1; i <= 10; i++) {
    console.log(`当前是第${i}次等待..`);
    await sleep(1000);
  }
};

start();
```
