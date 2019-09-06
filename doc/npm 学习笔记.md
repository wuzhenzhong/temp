# npm 学习笔记

2019.01.22 17:55:32字数 4725阅读 89

# NPM

NPM 是随同 Node 一起安装的包管理工具，能解决 Node 代码部署上的很多问题，常见的使用场景有以下几种：

- 允许用户从NPM服务器下载别人编写的第三方包到本地使用。
- 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
- 允许用户将自己编写的包或命令行程序上传到 NPM 服务器供别人使用。

## 1. NPM 是什么？

程序员自古以来就有社区文化。拥有相同职业或兴趣的人们，自发组织在一起，分享一些信息和资源。
这种社区文化在程序员这个群体中逐渐演变为一种 share 精神，或者说**开源文化**。

在 GitHub 出现之前，程序员们通过各自博客和技术官方网站分享代码。

GitHub 出现之后，非常迅速的兴起为世界上最大的程序员开源社区。在 GitHub 上程序员们可以分享源代码（Repositories）、讨论问题（Issue）、收集学习资源、甚至写博客等等。

但是当一个项目的依赖越来越多的时候，分别去下载每个依赖仍然是一件比较麻烦的事儿。

于是在某一天，一个拥有三大程序员美德（懒惰、急躁和傲慢）的程序员 [Isaac Z. Schlueter](https://github.com/isaacs)，用 JavaScript （运行在 Node.js 上）写了一个工具 NPM （Node Package Manager）把这些代码集中到一起进行管理。在这个工具里面，*这些代码* 被叫做**包**（package）。

NPM 由三个独立的部分组成：

- [网站](https://www.npmjs.com/) - 是开发者查找包（package）、设置参数以及管理 npm 使用体验的主要途径。
- 注册表（registry） - 是一个巨大的数据库，保存了每个包（package）的信息。
- [命令行工具（CLI）](https://docs.npmjs.com/cli/npm) - 通过命令行或终端运行。开发者通过 CLI 与 NPM 打交道。

## 2. 如何安装 NPM 并管理 NPM 版本

NPM 是用 Node.js 编写的，所以你需要安装 Node.js 才能使用 NPM。 您可以通过 Node.js 网站安装 NPM，也可以安装 Node Version Manager 或 NVM。

如果您只是想尝试 NPM，使用 Node.js 安装方法是最快的。 如果您是在不同项目使用多个版本的高级开发人员，请使用节点版本管理器。

### 2.1 从 Node.js 网站安装 NPM

如果您使用的是 OS X 或 Windows，请在 [Node.js 下载页面](https://nodejs.org/en/download/)中下载。 请务必安装标有 LTS 的版本。 其他版本尚未经过 NPM 测试。

如果你正在使用 Linux，你也可以通过滚动 [Node.js 下载页面](https://nodejs.org/en/download/) 找到安装程序，或者检查 [NodeSource's binary distributions](https://github.com/nodesource/distributions)，看看是否有一个适用于你的系统的更新版本。

安装完成后，运行 `node -v`。 版本应为 v8.9.1 或更高版本。

### 2.2 更新 NPM

安装 node.js 时，会自动安装 NPM。 但是，NPM 比 Node.js 更频繁地更新，因此请确保您拥有最新版本。

通过运行 `npm install npm@latest -g` 安装最新的通过官方测试的版本。

要尝试下一个要发布版本，请运行 `npm install npm@next -g`。

### 2.3 使用版本管理器安装 Node.js 和 NPM

由于 NPM 和 node.js 产品由不同的实体管理，因此更新和维护可能变得复杂。 此外，Node.js 安装过程将 NPM 安装在仅具有本地权限的目录中。 当您尝试全局运行包时，这可能会导致权限错误。

为了解决这两个问题，许多开发人员选择使用 vNode Version Manager 或 NVM 来安装 NPM。 版本管理器将避免权限错误，并将解决更新 Node.js 和 NPM 的复杂性。

此外，开发人员可以使用 nvm 在多个版本的 npm 上测试他们的应用程序。 nvm 使您可以轻松切换 npm 以及 node 版本。这样可以更轻松地确保您的应用程序适用于大多数用户，即使他们使用的是其他版本的 npm。

如果您决定安装版本管理器，请使用您选择的版本管理器的说明来学习如何切换版本，并了解如何使用最新版本的 npm 保持最新。

了解如何 [为 MacOs 安装 nvm](https://github.com/creationix/nvm/blob/master/README.md#installation)

要在 Windows 上安装和管理 npm 和 Node.js，建议使用 [nvm-windows](https://github.com/coreybutler/nvm-windows)。

了解如何 [为 Linux 安装 nvm](https://github.com/creationix/nvm/blob/master/README.md#installation)

了解有关 [如何使用 nvm 的更多信息](https://github.com/creationix/nvm/blob/master/README.md#usage)

## 3. 如何安装 npm 包

有两种方式用来安装 npm 包： **本地安装** 和 **全局安装**。至于选择哪种方式来安装，取决于我们如何使用这个包。

- 如果你自己的模块依赖于某个包，并通过 Node.js 的 `require` 加载，那么你应该选择本地安装，这种方式也是 `npm install` 命令的默认行为。
- 如果你想将包作为一个命令行工具，（比如 grunt CLI），那么你应该选择全局安装。

### 3.1 在本地下载和安装包

如果你自己的模块依赖于某个包，并通过 Node.js 的 `require` 加载，那么你应该选择本地安装，这种方式也是 `npm install` 命令的默认行为。

[docs](https://docs.npmjs.com/downloading-and-installing-packages-locally)

#### 3.1.1 通过命令行安装使用

可以使用下面的命令来**安装**一个包：

```xml
npm install <package_name>
```

上述命令执行之后将会在当前的目录下创建一个 *node_modules* 的目录（如果不存在的话），然后将下载的包保存到这个目录下。

为了确认 `npm install` 是正常工作的，可以检查 *node_modules* 目录是否存在，并且里面是否含有你安装的包的文件夹。

**实例，安装一个叫做 lodash 的包**。

新建一个文件夹 *npm-demo* ，CD 到该目录，运行 `npm install lodash`。

```python
PS F:\demo\wp_do\npm-demo> npm install lodash
npm WARN saveError ENOENT: no such file or directory, open 'F:\demo\wp_do\npm-demo\package.json'
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN enoent ENOENT: no such file or directory, open 'F:\demo\wp_do\npm-demo\package.json'
npm WARN npm-demo No description
npm WARN npm-demo No repository field.
npm WARN npm-demo No README data
npm WARN npm-demo No license field.

+ lodash@4.17.11
added 1 package from 2 contributors and audited 1 package in 3.224s
found 0 vulnerabilities
```

运行 `dir node_modules` 显示指定目录中的文件和子目录列表。

```undefined
PS F:\demo\wp_do\npm-demo> dir node_modules


    目录: F:\demo\wp_do\npm-demo\node_modules


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        2019/1/10     11:30                lodash
```

安装成功之后，如果 *node_modules* 目录下存在一个名为 lodash 的文件夹，则说明成功安装了这个包。

在本地目录中如果没有 package.json 这个文件的话，那么最新版本的包会被安装。如果存在 package.json 文件，则会在 package.json 文件中查找针对这个包所约定的[语义化版本规则](https://www.npmjs.cn/getting-started/semantic-versioning/)，然后安装符合此规则的最新版本。

**一旦将包安装到 node_modules 目录中，你就可以使用它了**。比如在你所创建的 Node.js 模块中，你可以 `require` 引入这个包。

在 *npm-demo* 文件夹中创建一个名为 **index.js** 的文件，并保存如下代码：

```jsx
// index.js
var lodash = require('lodash');
 
var output = lodash.without([1, 2, 3], 1);
console.log(output);
```

运行 `node index.js` 命令。应当输出 `[2, 3]`。

```css
PS F:\demo\wp_do\npm-demo> node index.js
[ 2, 3 ]
```

#### 3.1.2 使用 package.json

管理本地安装的 npm 包的最佳方法是创建 **package.json** 文件。

package.json 文件可以：

- 列出你的项目所依赖的包。
- 允许您使用 [语义化版本规则](https://www.npmjs.cn/getting-started/semantic-versioning/) 指定项目可以使用的包的版本。
- 使您的构建可重现，因此更容易与其他开发人员共享。

##### 要求（Requirements）

package.json 必须包括:

- "name" - 全是小写字母，允许使用破折号和下划线，但不能有空格。
- "version" - 形如 `x.x.x`，遵循 [语义版本控制规范](https://docs.npmjs.com/about-semantic-versioning)

例如：

```json
{
  "name": "my-awesome-package",
  "version": "1.0.0"
}
```

##### 创建 package.json

创建 package.json 文件有两种基本方法。

###### 运行 CLI 问卷

要使用您提供的信息创建 package.json 文件，请使用 `npm init` 命令。

在命令行上，导航到程序包的根目录 *npm-demo*，运行 `npm init` 命令。

```ruby
PS F:\demo\wp_do\npm-demo> npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: (npm-demo)
```

回答问题：包的名字是什么？（回车默认为 npm-demo ）

直接回车，或者输入其它信息后回车。

```css
version: (1.0.0)
```

回答问题：包的版本号是什么？（回车默认为 1.0.0 ）

直接回车，或者输入其它信息后回车。

```undefined
description:
```

请输入包描述信息？（为了使您的包更容易在 npm 网站上找到，请输入自定义内容）

直接回车，或者输入其它信息后回车。

```css
entry point: (index.js)
```

回答问题：包的主程序入口是那个文件？（回车默认为 index.js）

直接回车，或者输入其它信息后回车。

```bash
test command:
```

请输入包的测试命令（回车默认为空）

直接回车，或者输入其它信息后回车。

```undefined
git repository:
```

请输入包在 github 上的仓库地址（回车默认为空）

直接回车，或者输入其它信息后回车。

```undefined
keywords:
```

请输入包的关键字（回车默认为空）

直接回车，或者输入其它信息后回车。

```undefined
author:
```

请输入包的作者（回车默认为空）

直接回车，或者输入其它信息后回车。

```undefined
license: (ISC)
```

回答问题：包的开源许可证是那种？（回车默认为 ISC 许可）

ISC许可证是一种开放源代码许可证，在功能上与两句版的BSD许可证相同。



![img](https://upload-images.jianshu.io/upload_images/15545163-64f7cda7934d8fe3.png?imageMogr2/auto-orient/strip|imageView2/2/w/800/format/webp)

license2

直接回车，或者输入其它信息后回车。

```java
About to write to F:\demo\wp_do\npm-demo\package.json:

{
  "name": "npm-demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "dependencies": {
    "lodash": "^4.17.11"
  },
  "devDependencies": {},
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}


Is this OK? (yes)
```

根据问题生成的一个 package.json 文件，回车确认完成创建。在 *npm-demo* 目录下就会生成一个
package.json 文件

###### 创建一个默认的 package.json 文件

要想直接创建一个默认的 package.json 文件，只需要在 *npm-demo* 目录运行命令 `npm init --yes`就可以了。（可简写为 `npm init -y` ）。

```go
PS F:\demo\wp_do\npm-demo> npm init -y
Wrote to /home/ag_dubs/my_package/package.json:

{
  "name": "my_package",
  "description": "",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/ashleygwilliams/my_package.git"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/ashleygwilliams/my_package/issues"
  },
  "homepage": "https://github.com/ashleygwilliams/my_package"
}
```

简单介绍默认信息：

- name: 当前目录名称
- version: 总会是 1.0.0
- description: 来自 readme 文件，没有就是空字符串 `""`
- main: 总会是 index.js
- scripts: 默认情况下会创建一个空的测试脚本
- keywords: 为空
- author: 为空
- license: ISC
- bugs: 来自当前目录的信息（如果存在）
- homepage: 来自当前目录的信息（如果存在）

您还可以为 init 命令设置多个配置选项。 比如：

```bash
> npm set init.author.email "wombat@npmjs.com"
> npm set init.author.name "ag_dubs"
> npm set init.license "MIT"
```

设置后，在执行 `npm init` 默认的信息就会变成配置的信息。

**如何自定义 package.json 问卷？**

如果您希望创建许多 package.json 文件，您可能希望自定义在 init 过程中询问的问题，以便文件始终包含您期望的关键信息。 您可以自定义字段以及提出的问题。

为此，您可以在主目录 `〜/.npm-init.js` 下创建自定义 `.npm-init.js`。

一个简单的 `.npm-init.js` 看起来像这样：

```java
module.exports = {
  customField: 'Custom Field',
  otherCustomField: 'This field is really cool'
}
```

在您的主目录中使用此文件运行 `npm init` 将输出包含以下行的 package.json：

```css
{
  customField: 'Custom Field',
  otherCustomField: 'This field is really cool'
}
```

您还可以使用提示功能的自定义问题。

```java
 module.exports = prompt("what's your favorite flavor of ice cream, buddy?", "I LIKE THEM ALL");
```

要了解有关如何创建高级自定义的更多信息，请查看 [init-package-json文档](https://github.com/npm/init-package-json)

##### 指定依赖项

要指定项目所依赖的包，您需要列出要在 package.json 文件中使用的包。

您可以列出两种类型的包：

- "dependencies"：您的应用程序在生产中需要这些包。
- "devDependencies"：这些包仅用于开发和测试。

您可以手动编辑你的 package.json 文件，或者在命令行使用 `--save` 或 `--save-dev` 标记 `npm install` 命令。

添加依赖进入 package.json 文件的 `dependencies`：

```xml
npm install <package_name> --save
```

添加依赖进入 package.json 文件的 `devDependencies`：

```xml
npm install <package_name> --save-dev
```

> 注意： i 是 install 的简写，-S 是--save 的简写，-D 是--save-dev 的简写

> 注意：如果不使用 `--save` 或 `--save-dev` 标记，则会添加进 `"dependencies"`

### 3.2 全局下载和安装包

将包安装到全局，你应该使用 `npm install -g <package>` 命令，例如：

```undefined
npm install -g jshint
```

如果你遇到 EACCES 错误，请查看 [如何防止权限错误](https://www.npmjs.cn/getting-started/fixing-npm-permissions/)。

小技巧：如果你安装的 npm 是 5.2 或更高版本，可以使用 npx 运行全局安装的包。

## 4. 更新从注册表下载的包

更新从注册表下载的本地和全局程序包有助于保持代码和工具的稳定性，可用性和安全性。

### 4.1 更新本地安装的包

我们建议您定期更新项目所依赖的本地软件包，以便在对依赖项进行改进时改进代码。

1. 导航到项目的根目录并确保它包含一个package.json文件：

   ```bash
   cd /path/to/project
   ```

2. 在项目根目录中，运行以下

    

   ```
   update
   ```

    

   命令：

   ```cpp
   npm update                  // 更新项目所有依赖的包
   
   npm update <package_name>   // 更新项目某个依赖的包
   
   npm update <package_name> --save       // 更新项目（某个）生产环境依赖的包
   
   npm update <package_name> --save-dev   // 更新项目（某个）开发环境依赖的包
   ```

3. 要测试更新，请运行该

    

   ```
   outdated
   ```

    

   命令。不应该有任何输出。

   ```undefined
   npm outdated
   ```

### 4.2 更新全局安装的软件包

注意：如果您使用的是 npm 版本 2.6.0 或更低版本，请运行[此脚本](https://gist.github.com/othiym23/4ac31155da23962afd0e)以更新所有过时的全局包。但是，请考虑升级到最新版本的npm： `npm install npm@latest -g`

要查看需要更新哪些全局包，请在命令行上运行：

```undefined
npm outdated -g --depth=0
```

要更新单个全局程序包，请在命令行上运行：

```xml
npm update -g <package_name>
```

要更新所有全局包，请在命令行上运行：

```undefined
npm update -g
```

## 5. 在项目中使用 npm 包

一旦你已经在 *node_modules* 中安装了一个包，你就可以在你的代码中使用它。

### 5.1 在项目中使用无作用域范围的的包

如果你要创建（使用） Node.js 模块，可以通过将其作为参数传递给 require 函数来使用模块中的包。

例如，要在 Node.js 模块中使用 lodash 包，请在模块的根目录中创建一个包含以下内容的名为 *index.js* 的文件：

```jsx
// index.js
var lodash = require('lodash');

var output = lodash.without([1, 2, 3], 1);
console.log(output);
```

运行代码 `node index.js`。它应该输出 `[2, 3]`。

在 package.json 文件中，出依赖项下的包：

```json
{
  "dependencies": {
    "@scope/package_name": "^1.0.0"
  }
}
```

### 5.2 在项目中使用有作用域范围的的包

要使用有作用域范围的包，只需在使用包名称的任何位置包含范围。

在Node.js模块的index.js文件中需要范围包时，除了包名称之外，还必须引用范围：

```jsx
var projectName = require("@scope/package-name")
```

在 package.json 文件中：

```json
{
  "dependencies": {
    "@scope/package_name": "^1.0.0"
  }
}
```

## 6. 卸载包和依赖项

如果您不再需要在代码中使用包，我们建议您将其卸载并从项目的依赖项中删除它。

### 6.1 卸载本地软件包

要从 **node_modules** 目录中删除程序包，请在命令行上使用 `uninstall` 命令。如果包有作用域范围，请包括范围。

删除无作用域的包，使用 `npm uninstall <package_name>` 命令。

删除有作用域范围的包，使用 `npm uninstall <@scope/package_name>` 命令。

要从 *package.json* 的依赖项中删除包 ，请使用 `--save` 标志。如果包有作用域范围，请包括范围。

要同时删除无作用域的软件包和 *package.json* 文件指定的依赖项，请使用 `npm uninstall --save <package_name>` 命令。

要同时删除有作用域范围的软件包和 *package.json* 文件指定的依赖项，请使用 `npm uninstall --save <@scope/package_name>` 命令。

> 注意：如果您将软件包安装为 `"devDependency"`（即使用 `--save-dev` 安装），请使用 `--save-dev` 以卸载它： `npm uninstall --save-dev package_name`

### 6.2 卸载全局包

要卸载的全局包，请在命令行上使用带有 `-g` 标志的 `uninstall` 命令。如果包具有作用域，请包括范围。

删除无作用域的包，请使用 `npm uninstall -g <package_name>` 命令。

删除有作用域范围的包，请使用 `npm uninstall -g <@scope/package_name>` 命令。

## 7. 如何在项目中引入自己创建的未发布的包

记录一个完整的过程：从零开始写一个简单的包，然后在项目中引用。

### 7.1 创建一个简单的包

首先，新建一个文件夹 **npm-demo**，在文件夹中新建文件 *index.js*，并写入下面内容：

```jsx
;(function (global, factory) {
    typeof exports === 'object' && typeof module !== 'undefined' ? module.exports = factory() :
      typeof define === 'function' && define.amd ? define(factory) :
        global.moduleName = factory()
  }(this, (function () {
    var demo = {
      sayHi: function () {
        console.log('hi，module !');
      }
    };
   
    return demo
})))
```

这个简单的文件就当作是我们打包好的文件吧~

接着继续在文件夹中新建文件 *readme.md* ，并写入：

```bash
# 说明

用于测试在项目中引入本地自己创建的包
```

在命令行 `CD` 进文件夹 **npm-demo**，并运行命令 `npm init -y`：

```go
PS F:\demo\npm-demo> npm init -y
Wrote to F:\demo\npm-demo\package.json:

{
  "name": "npm-demo",
  "version": "1.0.0",
  "description": "用于测试在项目中引入本地自己创建的包",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

跳过问卷，直接根据目录信息创建一个默认的 package.json 文件，只需要在 *npm-demo* 目录运行命令 `npm init --yes` 就可以了。（可简写为 `npm init -y` ）。

这样我们一个本地的包就创建完成啦。

### 7.2 在前端项目中引入本地自己写的包

首先在前端根目录下新建文件夹 **local-modules**（用来保存没有发布出去的包，执行安装命令时，从这个目录读取本地自己写的包），然后把上一步创建的一个简单的包复制进文件夹。

然后 `CD` 到前端项目，并运行命令 `npm install --save ./local-modules/npm-demo`：

```ruby
PS F:\demo\webpack_demo\webpack-es6> npm install --save ./local-modules/npm-demo
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.4 (node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.4: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})

+ npm-demo@1.0.0
added 1 package and audited 5661 packages in 9.46s
found 0 vulnerabilities
```

检查前端项目 package.json 文件中的 `"dependencies"` 字段，是否含有：

```cpp
"npm-demo": "file:local-modules/npm-demo" // 来源在本地，而非线上的包。所以不像其它包那样是版本号
```

如果有就说明引入成功，在前端项目的 **node_modules** 文件夹可以找到 npm-demo 包。

接下来就可以在前端项目中，像使用其它包一样愉快的使用这个包啦~~~

### 7.3 在页面中使用自己的包

在页面引入：

```jsx
import test from "npm-demo"

test.sayHi()
console.log("npm-demo",test)

var myComponent = () => {
    let item = document.createElement("div");
    item.innerHTML = "<p>hello，二狗子！</p>"
    console.log(item)
  return item;
};

document.body.appendChild(myComponent());
```

运行项目，在页面控制台就能看到打印出来的内容：

```xml
hi，module !
npm-demo {sayHi: ƒ}
component <div>…</div>
```

## 8. 发布自己创建的包

要发布自己的包，首先要 [创建一个 npm 帐户](https://www.npmjs.com/signup)，该帐户将提供一个 `http://www.npmjs.com/~ yourusername` 地址。

使用 `npm login`命令登录新帐户。出现提示时，输入您的用户名，密码和电子邮件地址。[了解更多信息](https://docs.npmjs.com/creating-a-new-npm-user-account)

要测试您是否已成功登录，请键入：

```undefined
 npm whoami
```

应该显示您的 npm 用户名。

在命令行 `CD` 进文件夹 **npm-demo**，运行命令 `publish` 发布包：

```undefined
 npm publish npm-demo
```

通过 `unpublish` 命令删除已经发布的包：

```cpp
npm --force unpublish npm-demo    // force 为强制删除，发布超过24小时将不能删除
```

## 9. 学习资料

[官方文档](https://docs.npmjs.com/packages-and-modules/)

[中文文档-翻译中](https://www.npmjs.cn/)

[nodeSchool](https://nodeschool.io/zh-cn/)

[阮一峰的草稿](http://javascript.ruanyifeng.com/nodejs/npm.html#toc0)