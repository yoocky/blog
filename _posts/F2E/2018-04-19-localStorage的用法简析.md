---
layout:     post
title:      localStorage的用法简析
date:       2018-04-19 12:12:00
summary:    localStorage是一个
categories: linux
---


HTML5 提供了两种在客户端存储数据的新方法：
* localStorage - 没有时间限制的数据存储
* sessionStorage - 针对一个 session 的数据存储
之前，这些都是由 cookie 完成的。但是 cookie 不适合大量数据的存储，因为它们由每个对服务器的请求来传递，这使得 cookie 速度很慢而且效率也不高。
在 HTML5 中，数据不是由每个服务器请求传递的，而是只有在请求时使用数据。它使在不影响网站性能的情况下存储大量数据成为可能。
对于不同的网站，数据存储于不同的区域，并且一个网站只能访问其自身的数据。今天主要对localStorage的用法做一下简析。

#### 属性值的增删查
##### 增加属性值

    localStorage.setItem('test', 'hello word');
    localStorage.test = 'hello word';
    localStorage['test'] = 'hello word';

##### 自定义属性的获取

    console.log(localStorage.getItem('test'))
    console.log(localStorage.test)
    console.log(localStorage['test'])


##### 删除属性值
  * 删除单个属性

      localStorage.removeItem('test');
      delete localStorage.test

  * 删除所有自定义属性

      localStorage.clear()
    

#### 属性值的监听

    window.addEventListener('storage', function (e) {
          console.log('key', e.key);
          console.log('oldValue', e.oldValue);
          console.log('newValue', e.newValue);
          console.log('url', e.url);
      })

  注意：只能在同终端同源的的夸浏览器窗口的页面才能监听到，当天页面是不会触发storage事件的

#### 存储时间不受限制

  除非用户主动删除，没有过期时间的的，
  1  合理分配命名空间，防止脏数据污染，和key名的冲突
  2  不要存储用户的隐私数据，如需存储尽量用sessionStorage
  ...

#### 值的数据类型

  只能存储String的值，非string类型的都会调用存储对象的toString()方法进行转换
  #### test

  * null 
    
      localStorage.testNull = null; 
      console.log(localStorage.testNull)
      console.log(typeof localStorage.testNull)

  * Booloon 

      localStorage.testBool = true; 
      console.log(localStorage.testBool)
      console.log(typeof localStorage.Bool)

  * Number

      localStorage.testNum = 1;    
      console.log(localStorage.testNum)
      console.log(type of localStorage.testNum)
  
  * Array

      localStorage.testArray = ['a', 'b', 'c'];
      console.log(localStorage.testArray)
      console.log(type of localStorage.testArray)

  * Object

      localStorage.testObj = {};
      console.log(localStorage.testObj)
      console.log(type of localStorage.testObj)
      
  * toString

      localStorage.testTransfer = {toString() { return 'hellword'}}    // 调用了toString()方法
      console.log(type of localStorage.testTransfer)

#### 同源策略

  * 同源策略，它是由Netscape提出的一个著名的安全策略。
  * 现在所有支持JavaScript 的浏览器都会使用这个策略。
  * 所谓同源是指，域名，协议，端口相同。
  * 同源策略 ajax, sessionStorage, localStorage
  * src 载入的资源不受同源策略限制

#### 存储空间​​​​​​​（2M安全）

  安全大小2M，非持久化建议使用sessionStorage

    localStorage.testSize = '0'.repeat(1024*1024*5) //最新的Chrome
    localStorage.testSize = '0'.repeat(1024*1024*2) //最新的Safari