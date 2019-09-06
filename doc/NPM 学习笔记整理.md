# NPM 学习笔记整理

0.7162017.06.09 12:13:56字数 4526阅读 2505

# [什么是 NPM](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

npm之于Node，就像pip之于Python,gem之于Ruby,composer之于PHP。

npm是Node官方提供的包管理工具，他已经成了Node包的标准发布平台，用于Node包的发布、传播、依赖控制。npm提供了命令行工具，使你可以方便地下载、安装、升级、删除包，也可以让你作为开发者发布并维护包。

# [为什么要使用 NPM](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

npm是随同Node一起安装的包管理工具，能解决Node代码部署上的很多问题，常见的场景有以下几种：

允许用户从npm服务器下载别人编写的第三方包到本地使用。

允许用户从npm服务器下载并安装别人编写的命令行程序到本地使用。

允许用户将自己编写的包或命令行程序上传到npm服务器供别人使用。

npm的背后，是基于CouchDB的一个数据库，详细记录了每个包的信息，包括作者、版本、依赖、授权信息等。它的一个很重要的作用就是：将开发者从繁琐的包管理工作（版本、依赖等）中解放出来，更加专注于功能的开发。

# [如何使用 NPM](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

## [安装](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

npm不需要单独安装。在安装Node的时候，会连带一起安装npm。但是，Node附带的npm可能不是最新版本，最后用下面的命令，更新到最新版本。

$ sudo npm install npm@latest -g

如果是 Window 系统使用以下命令即可：

npm install npm -g

也就是使用npm安装自己。之所以可以这样，是因为npm本身与Node的其他模块没有区别。

然后，运行下面的命令，查看各种信息。

\#查看 npm 命令列表$ npmhelp#查看各个命令的简单用法$ npm -l#查看 npm 的版本$ npm -v#查看 npm 的配置$ npm config list -l

## [使用](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

### [npm init](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

npm init用来初始化生成一个新的package.json文件。它会向用户提问一系列问题，如果你觉得不用修改默认配置，一路回车就可以了。 如果使用了-f（代表force）、-y（代表yes），则跳过提问阶段，直接生成一个新的package.json文件。

$ npm init -y

### [npm set](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

npm set用来设置环境变量

$ npmsetinit-author-name'Your name'$ npmsetinit-author-email'Your email'$ npmsetinit-author-url'http://yourdomain.com'$ npmsetinit-license'MIT'

上面命令等于为npm init设置了默认值，以后执行npm init的时候，package.json的作者姓名、邮件、主页、许可证字段就会自动写入预设的值。这些信息会存放在用户主目录的~/.npmrc文件，使得用户不用每个项目都输入。如果某个项目有不同的设置，可以针对该项目运行npm config。

### [npm info](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

npm info命令可以查看每个模块的具体信息。比如，查看underscore模块的信息。

$ npm info underscore

上面命令返回一个JavaScript对象，包含了underscore模块的详细信息。这个对象的每个成员，都可以直接从info命令查询。

$ npm info underscore description

$ npm info underscore homepage

$ npm info underscore version

### [npm search](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

npm search命令用于搜索npm仓库，它后面可以跟字符串，也可以跟正则表达式。

$ npm search<搜索词>

### [npm list](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

npm list命令以树形结构列出当前项目安装的所有模块，以及它们依赖的模块。

$ npm list#加上 global 参数，会列出全局安装的模块$ npm list -global#npm list 命令也可以列出单个模块$ npm list underscore

### [npm install](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

使用npm安装包的命令格式为：npm [install/i] [package_name]

#### [本地模式和全局模式](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

