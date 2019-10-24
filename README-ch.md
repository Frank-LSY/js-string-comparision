# String Comparison



**[tdebatty/java-string-similarity](https://github.com/tdebatty/java-string-similarity)的 JavaScript 扩展版本**

本仓库主要利用JavaScript实现关于字符串的相似性、距离以及排序等方法。包括算法有:莱温斯坦距离、最长公共子序列、余弦相似性等。详细内容参见如下：


  - [下载&使用](#下载--使用)
  - [概览](#概览)
  - [规范化、度量、相似标准和距离](#normalized-metric-similarity-and-distance)
    - [规范化的字符串相似性和距离](#normalized-similarity-and-distance)
  - [莱温斯坦距离](#levenshtein)
  - [最长公共子序列](#longest-common-subsequence)
  - [最长公共子序列度量](#metric-longest-common-subsequence)
  - [余弦相似性](#cosine-similarity)
  - [Dice系数](#sorensen-dice-coefficient)
  - [API](#api)
    - [相似性](#similarity)
      - [参数(params)](#params)
      - [返回值(return)](#return)
    - [距离](#distance)
      - [参数(params)](#params-1)
      - [返回值(return)](#return-1)
    - [排序](#sortmatch)
      - [参数(params)](#params-2)
      - [返回值(return)](#return-2)
      - [参数(params)](#params)
      - [返回值(return)](#return)
  - [发布摘要](#release-notes)
    - [1.x version](#1x-version)
  - [MIT](#mit)


## 下载 & 使用

下载

```shell
npm install string-comparision --save
yarn add string-comparision
```
使用

```js
let stringComparision = require('string-comparision')

const Thanos = 'healed'
const Rival = 'sealed'
const Avengers = ['edward', 'sealed', 'theatre']

use by Consine
let cos = stringComparision.consine

console.log(cos.similarity(Thanos, Rival))
console.log(cos.distance(Thanos, Rival))
console.log(cos.sortMatch(Thanos, Avengers))

```

## 概览

The main characteristics of each implemented algorithm are presented below. The "cost" column gives an estimation of the computational cost to compute the similarity between two strings of length m and n respectively.

|                                                                                                                                      | 支持方法                              | 规范化? | 度量? | 形式    | 复杂度   | 典型应用   |
| ------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------- | ----------- | ------- | ------- | ------ | --------------- |
| [雅卡尔系数](https://github.com/luozhouyang/python-string-similarity/blob/master/README.md#jaccard-index)                         | 相似性<br/>距离<br />排序比较  | Yes✅         | Yes✅     | Set     | O(m+n) |                 |
| [余弦相似性](https://github.com/luozhouyang/python-string-similarity/blob/master/README.md#cosine-similarity)                 | 相似性<br/>距离<br />排序比较  | Yes✅         | No❌      | Profile | O(m+n) |                 |
| [Dice系数](https://github.com/luozhouyang/python-string-similarity/blob/master/README.md#sorensen-dice-coefficient) | 相似性<br/>距离<br />排序比较   | Yes✅         | No❌      | Set     | O(m+n) |                 |
| [莱温斯坦距离](https://github.com/luozhouyang/python-string-similarity/blob/master/README.md#levenshtein)                             | 相似性<br/>距离<br />排序比较 sortMatch | No❌          | Yes✅     |         | O(m*n) |                 |
| [Jaro-Winkler距离](https://github.com/luozhouyang/python-string-similarity/blob/master/README.md#jaro-winkler)                           | 相似性<br/>距离<br />排序比较       | Yes✅         | No❌      |         | O(m*n) | 拼写检查 |