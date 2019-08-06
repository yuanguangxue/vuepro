npm + webpack + vue 开发演示demo
===============================
1. 新建一个目录vuepro<br>
2. 初始化： <br>
* 在命令行下cd到该目录下，执行：
`$ npm init`<br>
* 之后可以一路回车，在最后输入“yes”后会生成package.json文件
3. 然后执行命令 `$ npm install vue webpack babel-cli babel-loader babel-preset-es2015 html-webpack-plugin --save-dev`
4. 建好目录：
* html 放置模板文件，jssrc 放置js文件。 最终编译好的文件放置webapp ，这个目录也就是我们网站的目录。
5. 在项目根目录下创建webpack配置文件：webpack.config.js
```
var HtmlWebpackPlugin = require('html-webpack-plugin');
var webpack=require("webpack");
module.exports =
{
    entry:
    {
        //入口文件
        "index":__dirname+'/src/jssrc/index.js',
    },
    output: {
        path: __dirname+'/src/webapp/js',  //输出文件夹
        filename:'[name].js'   //最终打包生成的文件名(只是文件名，不带路径的哦)
    },
    /*resolve: {
        alias: {
            vue: 'vue/dist/vue.js'
        }
    },*/
    externals: {
 
    },
    module:{
    },
    plugins:[
        new HtmlWebpackPlugin({
            filename: __dirname+'/src/webapp/index.html',   //目标文件
            template: __dirname+'/src/html/index.html', //模板文件
            inject:'body',
            hash:true,  //代表js文件后面会跟一个随机字符串,解决缓存问题
            chunks:["index"]
        })
 
    ]
}
```
6. 同样在根目录下创建babel配置文件：.babelrc
```
{
    "presets" : ["es2015"]
} 
``` 
然后就可以在webpack里面配置loader，我们上面webpack配置中已经写了:
```
loaders:[
    {test:/\.js$/,loader:"babel",query:{compact:true}},
]
```
这句话的意思就是：凡是.js文件都使用babel-loader，并且压缩。
###今天我们学习vue最简单的一个套路
思考：数据如何渲染？<br> 
套路如下：<br> 
1.首先要有个数据块标记<br> 
vue里面可以像模板引擎一样写上{{name}}<br> 
其中name就是变量名
练习：<br> 
<img src="https://img-blog.csdn.net/20161012212648807" alt="这里写图片描述" title="" style="margin-top:24px;margin-bottom:24px;"><br/>
index.html如下：
```

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
    <div id="me">
        我的年龄是{{age}}
    </div>
</body>
</html>
```
index.js如下：
```angular2html
import Vue from "vue"; //会去node_modules\vue\package.json
 
new Vue({
    el:"#me",
    data:{age:18}
})
```
至此，我们需要用webpack打包，打包到webapp目录下。 <br>
需要修改2个地方： <br>
(1)因为我们的webpack不是全局安装的，所以不能直接执行webpack命令，我们这里借助npm来执行。所以需要修改项目根目录下的package.json文件，加入：
```angular2html
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
```
表示：执行build，就会去node_modules.bin\下去寻找webpack命令。build 这个名字是自定义的。<br>
(2)还需要修改webpack配置文件：webpack.config.js

```angular2html
resolve: {
    alias: {
        vue: 'vue/dist/vue.js'
    }
},
```
我们之前把这个注释掉了，现在打开。此处的意义是找到node_modules/vue/dist/vue.js

最后，我们就来打包，看看结果是怎样的？ 
终端里还是cd到项目根目录下，执行：`$ npm run build`
<img src="https://img-blog.csdn.net/20161012213632976" alt="这里写图片描述" title="" style="margin-top:24px;margin-bottom:24px;"><br/>
index.html就是打包之后的模板文件，js/index.js就是打包之后的js文件，在index.html被引用了。
预览一下index.html: 
<img src='https://img-blog.csdn.net/20161012213920280'>
这样就完成了vueJS的一个简单案列