---
title: js-str2bool
date: 2020-03-31 19:52:18
tags: js
---
# JS中false字符串转boolean

+ js  false 认定场景

```js
var myBoolean=new Boolean(); 
//下面的所有的代码行均会创建初始值为 false 的 Boolean 对象。
var myBoolean=new Boolean();
var myBoolean=new Boolean(0);
var myBoolean=new Boolean(null);
var myBoolean=new Boolean("");
var myBoolean=new Boolean(false);//不带单引号的是false
var myBoolean=new Boolean(NaN);

```



+ js true 认定场景

```js
//下面的所有的代码行均会创初始值为 true 的 Boolean 对象：
var myBoolean=new Boolean(1);
var myBoolean=new Boolean(true);
var myBoolean=new Boolean("true");
var myBoolean=new Boolean("false");//带单引号的字符串false最终等于true
var myBoolean=new Boolean("Bill Gates");
```

**var myBoolean=new Boolean("false");//带单引号的字符串false最终等于true**



字符串的false/true  转 bool可以用下面代码

```js
str !== "false"
```

