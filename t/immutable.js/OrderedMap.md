# OrderedMap

一种具有额外保证条目的迭代顺序将成为它们的set()顺序的Map类型。

```javascript
class OrderedMap<K, V> extends Map<K, V>
```

#### 讨论

OrderedMap的迭代行为与原生ES6 Map和JavaScript Object相同。

请注意，这`OrderedMap`比无序更昂贵`Map`，可能会消耗更多的内存。`OrderedMap#set`分期付款O（log32 N），但不稳定。

## Construction

### OrderedMap()

创建一个新的不可变有序图。

```javascript
OrderedMap<K, V>(): OrderedMap<K, V>
OrderedMap<K, V>(iter: Iterable.Keyed<K, V>): OrderedMap<K, V>
OrderedMap<K, V>(iter: Iterable<any, Array<any>>): OrderedMap<K, V>
OrderedMap<K, V>(array: Array<Array<any>>): OrderedMap<K, V>
OrderedMap<V>(obj: {[key: string]: V}): OrderedMap<string, V>
OrderedMap<K, V>(iterator: Iterator<Array<any>>): OrderedMap<K, V>
OrderedMap<K, V>(iterable: Object): OrderedMap<K, V>
```

#### 讨论

使用与所提供的Iterable.Keyed或JavaScript对象相同的键值对创建，或期望可重用的K，V元组条目。

提供给此构造函数的键值对的迭代顺序将保留在OrderedMap中。

```javascript
var newOrderedMap = OrderedMap({key: "value"});
var newOrderedMap = OrderedMap([["key", "value"]]);
```

## 静态方法

### OrderedMap.isOrderedMap()

如果提供的值是OrderedMap，则为true。

```javascript
OrderedMap.isOrderedMap(maybeOrderedMap: any): boolean
```

## 成员

### OrderedMap#size

```javascript
size: number
```

#### 继承

```
Collection#size
```

## 持续变化

### OrderedMap#set()

返回一个新的Map也包含新的键值对。如果此地图中已存在相同的密钥，它将被替换。

```javascript
set(key: K, value: V): Map<K, V>
```

#### 继承

```
Map#set
```

### OrderedMap#delete()

返回排除这个的新地图`key`。

```javascript
delete(key: K): Map<K, V>
```

#### 继承

```
Map#delete
```

#### 别号

```
remove()
```

#### 讨论

注意：`delete`不能安全地在IE8中使用，但提供用于镜像ES6集合API。

### OrderedMap#clear()

返回不包含任何键或值的新映射。

```javascript
clear(): Map<K, V>
```

#### 继承

```
Map#clear
```

### OrderedMap#update()

返回一个新的Map，该Map更新了`key`此处的值，并`updater`使用现有值调用返回值，或者`notSetValue`未设置该键。如果只用一个参数调用，`updater`则用Map自身调用。

```javascript
update(updater: (value: Map<K, V>) => Map<K, V>): Map<K, V>
update(key: K, updater: (value: V) => V): Map<K, V>
update(key: K, notSetValue: V, updater: (value: V) => V): Map<K, V>
```

#### 继承

```
Map#update
```

#### 讨论

相当于：`map.set(key, updater(map.get(key, notSetValue)))`。

### OrderedMap#merge()

返回将提供的Iterables（或JS对象）合并到此Map中的新地图。换句话说，这需要每个迭代的每个条目并将其设置在该Map上。

```javascript
merge(...iterables: Iterable<K, V>[]): Map<K, V>
merge(...iterables: {[key: string]: V}[]): Map<string, V>
```

#### 继承

```
Map#merge
```

#### 讨论

如果提供的任何值`merge`不是Iterable（将返回false `Immutable.Iterable.isIterable`），那么它们`Immutable.fromJS`在合并前经过深度转换。但是，如果该值是一个Iterable但包含不可迭代的JS对象或数组，则这些嵌套值将被保留。

```javascript
var x = Immutable.Map({a: 10, b: 20, c: 30});
var y = Immutable.Map({b: 40, a: 50, d: 60});
x.merge(y) // { a: 50, b: 40, c: 30, d: 60 }
y.merge(x) // { b: 20, a: 10, d: 60, c: 30 }
```

### OrderedMap#mergeWith()

类似地`merge()`，`mergeWith()`返回一个新的Map，它将提供的Iterables（或JS对象）合并到此Map中，但使用该`merger`函数来处理冲突。

```javascript
mergeWith(merger: (previous?: V, next?: V, key?: K) => V,...iterables: Iterable<K, V>[]): Map<K, V>
mergeWith(merger: (previous?: V, next?: V, key?: K) => V,...iterables: {[key: string]: V}[]): Map<string, V>
```

