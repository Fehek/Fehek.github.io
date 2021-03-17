---
title: web前端学习笔记15——Vue源码分析
subtitle: Vue源码分析
abbrlink: c12f9186
tags:
  - web前端
  - Vue
categories: web前端学习笔记
date: 2021-03-16 09:46:00
index_img:
banner_img:
---



# 函数实现
```js
//监控obj对象的变化，在变化后调用callback，暂时只考虑对象只有一层(watch)
//方法是通过把obj的每个属性都改成getter，setter，在setter调用时，调用callback
function observe(obj, callback) {
  for (let key in obj) {
    let val = obj[key]
    Object.defineProperty(obj, key, {
      get: function () {
        return val
      },
      set: function (newVal) {
        if (newVal === val) {
          return
        }
        val = newVal
        callback()
      }
    })
  }
}

var obj = { a: 1, b: 2 }
observe(obj, function () {
  console.log('obj changed')
})
```
```js
// 对象有多层
function observe(obj, callback) {
  if (obj && typeof obj == 'object') {
    for (let key in obj) {
      let val = obj[key]
      observe(val, callback)

      Object.defineProperty(obj, key, {
        get: function () {
          return val
        },
        set: function (newVal) {
          if (newVal === val) {
            return
          }
          val = observe(newVal, callback)
          callback()
        }
      })
    }
  }
  return obj
}
```
