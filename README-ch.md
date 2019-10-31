<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

# 字符串比较 String Comparison  



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

###下载

```shell
npm install string-comparision --save
yarn add string-comparision
```
###使用

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
| [雅卡尔系数](#雅卡尔系数)                         | 相似性<br/>距离<br />排序比较  | Yes✅         | Yes✅     | Set     | O(m+n) |                 |
| [余弦相似性](#余弦相似性)                 | 相似性<br/>距离<br />排序比较  | Yes✅         | No❌      | Profile | O(m+n) |                 |
| [Dice系数](https://github.com/luozhouyang/python-string-similarity/blob/master/README.md#sorensen-dice-coefficient) | 相似性<br/>距离<br />排序比较   | Yes✅         | No❌      | Set     | O(m+n) |                 |
| [莱温斯坦距离](https://github.com/luozhouyang/python-string-similarity/blob/master/README.md#levenshtein)                             | 相似性<br/>距离<br />排序比较 | No❌          | Yes✅     |         | O(m*n) |                 |
| [Jaro-Winkler距离](https://github.com/luozhouyang/python-string-similarity/blob/master/README.md#jaro-winkler)                           | 相似性<br/>距离<br />排序比较       | Yes✅         | No❌      |         | O(m*n) | 拼写检查 |


## 规格化（单位）、度量、相似性和距离
很简单而且常见的问题，但总体来说我们有许多的算法来衡量文本之间的相似性和距离，我们对于他们做了一些总结归纳，并给出了一些接口。


### 规格化（单位）字符串相似性和距离

- 字符串相似性：一些算法定义了字符串之间的相似性，当数值为0时代表两字符串完全不同。
- 单位字符串相似性：将字符串相似性数值映射到[0.0,1.0]空间，典型算法：[Jaro-Winkler算法](#Jaro-Winkler算法)。
- 字符串距离：一些算法定义字符串之间的距离，当数值为0时代表两字符串完全相同，最大距离的取值依照于算法。典型算法：[莱温斯坦距离算法](#莱温斯坦算法)。
- 单位字符串距离：该接口为字符串距离(StringDistance)的扩展，计算所得距离在[0.0,1.0]空间内。典型算法：[规格化莱温斯坦距离算法]()。


## 算法简介

### 雅卡尔系数

- 雅卡尔指数（英语：Jaccard index），又称为并交比（Intersection over Union）、雅卡尔相似系数（Jaccard similarity coefficient），是用于比较样本集的相似性与多样性的统计量。雅卡尔系数能够量度有限样本集合的相似度，其定义为两个集合交集大小与并集大小之间的比例：
$$J(A,B)=\frac{|A \cap B|}{|A \cup B|}~=\frac{|A \cap B|}{|A|+|B|-|A \cap B|}$$
- 如果A与B完全重合，则定义J(A,B)=1。于是有 
$$0\leq J(A,B)\leq 1$$
- 雅卡尔距离是所有有限样本集合间的度量。

### 余弦相似性

- 余弦相似性通过测量两个向量的夹角的余弦值来度量它们之间的相似性。0度角的余弦值是1，而其他任何角度的余弦值都不大于1；并且其最小值是-1。从而两个向量之间的角度的余弦值确定两个向量是否大致指向相同的方向。两个向量有相同的指向时，余弦相似度的值为1；两个向量夹角为90°时，余弦相似度的值为0；两个向量指向完全相反的方向时，余弦相似度的值为-1。这结果是与向量的长度无关的，仅仅与向量的指向方向相关。余弦相似度通常用于正空间，因此给出的值为0到1之间。
- 二维向量余弦夹角计算：
$$\vec{a} = (x1,y1)$$
$$\vec{b}=(x2,y2)$$
$$cos\theta=\frac{\vec{a}*\vec{b}}{|\vec{a}| * |\vec{b}|}=\frac{x1x2+y1y2}{\sqrt{x1^{2}+y1^{2}} * \sqrt{x2^{2}+y2^{2}}}$$
- n维向量余弦夹角计算：
$$cos\theta=\frac{\vec{a}*\vec{b}}{|\vec{a}| * |\vec{b}|}=\frac{\sum_{i=1}^{n}xiyi}{\sqrt{ \sum_{i=1}^{n}xi^{2} } * \sqrt{ \sum_{i=1}^{n}yi^{2}} }$$
- 如何将字符转化为向量？参考 [文本相似度之余弦夹角度量算法](https://www.jianshu.com/p/5619e73e1322)
	- 第一步 分词
		- ```字串A：这只/皮靴/号码/大了。那只/号码/合适。```
		- ```字串B：这只/皮靴/号码/不/小，那只/更/合适。```
	- 第二步 列出两字串并集所有词
		- ```这只，皮靴，号码，大了。那只，合适，不，小，更```
	- 第三步 计算词频
		- ```字串A：这只1，皮靴1，号码2，大了1。那只1，合适1，不0，小0，更0```
		- ```字串B：这只1，皮靴1，号码1，大了0。那只1，合适1，不1，小1，更1```
	- 第四步 写出词频向量
		- ```字串A：(1，1，2，1，1，1，0，0，0)```
		- ```字串B：(1，1，1，0，1，1，1，1，1)```
	- 到这里，问题就变成了如何计算这两个向量的相似程度。我们可以把它们想象成空间中的两条线段，都是从原点（[0, 0, ...]）出发，指向不同的方向。两条线段之间形成一个夹角，如果夹角为0度，意味着方向相同、线段重合,这是表示两个向量代表的文本完全相等；如果夹角为90度，意味着形成直角，方向完全不相似；如果夹角为180度，意味着方向正好相反。因此，我们可以通过夹角的大小，来判断向量的相似程度。夹角越小，就代表越相似。



### Dice系数
<img src="http://latex.codecogs.com/gif.latex?cos\theta=\frac{\vec{a}*\vec{b}}{|\vec{a}| * |\vec{b}|}=\frac{x1x2+y1y2}{\sqrt{x1^{2}+y1^{2}} * \sqrt{x2^{2}+y2^{2}}}>


### 莱温斯坦算法
### Jaro-Winkler算法