#### 继承

```
Map#mergeWith
```

#### 例

```javascript
var x = Immutable.Map({a: 10, b: 20, c: 30});
var y = Immutable.Map({b: 40, a: 50, d: 60});
x.mergeWith((prev, next) => prev / next, y) // { a: 0.2, b: 0.5, c: 30, d: 60 }
y.mergeWith((prev, next) => prev / next, x) // { b: 2, a: 5, d: 60, c: 30 }
```

### OrderedMap#mergeDeep()

就像`merge()`，但是当两个Iterables发生冲突时，它也将它们合并，并通过嵌套数据深入递归。

```javascript
mergeDeep(...iterables: Iterable<K, V>[]): Map<K, V>
mergeDeep(...iterables: {[key: string]: V}[]): Map<string, V>
```

#### 继承

```
Map#mergeDeep
```

#### 例

```javascript
var x = Immutable.fromJS({a: { x: 10, y: 10 }, b: { x: 20, y: 50 } });
var y = Immutable.fromJS({a: { x: 2 }, b: { y: 5 }, c: { z: 3 } });
x.mergeDeep(y) // {a: { x: 2, y: 10 }, b: { x: 20, y: 5 }, c: { z: 3 } }
```

### OrderedMap#mergeDeepWith()

就像`mergeDeep()`，但是当两个非Iterables冲突时，它使用`merger`函数来确定结果值。

```javascript
mergeDeepWith(merger: (previous?: V, next?: V, key?: K) => V,...iterables: Iterable<K, V>[]): Map<K, V>
mergeDeepWith(merger: (previous?: V, next?: V, key?: K) => V,...iterables: {[key: string]: V}[]): Map<string, V>
```

#### 继承

```
Map#mergeDeepWith
```

#### 例

```javascript
var x = Immutable.fromJS({a: { x: 10, y: 10 }, b: { x: 20, y: 50 } });
var y = Immutable.fromJS({a: { x: 2 }, b: { y: 5 }, c: { z: 3 } });
x.mergeDeepWith((prev, next) => prev / next, y)
// {a: { x: 5, y: 10 }, b: { x: 20, y: 10 }, c: { z: 3 } }
```

## 深入持久的变化

### OrderedMap#setIn()

返回已`value`在此设置的新地图`keyPath`。如果任何键`keyPath`不存在，将在该键上创建一个新的不可变Map。

```javascript
setIn(keyPath: Array<any>, value: any): Map<K, V>
setIn(KeyPath: Iterable<any, any>, value: any): Map<K, V>
```

#### 继承

```
Map#setIn
```

### OrderedMap#deleteIn()

返回已删除此值的新地图`keyPath`。如果任何键`keyPath`不存在，则不会发生变化。

```javascript
deleteIn(keyPath: Array<any>): Map<K, V>
deleteIn(keyPath: Iterable<any, any>): Map<K, V>
```

#### 继承

```
Map#deleteIn
```

#### 别号

```
removeIn()
```

### OrderedMap#updateIn()

返回已应用`updater`到在keyPath处找到的条目的新Map 。

```javascript
updateIn(keyPath: Array<any>, updater: (value: any) => any): Map<K, V>
updateIn(keyPath: Array<any>,notSetValue: any,updater: (value: any) => any): Map<K, V>
updateIn(keyPath: Iterable<any, any>, updater: (value: any) => any): Map<K, V>
updateIn(keyPath: Iterable<any, any>,notSetValue: any,updater: (value: any) => any): Map<K, V>
```

#### 继承

```
Map#updateIn
```

#### 讨论

如果任何键`keyPath`不存在，那么`Map`将在这些键上创建新的Immutable 。如果该`keyPath`值尚未包含值，则该`updater`函数将被调用`notSetValue`（如果提供），否则`undefined`。

```javascript
var data = Immutable.fromJS({ a: { b: { c: 10 } } });
data = data.updateIn(['a', 'b', 'c'], val => val * 2);
// { a: { b: { c: 20 } } }
```

如果`updater`函数返回与之相同的值，则不会发生更改。如果`notSetValue`提供，这仍然是正确的。

```javascript
var data1 = Immutable.fromJS({ a: { b: { c: 10 } } });
data2 = data1.updateIn(['x', 'y', 'z'], 100, val => val);
assert(data2 === data1);
```

### OrderedMap#mergeIn()

的组合`updateIn`和`merge`，返回一个新的地图，但在一个点进行合并通过遵循的keyPath到达。换句话说，这两条线是等价的：

