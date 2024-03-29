---
title: 常见的算法题1
abbrlink: 124edd95
tags:
  - front-end
  - JavaScript
categories: web前端学习笔记
date: 2020-11-16 17:17:00
index_img:
banner_img:
---

# 分离正整数数位
```js
  var n = Number(prompt("输入一个整数:"))
  while (n > 0) {
    var x = n % 10
    n = (n - x) / 10
    console.log(x)
  }
```

# 数字反转
```js
  var x = 0
  var flag = 0
  var n = Number(prompt("输入一个整数:"))
  if (n < 0) {
    n = -n
    flag = 1
  }
  while (n) {
    var digit = n % 10
    x = x * 10 + digit
    n = (n - digit) / 10
  }
  if (flag == 1) {
    console.log(-x)
  } else {
    console.log(x)
  }
```

# 判断质数
```js
  var isPrime = function (n) {
    if (n < 2)
      return false
    for (var i = 2; i <= Math.sqrt(n); i++) {
      if (n % i == 0) {
        return false
      }
    }
    return true
  }
```

# 求平方根
```js
//二分法
  function sqrt(x) {
    if (x <= 1) return x
    var left = 0;
    var right = x;
    while (right - left > 1) {
      var mid = Math.floor((right + left) / 2);
      // var mid = (right + left) >> 1;
      if (mid * mid > x) {
        right = mid;
      } else {
        left = mid;
      }
    }
    return left;
  }
```
```js
//求导算出公式
  function sqrt(n) {
    var x = n
    while (Math.abs(x * x - n) > 0.00000001) {
      x = (x + n / x) / 2
    }
    return x
  }
```
[平方根倒数速算法](https://zh.wikipedia.org/wiki/%E5%B9%B3%E6%96%B9%E6%A0%B9%E5%80%92%E6%95%B0%E9%80%9F%E7%AE%97%E6%B3%95)

# 水仙花数
```js
  function digitWidth(m) {  //计算n的十进制位宽
    var width = 0
    do {
      var digit = m % 10
      m = (m - digit) / 10
      width++
    } while (m > 0) {
      return width
    }
  }
  function power(x, n) {  //计算x的n次方
    var exp = 1
    for (var i = 0; i < n; i++) {
      exp *= x
    }
    return exp
  }
  function isNarcissistic(n) {  //是否为水仙花数
    var width = digitWidth(n)
    var m = n
    var sum = 0
    do {
      var digit = m % 10
      sum += power(digit, width)
      m = (m - digit) / 10
    } while (m > 0)
    if (sum == n) {
      return true
    } else {
      return false
    }
  }
```

# 回文数
```js
  function isPalindrome(n) {
    var m = n
    var revert = 0
    while (m > 0) {  //数字反转
      var digit = m % 10
      revert = revert * 10 + digit
      m = (m - digit) / 10
    }
    if (revert == n) {
      return true
    } else {
      return false
    }
  }
```
```js
  function isPalindrome(n) {
    var width = digitWidth(n)  //调用之前写过的十进制位宽函数
    for (var i = 0; i < (width - 1) / 2; i++) {
      var j = width - i - 1
      //i,j分别指向顺数/倒数第n个数
      var di = Math.floor(n / Math.pow(10, i)) % 10
      var dj = Math.floor(n / Math.pow(10, j)) % 10
      if (di !== dj) {
        return false
      }
    }
    return true
  }
```

# 2的幂
```js
  var isPowerOfTwo = function (n) {
    while (n && (n % 2 == 0)) {
      n /= 2;
    }
    return n == 1;
  } 
```
```js
// 如果一个数是2的次方数，那么它的二进数必然是最高位为1，其它都为0。
// 如果此时减1的话，则最高位会降一位，其余为0的位都变为1。
// 把两数相与，就会得到0。
  var isPowerOfTwo = function (n) {
    return ((n > 0) && ((n & (n - 1)) == 0))
  }
```

# 汉明距离
计算两个数二进制有几个不相同的位
```js
// z & (z - 1)可以快速地移除最右边的bit 1，一直循环到num为0，总的循环数就是z中bit 1的个数
  var hammingDistance = function (x, y) {
    var z = x ^ y
    var a = 0
    while (z) {
      z = z & (z - 1)
      a++
    }
    return a
  }
```

# x的n次方
```js
  var myPow = function (x, n) {
    if (n == 0) return 1 // n=0直接返回1
    if (n < 0) {  //n<0时，x的n次方等于1除以x的-n次方分
      return 1 / myPow(x, -n)
    }
    if (n % 2) {  //n是奇数时，x的n次方等于x*x的n-1次方
      return x * myPow(x, n - 1)
    }
    return myPow(x * x, n / 2)  //n是偶数，使用分治，一分为二，等于 x*x 的 n/2 次方 
  }
```

# 单个数字
数组中每个元素会出现两个，只有一个例外，找出它
```js
  var singleNumber = function (nums) {
    var sum = 0
    for (var i = 0; i < nums.length; i++) {
      sum ^= nums[i]
    }
    return sum
  };
  // 把所有的数字求异或，相当于 sum = 0⊕num[0]⊕num[1]⊕num[2]...⊕num[n]
  // 根据X⊕X = 0，会把相同的数都消去
  // 因为X⊕0 = X，结果sum = 0⊕num[只有一个的数] = 只有一个的数
```

# 颠倒二进制位
```js
  var reverseBits = function (n) {
    var res = 0
    for (var i = 0; i < 32; i++) {
      //res只执行左移操作，最后一位为0
      //若n最后一位为1，与1做与运算为1，此时使res最后一位 = n最后一位 = 1
      res <<= 1
      if ((n & 1) == 1) res++
      //n右移，去掉的每一位都被相同的n取得
      n >>= 1
    }
    //将number转换为无符号的32bit数据，即Uint32类型
    return res >>> 0
  }
```

# 数比特位
统计从0到n每个数的二进制写法的1的个数，存入数组中返回
```js
// 偶数：二进制表示中，偶数中 1 的个数一定和除以 2 之后的那个数一样多。
// 因为最低位是 0，除以 2 就是右移一位，也就是把那个 0 抹掉而已，所以 1 的个数是不变的。
// 奇数：二进制表示中，奇数一定比前面那个偶数多一个 1
  var countBits = function (num) {
    var count = [0]
    for (let i = 0; i <= num; i++) {
      if (i % 2 == 0) {
        count[i] = count[i / 2];
      } else {
        count[i] = count[i - 1] + 1;
      }
    }
    return count;
  }
```
```js
// 可以发现每个i值都是 i&(i-1) 对应的值加1
  var countBits = function (num) {
    var count = [0]
    for (var i = 1; i <= num; i++) {
      count[i] = count[i & (i - 1)] + 1
    }
    return count
  }
```

# 最大子数组
```js
// 动态规划
  var maxSubArray = function (nums) {
    var max = nums[0]
    var preMax = nums[0]
    for (var i = 1; i < nums.length; i++) {
// 只有当前一个子数组大于0时，才能对加一个元素的数组产生正增益
// 否则直接从后一个数重新开始
      if (preMax > 0) {
        preMax += nums[i]
      } else {
        preMax = nums[i]
      }
// 每次都把此次最大数存下来，和下一次作比较
      max = Math.max(preMax, max)
    }
    return max
  }
```

# 数质数
数在某个数之前有多少个质数
```js
  var countPrimes = function (n) {
// 厄拉多塞筛法
    var count = 0
    var signs = []
    for (let i = 2; i < n; i++) {
      if (!signs[i]) {
        count++
        for (let j = 2 * i; j < n; j += i) {
// 把找过数的倍数，对应的sign设置为true。外循环就不会进入if循环，也就不会计数了。
          signs[j] = true
        }
      }
    }
    return count
  }
```

# 一周中的星期
给一个日期，判断它是星期几
```js
  var dayOfTheWeek = function (day, month, year) {
    if (month == 1 || month == 2) {
      month += 12
      year--
    }
    // 把一月和二月看成是上一年的十三月和十四月
    var iWeek = (day + 2 * month + Math.floor(3 * (month + 1) / 5) + year + Math.floor(year / 4) - Math.floor(year / 100) + Math.floor(year / 400)) % 7
    //基姆拉尔森计算公式
    var result = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]
    return result[iWeek]
  }
```

# 三角形
给定一个三角形，求从上到下的最小路径和。每一步只能移动到下一行中相邻的结点。
```js
  var minimumTotal = function(triangle) {
    var dp = Array(triangle.length + 1).fill(0)
    for (var i = triangle.length - 1; i >= 0; i--) {
      for (var j = 0; j < triangle[i].length; j++) {
        //  dp[i][j] = Math.min(dp[i + 1][j], dp[i + 1][j + 1]) + triangle[i][j]
        dp[j] = Math.min(dp[j], dp[j + 1]) + triangle[i][j]
      }
    }
    return dp[0]
  };
```

# 不同路径
位于网格的左上角，到达网格的右下角，每次只能向下或者向右移动一步。总共有多少条不同的路径？
```js
  var uniquePaths = function (m, n) {
    let dp = Array.from({ length: m }, () => Array.from({ length: n }, () => 0))
    // 第一列的走法只有一种
    for (let i = 0; i < m; i++) {
      dp[i][0] = 1
    }
    // 第一行的走法只有一种
    for (let j = 0; j < n; j++) {
      dp[0][j] = 1
    }
    for (let i = 1; i < m; i++) {
      for (let j = 1; j < n; j++) {
        // 当前位置是从左边或上边走过来的
        dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
      }
    }
    return dp[m - 1][n - 1]
  };
```
```js
  var uniquePaths = function (m, n) {
    let dp = new Array(m).fill(1)
    for (let i = 1; i < n; i++) {
      for (let j = 1; j < m; j++) {
        dp[j] += dp[j - 1]
      }
    }
    return dp[m - 1]
  };
```

# 排序
```js
// 冒泡排序法
  var bubbleSort = function (nums) {
    // 外层每趟循环，找出一个最大的数
    for (let i = 0; i < nums.length - 1; i++) {
      // 判断每次是否需要交换
      for (let j = 0; j < nums.length - 1 - i; j++) {
        if (nums[j] > nums[j + 1]) {
          // ES6 交换两个变量
          [nums[j], nums[j + 1]] = [nums[j + 1], nums[j]]
          // ES5 交换两个变量
          // const temp = nums[j]
          // nums[j] = nums[j + 1]
          // nums[j + 1] = nums[j]
        }
      }
    }
    return nums
  };
```

```js
// 插入排序法
  var insertionSort = function (nums) {
    for (let i = 1; i < nums.length; i++) {
      for (var j = i; j >= 1; j--) {
        if (nums[j] < nums[j - 1]) {
          [nums[j], nums[j - 1]] = [nums[j - 1], nums[j]]
        } else {
          break
        }
      }
    }
    return nums
  };
```

```js
// 选择排序
// 主要思想：每轮选择无序范围内最小的元素放入有序范围的最后
  var selectSort = function (nums) {
    // 控制要找到最小的数放入i位置
    for (var i = 0; i < nums.length - 1; i++) {
      var minIdx = i //把当前范围的第一个数当成最小数，但只记录其下标
      for (var j = i + 1; j < nums.length; j++) {
        if (nums[minIdx] > nums[j])
          minIdx = j
      }
      [nums[minIdx], nums[i]] = [nums[i], nums[minIdx]]
    }
    return nums
  };
```

```js
// 归并排序
// 主要思想：将数组一分为二，各自排序，之后合并起来
  var mergeSort = function (nums) {
    if (nums.length == 0 || nums.length == 1) {
      return [...nums]
    }
    var mid = nums.length >> 1
    var leftAry = nums.slice(0, mid)
    var rightAry = nums.slice(mid)
    var leftSorted = mergeSort(leftAry)
    var rightSorted = mergeSort(rightAry)
    var result = []
    var i = 0, j = 0
    while (i < leftSorted.length && j < rightSorted.length) {
      if (leftSorted[i] < rightSorted[j]) {
        result.push(leftSorted[i++])
      } else {
        result.push(rightSorted[j++])
      }
    }
    while (i < leftSorted.length) {
      result.push(leftSorted[i++])
    }
    while (j < rightSorted.length) {
      result.push(rightSorted[j++])
    }
    return result
  };
```

```js
// 快速排序
// 主要思想：随机选择数组中的一个数字，将数组分为三部分：小于该数的，等于该数的，大于该数的
// 对于小于该数的部分和大于该数的部分进行排序，然后拼接
  function quickSort(ary) {
    if (ary.length < 2) {
      return ary.slice()
    }
    var randomIdx = Math.floor(Math.random() * ary.length)
    var pivot = ary[randomIdx]
    let left = []
    let middle = []
    let right = []
    for (let i = 0; i < ary.length; i++) {
      if (ary[i] < pivot) {
        left.push(ary[i])
      }
      if (ary[i] > pivot) {
        right.push(ary[i])
      }
      if (ary[i] == pivot) {
        middle.push(ary[i])
      }
    }
    left = quickSort(left)
    right = quickSort(right)
    return left.concat(middle, right)
  }
```
