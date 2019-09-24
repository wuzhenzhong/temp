# Flask 入门指南

> 本文首发于[Gevin的博客](https://link.juejin.im/?target=http%3A%2F%2Fblog.igevin.info)
>
> 原文链接：[Flask 入门指南](https://link.juejin.im/?target=http%3A%2F%2Fblog.igevin.info%2Fposts%2Fflask-startup-guideline%2F)
>
> 未经 Gevin 授权，禁止转载

# 1. 初识Flask

Flask 是一个微框架（Micro framework），其官方文档的前言部分对Flask做了很清楚的定性介绍：

> ### “微”（Micro）是什么意思？
>
> “微”(micro) 并不意味着你要把整个web应用放到一个python文件里（虽然确实可以），也不意味着Flask 在功能上有所欠缺。微框架中的“微”意味着 Flask 旨在保持核心功能的简单而易于扩展。Flask 不会替你做出太多决策，比如使用何种数据库。而那些 Flask 帮你做好的决策（比如使用哪种模板引擎），都是很容易替换。除此之外的一切都由可由你掌握。
>
> 默认情况下，Flask 不包含数据库抽象层、表单验证，或是任何已在其它已库中处理的很好的功能。相反，Flask 支持通过扩展来给应用添加这些功能，如同是 Flask 本身实现的一样。众多的扩展提供了数据库集成、表单验证、上传处理及各种各样的开放认证技术等功能。Flask 也许是“微小”的，但它已准备好在复杂的生产环境中投入使用。

Flask尽量避免重复制造轮子，而是与已有的优秀轮子去结合，这使得Flask灵活、强大，且定制性更强。 Flask 配置选项众多，均设置了合理的默认值，并会遵循一些惯例；配置选项均可以修改，但通常没必要修改，尤其是刚开始的时候。这使得Flask易于上手。

# 2. Flask VS Django

Django和Flask是GitHub上star数量最多的2个Python web框架，网上也有很多文章对这两个web框架进行分析和对比，如：

- [Quora:Should I learn Flask or Django?](https://link.juejin.im/?target=https%3A%2F%2Fwww.quora.com%2FShould-I-learn-Flask-or-Django)
- [Django vs Flask vs Pyramid: Choosing a Python Web Framework](https://link.juejin.im/?target=https%3A%2F%2Fwww.airpair.com%2Fpython%2Fposts%2Fdjango-flask-pyramid)
- [“海盗”用Flask，“海军”用Django](https://link.juejin.im/?target=http%3A%2F%2Fpython.jobbole.com%2F81803%2F)
- ...

Flask和Django各有优劣，Gevin最初是基于Django做开发的，后来转向Flask，目前两个框架都在使用，就我个人而言，我对这两个框架比较如下：

（1）Flask

- Flask确实很“轻”，不愧是Micro Framework，从Django转向Flask的开发者一定会如此感慨，除非二者均为深入使用过
- Flask自由、灵活，可扩展性强，第三方库的选择面广，开发时可以结合自己最喜欢用的轮子，也能结合最流行最强大的Python库
- 入门简单，即便没有多少web开发经验，也能很快做出网站
- 非常适用于小型网站
- 非常适用于开发web服务的API
- 开发大型网站无压力，但代码架构需要自己设计，开发成本取决于开发者的能力和经验
- 各方面性能均等于或优于Django
- Django自带的或第三方的好评如潮的功能，Flask上总会找到与之类似第三方库
- Flask灵活开发，Python高手基本都会喜欢Flask，但对Django却可能褒贬不一
- Flask与关系型数据库的配合使用不弱于Django，而其与NoSQL数据库的配合远远优于Django
- Flask比Django更加Pythonic，与Python的philosophy更加吻合

（2）Django

- Django太重了，除了web框架，自带ORM和模板引擎，灵活和自由度不够高
- Django能开发小应用，但总会有“杀鸡焉用牛刀”的感觉
- Django的自带ORM非常优秀，综合评价略高于SQLAlchemy
- Django自带的模板引擎简单好用，但其强大程度和综合评价略低于Jinja
- Django自带ORM也使Django与关系型数据库耦合度过高，如果想使用MongoDB等NoSQL数据，需要选取合适的第三方库，且总感觉Django+SQL才是天生一对的搭配，Django+NoSQL砍掉了Django的半壁江山
- Django目前支持Jinja等非官方模板引擎
- Django自带的数据库管理app好评如潮
- Django非常适合企业级网站的开发：快速、靠谱、稳定
- Django成熟、稳定、完善，但相比于Flask，Django的整体生态相对封闭
- Django是Python web框架的先驱，用户多，第三方库最丰富，最好的Python库，如果不能直接用到Django中，也一定能找到与之对应的移植
- Django上手也比较容易，开发文档详细、完善，相关资料丰富

就我个人而言，虽然我最初选择的Python web框架是Django，但当我开始研究Flask后，Flask就立刻成为我最喜欢的框架，虽然有时基于Flask做开发时会借鉴Django上的一些优点，但Flask的地位却无可动摇。

另外，我记得Django的开发者曾经说过，Django出现的太早了，Flask才是Django最初预期的样子，个中意思，请自己品味，原文原话，我暂时没有找到，如果有朋友看到原话，麻烦给个链接。

# 3. Flask从入门到精通

## 3.1 Flask入门

Flask上手非常容易，仅仅看完Flask官方文档中的[Quickstart](https://link.juejin.im/?target=http%3A%2F%2Fflask.pocoo.org%2Fdocs%2F0.10%2Fquickstart%2F)部分，就可以实现简单的网站了。当然，网站看起来好不好，与html, CSS和js水平有关，这不在Flask的使用范围之内。

接下来就[Quickstart](https://link.juejin.im/?target=http%3A%2F%2Fflask.pocoo.org%2Fdocs%2F0.10%2Fquickstart%2F)章节的内容做一点解释。

文中一开始就指明了，以下简单几行代码，即可拉起一个网站：

```
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World!'

if __name__ == '__main__':
    app.run()复制代码
```

简言之，只要创建一个Flask的实例（如上面的app），调用run()方法，网站即被拉起。 `@app.route('/')`设置网站的url路由，下面的函数实现了访问该url时要运行的功能。仅基于以上简单代码承载的逻辑，即可把任一个python 库的功能转换成网站或简单的web api，如果想开放RESTful API，基于这个逻辑从零开始做完成没有问题，虽然更好的方式是基于RESTful相关第三方库。随着网站的功能的增加，代码的行数也会剧增，这时所有的代码都放到一个文件中会很难管理，因此，Flask虽然一个文件就可以实现一个网站，但即便做一个小型网站，最好的方式也是将不同的逻辑放到不同的文件中，文件的组织方式随个人喜好而定，可以借鉴他人优秀的代码，也可以自己摸索。

做一个网站，最基本的功能莫过于信息/错误的返回，页面的渲染，静态文件的加载，URL构建，session/cookie的使用，request/response的处理等，这些在Quickstart中都有涉及，掌握这些内容，做简单网站就足够了，即入门从Quickstart着手即可。

## 3.2 Flask精通

若要精通Flask，仅仅通过Quickstart快速入门完全不够，那怎样才能把Flask用好呢？用好Flask没什么特殊的捷径，靠的依然是『多使用、多思考、多看他人优秀的代码』，经验可能是编程世界里最恰当称之为掌握新技术的捷径的东西，而经验的积累却又是一个漫长的过程，靠的也是『多使用、多思考、多看他人优秀的代码』。Flask的精通要靠自己努力，优秀的资料可以分享。我在这里推荐几个我觉得不错的资料，欢迎大家补充。

(1) [Flask官方文档](https://link.juejin.im/?target=http%3A%2F%2Fflask.pocoo.org%2Fdocs%2F0.10%2F)

Flask官方文档写的还是相当不错的，作为学习Flask的第一手资料没有问题，且Flask官方的[Snippets](https://link.juejin.im/?target=http%3A%2F%2Fflask.pocoo.org%2Fsnippets%2F)，对开发很有帮助，也能避免开发者花太多时间考虑重复的轮子如何开发

(2) [Flask Web开发](https://link.juejin.im/?target=https%3A%2F%2Fbook.douban.com%2Fsubject%2F26274202%2F)

这本书相当不错，通过这本书的学习，基本能做到Flask从入门到精通，也是我建议的第一手Flask学习资料。书中介绍的Flask构建大型项目的架构方式，很是受用，很有实际意义。

(3) [miguelgrinberg.com](https://link.juejin.im/?target=http%3A%2F%2Fblog.miguelgrinberg.com%2F)

上面图书作者的博客，是积累Flask开发经验的好资料

(4) [Write a Tumblelog Application with Flask and MongoEngine](https://link.juejin.im/?target=https%3A%2F%2Fdocs.mongodb.org%2Fecosystem%2Ftutorial%2Fwrite-a-tumblelog-application-with-flask-mongoengine%2F)

这是MongoDB官方文档中的一个教程，也是学习Flask开发的一个很好案例，尤其适合Flask+MongoDB开发的应用场景

(5) [Flask 源码](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fpallets%2Fflask)

Flask 本身是一个非常优秀的Python库，阅读其源码对理解Flask及提高Python开发水平非常有帮助。

(6) [The Hitchhiker’s Guide to Python!](https://link.juejin.im/?target=http%3A%2F%2Fdocs.python-guide.org%2Fen%2Flatest%2F)

这个资料虽然不直接与Flask有关，但对初学者，绝对有学习的价值

# 4. Flask与数据库

大部分网站都要用数据库软件来存储数据，数据库对于任何应用而言都是举足轻重的，因此本节将Flask与数据库的配合作为一个专题展开讨论。

Flask与数据库直连来存取数据虽然没有问题，但会使应用与数据库紧耦合，不利于扩展，也会增加维护代码逻辑的工作量。使用数据库抽象层，会简化Flask与数据库交互的逻辑，也使得Flask与数据库交互的逻辑转移到数据库抽象层搞定，使业务逻辑更清晰，开发也更专注。由于Flask是Micro Framework，自身没有数据库抽象层，而Python生态下有非常多的数据库抽象层可供使用，对Flask而言option非常多，开发者可以选择最好的或自己最熟悉的数据库抽象层应用于开发。

接下来列举一些我熟悉或使用过的数据库抽象层。

## 4.1 适用于关系型数据库的数据库抽象层

适用于关系型数据库的数据库抽象层及ORM（Object Relation Mapping），适用于Flask有：

- [flask-sqlalchemy](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fmitsuhiko%2Fflask-sqlalchemy) `推荐`
- [flask-peewee](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fcoleifer%2Fflask-peewee)

## 4.2 适用于NoSQL数据库的数据库抽象层

NoSQL数据库非常多，MongoDB是其中一种文档型的NoSQL数据库，由于MongoDB天生就是为互联网开发服务的，而我只用Flask + MongoDB 做过开发，下面仅列出MongoDB的抽象层：

- [Flask-MongoEngine](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2FMongoEngine%2Fflask-mongoengine) `推荐`
- [MongoKit](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fnamlook%2Fmongokit)
- [Flask-PyMongo](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fdcrosta%2Fflask-pymongo)

# 5. Flask 常用库

一个强大的框架不仅要练好自己的内功，还要具备各种趁手的武器和装备。Flask 易于扩展，为自己营造了优秀的生态环境，Flask生态下有各种各样的第三方库可供使用，这让我们开发自己的应用时，更高效、更专注，也更稳定。

下面列举一些我常用或了解的库，作为入门，熟悉了这些库，应该很容易开发一个健壮强大的应用了。

## 5.1 Flask 常用库列表

- [flask-script](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fsmurfix%2Fflask-script)
- [flask-login](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fmaxcountryman%2Fflask-login)
- [flask-admin](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fflask-admin%2Fflask-admin)
- [Flask-WTF](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Flepture%2Fflask-wtf)
- [flask-principal](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fmattupstate%2Fflask-principal)
- [flask-restful](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fflask-restful%2Fflask-restful)
- [flask-api](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Ftomchristie%2Fflask-api)
- [Flask-Mail](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fmattupstate%2Fflask-mail)
- [Flask-User](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Flingthio%2FFlask-User)
- [flask-security](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fmattupstate%2Fflask-security)
- [flask-babel](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fpython-babel%2Fflask-babel)
- [flask-locale](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fderkan%2Fflask-locale)

## 5.2 Flask 常用库详情

### [flask-script](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fsmurfix%2Fflask-script)

为Flask提供强大的命令行操作，与Django shell类似。

### [flask-login](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fmaxcountryman%2Fflask-login)

Flask user session 管理，提供诸如`login_user`, `logout_user`, `login_required`, `current_user`等功能，也是其他很多Flask库的基础。

### [flask-admin](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fflask-admin%2Fflask-admin)

为Flask应用提供操作简单且易于扩展的数据库管理界面，与Django自带的数据库管理app类似。

### [Flask-WTF](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Flepture%2Fflask-wtf)

Flask与WTForms的集成，提供强大的Form安全和校验机制，与Django内置的Form功能类似。

### [flask-principal](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fmattupstate%2Fflask-principal)

Flask强大的权限管理机制，灵活性强，提供了一个权限管理的基础框架，是很多Flask权限相关扩展的基础。

### [flask-restful](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fflask-restful%2Fflask-restful)

一个强大的Flask RESTful框架，简单好用。

### [flask-api](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Ftomchristie%2Fflask-api)

相当于[Django REST Framework](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Ftomchristie%2Fdjango-rest-framework)的Flask版，是另一个强大的Flask RESTful框架。

### [Flask-Mail](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fmattupstate%2Fflask-mail)

Flask-Mail 为Flask应用添加了SMTP 邮件发送功能

### [Flask-User](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Flingthio%2FFlask-User)

Flask-User集成了用户管理相关功能，并允许对功能做定制性修改，其相关功能包括Register, Confirm email, Login, Change username, Change password, Forgot password等。

Flask-User 基于`Flask-SQLAlchemy`，NoSQL数据库无法使用。

### [flask-security](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fmattupstate%2Fflask-security)

Flask-Security让开发者能够很快的为应用添加常用的安全机制，其整合了`Flask-Login`, `Flask-Mail`, `Flask-Principal`, `Flask-Script`等应用。其安全机制包括：

- Session based authentication
- Role management
- Password encryption
- Basic HTTP authentication
- Token based authentication
- Token based account activation (optional)
- Token based password recovery / resetting (optional)
- User registration (optional)
- Login tracking (optional)
- JSON/Ajax Support

### [flask-babel](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fpython-babel%2Fflask-babel)

Flask国际化和本地化扩展，基于[Babel](https://link.juejin.im/?target=http%3A%2F%2Fbabel.pocoo.org%2Fen%2Flatest%2F)

### [flask-locale](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fderkan%2Fflask-locale)

为Flask应用添加语言包，与`flask-babel`类似。

# 6. 最后

我个人对Flask的研究，也基本上是按本文思路一步步展开的，希望对初学Flask的朋友有所帮助。关于Flask或者Python的学习，再补充一点，开发Flask第三方库，或者完善已有的Flask库，也是一种很好的实践方法。

欢迎大家与我交流Flask的使用。