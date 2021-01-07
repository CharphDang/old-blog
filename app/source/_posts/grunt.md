---
title: grunt
date: 2019-08-27 22:54:08
tags: 
- 自动化构建
categories:
- 自动化构建
---

# grunt

1. 安装环境

   - npm init -y

   - grunt `npm install grunt --save-dev`

   - grunt-cli `npm install grunt-cli -g`, 这里我是全局安装grunt-cli

     

2. 配置文件

   - 在package.json同级目录下，创建`Gruntfile.js`文件

     ```js
     module.exports = function(grunt) {
       // Do grunt-related things in here
     };
     ```

3. 为工程增加babel转义功能

   - 为何要这么做？

     - 由于现代浏览器对es6+的语法支持度不够高，对于...解构、async/await 等无法识别，所以需要将我们的代码编译为浏览器认识的代码（方便我们使用最新的语法去编写代码，使代码更清晰明了高大上）

   - 安装依赖

     - `npm install --save-dev grunt-babel @babel/core`
     - `npm install @babel/preset-env --save-dev`

   - 配置文件

     - Gruntfile.js

       ```js
       require("load-grunt-tasks")(grunt); // npm install --save-dev load-grunt-tasks
       
       grunt.initConfig({
         "babel": {
           options: {
             sourceMap: false,
             presets: [
               [
                 '@babel/preset-env',
                 {
                   targets: {
                     browsers: ['> 1%', 'last 2 versions', 'not ie <= 8']
                   }
                 }
               ]
             ]
           },
           dist: {
             files: {
               "dist/app.js": "src/app.js"
             }
           }
         }
       });
       
       grunt.registerTask("default", ["babel"]);
       ```

       ```
       grunt.loadNpmTasks('grunt-babel');
       ```

       

     - `@babel/core` 是实际用来转义es6+代码的

     - `@babel/preset-env`是做转义预设，指定转义规则的

4. 为环境增加全局变量`babel-polyfill`

   - 为何要这样做？

     - 当babel转义后，其实还有一些东西无法转义，比如es6+中的全局变量中的Promise, Object.assign()方法等，这就需要我们对当前环境进行一个polyfill，填充他们缺少的功能。（如果不填充，浏览器如果不认识Promise，那肯定运行的时候报错噻）

   - 安装依赖

     - `npm install --save-dev @babel/polyfill`

   - 配置文件

     - Gruntfile.js

       ```js
       presets: [
               [
                 '@babel/preset-env',
                 {
                   targets: {
                     browsers: ['> 1%', 'last 2 versions', 'not ie <= 8']
                   },
                   modules: 'commonjs', // 选项用于模块转化规则设置，可选配置包括："amd" | "umd" | "systemjs" | "commonjs" | false, 默认使用 "commonjs"。即，将代码中的ES6的import转为require。
                   corejs: '3', // 声明corejs版本
                   useBuiltIns: 'usage' // 不用显示引用@babel/polyfill
                 }
               ]
             ]
       ```

       

5. 为环境增加browserify功能

   - 为何这样做

     - 我们会发现babel转义出来的代码里面，会有require()/import语法，这都是引入的polyfill，但是浏览器不认识require() 这种commonJs 的CMD语法，也不认识import/exports 等语法，必须转换为浏览器认识的。
     - browserify默认是将require进行转换。
     - 对前端define的转换还不知道怎么配置。

   - 安装依赖

     - `npm install --save-dev browserify`
     - `npm install --save-dev grunt-browserify`

   - 配置文件

     - Gruntfile.js

       ```js
       // 让浏览器适配require(commonJS,CMD)
          browserify: {
             dist: {
               files: [
                 {
                   expand: true,
                   src: ['**/*.js'], //所有js文件
                   dest: tmpDirBabel, //输出到此目录下
                   cwd: tmpDirBabel //js目录下
                 }
               ]
             }
           }
       ```

       ```
       grunt.loadNpmTasks('grunt-browserify');
       ```

6. 配置压缩功能

   - 为何这样做？

     - js文件进行压缩体积，优化方向

   - 安装依赖

     - `npm install grunt-contrib-uglify --save-dev`

   - 配置文件

     - Gruntfile.js

       ```js
       // 自定义压缩
           uglify: {
             options: {
               mangle: true, //混淆变量名
               comments: 'false' //false（删除全部注释），some（保留@preserve @license @cc_on等注释）
             },
             uglifyFile: {
               files: [
                 {
                   expand: true,
                   src: ['**/*.js'], //所有js文件
                   dest: tmpDirBabel, //输出到此目录下
                   cwd: tmpDirBabel //js目录下
                 }
               ]
             }
           }
       ```

       ```js
       grunt.loadNpmTasks('grunt-contrib-uglify');
       ```

7. 配置监听

   - 为何这么做？

     - 当文件发生更改时，需要做的一些任务，通常配置dev环境，比如less的编译， 本地js的转义等

   - 安装依赖

     - `npm install grunt-contrib-watch grunt-contrib-livereload --save-dev`

   - 配置文件

     - Gruntfile.js

       ```js
       watch: {
             js: {
               files: ['localApp/**/*.js'], //所有js
               tasks: [
                 'babel',
                 'browserify'
               ],
               options: {
                 interrupt: true, // 如果文件再次更改，则停止上一次的任务，重新构建新的
                 livereload: true
               }
             },
             others: {
               files: ['localApp/**/*', '!localApp/**/*.js'], // 除了js
               tasks: ['copy:copyOthersToWebApp'],
               options: {
                 interrupt: true,
                 livereload: true
               }
             }
           }
       
       ```

       ```js
        grunt.loadNpmTasks('grunt-contrib-watch');
        grunt.loadNpmTasks('grunt-contrib-livereload');
       
       ```

8. 加载npm任务

   ```js
     grunt.loadNpmTasks('grunt-babel');
   
   ```

   

9. 注册自定义任务

   ```js
   grunt.registerTask('dev', ['watch']);
   
   ```

   