```javascript
mergeIn(keyPath: Iterable<any, any>, ...iterables: Iterable<K, V>[]): Map<K, V>
mergeIn(keyPath: Array<any>, ...iterables: Iterable<K, V>[]): Map<K, V>
mergeIn(keyPath: Array<any>, ...iterables: {[key: string]: V}[]): Map<string, V>
```

#### 继承

```
Map#mergeIn
```

#### 例

```javascript
x.updateIn(['a', 'b', 'c'], abc => abc.merge(y));
x.mergeIn(['a', 'b', 'c'], y);
```

### OrderedMap#mergeDeepIn()

组合`updateIn`和`mergeDeep`，返回一个新的地图，但在执行点深合并通过遵循的keyPath到达。换句话说，这两条线是等价的：

```javascript
mergeDeepIn(keyPath: Iterable<any, any>,...iterables: Iterable<K, V>[]): Map<K, V>
mergeDeepIn(keyPath: Array<any>, ...iterables: Iterable<K, V>[]): Map<K, V>
mergeDeepIn(keyPath: Array<any>,...iterables: {[key: string]: V}[]): Map<string, V>
```

#### 继承

```
Map#mergeDeepIn
```

#### 例

```javascript
x.updateIn(['a', 'b', 'c'], abc => abc.mergeDeep(y));
x.mergeDeepIn(['a', 'b', 'c'], y);
```

## 瞬态变化

### OrderedMap#withMutations()

每次调用上述函数之一时，都会创建一个新的不可变Map。如果一个纯函数调用其中的一些来产生最终返回值，那么通过创建所有中间不可变映射来支付对性能和内存的损失。

```javascript
withMutations(mutator: (mutable: Map<K, V>) => any): Map<K, V>
```

#### 继承

```
Map#withMutations
```

#### 讨论

如果您需要应用一系列突变来产生新的不可变Map，请`withMutations()`创建Map 的临时可变拷贝，以高性能的方式应用突变。事实上，这正是如此复杂的突变`merge`。

例如，这会导致创建2个而不是4个新的Maps：

```javascript
var map1 = Immutable.Map();
var map2 = map1.withMutations(map => {
  map.set('a', 1).set('b', 2).set('c', 3);
});
assert(map1.size === 0);
assert(map2.size === 3);
```

注意：并非所有方法都可用于可变集合或内部`withMutations`！只有`set`和`merge`可能会使用变化。

### OrderedMap#asMutable()

另一种避免创建中间不可变映射的方法是创建该集合的可变副本。可变副本*总是*返回`this`，因此不应该用于平等。你的函数不应该返回一个集合的可变副本，只能在内部使用它来创建一个新的集合。如果可能，请使用`withMutations`它，因为它提供了更易于使用的API。

```javascript
asMutable(): Map<K, V>
```

#### 继承

```
Map#asMutable
```

#### 讨论

注意：如果集合已经是可变的，则`asMutable`返回它自己。

注意：并非所有方法都可用于可变集合或内部`withMutations`！只有`set`和`merge`可能会使用变化。

### OrderedMap#asImmutable()

`asMutable`阳的阴。因为它适用于可变集合，所以此操作是*可变的*并返回自身。一旦执行，可变副本变得不可变，并且可以从函数安全地返回。

```javascript
asImmutable(): Map<K, V>
```

#### 继承

```
Map#asImmutable
```

## 转换为Seq

### OrderedMap#toSeq()

Returns Seq.Keyed.

```javascript
toSeq(): Seq.Keyed<K, V>
```

#### 继承

```
Collection.Keyed#toSeq
```

### OrderedMap#toKeyedSeq()

从此Iterable返回一个Seq.Keyed，其索引被视为键。

```javascript
toKeyedSeq(): Seq.Keyed<K, V>
```

#### 继承

```
Iterable#toKeyedSeq
```

#### 讨论

如果您想要对Iterable.Indexed进行操作并保留索引，这值非常有用。

返回的Seq将具有与此Iterable相同的迭代顺序。

例：

```javascript
var indexedSeq = Immutable.Seq.of('A', 'B', 'C');
indexedSeq.filter(v => v === 'B').toString() // Seq [ 'B' ]
var keyedSeq = indexedSeq.toKeyedSeq();
keyedSeq.filter(v => v === 'B').toString() // Seq { 1: 'B' }
```

### OrderedMap#toIndexedSeq()

