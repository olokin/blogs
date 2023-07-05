**Promise.all**

1. Promise.all 的返回值是一个新的 Promise 实例。
2. Promise.all 接受一个可遍历的数据容器，容器中每个元素都应是 Promise 实例
3. 数组中每个 Promise 实例都成功时（由 pendding 状态转化为 fulfilled 状态），会按照原来的顺序集合在一个数组中作为 Promise.all 的 resolve 的结果。
4. 数组中只要有一个 Promise 实例失败（由 pendding 状态转化为 rejected 状态），Promise.all 的 .catch() 会捕获到这个 reject。

**手动实现**

```js
// Promise.race
Promise.myRace = function (promises) {
  return new Promise((resolve, reject) => {
    // 这里不需要使用索引，只要能循环出每一项就行
    for (const item of promises) {
      Promise.resolve(item).then(resolve, reject)
    }
  })
}

// Promise.any
Promise.myAny = function (promises) {
  let count = 0

  return new Promise((resolve, reject) => {
    promises.forEach((item, i) => {
      Promise.resolve(item).then(resolve, (err) => {
        count += 1
        if (count === promises.length) reject(new Error('没有promise成功'))
      })
    })
  })
}

// Promise.all
Promise.myAll = function (promises) {
  let count = 0
  let arr = []

  return new Promise((resolve, reject) => {
    promises.forEach((item, i) => {
      Promise.resolve(item).then((res) => {
        count++
        arr[i] = res

        if (count === promises.length) resolve(arr)
      }, reject)
    })
  })
}

// Promise.allSettled
Promise.myAllSettled = function (promises) {
  let count = 0
  let arr = []

  return new Promise((resolve, reject) => {
    const processResult = (res, index, status) => {
      arr[index] = { status: status, val: res }
      count++
      if (count === promises.length) resolve(arr)
    }

    promises.forEach((item, i) => {
      Promise.resolve(item).then(
        (res) => {
          processResult(res, i, 'fulfilled')
        },
        (err) => {
          processResult(err, i, 'rejected')
        }
      )
    })
  })
}
```
