# String Comparison



**[tdebatty/java-string-similarity](https://github.com/tdebatty/java-string-similarity)的 JavaScript 扩展版本**

本仓库主要利用JavaScript实现关于字符串的相似性、距离以及排序等方法。包括算法有:莱温斯坦距离、最长公共子序列、余弦相似性等。详细内容参见如下：


  - [下载&使用](#下载--使用)
  - [概览](#概览)
  - [规格化（单位）、度量、相似标准和距离](#规范化、度量、相似性和距离)
    - [规格化(单位)字符串相似性和距离](#normalized-similarity-and-distance)
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
对于给出的算法，他们一些特性见下表：

ps:对于复杂度一列，我们给出的是计算两个长度分别为m和n的字串的相似性的时间复杂度。

|                                                                                                                                      | 支持方法                              | 单位（规格化）? | 度量? | 形式    | 复杂度   | 典型应用   |
| ------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------- | ----------- | ------- | ------- | ------ | --------------- |
| [雅卡尔系数](https://github.com/luozhouyang/python-string-similarity/blob/master/README.md#jaccard-index)                         | 相似性<br/>距离<br />排序比较  | Yes✅         | Yes✅     | Set     | O(m+n) |                 |
| [余弦相似性](https://github.com/luozhouyang/python-string-similarity/blob/master/README.md#cosine-similarity)                 | 相似性<br/>距离<br />排序比较  | Yes✅         | No❌      | Profile | O(m+n) |                 |
| [Dice系数](https://github.com/luozhouyang/python-string-similarity/blob/master/README.md#sorensen-dice-coefficient) | 相似性<br/>距离<br />排序比较   | Yes✅         | No❌      | Set     | O(m+n) |                 |
| [莱温斯坦距离](https://github.com/luozhouyang/python-string-similarity/blob/master/README.md#levenshtein)                             | 相似性<br/>距离<br />排序比较 | No❌          | Yes✅     |         | O(m*n) |                 |
| [Jaro-Winkler距离](https://github.com/luozhouyang/python-string-similarity/blob/master/README.md#jaro-winkler)                           | 相似性<br/>距离<br />排序比较       | Yes✅         | No❌      |         | O(m*n) | 拼写检查 |


## 规格化（单位）、度量、相似性和距离
很简单而且常见的问题，但总体来说我们有许多的算法来衡量文本之间的相似性和距离，我们对于他们做了一些总结归纳，并给出了一些接口。


### 规格化（单位）字符串相似性和距离

- 字符串相似性：一些算法定义了字符串之间的相似性，当数值为0时代表字符串完全不同。
- 单位字符串相似性：将字符串相似性数值映射到[0.0,1.0]空间，典型算法：Jaro-Winkler算法。
- 字符串距离：一些算法定义字符串之间的距离，当数值为0时代表两字符串完全相同，最大距离的取值依照于算法。典型算法：莱温斯坦距离算法。
- 单位字符串距离：该接口为字符串距离(StringDistance)的扩展，计算所得距离在[0.0,1.0]空间内。典型算法：规格化莱温斯坦距离算法。

