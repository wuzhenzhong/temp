# 第三章 从Flux到Redux（Flux篇）

0.2312018.07.31 11:29:35字数 7814阅读 251

​        在前一章中我们已经感受到完全用React来管理应用数据的麻烦，在这一章中，我们将介绍Redux这种管理应用状态的框架，包含以下的内部：

​        单项数据流框架的始祖Flux

​        Flux理念的一个更强实现Redux；

​        结合React和Redux。

------

**3.1  Flux**

​        要了解Redux，首先要从Flux说起，可以认为Redux是Flux思想的另一种实现方式，通过了解Flux，可以认为Redux是Flux思想的另一种实现方式，通过了解Flux，我们可以知道Flux一族框架（其中包括(Redux)贯彻的最重要的观点--单向数据流，更重要的事，我们可以发现Flux框架的缺点，从而深刻地认识到Redux相对Flux的改进之处。

让我们来看看Flux的历史，实际上，Flux是和React同时面世的，在2013年，Facebook公司让React亮相的同时，也推出了Flux框架，React和Flux相辅相成，Facebook认为两者结合起来才能构建大型的JavaScript应用。

做一个容易理解的对比，React是用来替换jQuery的，那么Flux就是以替换Backbone.js、Ember.js等MVC一族框架为目的。

在MVC的世界里，React相当于V(也就是View)的部分，只涉及页面的渲染，一旦涉及应用的数据管理部分，还是交给Model和Controller，不过，Flux并不是一个MVC框架，事实上，Flux认为MVC框架存在很大问题，它推翻了MVC框架，并用一个新的思维来管理数据流转。

**3.1.1 MVC框架的缺陷**

MVC框架是业界广泛接受的一种前端应用框架类型，这种框架把应用分为三个部分：

Model（模型）负责管理数据，大部分业务逻辑也应该放在Model中；

View（视图）负责渲染用户界面，应该避免在View中涉及业务逻辑；

Controller（控制器）负责接受用户输入，根据用户输入调用对应的Model部分逻辑，把产生的数据结果交给View部分，让View渲染出必要的输出。

这样的逻辑划分，实质上把以一个应用划分为多个组件一样，就是“分而治之”。毫无疑问，相比把业务逻辑和界面渲染混在一起，MVC框架要先进的多。这种方式得到了广发认可，Facebook最初也是用这种框架。

但是，Facebook的工程部门逐渐发现，对于巨大的代码库和庞大的组织，MVC很快变的非常复实际的框架实现中，总是允许View和Model可以直接通信，而在MVC中直接让其对话是一种灾难。

有这么一个有意思的现象：凡是只在服务器端使用过MVC框架的朋友，就很容易理解和接受Flux。而对于已有很多浏览器端MVC框架经验的朋友，理解MVC和Flux要费点劲。

这个原因就是：服务器端MVC往往就是每个请求就只在Controller-Model-View三者之间走一圈，结果就返回给浏览器去渲染或者其他处理了。然后这个生命周期的Controller-Model-View就可以回收销毁了，这是一个严格意义的单向数据流；对于浏览器MVC框架，存在用户的交互处理，界面渲染出来后，Model和View依然存在于浏览器中，这时候就会诱发开发者为了简便，让现存的Model和View直接对话。

对于MVC框架，为了让数据流可控，Controller应该是中心，当View要传递信息给Model时，应该调用Controller的方法，同样，当Model要更新View时，也应该通过Controller引发新的渲染。

当Facebook推出Flux时，招致了很多质疑。很多人说，Flux不过是对数据流管理更加严格的MVVC框架而已，这种说明不准确，但一定意义说出了Flux的一个特点：更严格的数据流控制。

一个Flux应该包含四个部分，我们先粗略了解一下：

①Dispatcher，处理动作分发，维持Store之间的依赖关系。

②Store，负责存储数据处理数据相关逻辑。

③Action，驱动Dispatcher的JavaScript对象。

④View，视图部分，负责显示用户界面。