返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
toIndexedSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#toIndexedSeq
```

### OrderedMap#toSetSeq()

返回一个Seq.Set这个Iterable的值，丢弃键。

```javascript
toSetSeq(): Seq.Set<V>
```

#### 继承

```
Iterable#toSetSeq
```

## 价值平等

### OrderedMap#equals()

如果这和另一个Iterable具有值相等性，则为真，如下定义`Immutable.is()`。

```javascript
equals(other: Iterable<K, V>): boolean
```

#### Inherited from

```
Iterable#equals
```

#### 讨论

注意：这相当于`Immutable.is(this, other)`，但提供允许链式表达式。

### OrderedMap#hashCode()

计算并返回此Iterable的散列标识。

```javascript
hashCode(): number
```

#### 继承

```
Iterable#hashCode
```

#### 讨论

在`hashCode`一个可迭代的用于确定潜在平等，和添加这一个当使用`Set`或作为一个键`Map`，经由不同的实例实现查找。

```javascript
var a = List.of(1, 2, 3);
var b = List.of(1, 2, 3);
assert(a !== b); // different instances
var set = Set.of(a);
assert(set.has(b) === true);
```

如果两个值相同`hashCode`，则不能保证相等（[http://en.wikipedia.org/wiki/Collision_（computer_science％29](http://en.wikipedia.org/wiki/Collision_)）。如果两个值有不同的`hashCode`s，则它们不能相等。

## 读取值

### OrderedMap#get()

返回与提供的键相关联的值，如果Iterable不包含此键，则返回notSetValue。

```javascript
get(key: K, notSetValue?: V): V
```

#### 继承

```
Iterable#get
```

#### 讨论

注意：一个键可能与一个`undefined`值相关联，所以如果`notSetValue`没有提供并且该方法返回`undefined`，那么不能保证没有找到该键。

### OrderedMap#has()

如果此关键字存在`Iterable`，则为真，`Immutable.is`用于确定相等性

```javascript
has(key: K): boolean
```

#### 继承

```
Iterable#has
```

### OrderedMap#includes()

如果此值中存在值`Iterable`，则为true ，`Immutable.is`用于确定相等性

```javascript
includes(value: V): boolean
```

#### 继承

```
Iterable#includes
```

#### 别号

```
contains()
```

### OrderedMap#first()

Iterable中的第一个值。

```javascript
first(): V
```

#### 继承

```
Iterable#first
```

### OrderedMap#last()

Iterable中的最后一个值。

```javascript
last(): V
```

#### 继承

```
Iterable#last
```

## 读深刻的价值

### OrderedMap#getIn()

通过嵌套的Iterables返回键或索引路径的值。

```javascript
getIn(searchKeyPath: Array<any>, notSetValue?: any): any
getIn(searchKeyPath: Iterable<any, any>, notSetValue?: any): any
```

#### 继承

```
Iterable#getIn
```

### OrderedMap#hasIn()

如果通过嵌套的Iterables跟随键或索引路径的结果导致设置值，则返回true。

```javascript
hasIn(searchKeyPath: Array<any>): boolean
hasIn(searchKeyPath: Iterable<any, any>): boolean
```

#### 继承

```
Iterable#hasIn
```

## 转换为JavaScript类型

### OrderedMap#toJS()

将此Iterable深度转换为等效的JS。

```javascript
toJS(): any
```

#### 继承

```
Iterable#toJS
```

#### 别号

```
toJSON()
```

#### 讨论

`Iterable.Indexeds`，并`Iterable.Sets`成为阵列，同时`Iterable.Keyeds`成为物体。

### OrderedMap#toArray()

浅显地将这个迭代器转换为一个Array，丢弃键。

```javascript
toArray(): Array<V>
```

#### 继承

```
Iterable#toArray
```

### OrderedMap#toObject()

将此Iterable浅转换为Object。

```javascript
toObject(): {[key: string]: V}
```

#### 继承

```
Iterable#toObject
```

#### 讨论

如果键不是字符串，则抛出。

## 转换为集合

### OrderedMap#toMap()

将此Iterable转换为Map，如果键不可哈希则抛出。

```javascript
toMap(): Map<K, V>
```

#### 继承

```
Iterable#toMap
```

#### 讨论

注意：这相当于`Map(this.toKeyedSeq())`，但为方便起见并允许链接表达式。

### OrderedMap#toOrderedMap()

将此Iterable转换为Map，并保持迭代顺序。

```javascript
toOrderedMap(): OrderedMap<K, V>
```

#### 继承

```
Iterable#toOrderedMap
```

#### 讨论

注意：这相当于`OrderedMap(this.toKeyedSeq())`，但为方便起见并允许链接表达式。

### OrderedMap#toSet()

将此Iterable转换为Set，放弃键。如果值不可哈希则抛出。

```javascript
toSet(): Set<V>
```

#### 继承

```
Iterable#toSet
```

#### 讨论

注意：这相当于`Set(this)`，但提供允许链式表达式。

### OrderedMap#toOrderedSet()

将此Iterable转换为Set，保持迭代顺序并丢弃键。

```javascript
toOrderedSet(): OrderedSet<V>
```

#### 继承

```
Iterable#toOrderedSet
```

#### 讨论

注意：这相当于`OrderedSet(this.valueSeq())`，但为方便起见并允许链接表达式。

### OrderedMap#toList()

将此Iterable转换为List，放弃键。

```javascript
toList(): List<V>
```

#### 继承

```
Iterable#toList
```

#### 讨论

注意：这相当于`List(this)`，但提供允许链式表达式。

### OrderedMap#toStack()

将此Iterable转换为堆栈，丢弃键。如果值不可哈希则抛出。

```javascript
toStack(): Stack<V>
```

#### 继承

```
Iterable#toStack
```

#### 讨论

注意：这相当于`Stack(this)`，但提供允许链式表达式。

## 迭代器

### OrderedMap#keys()

这个`Iterable`键的迭代器。

```javascript
keys(): Iterator<K>
```

#### 继承

```
Iterable#keys
```

#### 讨论

注意：这将返回一个不支持Immutable JS序列算法的ES6迭代器。使用`keySeq`替代，如果这是你想要的。

### OrderedMap#values()

这个`Iterable`值的迭代器。

```javascript
values(): Iterator<V>
```

#### 继承

```
Iterable#values
```

#### 讨论

注意：这将返回一个不支持Immutable JS序列算法的ES6迭代器。使用`valueSeq`替代，如果这是你想要的。

### OrderedMap#entries()

这个`Iterable`条目的迭代器作为`[key, value]`元组。

```javascript
entries(): Iterator<Array<any>>
```

#### 继承

```
Iterable#entries
```

#### 讨论

注意：这将返回一个不支持Immutable JS序列算法的ES6迭代器。使用`entrySeq`替代，如果这是你想要的。

## Iterables (Seq)

### OrderedMap#keySeq()

返回此Iterable的新键的Seq.Indexed，放弃值。

```javascript
keySeq(): Seq.Indexed<K>
```

#### 继承

```
Iterable#keySeq
```

### OrderedMap#valueSeq()

返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
valueSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#valueSeq
```