npm在默认情况下会从[NPM](https://link.jianshu.com/?t=http://npmjs.org/)搜索或下载包，将包安装到当前目录的node_modules子目录下。

如果你熟悉Ruby的gem或者Python的pip，你会发现npm与它们的行为不同，gem或pip总是以全局模式安装，使包可以供所有的程序使用，而npm默认会把包安装到当前目录下。这反映了npm不同的设计哲学。如果把包安装到全局，可以提供程序的重复利用程度，避免同样的内容的多分副本，但坏处是难以处理不同的版本依赖。如果把包安装到当前目录，或者说本地，则不会有不同程序依赖不同版本的包的冲突问题，同时还减轻了包作者的API兼容性压力，但缺陷则是同一个包可能会被安装许多次。

我们在使用supervisor的时候使用了npm install -g supervisor命令，就是以全局模式安装supervisor。

这里注意一点的就是，supervisor必须安装到全局，如果你不安装到全局，错误命令会提示你安装到全局。如果不想安装到默认的全局，也可以自己修改全局路径到当前路径npm config set prefix "路径"安装完以后就可以用supervisor来启动服务了。supervisor可以帮助你实现这个功能，它会监视你对代码的驱动，并自动重启Node。

一般来说，全局安装只适用于工具模块，比如eslint和gulp。关于使用全局模式，多数时候并不是因为许多程序都有可能用到了它，为了减少多重副本而使用全局模式，而是因为**本地模式不会注册PATH环境变量**。 “本地安装”指的是将一个模块下载到当前项目的node_modules子目录，然后只有在项目目录之中，才能调用这个模块。

本地模式和全局模式的特点如下：

模式可通过 require 使用注册 PATH

本地模式是否

全局模式否是

\#本地安装$ npm install#全局安装$ sudo npm install -global$ sudo npm install -g

npm install也支持直接输入Github代码库地址。

$ npm install git://github.com/package/path.git

$ npm install git://github.com/package/path.git#0.1.0

安装之前，npm install会先检查，node_modules目录之中是否已经存在指定模块。如果存在，就不再重新安装了，即使远程仓库已经有了一个新版本，也是如此。

如果你希望，一个模块不管是否安装过，npm都要强制重新安装，可以使用-f或--force参数。

$ npm install--force

### [安装不同版本](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

install命令总是安装模块的最新版本，如果要安装模块的特定版本，可以在模块名后面加上@和版本号。

$ npm install sax@latest$ npm install sax@0.1.1$ npm install sax@">=0.1.0 <0.2.0"

install命令可以使用不同参数，指定所安装的模块属于哪一种性质的依赖关系，即出现在packages.json文件的哪一项中。

--save：模块名将被添加到 dependencies，可以简化为参数-S。 --save-dev：模块名将被添加到 devDependencies，可以简化为参数-D。

$ npm install sax --save$ npm install node-tap --save-dev#或者$ npm install sax -S$ npm install node-tap -D

### [dependencies 依赖](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

这个可以说是我们npm核心一项内容，依赖管理，这个对象里面的内容就是我们这个项目所依赖的js模块包。下面这段代码表示我们依赖了markdown-it这个包，版本是^8.1.0，代表最小依赖版本是8.1.0，如果这个包有更新，那么当我们使用npm install命令的时候，npm会帮我们下载最新的包。当别人引用我们这个包的时候，包内的依赖包也会被下载下来。

"dependencies":{"markdown-it":"^8.1.0"}

### [devDependencies 开发依赖](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

在我们开发的时候会用到的一些包，只是在开发环境中需要用到，但是在别人引用我们包的时候，不会用到这些内容，放在devDependencies的包，在别人引用的时候不会被npm下载。

"devDependencies":{"autoprefixer":"^6.4.0","babel-preset-es2015":"^6.0.0","babel-preset-stage-2":"^6.0.0","babel-register":"^6.0.0","webpack":"^1.13.2","webpack-dev-middleware":"^1.8.3","webpack-hot-middleware":"^2.12.2","webpack-merge":"^0.14.1","highlightjs":"^9.8.0"}

当你有了一个完整的package.json文件的时候，就可以让人一眼看出来，这个模块的基本信息，和这个模块所需要依赖的包。我们可以通过npm install就可以很方便的下载好这个模块所需要的包。

npm install默认会安装dependencies字段和devDependencies字段中的所有模块，如果使用--production参数，可以只安装dependencies字段的模块。

$ npm install --production#或者$ NODE_ENV=production npm install

一旦安装了某个模块，就可以在代码中用require命令加载这个模块。

varbackbone=require('backbone')console.log(backbone.VERSION)

### [npm run](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

npm不仅可以用于模块管理，还可以用于执行脚本。package.json文件有一个scripts字段，可以用于指定脚本命令，供npm直接调用。package.json文件内容：

{"name":"myproject","devDependencies":{"jshint":"latest","browserify":"latest","mocha":"latest"},"scripts":{"lint":"jshint **.js","test":"mocha test/"}}

### [scripts 脚本](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

顾名思义，就是一些脚本代码，可以通过npm run script-key来调用，例如在这个package.json的文件夹下使用npm run dev就相当于运行了node build/dev-server.js这一段代码。使用scripts的目的就是为了把一些要执行的代码合并到一起，使用 npm run 来快速的运行，方便省事。npm run是npm run-script的缩写，一般都使用前者，但是后者可以更好的反应这个命令的本质。

//脚本"scripts":{"dev":"node build/dev-server.js","build":"node build/build.js","docs":"node build/docs.js","build-docs":"npm run docs & git checkout gh-pages & xcopy /sy dist\\* . & git add . & git commit -m 'auto-pages' & git push & git checkout master","build-publish":"rmdir /S /Q lib & npm run build &git add . & git commit -m auto-build & npm version patch & npm publish & git push","lint":"eslint --ext .js,.vue src"}

npm run如果不加任何参数，直接运行，会列出package.json里面所有可以执行的脚本命令。npm内置了两个命令简写，npm test等同于执行npm run test，npm start等同于执行npm run start。

"build":"npm run build-js && npm run build-css"

上面的写法是先运行npm run build-js，然后再运行npm run build-css，两个命令中间用&&连接。如果希望两个命令同时平行执行，它们中间可以用&连接。

写在scripts属性中的命令，也可以在node_modules/.bin目录中直接写成bash脚本。下面是一个bash脚本。

\#!/bin/bashcdsite/mainbrowserify browser/main.js|uglifyjs -mc>static/bundle.js

假定上面的脚本文件名为build.sh，并且权限为可执行，就可以在scripts属性中引用该文件。

"build-js":"bin/build.sh"

### [pre- 和 post- 脚本](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

npm run为每条命令提供了pre-和post-两个钩子（hook）。以npm run lint为例，执行这条命令之前，npm会先查看有没有定义prelint和postlint两个钩子，如果有的话，就会先执行npm run prelint，然后执行npm run lint，最后执行npm run postlint。

{"name":"myproject","devDependencies":{"eslint":"latest""karma":"latest"},"scripts":{"lint":"eslint --cache --ext .js --ext .jsx src","test":"karma start --log-leve=error karma.config.js --single-run=true","pretest":"npm run lint","posttest":"echo 'Finished running tests'"}}

上面代码是一个package.json文件的例子。如果执行npm test，会按下面的顺序执行相应的命令。

pretest

test

posttest

如果执行过程出错，就不会执行排在后面的脚本，即如果prelint脚本执行出错，就不会接着执行lint和postlint脚本。

### [npm bin](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

npm bin命令显示相对于当前目录的，Node模块的可执行脚本所在的目录（即.bin目录）。

\#项目根目录下执行$ npm bin./node_modules/.bin

# [创建全局链接](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

npm提供了一个有趣的命令npm link，它的功能是在本地包和全局包之间创建符号链接。我们说过使用全局模式安装的包不能直接通过require使用。但通过npm link命令可以打破这一限制。举个例子，我们已经通过npm install -g express安装了express，这时在工程的目录下运行命令：npm link express ./node_modules/express -> /user/local/lib/node_modules/express我们可以在node_modules子目录中发现一个指向安装到全局的包的符号链接。通过这种方法，我们就可以把全局包当做本地包来使用了。 除了将全局的包链接到本地以外，使用npm link命令还可以将本地的包链接到全局。使用方法是在包目录（package.json所在目录）中运行npm link命令。如果我们要开发一个包，利用这种方法可以非常方便地在不同的工程间进行测试。

# [创建包](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

包是在模块基础上更深一步的抽象，Node的包类似于C/C++的函数库或者Java、.Net的类库。它将某个独立的功能封装起来，用于发布、更新、依赖管理和版本控制。Node根据CommonJS规范实现了包机制，开发了npm来解决包的发布和获取需求。Node的包是一个目录，其中包含了一个JSON格式的包说明文件package.json。严格符合CommonJS规范的包应该具备以下特征：

package.json必须在包的顶层目录下；

二进制文件应该在bin目录下；

JavaScript代码应该在lib目录下；

文档应该在doc目录下；

单元测试应该在test目录下。

Node对包的要求并没有这么严格，只要顶层目录下有package.json，并符合一些规范即可。当然为了提高兼容性，我们还是建议你在制作包的时候，严格遵守CommonJS规范。

我们也可以把文件夹封装为一个模块，即所谓的包。包通常是一些模块的集合，在模块的基础上提供了更高层的抽象，相当于提供了一些固定接口的函数库。通过定制package.json，我们可以创建更复杂，更完善，更符合规范的包用于发布。

Node在调用某个包时，会首先检查包中packgage.json文件的main字段，将其作为包的接口模块，如果package.json或main字段不存在，会尝试寻找 index.js 或 index.node 作为包的接口。

package.json是CommonJS规定的用来描述包的文件，完全符合规范的package.json文件应该含有以下字段： name: 包的名字，必须是唯一的，由小写英文字母、数字和下划线组成，不能包含空格。 description: 包的简要说明。 version: 符合语义化版本识别规范的版本字符串。 keywords: 关键字数组，通常用于搜索。 maintainers: 维护者数组，每个元素要包含name、email(可选)、web(可选)字段。 contributors: 贡献者数组，格式与maintainers相同。包的作者应该是贡献者数组的第一个元素。 bugs: 提交bug的地址，可以是网址或者电子邮件地址。 licenses: 许可证数组，每个元素要包含type（许可证的名称）和 url（链接到许可证文本的地址）字段。 repositories: 仓库托管地址数组，每个元素要包含type（仓库的类型，如 git）、URL（仓库的地址）和 path（相对于仓库的路径，可选）字段。 dependencies: 包的依赖，一个关联数组，由包名称和版本号组成。

# [包的发布](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

通过使用npm init可以根据交互式回答产生一个符合标准的package.json。创建一个index.js作为包的接口,一个简单的包就制作完成了。 在发布前,我们还需要获得一个账号用于今后维护自己的包,使用npm adduser根据提示完成账号的创建 完成后可以使用npm whoami检测是否已经取得了账号。 接下来,在package.json所在目录下运行npm publish，稍等片刻就可以完成发布了，打开浏览器，访问[NPM搜索](https://link.jianshu.com/?t=http://search.npmjs.org/)就可以找到自己刚刚发布的包了。现在我们可以在世界的任意一台计算机上使用npm install neveryumodule命令来安装它。 如果你的包将来有更新,只需要在package.json文件中修改version字段,然后重新使用npm publish命令就行了。 如果你对已发布的包不满意，可以使用npm unpublish命令来取消发布。

*需要说明的是： `json` 文件不能有注释*

# [参考链接](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)

[https://blog.ihoey.com/posts/Node/2017-05-10-npm.html](https://link.jianshu.com/?t=https://blog.ihoey.com/posts/Node/2017-05-10-npm.html)