若非要将MVC与Flux做个结构对比：那么，Flux的Dispatcher相当于MVC的Controller，Flux的Store相当于MVC的Model，Flux的View对应MVC的View，至于多出来的Action，可以理解为对应给MVC框架的用户请求。

在MVC框架中，系统能够提供什么样的服务，通过Controller暴露函数来实现。每增加一个功能，Controller就要增加一个函数；在Flux的世界里，新增加功能并不需要Dispatcher增加新的函数，实际上，Dispatcher自始至终只需要暴露一个函数Dispatch，当需要增加新的功能时，要做的事增加一种新的Action类型，Dispatcher的对外接口并不用改变。

我们已基本了解Flux是怎么一回事，那么接下来实践一下看看怎么用Flux改进一下我们的React应用。

**3.1.2 Flux应用**

因为Redux其实与Flux一脉相承，从Flux例子入手，在理解Redux的时候就会感觉非常顺畅了。

为了理解Flux，首先通过命令行在项目目录下安装Flux。

npm install --save flux

利用Flux来实现ControlPanel应用的相关代码在-https://giyhub.com/mocheng/react-and-redux/tree/master/chapter-03/flux-上可以找到。最终页面应用效果和第二章完全 一样，通过不同实现方式，来体会每个方式的优劣。

**1.Dispatcher**

首先，创造一个Dispatcher，几乎所有应用都只需要拥有一个Dispatcher，本例也不例外。在src/AppDispatcher.js中，我们创建这个唯一Dispatcher对象：

> import  {Dispatcher } from 'flux';
>
> export default new Dispatcher();

非常简单，我们引入flux库中的Dispatcher类，然后创造一个新的对象作为这个文件的默认输出就足够了。在其他代码中，将会引用这个全局唯一飞Dispatcher对象。

Dispatcher存在的作用，就是用来派发action，接下来我们来定义应用中涉及的action。

**2.action**

action顾名思义代表一个“动作“，不过这个 动作只是一个普通的JavaScript对象代表一个动作的纯数据，类似DOM API中的事件（event）。甚至，和事件相比，action其实还是更加纯粹的数据对象，因为事件往往还包含一些方法，但是action对象不自带方法，就是纯粹的数据。

作为管理，action对象必须有一个名为type的字段，代表这个action对象的类型，为了记录日志和debug方便，这个type应该是字符串类型。

定义action通常需要两个文件，一个定义action类型，一个定义action的构造函数（也称为action creator）。分成两个文件的主要原因是在Store中会根据 action类型做不同操作，也就有单独导入action类型的需要。

在src/ActionTypes.js中，我们定义action的类型：

> export  const INCREMENT ='increment';
>
> export  const DECREMENT ='decrement';

在这个例子中，用户只能做两个动作，一个是点击“+”按钮，一个是点击"-"按钮，所以我们只有两个action类型INCREMENT和DECREMENT。

现在我们在src/Actions.js文件中定义action构造函数：

> import * as ActionTypes from './ActionTypes';
>
> import AppDispatcher from './AppDispatcher.js';
>
> export const increment = (counterCaption)=>{
>
> AppDispatcher.dispatcher({
>
> type:ActionTypes.INCREMENT,
>
> counterCaption:counterCaption
>
> });
>
> };
>
> export const decrement = (counterCaption)=>{
>
> AppDispatcher.dispatcher({
>
> type:ActionTypes.DECREMENT,
>
> counterCaption:counterCaption
>
> });
>
> };

这个Action.js导出了两个action构造函数increment和decrement，当这两个函数被调用的时候，创造了对应的action对象，并立即通过AppDispatcher.dispatcher函数派发出去。

派发出去的action对象最后怎么样了呢？在下面关于Store的部分可以看到。

**3.Store**

一个Store也是一个对象，这个对象存储应用状态，同时还要接受Dispatcher派发的动作，根据动作来决定是否更新应用状态。

接下来创造Store相关代码，便于代码维护，我们在src下创建一个子目录stores，在这个目录放置所有Store代码。

