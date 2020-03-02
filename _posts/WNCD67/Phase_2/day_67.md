title: Java x D67 | JavaScript
date: 2020-02-21
updated: 
comments: true
tags:
  - learn
  - JavaScript
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">homework</span>
综合应用高阶函数、箭头函数等对DOM进行排序
{% endcq %}
<!--more-->

# 综合应用高阶函数、箭头函数等对DOM进行排序
[廖雪峰老师的原题](https://www.liaoxuefeng.com/wiki/1022910821149312/1026155949848768)
题目：将下面的五种编程语言按字符顺序进行重新排序
```html
<ol id="test-list">
    <li class="lang">Scheme</li>
    <li class="lang">JavaScript</li>
    <li class="lang">Python</li>
    <li class="lang">Ruby</li>
    <li class="lang">Haskell</li>
</ol>
```

解:
```javascript
`use strict`;
var arr = document.getElementsByClassName('lang'); // 这里获取到的 arr 是个伪数组，只有 length 属性和访问方法 []
arr = Array.from(arr); // 转换为数组
arr.sort(
    (x,y) => { // 用箭头函数对数组进行排序
        return x.innerText.toLowerCase()<y.innerText.toLowerCase()?-1:1;
    }
);
arr.forEach( // 依次 append 到 test-list 的尾部
    (x) => {
        document.getElementById('test-list').appendChild(x);
    }
);
```