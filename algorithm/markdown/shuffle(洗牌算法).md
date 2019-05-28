# 洗牌算法

## Fisher-Yates Shuffle

Fisher–Yates 随机置乱算法，通俗说就是生成一个有限集合的随机排列。

描述：

- 写下从 1 到 N 的数字
- 取一个从 1 到剩下的数字（包括这个数字）的随机数 k
- 从低位开始，得到第 k 个数字（这个数字还没有被取出），把它写在独立的一个列表的最后一位
- 重复第 2 步，直到所有的数字都被取出
- 第 3 步写出的这个序列，现在就是原始数字的随机排列

```js
function getRandom(arr, length) {
  const random = Math.floor(Math.random() * length)

  // 原始算法将在此处消耗较多资源
  if (arr.includes(random)) {
    return getRandom(arr, length)
  } else {
    return random
  }
}

function randomArray(arr) {
  let newArr = []
  const length = arr.length
  let index = length - 1
  let indexArr = []
  while (index >= 0) {
    const random = getRandom(indexArr, length)
    indexArr.push(random)
    newArr.push(arr[random])
    --index
  }

  return newArr
}
```

## Knuth-Durstenfeld Shuffle

Knuth 和  Durstenfeld   在 Fisher 等人的基础上对算法进行了改进，不借助新的组数，直接在原地交换。该算法的基本思想和 Fisher 类似，**每次从未处理的数据中随机取出一个数，并和最后一个剩余元素交换**，然后剩余元素的数量减一。

```js
function randomArray(arr) {
  const length = arr.length
  for (let index = length - 1; index > 0; index--) {
    const random = Math.floor(Math.random() * index)
    const temp = arr[index]
    arr[index] = arr[random]
    arr[random] = temp
  }

  return arr
}
```

## Inside-Out Algorithm

Inside-Out Algorithm 算法的思想是从前往后，借助旧数组，将**新数组中位置 k 和位置 i 的数字进行交互**。

描述

- 拷贝数组
- 从 i(0 - N)扫描数组，选择一个随机数 k( 0 <= k <= i)
- 新数组的[i] = 新数组的[k], 新数组的[k] = 原始数组[i]
- 重复第 2 步，直到末尾
- 最终的新数组就是随机的

```js
function randomArray(arr) {
  let newArr = arr.concat([])
  const length = arr.length
  for (let index = 0; index < length; index++) {
    const random = Math.floor(Math.random() * (index + 1))
    newArr[index] = newArr[random]
    newArr[random] = arr[index]
  }

  return newArr
}
```

# 参考

- [三种洗牌算法 shuffle](https://blog.csdn.net/qq_26399665/article/details/79831490)
- [Fisher–Yates shuffle From Wikipedia](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle)
