---
title: webdriverio-cucumber
date: 2020-10-20 00:14:41
tags:
- e2e
- webdriverio
- cucumber
categories:
- e2e
- webdriverio
- cucumber
---

# webdriverIO-cucumber

[博客地址](https://github.com/dangchaofeng/webdriverIO-cucumber) 【直接下载，`npm install` ， `npm run test` 即可快速开始进入demo code】
> 本用例使用的是webdriverIo基本的cli架构，然后自定义使用cucumber来进行e2e的语法定义以及文档输出。

- 开场提示： 
  - 对node有版本要求， 10.14.0+
  - 如果chromDriver下载失败，可能就需要翻墙了
  - 案例中的网址，是需要翻墙才能访问的

- [webdriverio参考](https://webdriver.io/docs/gettingstarted.html)
- [cucumber基本语法入门](https://www.jianshu.com/p/3857f2c3a8d4)
- `Expect `强烈建议使用webdriverio内置的，不需要额外加载chai的断言库，因为这是e2e基于浏览器的实现，很多基于DOM操作的内容，使用内置的更加方便合理，使用chai反而增加了复杂度
- 为何不使用selenium-standalone-service
  - 其一：个人学习使用的话，本地基于chromedriver浏览器足以
  - 其二：selenium-standalone-service 启动的时候，会比webdriver卡很多(也有可能是本人电脑配置过低导致，但总体来说，个人练习足够)
- 什么时候使用selenium-standalone-service
  - 当你真正项目中使用e2e的时候，selenium-standalone提供良好浏览器的支持。
    - [ChromeDriver](https://github.com/SeleniumHQ/selenium/wiki/ChromeDriver)
    - [FirefoxDriver](https://github.com/SeleniumHQ/selenium/wiki/FirefoxDriver)
    - [IEDriver](https://github.com/SeleniumHQ/selenium/wiki/InternetExplorerDriver)
    - [Edge WebDriver](https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/#downloads)
    - [Chromium Edge WebDriver](https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/#downloads)
- 你也可以根据公司的实际需求使用其他的service,eg: `sauce` 

- 为什么使用cucumber

  - 来看看最最简单的实用语法示例

    ```feature
    Feature: Login Feature
    
        Scenario: My Login application
    
            Given I open my loginpage
            When I login into my account
            Then Something is exit
    ```

  - 上面可以看出，语法的三大要素很明确

    - Given 给定一个初始场景
    - When 给定一个条件
    - Then 开始进行结果断言

  - 这就是理由！