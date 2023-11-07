+++
title = 'Javascript Sleep'
date = 2023-11-07T18:34:56+08:00
draft = true
+++

javascript 无 sleep函数

用Promise

```javascript
const sleep = async(duration)=>new Promise(resolve=>setTimeout(resolve, duration))
```