在前面章节的ControlPanel应用例子里，有三个Counter组件，还有一个统计三个Counter计数值之和的功能，我们遇到的麻烦就是这两者之间的状态如何同步的问题，现在，我们创造两个Store，一个是为Counter组件服务的CounterStore，另一个就是为总数服务的SummaryStore。

> src/store/CounterStore.js,代码如下：
>
> const counterValues = {
>
> 'First':0,
>
> 'Second':10,
>
> 'Third':30
>
> }
>
> const CounterStore = Object.assign({},EventEmitter.prototype,{
>
> getCounterVlues:function(){
>
> return counterValues;
>
> },
>
> emitChange : function(){
>
> this.emit(CHANNGE_EVENT);
>
> },
>
> addChangeListener:function(callback){
>
> this.on(CHANGE_EVENT,callback);
>
> },
>
> removeChangeListener:function(callback){
>
> this.removeListener(CHANGE_EVENT,callback);
>
> }
>
> });

当Store状态发生变化的时候，需要通知应用的其他部分做必要的响应。在我们的应用中，做出响应的部分当然就是View部分，但是我们不应该硬编码这种联系，应该用消息的方式建立Store和View的联系。这就是为什么我们用CounterStore扩展了EventEmitter.prototype，等于让CounterStore成了EventEmitter对象，一个EventEmitte实例对象支持下列相关函数。

①emit函数，可以广播一个特定事件，第一个参数是字符串类型的事件名称。

②on函数，可以增加一个挂在这个EventEmitter对象特定事件上的处理函数，第一个参数是字符串类型的事件名称，第二个参数是处理函数。

③removeListener函数，和on函数做的事情相反，删除挂在这个EventEmitter对象特定事件上的处理函数，和on函数一样，第一个参数是事件名称，第二个参数是处理函数。要注意，如果要调用removeListener函数，就一定要保留对处理函数的引用。

对于CounterStorer对象，emitChange、addChangeListener和removeChangeListener函数就是利用EvenetEmitter上述的三个函数完成对CounterStore状态更新的广播、添加监听函数和删除监听函数等操作。

CounterStore函数还提供了一个getCounterValues函数，用于让应用中其他模块可以读取当前的计数值，当前的计数值存储在文件模块级的变量counterValues中。

上面实现的Store只有注册到Dispatcher实例上才能真正发挥作用，所以还需要增加下列代码：

> import AppDispatcher from '../AppDispatcher.js';
>
> CounterStore.dispatcherToken =AppDispatcher.register((action) =>{
>
> if(action.type ===ActionTypes.INCREMENT){
>
> CounterValues[action.counterCaption]++;
>
> CounterStore.emitChange();
>
> }else if(action.type ===ActionTypes.DECREMENT){
>
> CounterValues[action.counterCaption]--;
>
> CounterStore.emitChange();
>
> }
>
> });

这是最重要的一个步骤，要把CounterStore注册到全局唯一的Dispatcher上去。Dispatcher有一个函数叫做register，接受一个回调函数作为参数。返回值是一个token，这个token可以用于Store之间的同步，我们在CounterStore中还用不上这个返回值，在稍后的SummaryStore中会用到，现在我们只是把register函数的返回值保存在CounterStore对象的dispatcherToken字段上，待会就会用得到。

现在我们仔细看看register接受的这个回调函数参数，这是Flux流程中最核心的部分，当通过register函数把一个回调函数注册到Dispatcher之后，所有派发给Dispatcher的action对象，都会传递到这个回调函数中来。

比如通过Dispatcher派发一个动作，代码如下：

> AppDispatcher.dispatcher({
>
> type:ActionTypes.INCREMENT,
>
> counterCaption:'First'
>
> });

那么CounterStore注册的回调函数就会被调用，唯一的一个参数就是那个action对象，回调函数要做的 ，就是根据action对象来决定该如何更新自己的状态。

