title: 滑动窗口算法（sliding window algorithm）
date: 2019-03-12 00:47:04
tags:
---

# 概念

滑动窗口算法可以用以解决数组/字符串的子元素问题，它可以将嵌套的循环问题，转换为单循环问题，降低时间复杂度。

# 示例1

给定一个整数数组，计算长度为 'k' 的子数组的最大总和。

```
输入：arr [] = {100,200,300,400}
     k = 2

输出：700

解释：300 + 400 = 700
```

## 暴力法

我们可以很容易想到暴力法来解决这个问题，使用两个 for 循环来不断查找长度为 k 的最大总和。

JavaScript 示例代码

```js
function maxSum(arr, k) {
  const n = arr.length
  let maxSum = Number.MIN_SAFE_INTEGER;

  for (let i = 0; i < n - k + 1; i++) {
    let currentSum = 0;
    for (let j = 0; j < k; j++) {
      currentSum = currentSum + arr[i + j];
    }

    maxSum = Math.max(currentSum, maxSum);
  }

  return maxSum;
}
```

因为包含两个 for 循环，它的时间复杂度是 O（k * n）。

## 滑动窗口算法

滑动窗口算法可以将嵌套的循环问题，转换为单循环问题，降低时间复杂度。
根据示例，当 k 等于 2 时，我们维护一个长度为 2 的窗口。

- 窗口内的值的和保存在一个变量中；
- 通过不断的往右滑动来算出当前窗口的值，并与保存的最大值作比较；
- 当窗口滑动到最右边时终止滑动；