### OrderedMap#entrySeq()

返回一个新的Seq.Indexed键值值元组。

```javascript
entrySeq(): Seq.Indexed<Array<any>>
```

#### 继承

```
Iterable#entrySeq
```

## 序列算法

### OrderedMap#map()

使用通过`mapper`函数传递的值返回相同类型的新Iterable 。

```javascript
map<M>(mapper: (value?: V, key?: K, iter?: Iterable<K, V>) => M,context?: any): Iterable<K, M>
```

#### 继承

```
Iterable#map
```

#### 例

```javascript
Seq({ a: 1, b: 2 }).map(x => 10 * x)
// Seq { a: 10, b: 20 }
```

### OrderedMap#filter()

仅`predicate`返回函数返回true 的条目返回相同类型的新Iterable 。

```javascript
filter(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### 继承

```
Iterable#filter
```

#### 例

```javascript
Seq({a:1,b:2,c:3,d:4}).filter(x => x % 2 === 0)
// Seq { b: 2, d: 4 }
```

### OrderedMap#filterNot()

仅`predicate`返回函数返回false 的条目返回相同类型的新Iterable 。

```javascript
filterNot(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### 继承

```
Iterable#filterNot
```

#### 例

```javascript
Seq({a:1,b:2,c:3,d:4}).filterNot(x => x % 2 === 0)
// Seq { a: 1, c: 3 }
```

### OrderedMap#reverse()

按相反顺序返回相同类型的新Iterable。

```javascript
reverse(): Iterable<K, V>
```

#### 继承

```
Iterable#reverse
```

### OrderedMap#sort()

返回包含相同条目的相同类型的新Iterable，通过使用`comparator`进行稳定排序。

```javascript
sort(comparator?: (valueA: V, valueB: V) => number): Iterable<K, V>
```

#### 继承

```
Iterable#sort
```

#### 讨论

如果`comparator`没有提供，默认比较器使用`<`和`>`。

`comparator(valueA, valueB)`:

- 返回`0`元素不应该交换的情况。
- 返回`-1`（或任何负数）如果`valueA`之前`valueB`
- 返回`1`（或任何正数）如果`valueA`后来`valueB`
- 它是纯粹的，即i.e.它必须始终为同一对值返回相同的值。

排序没有定义顺序的集合时，它们的顺序等价物将被返回。例如`map.sort()`返回OrderedMap。

### OrderedMap#sortBy()

就像`sort`，但也接受一个`comparatorValueMapper`允许更复杂的手段进行排序的一个：

```javascript
sortBy<C>(comparatorValueMapper: (value?: V, key?: K, iter?: Iterable<K, V>) => C,comparator?: (valueA: C, valueB: C) => number): Iterable<K, V>
```

#### 继承

```
Iterable#sortBy
```

#### 例

```javascript
hitters.sortBy(hitter => hitter.avgHits);
```

### OrderedMap#groupBy()

返回`Iterable.Keyed`的`Iterable.Keyeds`，由返回值分组`grouper`功能。

```javascript
groupBy<G>(grouper: (value?: V, key?: K, iter?: Iterable<K, V>) => G,context?: any): Seq.Keyed<G, Iterable<K, V>>
```

#### 继承

```
Iterable#groupBy
```

#### 讨论

注意：这总是一个急切的操作。

## 副作用

### OrderedMap#forEach()

该`sideEffect`是在可迭代的每个条目执行。

```javascript
forEach(sideEffect: (value?: V, key?: K, iter?: Iterable<K, V>) => any,context?: any): number
```

#### 继承

```
Iterable#forEach
```

#### 讨论

不同的是[`Array#forEach`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)，如果有任何`sideEffect`回报的话`false`，迭代将停止。返回迭代的条目数（包括返回false的最后一次迭代）。

## 创建子集

### OrderedMap#slice()

返回一个新的Iterable，其类型代表这个Iterable从开始到结束的一部分。

```javascript
slice(begin?: number, end?: number): Iterable<K, V>
```

#### 继承

```
Iterable#slice
```

#### 讨论

如果begin是负数，它将从Iterable的末尾偏移。例如`slice(-2)`返回最后两个条目的Iterable。如果没有提供，则新的Iterable将在此Iterable开始时开始。

如果end是负数，它将从Iterable的末尾偏移。例如`slice(0, -1)`返回除最后一项之外的所有内容的Iterable。如果没有提供，那么新的Iterable将会持续到这个Iterable的结尾。

如果所请求的分片等同于当前的Iterable，那么它将自行返回。

### OrderedMap#rest()

返回包含除第一个以外的所有条目的同一类型的新Iterable。

```javascript
rest(): Iterable<K, V>
```

#### 继承

```
Iterable#rest
```

### OrderedMap#butLast()

返回包含除最后一个以外的所有条目的同一类型的新Iterable。

```javascript
butLast(): Iterable<K, V>
```

#### 继承

```
Iterable#butLast
```

### OrderedMap#skip()

返回`amount`从此Iterable中排除第一个条目的同一类型的新Iterable。

```javascript
skip(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skip
```

### OrderedMap#skipLast()

返回`amount`从此Iterable中排除最后一个条目的同一类型的新Iterable。

```javascript
skipLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skipLast
```

### OrderedMap#skipWhile()

返回包含从`predicate`第一个返回false 时开始的相同类型的新Iterable 。

```javascript
skipWhile(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### 继承

```
Iterable#skipWhile
```

#### 例

```javascript
Seq.of('dog','frog','cat','hat','god')
  .skipWhile(x => x.match(/g/))
// Seq [ 'cat', 'hat', 'god' ]
```

### OrderedMap#skipUntil()

返回包含从`predicate`第一个返回true 时开始的相同类型的新Iterable 。

```javascript
skipUntil(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### 继承

```
Iterable#skipUntil
```

#### 例

```javascript
Seq.of('dog','frog','cat','hat','god')
  .skipUntil(x => x.match(/hat/))
// Seq [ 'hat', 'god' ]
```

### OrderedMap#take()

返回包含`amount`此Iterable中第一个条目的相同类型的新Iterable。

```javascript
take(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#take
```

### OrderedMap#takeLast()

返回包含`amount`此Iterable中最后一个条目的相同类型的新Iterable。

```javascript
takeLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#takeLast
```

### OrderedMap#takeWhile()

返回包含来自此Iterable的条目的相同类型的新Iterable，只要`predicate`返回值为true即可。

```javascript
takeWhile(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### 继承

```
Iterable#takeWhile
```

#### 例

```javascript
Seq.of('dog','frog','cat','hat','god')
  .takeWhile(x => x.match(/o/))
// Seq [ 'dog', 'frog' ]
```

### OrderedMap#takeUntil()

返回包含来自此Iterable的条目的相同类型的新Iterable，只要`predicate`返回false即可。

```javascript
takeUntil(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### 继承

```
Iterable#takeUntil
```

#### 例

```javascript
Seq.of('dog','frog','cat','hat','god').takeUntil(x => x.match(/at/))
// ['dog', 'frog']
```

## 组合

### OrderedMap#concat()

用其他值返回一个具有相同类型的新Iterable，并将其连接到此类。

```javascript
concat(...valuesOrIterables: any[]): Iterable<K, V>
```

#### 继承

```
Iterable#concat
```

#### 讨论

对于Seqs，即使它们具有相同的密钥，所有条目也会出现在所得到的迭代中。

### OrderedMap#flatten()

压扁嵌套的Iterables。

```javascript
flatten(depth?: number): Iterable<any, any>
flatten(shallow?: boolean): Iterable<any, any>
```

#### 继承

```
Iterable#flatten
```

#### 讨论

默认情况下会严格地将Iterable扁平化，返回一个相同类型的Iterable，但`depth`可以以数字或布尔值的形式提供（其中true表示浅层扁平化）。深度为0（或者浅：假）将会变得很平坦。

仅将其他的Iterable变为Flattens，而不是阵列或对象。

注意：`flatten(true)`在Iterable>上运行并返回Iterable

### OrderedMap#flatMap()

平面映射Iterable，返回相同类型的Iterable。

```javascript
flatMap<MK, MV>(mapper: (value?: V, key?: K, iter?: Iterable<K, V>) => Iterable<MK, MV>,context?: any): Iterable<MK, MV>
flatMap<MK, MV>(mapper: (value?: V, key?: K, iter?: Iterable<K, V>) => any,context?: any): Iterable<MK, MV>
```

#### 继承

```
Iterable#flatMap
```

#### 讨论

类似于`iter.map(...).flatten(true)`。

## 降低价值

### OrderedMap#reduce()

通过调用Iterable中的`reducer`每个条目并传递缩小的值，将Iterable减少为一个值。

```javascript
reduce<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### 继承

```
Iterable#reduce
```

#### 参见

[`Array#reduce`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce).

#### 讨论

如果`initialReduction`未提供，或者为空，则将使用Iterable中的第一项。

### OrderedMap#reduceRight()

反向（从右侧）减少Iterable。

```javascript
reduceRight<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### 继承

```
Iterable#reduceRight
```

#### 讨论

注意：类似于this.reverse()。reduce()，并提供与奇偶校验[`Array#reduceRight`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight)。

### OrderedMap#every()

如果`predicate`对Iterable中的所有条目返回true，则返回true。

```javascript
every(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#every
```

### OrderedMap#some()

如果`predicate`对Iterable中的任何条目返回true，则返回true。

```javascript
some(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#some
```

### OrderedMap#join()

将值作为字符串连接在一起，在每个值之间插入一个分隔符。默认分隔符是`","`。

```javascript
join(separator?: string): string
```

#### 继承

```
Iterable#join
```

### OrderedMap#isEmpty()

如果此Iterable不包含任何值，则返回true。

```javascript
isEmpty(): boolean
```

#### 继承

```
Iterable#isEmpty
```

#### 讨论

对于一些懒惰的人`Seq`，`isEmpty`可能需要迭代以确定空虚。至多会发生一次迭代。

### OrderedMap#count()

返回此Iterable的大小。

```javascript
count(): number
count(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): number
```

#### 继承

```
Iterable#count
```

#### 讨论

不管这个Iterable是否可以懒惰地描述它的大小（有些Seqs不能），这个方法总是会返回正确的大小。例如，`Seq`如果需要，它会评估一个懒惰。

如果`predicate`提供，则返回Iterable中`predicate`返回值为true 的条目的计数。

### OrderedMap#countBy()

返回一个`Seq.Keyed`计数，按`grouper`函数的返回值分组。

```javascript
countBy<G>(grouper: (value?: V, key?: K, iter?: Iterable<K, V>) => G,context?: any): Map<G, number>
```

#### 继承

```
Iterable#countBy
```

#### 讨论

注意：这不是一个懒惰的操作。

## 搜索价值

### OrderedMap#find()

返回`predicate`返回true 的第一个值。

```javascript
find(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### 继承

```
Iterable#find
```

### OrderedMap#findLast()

返回返回值为`predicate`true 的最后一个值。

```javascript
findLast(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### 继承

```
Iterable#findLast
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### OrderedMap#findEntry()

返回返回值为true的第一个键值对`predicate`。

```javascript
findEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

#### 继承

```
Iterable#findEntry
```

### OrderedMap#findLastEntry()

返回返回值为true的最后一个键值对`predicate`。

```javascript
findLastEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

#### 继承

```
Iterable#findLastEntry
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### OrderedMap#findKey()

返回`predicate`返回true 的键。

```javascript
findKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

#### 继承

```
Iterable#findKey
```

### OrderedMap#findLastKey()

返回`predicate`返回true 的最后一个键。

```javascript
findLastKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

#### 继承

```
Iterable#findLastKey
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### OrderedMap#keyOf()

返回与搜索值关联的键，或者未定义。

```javascript
keyOf(searchValue: V): K
```

#### 继承

```
Iterable#keyOf
```

### OrderedMap#lastKeyOf()

返回与搜索值关联的最后一个键，或者未定义。

```javascript
lastKeyOf(searchValue: V): K
```

#### 继承

```
Iterable#lastKeyOf
```

### OrderedMap#max()

返回此集合中的最大值。如果任何值相当相等，则找到的第一个将被返回。

```javascript
max(comparator?: (valueA: V, valueB: V) => number): V
```

#### 继承

```
Iterable#max
```

#### 讨论

在`comparator`以同样的方式使用`Iterable#sort`。如果未提供，则默认比较器为`>`。

当两个值被认为是等价的，遇到的第一个将被返回。否则，`max`只要比较器是可交换的，将独立于输入的顺序进行操作。默认比较器*只有*在类型不相同*时才*`>`可以交换。

如果`comparator`返回0，且其中任一值为NaN，undefined或null，则将返回该值。

### OrderedMap#maxBy()

就像`max`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

```javascript
maxBy<C>(comparatorValueMapper: (value?: V, key?: K, iter?: Iterable<K, V>) => C,comparator?: (valueA: C, valueB: C) => number): V
```

#### 继承

```
Iterable#maxBy
```

#### 例

```javascript
hitters.maxBy(hitter => hitter.avgHits);
```

### OrderedMap#min()

返回此集合中的最小值。如果任何值相当相等，则找到的第一个将被返回。

```javascript
min(comparator?: (valueA: V, valueB: V) => number): V
```

#### 继承

```
Iterable#min
```

#### 讨论

在`comparator`以同样的方式使用`Iterable#sort`。如果未提供，则默认比较器为`<`。

当两个值被认为是等价的，遇到的第一个将被返回。否则，`min`只要比较器是可交换的，将独立于输入的顺序进行操作。默认比较器*只有*在类型不相同*时才*`<`可以交换。

如果`comparator`返回0，且其中任一值为NaN，undefined或null，则将返回该值。

### OrderedMap#minBy()

喜欢`min`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

```javascript
minBy<C>(comparatorValueMapper: (value?: V, key?: K, iter?: Iterable<K, V>) => C,comparator?: (valueA: C, valueB: C) => number): V
```

#### 继承

```
Iterable#minBy
```

#### 例

```javascript
hitters.minBy(hitter => hitter.avgHits);
```

## 对照

### OrderedMap#isSubset()

如果`iter`包含此Iterable中的每个值，则为真。

```javascript
isSubset(iter: Iterable<any, V>): boolean
isSubset(iter: Array<V>): boolean
```

#### 继承

```
Iterable#isSubset
```

### OrderedMap#isSuperset()

如果此Iterable包含每个值，则为真`iter`。

```javascript
isSuperset(iter: Iterable<any, V>): boolean
isSuperset(iter: Array<V>): boolean
```

#### 继承

```
Iterable#isSuperset
```

## 序列功能

### OrderedMap#flip()

返回键和值已翻转的同一类型的新Iterable.Keyed。

```javascript
flip(): Iterable.Keyed<V, K>
```

#### 继承

```
Iterable.Keyed#flip
```

#### 例

```javascript
Seq({ a: 'z', b: 'y' }).flip() // { z: 'a', y: 'b' }
```

### OrderedMap#mapKeys()

使用通过`mapper`函数传递的键返回相同类型的新Iterable.Keyed 。

```javascript
mapKeys<M>(mapper: (key?: K, value?: V, iter?: Iterable.Keyed<K, V>) => M,context?: any): Iterable.Keyed<M, V>
```

#### 继承

```
Iterable.Keyed#mapKeys
```

#### 例

```javascript
Seq({ a: 1, b: 2 })
  .mapKeys(x => x.toUpperCase())
// Seq { A: 1, B: 2 }
```

### OrderedMap#mapEntries()

通过`mapper`函数传递条目（键，值元组），返回相同类型的新Iterable.Keyed 。

```javascript
mapEntries<KM, VM>(mapper: (entry?: Array<any>,index?: number,iter?: Iterable.Keyed<K, V>) => Array<any>,context?: any): Iterable.Keyed<KM, VM>
```

#### 继承于

```
Iterable.Keyed#mapEntries
```

示例

```javascript
Seq({ a: 1, b: 2 })
  .mapEntries(([k, v]) => [k.toUpperCase(), v * 2])
// Seq { A: 2, B: 4 }
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-04-11