作为一个普遍接受的传统，action对象中必有一个type字段，类型是字符串，用于表示这个action对象是什么类型。

在我们的例子中，action对象的type和counter Caption字段结合在一起，可以确定是哪个计数器应该做加一或减一的动作，上面例子中的动作含义就是：“名字为First的 计数器要做加一动作”。

无论是加一或者减一，最后都要调用CounterStore.emitChange函数，假如有调用者通过Counter.addChangeListner关注了CounterStore的状态变化，这个emitChange函数调用就会 引发监听函数的执行。

接下来，我们再来看看另一个Store，也就是代表所有计数器计数值总和的Store，在src/stores/SummaryStore.js中编写源代码。

SummaryStore也有emitChange、addChangeListener还有removeChangeListener函数，功能一样也是用于通知监听者状态变化，这几个函数的代码和CounterStore中 完全重复，不同点是对获取状态函数的定义，代码如下：

>   function  computeSummary(counterValues){
>
> let summary = 0;
>
> for(const key in counterValues){
>
> if(counterValues hasOwnPoperty(key)){
>
> summary+=counterVlues(key);
>
> }
>
> }
>
> return summary;
>
> }
>
> const SummaryStore = Object.assign({},EventEmitter.prototype,{
>
> getSummary:function(){
>
> return computeSummary(CounterStore.getCounterValues());
>
> }
>
> });

可以注意到，SummaryStore并没有存储自己的状态，当getSummary被调用时，它是直接从CounterStore里获取状态 计算的。

CounterStore提供了getCounterValues函数让其他模块能够获得所有计数器的值，SummaryStore也提供了getSummary让其他模块可以获得所有计数器当前值的总和。不过，既然总可以通过CounterStore.getCounterValues函数获取最新鲜的数据，SummaryStore似乎也就没有必要把计数器当前值总和存储到某个变量里。事实上，可以看到SummaryStore并不像CounterStore一样用一个变量counterValus存储数据，SummaryStorer不存储数据，而是每次对getSummary的调用，都实时读取CounterStore.getCounterValus，然后实时计算出总和返回给调用者。

可见，虽然名为Store，但并不表示一个Store必须要存储什么东西，Store只是提供获取数据的方法，而Store提供的数据完全可以用另一个Store计算得来。。

SummaryStore在Dispatcher上注册的回调函数

也和CounterStore很不一样，代码如下：

> SummaryStore.dispatcherToken = Dispatcher.register((action)=>{
>
> ​    if((action.type===ActionTypes.INCREMENT)||
>
> (action.type===ActionTypes.DECREMENT)){
>
> AppDispatcher.waitFor([CounterStore.dispatchehToken]);
>
> SummaryStore.emitChange();
>
>   }
>
> })；

SummaryStore同样也通过AppDispatcher.register函数注册一个回调函数，用于接受派发的action对象。在回调函数中，也只关注INCREMENT和DECREMENT类型的action对象，并通过emitChange通知监听者，注意在这里使用了waitFor函数，这个函数解决的是下面描述的问题。

既然一个action对象会被派发给所有回调函数，这就产生了一个问题，到底是按照什么顺序调用各个回调函数呢？

即使Flux按照register调用的顺序去调用各个回调函数，我们也完全无法把握各个Store哪个先装载从而调用register函数。所以，可以认为Dispatcher调用回调函数的顺序是无法预期的，不要假设它会按照我们期望的顺序逐个调用。

设想一下，当一个INCREMENT类型的动作被派发了，如果首先调用SummaryStore的回调函数，在这个回调函数中立即用emitChange通知了监听者，这时监听者会立即通过SummarryStore的getSummary获取结果，而这个getSummary是通过CounterStore暴露的getCounterValues函数获取当前计数器值，计算总和返回......然而，这时候，INCREMENT动作还没来得及派发到CounterStore啊！也就是说，CounterStore的getCounterrValues返回的还是一个未更新的值，那样SummaryStore的getSummary返回值也就是一个错误值了。

