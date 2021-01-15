---
title: axios
date: 2021-01-15 11:01:48
tags:
- axios
- catch
categories:
- axios
- catch
---

## 问题： 在使用axios时，如何捕获错误 ？

1. 回调写法： 使用自身的catch方法

   ```js
   axios.get(`/vue/table`, {
       params: {
           name: this.name
       }
   }).then((data)=>{
       // handle result
   }).catch(err=>{
       // handle error
   });
   ```

2.  使用async/await： 两种选择，不同处理

   1.  使用自身的catch回调捕获

      ```js
      async _getTableDataFun() {
              const { data } = await axios.get(`/vue/table`, {
                  params: {
                      name: this.name
                  }
              }).catch(err=>{
                  return {
                      data: [] 
                  }
              });
          	this.tableData = data;
      }
      ```

      > catch 中的return 返回这一句很重要，非常重要，由于promise的链式操作，我们捕获了错误之后，await真正返回的结果就是catch中的返回内容， 我们有对返回结果进行解构 `const  { data } = ...` , 如果我们catch中不返回相应的结构，结构就会失败报错。

   2. 使用try/catch 捕获

      ```js
      async _getTableDataFun() {
          try {
              const { data } = await axios.get(`/vue/table`, {
                  params: {
                      name: this.name
                  }
              });
              this.tableData = data;
          } catch (error) {
              console.log('err');
          }
      }
      ```

      > 使用try/catch 进行捕获时，不论是结构失败报错，还是axios自己请求http的报错，都会被catch捕获到



个人总结： 不论使用哪种方式捕获错误，目的都是对已知的某种错误场景进行正确的处理，而不是将错误直接抛给浏览器。