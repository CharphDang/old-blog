---
title: karma 前端单元测试工具
date: 2021-01-25 11:33:24
tags:
categories:
---

- 支持es6的语法检测及其覆盖率

  - `npm install karma-coverage-istanbul-instrumenter karma-coverage-istanbul-reporter -D`

  - karma.conf.js

    ```js
    preprocessors: {
        'webapp/controller/*.js': ['karma-coverage-istanbul-instrumenter'] // 可以识别这些文件的es6语法
    },
    coverageIstanbulInstrumenter: {
        esModules: false
    },
    reporters: ['progress', 'coverage-istanbul'],
    coverageIstanbulReporter: {
        dir: './target/coverage',
        includeAllSources: true,
        // 生成的是xml，cobertura改为html将生成html文件的覆盖率报告
        reports: ['cobertura', 'lcovonly', 'text-summary'] 
    },
    plugins: ['karma-*']
    ```

    