于是，解决这个问题就靠Dispatcher的waitFor函数。在SummaryStore的回调函数中，之前在CounterStore中注册回调函数时保存下来的dispatcherToken终于派上用场了。

Dispatcher的waitFor可以接收一个数组作为参数，数组中的每个元素都是一个Disppatcher.register函数的返回结果，也就是所谓的dispatcherToken。这个waitFor函数告Dispatcher，当前的处理必须要暂停，直到dispatcherToken代表的那些已注册回调函数执行结束才能继续。

我们知道，JavaScript是单线程的语音，不可能有线程等待这回事，这个waitFor函数当然并不是多线程实现的，只是在调用waitFor的时候，把控制权交给Dispatcher，让Dispatcher检查一下dispatcherToken代表的回调函数有没有被执行，如果已经执行，那就直接继续，如果还没有执行，那就调用dispatcherToken代表的回调函数之后waitFor才返回。

回到上面假设的例子，即使SummaryStore比CounterStore提前接收到了action对象，在emitChange中调用waitFor，也就能够保证在emitChange函数被调用的时候，CounterStore也已经处理过这个action对象，一切完美解决。

这里要注意一个事实，Dispatcher的register函数，只提供了注册一个回调函数的功能，但却不能让调用者在register时选择只监听某些action，换句话说，每个register的调用者只能这样请求：“当有任何动作被派发时，请调用我。”但不能够这么请求：“当这种类型或者那种类型被派发的时候，请调用我”。

当一个动作被派发的时候，Dispatcher就是简单地把所有注册的回调函数全部都调用一遍，至于这个动作是不是对方关心的，Flux的Dispatcher不关心，要求每个回调函数去鉴别。

看起来，这似乎是一种浪费，但是这个设计让Flux的Dispatcher逻辑最简单话，Dispatcher的责任越简单，就越不会出现问题。毕竟，由回调函数全权决定如何处理actioon对象，也是非常合理的。

**4. view**

首先需要说明，Flux框架下，View并不是说必须使用React，View本身是一个独立的部分，可以用任何一种UI 库来实现。

不过，话说回来，既然我们都使用上Flux了，除非项目有大量历史遗留代码需要利用，否则实在没有理由不用React来实现view。

存在于Flux框架中的React组件需要实现以下几个功能：

①创建时要读取Store上状态来初始化组件内部状态。

②当Store上状态发生变化时，组件要立刻同步更新组件内部状态保持一致；

③View如果要改变Store状态，必须而且只能派发action。

最后让我们来看看例子中的View部分，为了方便管理，所有的View文件都放在src/views目录下。

先看src/vies/ControlPanel.js中的ControlPanel组件，其中render函数的实现和上一章很不一样，代码如下：

> render(){
>
> return(
>
> <Counter caption ="First"/>
>
> <Counter caption ="Seond"/>
>
> <Counter caption ="Third"/>
>
> <hrr/>
>
> <Summary />
>
> </div>
>
> );
>
> }

可以注意到，和前面章节中的ControlPanel不同，Counter组件实例只有caption属性，没有initValues属性。因为我们把计数值包括初始值全都放到CounterStore中去了，所以在创造Counter组件实例的时候就没有必要指定initValue了。

接着看src/views/Counter组件，构造函数中初始化this.state的方式有了变化，代码如下：

> constructor(props){
>
> super(props);
>
> this.onChange = this.onChange.bind(this);
>
> this.onClickIncrementButton =this.onClickIncrementButton.bind(this);
>
> this.onClickDecrementButton= this.onClickDecrementButton.bind(this);
>
> this.state = {
>
> count:CounterStore.getCounterValues()[props.caption]
>
> }
>
> }

在构造函数中，CounterStore.getCounterValues函数获得了所有计数器的当前值，然后把this.state初始化为对应caption字段的值，也就是说Counter组件store来源不再是prop，而是Flux的Store。

Counter组件中的state应该成为Flux Store上状态的一个同步镜像，为了保持两者一致，除了在构造函数中的初始化之外，在之后CounterStore上状态变化时，Counter组件也要对应变化，相关代码如下：