![滑动窗口](https://i.loli.net/2019/03/12/5c8691a4a0ae1.jpg)

JavaScript 示例代码

```js
function maxSum(arr, k) {
  const n = arr.length;
  if (n < k) {
    return -1;
  }

  // 计算出第一个窗口的值
  let maxSum = 0;
  for (let i = 0; i < k; i++) {
    maxSum += arr[i];
  }

  let sum = maxSum;
  for (let i = k; i < n; i++) {
    // 新窗口的和 = 前一个窗口的和 + 新进入窗口的值 - 移出窗口的值
    sum += arr[i] - arr[i - k];
    maxSum = Math.max(maxSum, sum);
  }

  return maxSum;
}
```

我们在一个循环中计算出了长度为 'k' 的子数组的最大总和，它的时间复杂度是 O（n）。我们可以使用滑动窗口算法解决 查找最大/最小k子阵列，XOR，乘积，求和等问题。

# 示例2

给定一个字符串 S 和一个字符串 T，请在 S 中找出包含 T 所有字母的最小子串。(minimum-window-substring)

```
输入: S = "ADOBECODEBANC", T = "ABC"
输出: "BANC"
```

这个问题让我们无法按照示例 1 中的方法进行查找，因为它不是给定了窗口大小让你找对应的值，而是给定了对应的值，让你找最小的窗口。

我们仍然可以使用滑动窗口算法，只不过需要换一个思路。

既然是找最小的窗口，我们先定义一个最小的窗口，也就是长度为 0  的窗口。

![长度为 0  的窗口](https://i.loli.net/2019/03/12/5c8691a49e55a.jpg)

我们比较一下当前窗口在的位置的字母，是否是 T 中的一个字母。

很明显， A 是 ABC 中的一个字母，也就是 T 所有字母的最小子串 可能包含当前位置的 S 的值。

如果包含，我们开始扩大窗口，直到扩大后的窗口能够包含 T 所有字母。

![AD 不能够包含 T 所有字母](https://i.loli.net/2019/03/12/5c8691a499333.jpg)

![ADOBEC 包含 T 的所有字母](https://i.loli.net/2019/03/12/5c8691a495307.jpg)

假设题目是 **在 S 中找出包含 T 所有字母的第一个子串**，我们就已经解决问题了，但是题目是找到最小的子串，就会存在一些问题。

- 当前窗口内可能包含了一个更小的能满足题目的窗口
- 窗口没有滑动到的位置有可能包含了一个更小的能满足题目的窗口

为了解决可能出现的问题，当我们找到第一个满足的窗口后，就从左开始缩小窗口。

- 如果缩小后的窗口仍满足包含 T 所有字母的要求，则当前窗口可能是最小能满足题目的窗口，储存下来之后，继续从左开始缩小窗口。
- 如果缩小后的窗口不能满足包含 T 所有字母的要求，则缩小窗口停止，从右边开始扩大窗口。

![DOBEC 不包含 T 的所有字母](https://i.loli.net/2019/03/12/5c8691a48b750.jpg)

![DOBECODEBA 包含 T 的所有字母](https://i.loli.net/2019/03/12/5c8691a47dbd9.jpg)

不断重复上面的步骤，直到窗口滑动到最右边，且找不到合适的窗口为止。最小满足的窗口就是我们要找的 S 中包含 T 所有字母的最小子串。

![BANC 包含 T 的所有字母，且是最小子串](https://i.loli.net/2019/03/12/5c8691a47bfad.jpg)

JavaScript 示例代码

```js
/**
 * @param {string} s
 * @param {string} t
 * @return {string}
 */
var minWindow = function(s, t) {
  const map = {};
  for (let i = 0; i < t.length; i++) {
    if (map[t[i]]) {
      map[t[i]]++;
    } else {
      map[t[i]] = 1;
    }
  }

  let left = 0;
  let right = 0;
  let count = t.length;
  let max = Number.MAX_SAFE_INTEGER;
  let res = s;

  while (right < s.length) {
    if (map[s[right]] > 0) {
      count--;
    }
    map[s[right]]--;
    right++;



    while (count === 0) {
      if (right - left < max) {
        max = right - left;
        res = s.slice(left, right);
      }
      map[s[left]]++;
      if (map[s[left]] > 0) {
        count++;
      }
      left++;
    }
  }
  return max === Number.MAX_SAFE_INTEGER ? "" : res;
};
```

它的时间复杂度是 O（s + t）

# 示例3

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。(longest-substring-without-repeating-characters)

```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
和示例 2 相似，我们不断的扩大/缩小窗口，把无重复字母的窗口大小保存下来，直到窗口滑动结束，就找到了不含有重复字符的 最长子串 的长度。
```

JavaScript 示例代码

```js
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
  const map = {};

  let left = 0;
  let right = 0;

  let max = 0;
  while (right < s.length) {
    if (!map[s[right]]) {
      map[s[right]] = 1;
      right++;
    } else {
      while (left < right) {
        delete map[s[left]];
        if (map[s[left++]] === map[s[right]]) {
          break;
        }
      }
    }

    max = Math.max(max, right - left);
  }

  return max;
};
```

时间复杂度是 O（s + s）

# 示例4
给定一个字符串 s 和一个非空字符串 p，找到 s 中所有是 p 的字母异位词的子串，返回这些子串的起始索引。(find-all-anagrams-in-a-string)

```
输入:
s: "cbaebabacd" p: "abc"

输出:
[0, 6]

解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的字母异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的字母异位词。
```

与示例 1 类似，我们维护一个长度为 p 的窗口，然后不断往右滑动查找当前窗口是否为 p 的字母异位词。

```js
var findAnagrams = function(s, p) {
  const map = new Array(26).fill(0);

  for (let i = 0; i < p.length; i++) {
    map[p[i].charCodeAt() - 97]++;
  }

  const r = [];
  for (let i = 0, j = 0; i < s.length; i++) {
    const c = s[i].charCodeAt() - 97;
    map[c]--;
    while (map[c] < 0) {
      const c2 = s[j].charCodeAt() - 97;
      j++;
      map[c2]++;
    }
    if (i - j + 1 === p.length) {
      r.push(j);
    }
  }

  return r;
};
```

时间复杂度为 O（s + p）

# 滑动窗口算法的应用

TCP 流量控制


# 参考：
Window Sliding Technique - GeeksforGeeks
https://www.youtube.com/watch?v=eS6PZLjoaq8&t=548s