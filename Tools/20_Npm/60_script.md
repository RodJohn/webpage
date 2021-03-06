# 参考

http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html

https://www.cnblogs.com/itlkNote/p/6830682.html


# 概述

npm 允许在package.json文件里面，使用scripts字段定义脚本命令。


npm run 是 npm run-script 的缩写。



直接运行 npm run 会列出当前项目的 package.json 中 scripts 属性下的所有脚本命令。

# 原理

npm 脚本的原理非常简单。每当执行npm run，就会自动新建一个 Shell，在这个 Shell 里面执行指定的脚本命令。因此，只要是 Shell（一般是 Bash）可以运行的命令，就可以写在 npm 脚本里面。

特点

比较特别的是，npm run新建的这个 Shell，会将当前目录的node_modules/.bin子目录加入PATH变量，执行结束后，再将PATH变量恢复原样

这意味着，当前目录的node_modules/.bin子目录里面的所有脚本，都可以直接用脚本名调用，而不必加上路径。比如，当前项目的依赖里面有 Mocha，只要直接写mocha test就可以了。

由于 npm 脚本的唯一要求就是可以在 Shell 执行，因此它不一定是 Node 脚本，任何可执行文件都可以写在里面。

npm 脚本的退出码，也遵守 Shell 脚本规则。如果退出码不是0，npm 就认为这个脚本执行失败。


# 通配符

由于 npm 脚本就是 Shell 脚本，因为可以使用 Shell 通配符。


"lint": "jshint *.js"
"lint": "jshint **/*.js"
上面代码中，*表示任意文件名，**表示任意一层子目录。

如果要将通配符传入原始命令，防止被 Shell 转义，要将星号转义。


"test": "tap test/\*.js"

# 传参

四、传参
向 npm 脚本传入参数，要使用--标明。


"lint": "jshint **.js"
向上面的npm run lint命令传入参数，必须写成下面这样。


$ npm run lint --  --reporter checkstyle > checkstyle.xml
也可以在package.json里面再封装一个命令。


"lint": "jshint **.js",
"lint:checkstyle": "npm run lint -- --reporter checkstyle > checkstyle.xml"

# 执行顺序

如果 npm 脚本里面需要执行多个任务，那么需要明确它们的执行顺序。

如果是并行执行（即同时的平行执行），可以使用&符号。


$ npm run script1.js & npm run script2.js
如果是继发执行（即只有前一个任务成功，才执行下一个任务），可以使用&&符号。


$ npm run script1.js && npm run script2.js
这两个符号是 Bash 的功能。此外，还可以使用 node 的任务管理模块：script-runner、npm-run-all、redrun。

# 默认值

  一般来说，npm 脚本由用户提供。但是，npm 对两个脚本提供了默认值。也就是说，这两个脚本不用定义，就可以直接使用。
  
  
  "start": "node server.js"，
  "install": "node-gyp rebuild"
  上面代码中，npm run start的默认值是node server.js，前提是项目根目录下有server.js这个脚本；npm run install的默认值是node-gyp rebuild，前提是项目根目录下有binding.gyp文件。
  
  
# 钩子
  npm 脚本有pre和post两个钩子。举例来说，build脚本命令的钩子就是prebuild和postbuild。
  
  
  "prebuild": "echo I run before the build script",
  "build": "cross-env NODE_ENV=production webpack",
  "postbuild": "echo I run after the build script"
  用户执行npm run build的时候，会自动按照下面的顺序执行。
  
  
  npm run prebuild && npm run build && npm run postbuild
  因此，可以在这两个钩子里面，完成一些准备工作和清理工作。下面是一个例子。
  
  
  "clean": "rimraf ./dist && mkdir dist",
  "prebuild": "npm run clean",
  "build": "cross-env NODE_ENV=production webpack"
  npm 默认提供下面这些钩子。
  
  prepublish，postpublish
  preinstall，postinstall
  preuninstall，postuninstall
  preversion，postversion
  pretest，posttest
  prestop，poststop
  prestart，poststart
  prerestart，postrestart
  自定义的脚本命令也可以加上pre和post钩子。比如，myscript这个脚本命令，也有premyscript和postmyscript钩子。不过，双重的pre和post无效，比如prepretest和postposttest是无效的。
  
  npm 提供一个npm_lifecycle_event变量，返回当前正在运行的脚本名称，比如pretest、test、posttest等等。所以，可以利用这个变量，在同一个脚本文件里面，为不同的npm scripts命令编写代码。请看下面的例子。
  
  
  const TARGET = process.env.npm_lifecycle_event;
  
  if (TARGET === 'test') {
    console.log(`Running the test task!`);
  }
  
  if (TARGET === 'pretest') {
    console.log(`Running the pretest task!`);
  }
  
  if (TARGET === 'posttest') {
    console.log(`Running the posttest task!`);
  }
  注意，prepublish这个钩子不仅会在npm publish命令之前运行，还会在npm install（不带任何参数）命令之前运行。这种行为很容易让用户感到困惑，所以 npm 4 引入了一个新的钩子prepare，行为等同于prepublish，而从 npm 5 开始，prepublish将只在npm publish命令之前运行。  