> componentDidMount(){
>
> CounterStore.addChangeListener(this.onChange);
>
> }
>
> componentWillUnmount(){
>
> CounterStore.removeChangeListener(this.onCange);
>
> }
>
> onChange(){
>
> const newCount = CounterStore.getCounterValues()[this.props.caption];
>
> this.setState({count:newCount});
>
> }

如上面的代码所示，在componentDidMount函数中通过CounterStore.addChangeListener函数监听了CounterStore的变化之后，只有CounterStore发生化，Couunter组件的onChange函数就会被调用。与componentDidMount函数中监听事件相对应，在componentWillUnmount函数中删除了这个监听。

接下来，要看React组件如何派发action，代码如下：

> onClickIncrementButton(){
>
> Action.increment(this.props.caption);
>
> }
>
> onClickDecrementButton(){
>
> Actions.decrement(this.props.caption);
>
> }
>
> render(){
>
> const { caption } =this.props;
>
> return(
>
> <button style={buttonStyle} onClick={this.onClickIncrementButton} >+</button>
>
> <button style={buttonStyle} onClick={this.onClickDecrementButton} >-</button>
>
> <span>{caption} count:{this.state.count}</span>
>
> </div>
>
> );
>
> }

可以注意到，在Counter组件中有两处用到CounterStore的getCounterValues函数的地方，第一处是在构造函数中初始化this.state的时候，第二处是在响应CounterStorre状态变化的onChange函数中，同样一个Store状态，为了转换为React组件的状态，有两次重复的调用，这看起来似乎不是很友好。但是，React组件的状态就是这样，在构造函数中要对this.state初始化，要更新它就要调用this.setState函数。

有没有更简洁的办法？比如说只使用CounterStore.getCounterValues一次？可惜，只要我们想用组件的状态来驱动组件的渲染，就不可避免要有这两步。那么如果我们不利于用组件的状态呢？

如果不使用组件的状态，那么我们就可以逃出这个必须在代码中使用Store两次的宿命，在接下来的章节里，我们会遇到这种“无状态”组件。

Summary组件，存在于src/views/Summary.js，和Counter类似，在constructor中初始化组件状态，通过在componentDidMount中添加SummaryStore的监听来同步状态，因为这个View不会有任何交互功能，所以没有派发出任何action。

**3.1.3Flux的优势**

本章例子和第二章只用React的实现效果一样，但是工作方式有了大变化。

回顾一下完全只使用React实现的版本，应用的状态数据只存在于React组件之中，每个组件都要维护驱动自己渲染的状态数据，单个组件的状态还好维护，但是如果多个组件之间的状态有关联，那就麻烦了。比如Counter组件和Summary组件，Summary组件要维护所有Counter组件计数值的总和，Counter组件和Summary分别维护自己的状态，如何同步Summary和Counter状态就成了问题，React只提供了props方法让组件之间通信，组件之间关系稍微复杂一点，这种方式就显得非常笨拙。。

Flux的架构下，应用的状态放在了Store中，React组件只是扮演View的作用，被动根据Store的状态来渲染。在上面的例子中，React组件依然有自己的状态，但是已经完全沦为Store组件的一个映射，而不是主动变化的数据。

在完全只用React实现的版本里，用户的交互操作，比如点击“+“按钮，引发的时间处理函数直接通过this.setState改变组件的状态。在Flux的实现版本里，用户的操作引发的是一个“动作“的派发，这个派发的动作会发送给所有的Store对象，引起Store对象的状态改变，而不是直接引发组件的状态改变。因为组件的状态是Store状态的映射，所以改变了Store对象也就触发了React组件对象的状态改变，从而引发了界面重新渲染。

Flux带来了什么好处呢？最重要的就是“单向数据流”的管理方式。

在Flux的理念里，如果要改变界面，必须改变Store中的状态，如果要改变Store中的状态，必须派发一个action对象，这就是规矩。在这个规矩之下，想要追溯一个应用的逻辑就变得非常容易。

