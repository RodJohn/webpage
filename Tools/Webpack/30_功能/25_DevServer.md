

# 概述

    在webpack中有一个在开发时很好用的功能，热加载（Hot-replace），
    在react开发中，结合上react-hot-loader插件，
    开发工作将会变得更棒：
    更改代码之后，你甚至都不需要去刷新你的浏览器，界面就会自动刷新。
    但是，要使用这一功能的话，我们就需要使用webpack-dev-server。

webpack-dev-server
    
    webpack-dev-server是一个小型的node.js Express服务器,
    它使用webpack-dev-middleware中间件来为通过webpack打包生成的资源文件提供Web服务。
    它还有一个通过Socket.IO连接着webpack-dev-server服务器的小型运行时程序。
    webpack-dev-server发送关于编译状态的消息到客户端，客户端根据消息作出响应。


# 安装配置

安装依赖

    npm install --save-dev webpack-dev-server
    
配置
    
    publicPath是webpack-dev-server启动服务时在内存中文件生成的地址。
    contentBase	
                默认webpack-dev-server会为根文件夹提供本地服务器，
                如果想为另外一个目录下的文件提供本地服务器，应该在这里设置其所在目录（本例设置到“public"目录）
    port	
                设置默认监听端口，如果省略，默认为”8080“
    inline	    
                设置为true，当源文件改变时会自动刷新页面
    historyApiFallback	
                在开发单页应用时非常有用，它依赖于HTML5 history API，如果设置为true，所有的跳转将指向index.html


示例

    配置文件webpack.config.js如下所示

    module.exports = {
      devtool: 'eval-source-map',
    
      entry:  __dirname + "/app/main.js",
      output: {
        path: __dirname + "/public",
        filename: "bundle.js"
      },
    
      devServer: {
        contentBase: "./public",
        historyApiFallback: true,
        inline: true
      } 
    }


# npm script

在package.json中的scripts对象中添加如下命令，用以开启本地服务器：

    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
      "start": "webpack",
      "server": "webpack-dev-server --open"
    },





