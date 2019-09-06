# npm使用过程中的一些错误解决办法及npm常用命令

0.1182017.08.24 12:33:02字数 3337阅读 3235

[npm使用过程中的一些错误解决办法及npm常用命令](https://link.jianshu.com/?t=http://www.cnblogs.com/kidsitcn/p/4557548.html)

node，npm在前端开发流程中提供了非常完善的自动化工具链，但是同样由于其复杂性导致有很多奇奇怪怪的问题。本文将记录使用过程中出现的一些问题及其解决方法备案。

国内由于gfw问题，导致很多国外的网站不能访问，比如bitbucket就是一个host代码的很优秀平台，但是由于该平台可能被block住，从而导致npm安装时出现奇奇怪怪的问题。有以下方法解决：

1.使用一个proxy来代理访问，但是这个方法速度可能比较慢；

2.可以通过修改npm的配置文件让npm到另外的pacakge mirror站点去找package，通过如下命令

$npm configsetregistry https://registry.npm.taobao.org

$ npm config set registry http://r.cnpmjs.org

或者：npm config set registry http://registry.npmjs.eu

随后再执行

npm install

或者直接在命令行中指定某些参数，比如phantomjs是一个无图形界面的浏览器，在自动化测试中应用广泛，可能的安装方式：

*npm install phantomjs --phantomjs_cdnurl=http://cnpmjs.org/downloads*

如果上述方法都不奏效，那么可能需要 配置自己网卡的dns为国外的dns

如果你在企业防火墙的后面，上网是通过企业的代理来上的，那么需要配置proxy和https-proxy

npm config set proxy http://proxy.company.com:8080npm config set https-proxy http://proxy.company.com:8080

在linux下面，你可能需要使用-g参数安装一些package作为global，比如grunt,gulp,bower等，但是你又没有root权限，就有可能出现下面的错误：





![img](https://upload-images.jianshu.io/upload_images/6894705-869ddb39f71f2178.gif?imageMogr2/auto-orient/strip|imageView2/2/w/20/format/webp)

$ npminstall-g gulp

npm ERR!tar.unpack untar error /home/cabox/.npm/gulp/3.9.0/package.tgz

npm ERR! Linux2.6.32-042stab104.1npm ERR! argv"/usr/local/bin/node""/usr/local/bin/npm""install""-g""gulp"npm ERR! node v0.12.3npm ERR! npm  v2.9.1npm ERR! path /usr/local/lib/node_modules/gulp

npm ERR!codeEACCESnpm ERR! errno -13npm ERR! Error: EACCES,mkdir'/usr/local/lib/node_modules/gulp'npm ERR!at Error (native)

npm ERR!  { [Error: EACCES,mkdir'/usr/local/lib/node_modules/gulp']

npm ERR!  errno: -13,

npm ERR!  code:'EACCES',

npm ERR!  path:'/usr/local/lib/node_modules/gulp',

npm ERR!  fstream_type:'Directory',

npm ERR!  fstream_path:'/usr/local/lib/node_modules/gulp',

npm ERR!  fstream_class:'DirWriter',

npm ERR!fstream_stack:

npm ERR!    ['/usr/local/lib/node_modules/npm/node_modules/fstream/lib/dir-writer.js:36:23',

npm ERR!'/usr/local/lib/node_modules/npm/node_modules/mkdirp/index.js:46:53',

npm ERR!'FSReqWrap.oncomplete (fs.js:95:15)'] }

npm ERR!npm ERR! Please try running this command again as root/Administrator.

npm ERR! Please include the followingfilewith any support request:

npm ERR!    /home/cabox/npm-debug.log





![img](https://upload-images.jianshu.io/upload_images/6894705-20a1a535e1c92ab3.gif?imageMogr2/auto-orient/strip|imageView2/2/w/20/format/webp)

可能的解决方案是修改npm将安装的目标目录的ownershipi：

$ npm config get prefix/usr/local

$whoamicabox

上面的命令可以查到你是以cabox用户来运行命令的，npm将全局package安装package到/usr/local下面的lib/node_modules目录下面，比如gulp,bower,grunt等需要全局安装的node module都将存放到这里，而如果你对该目录没有写的权限，则会出现问题，因此你可以做的是chown -R /usr/local your_username

但是这个方案也是有缺点的，特别是当一个系统中有多个用户使用时，你把这些公共目录都搞成你自己的ownership，可能会存在问题。

另外一种可能的解决方案修改上述prefix，指定npm的全局package安装目录为自己的Home目录下面的子目录，同时需要将上述子目录放到path中去，这样就能够将npm的全局package安装到这个我们有权限控制的目录中了





![img](https://upload-images.jianshu.io/upload_images/6894705-d2ff4ceb9aa9b756.gif?imageMogr2/auto-orient/strip|imageView2/2/w/20/format/webp)

$ npm config set prefix /home/cabox/npm-global/$ npm config get prefix/home/cabox/npm-global

$ gulp-bash: gulp: command not found

$ npminstall-g gulp/home/cabox/npm-global/bin/gulp -> /home/cabox/npm-global/lib/node_modules/gulp/bin/gulp.js

gulp@3.9.0/home/cabox/npm-global/lib/node_modules/gulp

├── pretty-hrtime@1.0.0├── interpret@0.6.2├── deprecated@0.0.1├── archy@1.0.0├── minimist@1.1.1├── tildify@1.0.0(user-home@1.1.1)

├── v8flags@2.0.5(user-home@1.1.1)

├── chalk@1.0.0(escape-string-regexp@1.0.3, ansi-styles@2.0.1, supports-color@1.3.1, strip-ansi@2.0.1, has-ansi@1.0.3)

├── semver@4.3.6├── orchestrator@0.3.7(stream-consume@0.1.0, sequencify@0.0.7, end-of-stream@0.1.5)

├── liftoff@2.1.0(extend@2.0.1, rechoir@0.6.1, flagged-respawn@0.3.1, resolve@1.1.6, findup-sync@0.2.1)

├── gulp-util@3.0.5(array-differ@1.0.0, array-uniq@1.0.2, lodash._reescape@3.0.0, lodash._reevaluate@3.0.0, beeper@1.1.0, lodash._reinterpolate@3.0.0,object-assign@2.1.1, replace-ext@0.0.1, vinyl@0.4.6, lodash.template@3.6.1, through2@0.6.5, multipipe@0.1.2, dateformat@1.0.11)

└── vinyl-fs@0.3.13(graceful-fs@3.0.8, strip-bom@1.0.0, defaults@1.0.2, vinyl@0.4.6, mkdirp@0.5.1, through2@0.6.5, glob-stream@3.1.18, glob-watcher@0.0.6)





![img](https://upload-images.jianshu.io/upload_images/6894705-92e173109e571e24.gif?imageMogr2/auto-orient/strip|imageView2/2/w/20/format/webp)

上面使用cabox用户安装gulp到/home/cabox/npm-global目录中去了。

详细参照下面的两篇文章：

http://www.johnpapa.net/how-to-use-npm-global-without-sudo-on-osx/

https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md

但是现在还有一个问题，那就是系统path中并没有包含该目录，因此直接运行gulp还是无法找到的

在.bash_profile中添加以下：

PATH=$PATH:$HOME/bin:$HOME/npm-global/bin

**npm config ls 将列出所有已经配置好的npm选项**

如果遇到一些莫名其妙的问题，你首先可以更新一下npm版本，删除npm的cache

npminstall-g npm@latest或npm install npm -g

npm cacheclear&&rm-rf node_modules && npminstall

npm ls 和 npm uninstal删除已安装的package. npm ls packagewitherror将列出有问题的那个package安装在哪里，这样你就可以直接到那个目录重新单独安装那个pacakge，调查不能安装成功的原因





![img](https://upload-images.jianshu.io/upload_images/6894705-29f40961dd2a3292.gif?imageMogr2/auto-orient/strip|imageView2/2/w/20/format/webp)

[cabox@box-codeanywhere npmtest]$ npminstall--save lodash

npm WARN package.json kidsit@1.0.0No repository field.

npm WARN package.json kidsit@1.0.0No README data

lodash@3.9.3node_modules/lodash

[cabox@box-codeanywhere npmtest]$lsnode_modules  package.json

[cabox@box-codeanywhere npmtest]$vipackage.json

[cabox@box-codeanywhere npmtest]$lsnode_modules  package.json

[cabox@box-codeanywhere npmtest]$ npmlskidsit@1.0.0/home/cabox/workspace/npmtest

└── lodash@3.9.3[cabox@box-codeanywhere npmtest]$ npm uninstall lodash --save

unbuild lodash@3.9.3

在这里如果node系统的模块安装有问题，这里会主动列出来：

├── camelcase@1.1.0

├─┬ cliui@2.1.0

│ ├─┬ center-align@0.1.1

│ │ └─┬ align-text@0.1.1

│ │ ├── kind-of@1.1.0

│ │ ├── longest@1.0.1

│ │ └── repeat-string@1.5.2

│ ├─┬ right-align@0.1.1

│ │ └─┬ align-text@0.1.1

│ │ ├── kind-of@1.1.0

│ │ ├── longest@1.0.1

│ │ └── repeat-string@1.5.2

│ └── wordwrap@0.0.2

├── decamelize@1.0.0

└── window-size@0.1.0

npm ERR! missing: bytes@1.0.0, required by body-parser@1.12.4

npm ERR! missing: content-type@~1.0.1, required by body-parser@1.12.4

npm ERR! missing: depd@~1.0.1, required by body-parser@1.12.4

npm ERR! missing: iconv-lite@0.4.8, required by body-parser@1.12.4

npm ERR! missing: on-finished@~2.2.1, required by body-parser@1.12.4

npm ERR! missing: qs@2.4.2, required by body-parser@1.12.4

npm ERR! missing: raw-body@~2.0.1, required by body-parser@1.12.4

npm ERR! missing: type-is@~1.6.2, required by body-parser@1.12.4





![img](https://upload-images.jianshu.io/upload_images/6894705-257ddae35c0b1833.gif?imageMogr2/auto-orient/strip|imageView2/2/w/20/format/webp)

如果你的node_modules目录中已经安装了一个package，但是package.json中并没有对该package做依赖，那么这个package就应该被删除。这时如果执行npm ls命令则指示有一个package not used。为了清理代码，你需要执行npm prune





![img](https://upload-images.jianshu.io/upload_images/6894705-fcc8bfdd0bd87823.gif?imageMogr2/auto-orient/strip|imageView2/2/w/20/format/webp)

[cabox@box-codeanywhere npmtest]$ npminstalllodash

npm WARN package.json kidsit@1.0.0No repository field.

npm WARN package.json kidsit@1.0.0No README data

lodash@3.9.3node_modules/lodash

[cabox@box-codeanywhere npmtest]$lsnode_modules/lodash

[cabox@box-codeanywhere npmtest]$ npmlskidsit@1.0.0/home/cabox/workspace/npmtest

└── lodash@3.9.3extraneous

npm ERR! extraneous: lodash@3.9.3/home/cabox/workspace/npmtest/node_modules/lodash

[cabox@box-codeanywhere npmtest]$ npm prune

npm WARN package.json kidsit@1.0.0No repository field.

npm WARN package.json kidsit@1.0.0No README data

unbuild lodash@3.9.3[cabox@box-codeanywhere npmtest]$ npmlskidsit@1.0.0/home/cabox/workspace/npmtest

└── (empty)





![img](https://upload-images.jianshu.io/upload_images/6894705-031e8b8804b5621e.gif?imageMogr2/auto-orient/strip|imageView2/2/w/20/format/webp)

npm ls -g --depth=0可以列出已经安装的全局package





![img](https://upload-images.jianshu.io/upload_images/6894705-07c0623a9a1d1ce6.gif?imageMogr2/auto-orient/strip|imageView2/2/w/20/format/webp)

[cabox@box-codeanywhere npmtest]$ npmls-g --depth=0/home/cabox/npm-global/lib

├── gulp@3.9.0└── npm@2.11.1[cabox@box-codeanywhere npmtest]$ npm uninstall -g gulp

unbuild gulp@3.9.0[cabox@box-codeanywhere npmtest]$ npmls-g --depth=0/home/cabox/npm-global/lib

└── npm@2.11.1





![img](https://upload-images.jianshu.io/upload_images/6894705-2ad586dbf5ea52c9.gif?imageMogr2/auto-orient/strip|imageView2/2/w/20/format/webp)

为了publish一个package，你需要有以下流程： npm init创建一个package.json，创建入口js，比如index.js，exports相应接口，随后npm adduser, npm publish就可以了。如果要更新你的package，注意需要将版本号做一下变更(npm version patch,npm version minor, npm version major或者手动修改本package的package.json文件的版本号！)，publish完成后，你可以在https://www.npmjs.com/~kidsit网页上查看是否确实publish了一个package。注意：npm package遵循semantic versioning method, major.minor.patch/patch表示bugfix，minor：表示new feature，major表示api无法保持兼容





![img](https://upload-images.jianshu.io/upload_images/6894705-78adc6bdc3732914.gif?imageMogr2/auto-orient/strip|imageView2/2/w/20/format/webp)

[cabox@box-codeanywhere npmtest]$ npm adduser

Username: kidsit

Password:

Email: (this IS public)1372921435@qq.com

[cabox@box-codeanywhere npmtest]$ npm configls; cli configs

user-agent ="npm/2.11.1 node/v0.10.28 linux x64"; userconfig/home/cabox/.npmrc

prefix="/home/cabox/npm-global"; node bin location= /usr/bin/node

; cwd= /home/cabox/workspace/npmtest

; HOME= /home/cabox

;'npm config ls -l'to show all defaults.

[cabox@box-codeanywhere npmtest]$ npm publish+ kidsit@0.0.1[cabox@box-codeanywhere npmtest]$





![img](https://upload-images.jianshu.io/upload_images/6894705-e8fb43ce6644019b.gif?imageMogr2/auto-orient/strip|imageView2/2/w/20/format/webp)

http://blog.izs.me/post/1675072029/10-cool-things-you-probably-didnt-realize-npm

npm一般在哪些目录搜索需要require的模块呢？

var utils = require("utils");上面的require命令将导致node按照以下顺序查找文件，注意不一定是index.js，这个由package.json中的main定义决定的./node_modules/utils.js

./node_modules/utils/index.js

./node_modules/utils/package.json

具体参考http://www.bennadel.com/blog/2169-where-does-node-js-and-require-look-for-modules.htm

在windows host中的vagrant box linux中使用npm install时，由于host os不支持linux的symbol link，所以必须使用：npm install**--no-bin-links**命令，否则会出现

infounbuild /home/nicholas/share/node_modules/yuitest

verbose link symlinking/usr/local/lib/node_modules/yuitest to /home/nicholas/share/node_modules/yuitest

ERR! Error: EPROTO, Protocol error'/usr/local/lib/node_modules/yuitest'

在执行npm install时如果出现大量的EPERM错误时，很有可能是反病毒软件惹的祸！，同时有可能是npm cache clear没有执行，参考：http://blogs.msdn.com/b/matt-harrington/archive/2012/02/23/how-to-fix-node-js-npm-permission-problems.aspx

Error: EPERM, operation not permitted'C:\...\noam\node_modules\phantomjs\tmp\phantomjs-1.7.0-windows'at Object.fs.renameSync (fs.js:439:18)

gulp 是一个task runner，它和grunt的功能差不多，但是却比grant要快很多，它基于stream，pipe

在win7,win10下安装npm install时，很有可能出现*error MSB3411: Could not load the Visual C++ component "VCBuild.exe". ...  的错误，一个可行的方案是：*

npm config set msvs_version 2012 --global

如何列出一个特定模块的依赖关系？

有时我们希望看清楚一个模块到底是因为谁被安装进来的，也就是说它是哪个package的依赖，你可以使用npm ll命令：

比如下面的命令npm -- gulp-less命令你就可以看出，原来gulp-less这个模块在laravel-elixir模块中包含而安装的，也就是说laravel-elixir已经包含了less预处理功能！





![img](https://upload-images.jianshu.io/upload_images/6894705-96dd4e0217aeb41c.gif?imageMogr2/auto-orient/strip|imageView2/2/w/20/format/webp)

$ npm ll gulp-lesslaravel-elixir@5.0.0│ C:\Users\Administrator\devenvironment\Code\newkidsitfromscratch\node_modules\laravel-elixir

│ Laravel Elixir Core

│ git+https://github.com/laravel/elixir.git│ https://github.com/laravel/elixir└── gulp-less@3.1.0LessforGulp

git://github.com/plus3network/gulp-less.githttps://github.com/plus3network/gulp-less#readme





![img](https://upload-images.jianshu.io/upload_images/6894705-4994fc6dee2a6c1f.gif?imageMogr2/auto-orient/strip|imageView2/2/w/20/format/webp)

npm view gulp-less查看gup-less这个模块的meta信息

https://github.com/substack/stream-handbook

npm 命令大全参考：[http://browsenpm.org/help](https://link.jianshu.com/?t=http://browsenpm.org/help)

npm peerDependency问题：一个可行的思路是：首先升级npm3,然后再分析版本的依赖关系！

npm install安装时可能会涉及到phantomjs-2.1.1-windows.zip无法安装导致失败的问题，解决办法：手动下载phantomjs-1.9.7-windows.zip，复制到C:\Users\ADMINI~1\AppData\Local\Temp\phantomjs\目录，再次执行npm install mocha-phantomjs，安装成功 http://download.csdn.net/detail/f907279313/9575566

windows下如何平滑升级npm3?

参考这篇 https://github.com/felixrieseberg/npm-windows-upgrade 文章，写的非常详细，强烈建议升级到npm3，一些怪异的npm模块安装问题都会没有了！

windows下面的node-pre-gyp安裝依賴問題

fsevents: AttributeError:'MSVSProject'objecthas no attribute'iteritems'

上述錯誤的原因是windows机器上没有安装build tooling依赖，解决办法是:

npm install --global--production windows-build-tools

参考： https://github.com/nodejs/node-gyp

我们如何向npm run script传入参数？

version=1 npm run build -- homepage

上面我们的build脚本就可以得到version和homepage这个参数

https://jurosh.com/blog/npm-pass-parameters-into-script

**Nodejs的代码如何console.log查看object的值？**

https://github.com/Silviu-Marian/node-codein





![img](https://upload-images.jianshu.io/upload_images/6894705-406833e9b3d33abe.png?imageMogr2/auto-orient/strip|imageView2/2/w/1050/format/webp)

转载传送门:http://www.cnblogs.com/kidsitcn/p/4557548.html