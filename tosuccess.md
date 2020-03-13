**导语** 

*成长之路*: 不断在错误中让自己变得更加强大,多总结,多记录.

## webpack配置相关

### 1. SASS-LOADER: 版本问题

  **Module build failed:  TypeError:  this.getResolve is not a function 是因为sass-loader的版本过高导致的编译错误**

解决方式: 

1. sass-loader当前最高版本是8.x，需要退回到7.3.1

   

```javascript
npm uninstall sass-loader（卸载当前版本）
npm install sass-loader@7.3.1 --save-dev  （安装7.x版本）
```

​             2.找到webpack.base.config.js配置scss匹配规则

```javascript
module:{
    rules:[//第三方匹配规则
        {test:/\.scss$/,use:['style-loader','css-loader','sass-loader']},
    ]
},
```

### 2.CSS-LOADER 问题

```java
Module build failed:
/**
^
      Invalid CSS after "...load the styles": expected 1 selector or at-rule, was "var content = requi"
      in E:\Youku Files\myadmin\myadmin\src\styles\element-variables.scss (line 1, column 1)

 @ ./src/main.js 11:0-41
 @ multi (webpack)-dev-server/client?http://0.0.0.0:8080 webpack/hot/dev-server ./src/main.js

 error  in ./src/styles/index.scss

Module build failed:
@import './variables.scss';
^
      Invalid CSS after "...load the styles": expected 1 selector or at-rule, was "var content = requi"
      in E:\Youku Files\myadmin\myadmin\src\styles\index.scss (line 1, column 1)

 @ ./src/main.js 9:0-29
 @ multi (webpack)-dev-server/client?http://0.0.0.0:8080 webpack/hot/dev-server ./src/main.js
```

这部分错误是说   **vue-cli的坑，loader重复的锅 Invalid CSS after "...load the styles"**   

因为在使用scss时,我们难免会主动地去添加相关的loader,例如:

```javascript
{
    test: /\.scss$/,
    use: ['style-loader', 'css-loader', 'sass-loader'],
  }
```

然而当使用vue-cli脚手架创建项目时，可能出现如下错误：

```
Invalid CSS after "...load the styles": expected 1 selector or at-rule
```

这是loader重复导致的，在build/utils.js中的exports.cssLoaders中已经返回相关loader了，

```javascript
css: generateLoaders(),
    postcss: generateLoaders(),
    less: generateLoaders('less'),
    sass: generateLoaders('sass', { indentedSyntax: true }),
    scss: generateLoaders('sass'),
    stylus: generateLoaders('stylus'),
    styl: generateLoaders('stylus')
```

解决方案很简单：

​      就是自己不要加样式相关的loader了；
​      或者自己添加样式的loader但要注释掉这两行，当然如果你用的是less就注释掉        的行就可以了。

### 3.axios的二次封装以及项目中开发生产环境的接口配置