我们已经讨论过MVC框架的缺点，MVC最大的问题就是无法禁绝View和Model直接的直接对话，对应于MVC中View就是Flux中的View，对应于MVC中的Model就是Flux中的Store，在Flux中，Store只有get方法，没有set方法，根本不可能直接去修改其内部状态，View只能通过get方法获取Store的状态，无法直接去修改状态，如果View想要修改Store状态的话，只有派发一个action对象给Dispatcher。

这看起来是一个“限制”，但却是一个很好的“限制”，禁绝了数据流混乱的可能。

简单来说，在Flux的体系下，驱动界面改变始于一个动作的派发，别无他法。

**3.1.4Flux的不足**

任何工具不可能只有优点没有缺点，接下来让我们看看Flux的不足之处，只有了解了Flux的不足之处，才能理解为什么会出现Flux的改进框架Redux。

**1.Store之间依赖关系**

在Flux的体系中，如果两个Store之间有逻辑依赖关系，就必须用上Dispatcher的waitFor函数。在上面的例子中我们已经使用过waitFor函数，SummmarryStore对action类型的处理，依赖于CounterStore已经处理过了。所以，必须要通过waitFor函数告诉Dispatcher，先让CounterStorer处理这些action对象，只有CounterStore搞定之后SummaryStore才继续。

那么，SummaryStore如何标识CounterStore呢？靠的是register函数的返回值dispatchToken。

而dispatcherToken的产生，当然是CounterStore控制的，换句话说，要这样设计：

□ CounterStore必须要把注册回调函数产生的dispatchToken公布与众。

□ SummaryStore必须要在代码里建立对CounterStore的dispatchToken的依赖。

虽然Flux这个设计的确解决了Store之间的依赖关系，但是，这样明显的模块之间的依赖，看着还是让人感觉不太舒服，毕竟，最后的依赖管理是根本不让依赖产生的。

**2.难以进行服务器端渲染**

关于服务器端渲染，我们在后面的章节（第12章）“同构”中详细介绍，在这里，我们只需要知道，如果要在服务器端渲染，输出不是一个DOM树，而是一个字符串，准确来说就是一个全是HTML的字符串。

在Flux的体系中，有一个全局的Dispatch，然后每一个Store都是一个全局唯一的对象，这对于浏览器端应用完全没有问题，但是如果放在服务器端，就会有大问题。

和一个浏览器只服务于一个用户不同，在服务器端要同时接受很多用户的请求，如果每个Store都是全局唯一的对象，那不同请求的状态肯定就乱套了。

并不是说Flux不能做服务器端的渲染，只是说Flux做服务器端渲染很困难，实际上，Facebook也说的很清楚，Flux不是设计用作服务器端渲染的，他们也从来没有尝试过把Flux应用于服务器端。

**3.Store混杂了逻辑和状态**

Store封装了数据和处理数据的逻辑，用面向对象思维来看，这是一件好事，毕竟对象就是这样定义的。但是，当我们需要动态替换一个Store的逻辑时，只能把这个Store整体替换掉，那也就无法保持Store中存储的状态。

读者可能会问，有什么场景是需要替换Store的呢？

在开发模式下，开发人员要不停的对代码进行修改，如果Store在某个状态下引发了bug，如果能在不毁掉状态的情况下替换Store的逻辑，那就最好了，开发人员就可以不断地改进逻辑来验证这个状态下bug是否修复了。

还有一些应用，在生产环境下就要根据用户属性来动态加载不同的模块，而且动态加载模块还希望不要网页重新加载，这时候也希望能够在不修改应用状态的前提下重新加载应用逻辑，这就是热加载（Hot Load），在第12章会详细介绍如何实现热加载。

可能读者会觉得这里所说的“偷梁换柱”一样的替换应用逻辑是不能做到的。实际上，真的能够做到，Redux就能做到，所以让我们进入Redux的世界吧。