# 简写
八、简写形式
四个常用的 npm 脚本有简写形式。

npm start是npm run start
npm stop是npm run stop的简写
npm test是npm run test的简写
npm restart是npm run stop && npm run restart && npm run start的简写
npm start、npm stop和npm restart都比较好理解，而npm restart是一个复合命令，实际上会执行三个脚本命令：stop、restart、start。具体的执行顺序如下。

prerestart
prestop
stop
poststop
restart
prestart
start
poststart
postrestart


# 变量

变量
npm 脚本有一个非常强大的功能，就是可以使用 npm 的内部变量。

首先，通过npm_package_前缀，npm 脚本可以拿到package.json里面的字段。比如，下面是一个package.json。


{
  "name": "foo", 
  "version": "1.2.5",
  "scripts": {
    "view": "node view.js"
  }
}
那么，变量npm_package_name返回foo，变量npm_package_version返回1.2.5。


// view.js
console.log(process.env.npm_package_name); // foo
console.log(process.env.npm_package_version); // 1.2.5
上面代码中，我们通过环境变量process.env对象，拿到package.json的字段值。如果是 Bash 脚本，可以用$npm_package_name和$npm_package_version取到这两个值。

npm_package_前缀也支持嵌套的package.json字段。


  "repository": {
    "type": "git",
    "url": "xxx"
  },
  scripts: {
    "view": "echo $npm_package_repository_type"
  }
上面代码中，repository字段的type属性，可以通过npm_package_repository_type取到。

下面是另外一个例子。


"scripts": {
  "install": "foo.js"
}
上面代码中，npm_package_scripts_install变量的值等于foo.js。

然后，npm 脚本还可以通过npm_config_前缀，拿到 npm 的配置变量，即npm config get xxx命令返回的值。比如，当前模块的发行标签，可以通过npm_config_tag取到。


"view": "echo $npm_config_tag",
注意，package.json里面的config对象，可以被环境变量覆盖。


{ 
  "name" : "foo",
  "config" : { "port" : "8080" },
  "scripts" : { "start" : "node server.js" }
}
上面代码中，npm_package_config_port变量返回的是8080。这个值可以用下面的方法覆盖。


$ npm config set foo:port 80
最后，env命令可以列出所有环境变量。


"env": "env"

# 十、常用脚本示例
  
  // 删除目录
  "clean": "rimraf dist/*",
  
  // 本地搭建一个 HTTP 服务
  "serve": "http-server -p 9090 dist/",
  
  // 打开浏览器
  "open:dev": "opener http://localhost:9090",
  
  // 实时刷新
   "livereload": "live-reload --port 9091 dist/",
  
  // 构建 HTML 文件
  "build:html": "jade index.jade > dist/index.html",
  
  // 只要 CSS 文件有变动，就重新执行构建
  "watch:css": "watch 'npm run build:css' assets/styles/",
  
  // 只要 HTML 文件有变动，就重新执行构建
  "watch:html": "watch 'npm run build:html' assets/html",
  
  // 部署到 Amazon S3
  "deploy:prod": "s3-cli sync ./dist/ s3://example-com/prod-site/",
  
  // 构建 favicon
  "build:favicon": "node scripts/favicon.js",





