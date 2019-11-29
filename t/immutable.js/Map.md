# Map

Immutable Map是带有get `O(log32 N)`和`O(log32 N)`持久集的（key，value）对的无序Iterable.Keyed 。

```javascript
class Map<K, V> extends Collection.Keyed<K, V>
```

#### Discussion

Map的迭代次序是未定义的，但是稳定。同一个Map的多次迭代将以相同的顺序迭代。

Map的键可以是任何类型，并用于`Immutable.is`确定键的相等性。这允许使用任何值（包括NaN）作为关键。

由于`Immutable.is`基于值语义的返回等式和不可变集合被视为值，因此任何Immutable集合都可以用作关键字。

```javascript
Map().set(List.of(1), 'listofone').get(List.of(1));
// 'listofone'
```

任何JavaScript对象都可以用作关键字，但严格标识用于评估关键的相等性。两个相似的对象将代表两个不同的键。

由一个哈希数组映射trie实现。

## Construction

### Map()

创建一个新的不可变映射。

```javascript
Map<K, V>(): Map<K, V>
Map<K, V>(iter: Iterable.Keyed<K, V>): Map<K, V>
Map<K, V>(iter: Iterable<any, Array<any>>): Map<K, V>
Map<K, V>(array: Array<Array<any>>): Map<K, V>
Map<V>(obj: {[key: string]: V}): Map<string, V>
Map<K, V>(iterator: Iterator<Array<any>>): Map<K, V>
Map<K, V>(iterable: Object): Map<K, V>
```

#### Discussion

使用与所提供的Iterable.Keyed或JavaScript对象相同的键值对创建，或期望可重用的K，V元组条目。

```javascript
var newMap = Map({key: "value"});
var newMap = Map([["key", "value"]]);
```

请记住，使用JS对象构建Immutable Maps时，JavaScript Object属性始终是字符串，即使以不含引号的简写形式书写，而Immutable Maps也会接受任何类型的键。

```javascript
var obj = { 1: "one" };
Object.keys(obj); // [ "1" ]
obj["1"]; // "one"
obj[1];   // "one"

var map = Map(obj);
map.get("1"); // "one"
map.get(1);   // undefined
```

JavaScript对象的属性访问首先将该键转换为一个字符串，但由于Immutable Map键的类型可以是任何类型，所以该参数`get()`不会被修改。

## Static methods

### Map.isMap()

如果提供的值是Map，则为true

```javascript
Map.isMap(maybeMap: any): boolean
```

### Map.of()

从交替的键和值创建一个新的Map

```javascript
Map.of(...keyValues: any[]): Map<any, any>
```

## Members

### Map#size

```javascript
size: number
```

#### Inherited from

```
Collection#size
```

## Persistent changes

### Map#set()

返回一个新的Map也包含新的键值对。如果此地图中已存在相同的密钥，它将被替换。

```javascript
set(key: K, value: V): Map<K, V>
```

### Map#delete()

返回排除这个的新地图`key`。

```javascript
delete(key: K): Map<K, V>
```

#### alias

```
remove()
```

#### Discussion

注意：`delete`不能安全地在IE8中使用，但提供用于镜像ES6集合API。

### Map#clear()

返回不包含任何键或值的新映射。

```javascript
clear(): Map<K, V>
```

### Map#update()

返回一个新的Map，该Map更新了`key`此处的值，并`updater`使用现有值调用返回值，或者`notSetValue`未设置该键。如果只用一个参数调用，`updater`则用Map自身调用。

```javascript
update(updater: (value: Map<K, V>) => Map<K, V>): Map<K, V>
update(key: K, updater: (value: V) => V): Map<K, V>
update(key: K, notSetValue: V, updater: (value: V) => V): Map<K, V>
```

#### Discussion

相当于：`map.set(key, updater(map.get(key, notSetValue)))`。

### Map#merge()

返回将提供的Iterables（或JS对象）合并到此Map中的新地图。换句话说，这需要每个迭代的每个条目并将其设置在该Map上。

```javascript
merge(...iterables: Iterable<K, V>[]): Map<K, V>
merge(...iterables: {[key: string]: V}[]): Map<string, V>
```

#### Discussion

如果提供的任何值`merge`不是Iterable（将返回false `Immutable.Iterable.isIterable`），那么它们`Immutable.fromJS`在合并前经过深度转换。但是，如果该值是一个Iterable但包含不可迭代的JS对象或数组，则这些嵌套值将被保留。

```javascript
var x = Immutable.Map({a: 10, b: 20, c: 30});
var y = Immutable.Map({b: 40, a: 50, d: 60});
x.merge(y) // { a: 50, b: 40, c: 30, d: 60 }
y.merge(x) // { b: 20, a: 10, d: 60, c: 30 }
```

### Map#mergeWith()

类似地`merge()`，`mergeWith()`返回一个新的Map，它将提供的Iterables（或JS对象）合并到此Map中，但使用该`merger`函数来处理冲突。

```javascript
mergeWith(merger: (previous?: V, next?: V, key?: K) => V,...iterables: Iterable<K, V>[]): Map<K, V>
mergeWith(merger: (previous?: V, next?: V, key?: K) => V,...iterables: {[key: string]: V}[]): Map<string, V>
```

#### Example

```javascript
var x = Immutable.Map({a: 10, b: 20, c: 30});
var y = Immutable.Map({b: 40, a: 50, d: 60});
x.mergeWith((prev, next) => prev / next, y) // { a: 0.2, b: 0.5, c: 30, d: 60 }
y.mergeWith((prev, next) => prev / next, x) // { b: 2, a: 5, d: 60, c: 30 }
```

### Map#mergeDeep()

就像`merge()`，但是当两个Iterables发生冲突时，它也将它们合并，并通过嵌套数据深入递归。

```javascript
mergeDeep(...iterables: Iterable<K, V>[]): Map<K, V>
mergeDeep(...iterables: {[key: string]: V}[]): Map<string, V>
```

#### Example

```javascript
var x = Immutable.fromJS({a: { x: 10, y: 10 }, b: { x: 20, y: 50 } });
var y = Immutable.fromJS({a: { x: 2 }, b: { y: 5 }, c: { z: 3 } });
x.mergeDeep(y) // {a: { x: 2, y: 10 }, b: { x: 20, y: 5 }, c: { z: 3 } }
```

### Map#mergeDeepWith()

就像`mergeDeep()`，但是当两个非Iterables冲突时，它使用`merger`函数来确定结果值。

```javascript
mergeDeepWith(merger: (previous?: V, next?: V, key?: K) => V,...iterables: Iterable<K, V>[]): Map<K, V>
mergeDeepWith(merger: (previous?: V, next?: V, key?: K) => V,...iterables: {[key: string]: V}[]): Map<string, V>
```

#### Example

```javascript
var x = Immutable.fromJS({a: { x: 10, y: 10 }, b: { x: 20, y: 50 } });
var y = Immutable.fromJS({a: { x: 2 }, b: { y: 5 }, c: { z: 3 } });
x.mergeDeepWith((prev, next) => prev / next, y)
// {a: { x: 5, y: 10 }, b: { x: 20, y: 10 }, c: { z: 3 } }
```

## Deep persistent changes

### Map#setIn()

返回已`value`在此设置的新地图`keyPath`。如果任何键`keyPath`不存在，将在该键上创建一个新的不可变Map。

```javascript
setIn(keyPath: Array<any>, value: any): Map<K, V>
setIn(KeyPath: Iterable<any, any>, value: any): Map<K, V>
```

### Map#deleteIn()

返回已删除此值的新地图`keyPath`。如果任何键`keyPath`不存在，则不会发生变化。

```javascript
deleteIn(keyPath: Array<any>): Map<K, V>
deleteIn(keyPath: Iterable<any, any>): Map<K, V>
```

#### alias

```
removeIn()
```

### Map#updateIn()

返回已应用`updater`到在keyPath处找到的条目的新Map 。

```javascript
updateIn(keyPath: Array<any>, updater: (value: any) => any): Map<K, V>
updateIn(keyPath: Array<any>,notSetValue: any,updater: (value: any) => any): Map<K, V>
updateIn(keyPath: Iterable<any, any>, updater: (value: any) => any): Map<K, V>
updateIn(keyPath: Iterable<any, any>,notSetValue: any,updater: (value: any) => any): Map<K, V>
```

#### Discussion

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

### Map#mergeIn()

的组合`updateIn`和`merge`，返回一个新的地图，但在一个点进行合并通过遵循的keyPath到达。换句话说，这两条线是等价的：

```javascript
mergeIn(keyPath: Iterable<any, any>, ...iterables: Iterable<K, V>[]): Map<K, V>
mergeIn(keyPath: Array<any>, ...iterables: Iterable<K, V>[]): Map<K, V>
mergeIn(keyPath: Array<any>, ...iterables: {[key: string]: V}[]): Map<string, V>
```

#### Example

```javascript
x.updateIn(['a', 'b', 'c'], abc => abc.merge(y));
x.mergeIn(['a', 'b', 'c'], y);
```

### Map#mergeDeepIn()

的组合`updateIn`和`mergeDeep`，返回一个新的地图，但在执行点深合并通过遵循的keyPath到达。换句话说，这两条线是等价的：

```javascript
mergeDeepIn(keyPath: Iterable<any, any>,...iterables: Iterable<K, V>[]): Map<K, V>
mergeDeepIn(keyPath: Array<any>, ...iterables: Iterable<K, V>[]): Map<K, V>
mergeDeepIn(keyPath: Array<any>,...iterables: {[key: string]: V}[]): Map<string, V>
```

#### Example

```javascript
x.updateIn(['a', 'b', 'c'], abc => abc.mergeDeep(y));
x.mergeDeepIn(['a', 'b', 'c'], y);
```

## Transient changes

### Map#withMutations()

每次调用上述函数之一时，都会创建一个新的不可变Map。如果一个纯函数调用其中的一些来产生最终返回值，那么通过创建所有中间不可变映射来支付对性能和内存的损失。

```javascript
withMutations(mutator: (mutable: Map<K, V>) => any): Map<K, V>
```

#### Discussion

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

### Map#asMutable()

另一种避免创建中间不可变映射的方法是创建该集合的可变副本。可变副本*总是*返回`this`，因此不应该用于平等。你的函数不应该返回一个集合的可变副本，只能在内部使用它来创建一个新的集合。如果可能，请使用`withMutations`它，因为它提供了更易于使用的API。

```javascript
asMutable(): Map<K, V>
```

#### Discussion

注意：如果集合已经是可变的，则`asMutable`返回它自己。

注意：并非所有方法都可用于可变集合或内部`withMutations`！只有`set`和`merge`可能会使用变化。

### Map#asImmutable()

阴是为了达到`asMutable`阴。因为它适用于可变集合，所以此操作是*可变的*并返回自身。一旦执行，可变副本变得不可变，并且可以从函数安全地返回。

```javascript
asImmutable(): Map<K, V>
```

## Conversion to Seq

### Map#toSeq()

返回Seq.Keyed。

```javascript
toSeq(): Seq.Keyed<K, V>
```

#### Inherited from

```
Collection.Keyed#toSeq
```

### Map#toKeyedSeq()

从此Iterable返回一个Seq.Keyed，其索引被视为键。

```javascript
toKeyedSeq(): Seq.Keyed<K, V>
```

#### Inherited from

```
Iterable#toKeyedSeq
```

#### Discussion

如果您想要对Iterable.Indexed进行操作并保留索引，该值非常有用。

返回的Seq将具有与此Iterable相同的迭代顺序。

Example:

```javascript
var indexedSeq = Immutable.Seq.of('A', 'B', 'C');
indexedSeq.filter(v => v === 'B').toString() // Seq [ 'B' ]
var keyedSeq = indexedSeq.toKeyedSeq();
keyedSeq.filter(v => v === 'B').toString() // Seq { 1: 'B' }
```

### Map#toIndexedSeq()

返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
toIndexedSeq(): Seq.Indexed<V>
```

#### Inherited from

```
Iterable#toIndexedSeq
```

### Map#toSetSeq()

返回一个Seq.Set这个Iterable的值，丢弃键。

```javascript
toSetSeq(): Seq.Set<V>
```

#### Inherited from

```
Iterable#toSetSeq
```

## Value equality

### Map#equals()

如果这和另一个Iterable具有值相等性，则为真，如下定义`Immutable.is()`。

```javascript
equals(other: Iterable<K, V>): boolean
```

#### Inherited from

```
Iterable#equals
```

#### Discussion

注意：这相当于`Immutable.is(this, other)`，但提供允许链式表达式。

### Map#hashCode()

计算并返回此Iterable的散列标识。

```javascript
hashCode(): number
```

#### Inherited from

```
Iterable#hashCode
```

#### Discussion

在`hashCode`一个可迭代的用于确定潜在平等，和添加这一个当使用`Set`或作为一个键`Map`，经由不同的实例实现查找。

```javascript
var a = List.of(1, 2, 3);
var b = List.of(1, 2, 3);
assert(a !== b); // different instances
var set = Set.of(a);
assert(set.has(b) === true);
```

如果两个值相同`hashCode`，则不能保证相等（[http://en.wikipedia.org/wiki/Collision_（computer_science％29](http://en.wikipedia.org/wiki/Collision_)）。如果两个值有不同的`hashCode`s，则它们不能相等。

## Reading values

### Map#get()

返回与提供的键相关联的值，如果Iterable不包含此键，则返回notSetValue。

```javascript
get(key: K, notSetValue?: V): V
```

#### Inherited from

```
Iterable#get
```

#### Discussion

注意：一个键可能与一个`undefined`值相关联，所以如果`notSetValue`没有提供并且该方法返回`undefined`，那么不能保证没有找到该键。

### Map#has()

如果此关键字存在`Iterable`，则为真，`Immutable.is`用于确定相等性

```javascript
has(key: K): boolean
```

#### Inherited from

```
Iterable#has
```

### Map#includes()

如果此值中存在值`Iterable`，则为true ，`Immutable.is`用于确定相等性

```javascript
includes(value: V): boolean
```

#### Inherited from

```
Iterable#includes
```

#### alias

```
contains()
```

### Map#first()

Iterable中的第一个值。

```javascript
first(): V
```

#### Inherited from

```
Iterable#first
```

### Map#last()

Iterable中的最后一个值。

```javascript
last(): V
```

#### Inherited from

```
Iterable#last
```

## Reading deep values

### Map#getIn()

通过嵌套的Iterables返回键或索引路径的值。

```javascript
getIn(searchKeyPath: Array<any>, notSetValue?: any): any
getIn(searchKeyPath: Iterable<any, any>, notSetValue?: any): any
```

#### Inherited from

```
Iterable#getIn
```

### Map#hasIn()

如果通过嵌套的Iterables跟随键或索引路径的结果导致设置值，则返回true。

```javascript
hasIn(searchKeyPath: Array<any>): boolean
hasIn(searchKeyPath: Iterable<any, any>): boolean
```

#### Inherited from

```
Iterable#hasIn
```

## Conversion to JavaScript types

### Map#toJS()

将此Iterable深度转换为等效的JS。

```javascript
toJS(): any
```

#### Inherited from

```
Iterable#toJS
```

#### alias

```
toJSON()
```

#### Discussion

`Iterable.Indexeds`，并`Iterable.Sets`成为阵列，同时`Iterable.Keyeds`成为物体。

### Map#toArray()

浅显地将这个迭代器转换为一个Array，丢弃键。

```javascript
toArray(): Array<V>
```

#### Inherited from

```
Iterable#toArray
```

### Map#toObject()

将此Iterable浅转换为Object。

```javascript
toObject(): {[key: string]: V}
```

#### Inherited from

```
Iterable#toObject
```

#### Discussion

如果键不是字符串，则抛出。

## Conversion to Collections

### Map#toMap()

将此Iterable转换为Map，如果键不可哈希则抛出。

```javascript
toMap(): Map<K, V>
```

#### Inherited from

```
Iterable#toMap
```

#### Discussion

注意：这相当于`Map(this.toKeyedSeq())`，但为方便起见并允许链接表达式。

### Map#toOrderedMap()

将此Iterable转换为Map，并保持迭代顺序。

```javascript
toOrderedMap(): OrderedMap<K, V>
```

#### Inherited from

```
Iterable#toOrderedMap
```

#### Discussion

注意：这相当于`OrderedMap(this.toKeyedSeq())`，但为方便起见并允许链接表达式。

### Map#toSet()

将此Iterable转换为Set，放弃键。如果值不可哈希则抛出。

```javascript
toSet(): Set<V>
```

#### Inherited from

```
Iterable#toSet
```

#### Discussion

注意：这相当于`Set(this)`，但提供允许链式表达式。

### Map#toOrderedSet()

将此Iterable转换为Set，保持迭代顺序并丢弃键。

```javascript
toOrderedSet(): OrderedSet<V>
```

#### Inherited from

```
Iterable#toOrderedSet
```

#### Discussion

注意：这相当于`OrderedSet(this.valueSeq())`，但为方便起见并允许链接表达式。

### Map#toList()

将此Iterable转换为List，放弃键。

```javascript
toList(): List<V>
```

#### Inherited from

```
Iterable#toList
```

#### Discussion

注意：这相当于`List(this)`，但提供允许链式表达式。

### Map#toStack()

将此Iterable转换为堆栈，丢弃键。如果值不可哈希则抛出。

```javascript
toStack(): Stack<V>
```

#### Inherited from

```
Iterable#toStack
```

#### Discussion

注意：这相当于`Stack(this)`，但提供允许链式表达式。

## Iterators

### Map#keys()

这个`Iterable`键的迭代器。

```javascript
keys(): Iterator<K>
```

#### Inherited from

```
Iterable#keys
```

#### Discussion

注意：这将返回一个不支持Immutable JS序列算法的ES6迭代器。使用`keySeq`替代，如果这是你想要的。

### Map#values()

这个`Iterable`值的迭代器。

```javascript
values(): Iterator<V>
```

#### Inherited from

```
Iterable#values
```

#### Discussion

注意：这将返回一个不支持Immutable JS序列算法的ES6迭代器。使用`valueSeq`替代，如果这是你想要的。

### Map#entries()

这个`Iterable`条目的迭代器作为`[key, value]`元组。

```javascript
entries(): Iterator<Array<any>>
```

#### Inherited from

```
Iterable#entries
```

#### Discussion

注意：这将返回一个不支持Immutable JS序列算法的ES6迭代器。使用`entrySeq`替代，如果这是你想要的。

## Iterables (Seq)

### Map#keySeq()

返回此Iterable的新键的Seq.Indexed，放弃值。

```javascript
keySeq(): Seq.Indexed<K>
```

#### Inherited from

```
Iterable#keySeq
```

### Map#valueSeq()

返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
valueSeq(): Seq.Indexed<V>
```

#### Inherited from

```
Iterable#valueSeq
```

### Map#entrySeq()

返回一个新的Seq.Indexed键值值元组。

```javascript
entrySeq(): Seq.Indexed<Array<any>>
```

#### Inherited from

```
Iterable#entrySeq
```

## Sequence algorithms

### map()

使用通过`mapper`函数传递的值返回相同类型的新Iterable 。

```javascript
map<M>(mapper: (value?: V, key?: K, iter?: Iterable<K, V>) => M,context?: any): Iterable<K, M>
```

#### Inherited from

```
Iterable#map
```

#### Example

```javascript
Seq({ a: 1, b: 2 }).map(x => 10 * x)
// Seq { a: 10, b: 20 }
```

### Map#filter()

仅`predicate`返回函数返回true 的条目返回相同类型的新Iterable 。

```javascript
filter(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### Inherited from

```
Iterable#filter
```

#### Example

```javascript
Seq({a:1,b:2,c:3,d:4}).filter(x => x % 2 === 0)
// Seq { b: 2, d: 4 }
```

### Map#filterNot()

仅`predicate`返回函数返回false 的条目返回相同类型的新Iterable 。

```javascript
filterNot(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### Inherited from

```
Iterable#filterNot
```

#### Example

```javascript
Seq({a:1,b:2,c:3,d:4}).filterNot(x => x % 2 === 0)
// Seq { a: 1, c: 3 }
```

### Map#reverse()

按相反顺序返回相同类型的新Iterable。

```javascript
reverse(): Iterable<K, V>
```

#### Inherited from

```
Iterable#reverse
```

### Map#sort()

返回包含相同条目的相同类型的新Iterable，通过使用a进行稳定排序`comparator`。

```javascript
sort(comparator?: (valueA: V, valueB: V) => number): Iterable<K, V>
```

#### Inherited from

```
Iterable#sort
```

#### Discussion

如果`comparator`没有提供a，默认比较器使用`<`和`>`。

`comparator(valueA, valueB)`:

- 返回`0`元素不应该交换的情况。
- 返回`-1`（或任何负数）如果`valueA`之前`valueB`
- 返回`1`（或任何正数）如果`valueA`后来`valueB`
- 它是纯粹的，即 i.e. 它必须始终为同一对值返回相同的值。

排序没有定义顺序的集合时，它们的顺序等价物将被返回。例如`map.sort()`返回OrderedMap。

### Map#sortBy()

就像`sort`，但也接受一个`comparatorValueMapper`允许更复杂的手段进行排序的一个：

```javascript
sortBy<C>(comparatorValueMapper: (value?: V, key?: K, iter?: Iterable<K, V>) => C,comparator?: (valueA: C, valueB: C) => number): Iterable<K, V>
```

#### Inherited from

```
Iterable#sortBy
```

#### Example

```javascript
hitters.sortBy(hitter => hitter.avgHits);
```

### Map#groupBy()

返回`Iterable.Keyed`的`Iterable.Keyeds`，由返回值分组`grouper`功能。

```javascript
groupBy<G>(grouper: (value?: V, key?: K, iter?: Iterable<K, V>) => G,context?: any): Seq.Keyed<G, Iterable<K, V>>
```

#### Inherited from

```
Iterable#groupBy
```

#### Discussion

注意：这总是一个急切的操作。

## Side effects

### Map#forEach()

该`sideEffect`是在可迭代的每个条目执行。

```javascript
forEach(sideEffect: (value?: V, key?: K, iter?: Iterable<K, V>) => any,context?: any): number
```

#### Inherited from

```
Iterable#forEach
```

#### Discussion

不同的是[`Array#forEach`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)，如果有任何`sideEffect`回报的话`false`，迭代将停止。返回迭代的条目数（包括返回false的最后一次迭代）。

## Creating subsets

### Map#slice()

返回一个新的Iterable，其类型代表这个Iterable从开始到结束的一部分。

```javascript
slice(begin?: number, end?: number): Iterable<K, V>
```

#### Inherited from

```
Iterable#slice
```

#### Discussion

如果begin是负数，它将从Iterable的末尾偏移。例如`slice(-2)`返回最后两个条目的Iterable。如果没有提供，则新的Iterable将在此Iterable开始时开始。

如果end是负数，它将从Iterable的末尾偏移。例如`slice(0, -1)`返回除最后一项之外的所有内容的Iterable。如果没有提供，那么新的Iterable将会持续到这个Iterable的结尾。

如果所请求的分片等同于当前的Iterable，那么它将自行返回。

### Map#rest()

返回包含除第一个以外的所有条目的同一类型的新Iterable。

```javascript
rest(): Iterable<K, V>
```

#### Inherited from

```
Iterable#rest
```

### Map#butLast()

返回包含除最后一个以外的所有条目的同一类型的新Iterable。

```javascript
butLast(): Iterable<K, V>
```

#### Inherited from

```
Iterable#butLast
```

### Map#skip()

返回`amount`从此Iterable中排除第一个条目的同一类型的新Iterable。

```javascript
skip(amount: number): Iterable<K, V>
```

#### Inherited from

```
Iterable#skip
```

### Map#skipLast()

返回`amount`从此Iterable中排除最后一个条目的同一类型的新Iterable。

```javascript
skipLast(amount: number): Iterable<K, V>
```

#### Inherited from

```
Iterable#skipLast
```

### Map#skipWhile()

返回包含从`predicate`第一个返回false 时开始的相同类型的新Iterable 。

```javascript
skipWhile(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### Inherited from

```
Iterable#skipWhile
```

#### Example

```javascript
Seq.of('dog','frog','cat','hat','god')
  .skipWhile(x => x.match(/g/))
// Seq [ 'cat', 'hat', 'god' ]
```

### Map#skipUntil()

返回包含从`predicate`第一个返回true 时开始的相同类型的新Iterable 。

```javascript
skipUntil(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### Inherited from

```
Iterable#skipUntil
```

#### Example

```javascript
Seq.of('dog','frog','cat','hat','god')
  .skipUntil(x => x.match(/hat/))
// Seq [ 'hat', 'god' ]
```

### Map#take()

返回包含`amount`此Iterable中第一个条目的相同类型的新Iterable。

```javascript
take(amount: number): Iterable<K, V>
```

#### Inherited from

```
Iterable#take
```

### Map#takeLast()

返回包含`amount`此Iterable中最后一个条目的相同类型的新Iterable。

```javascript
takeLast(amount: number): Iterable<K, V>
```

#### Inherited from

```
Iterable#takeLast
```

### Map#takeWhile()

返回包含来自此Iterable的条目的相同类型的新Iterable，只要`predicate`返回值为true即可。

```javascript
takeWhile(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### Inherited from

```
Iterable#takeWhile
```

#### Example

```javascript
Seq.of('dog','frog','cat','hat','god')
  .takeWhile(x => x.match(/o/))
// Seq [ 'dog', 'frog' ]
```

### Map#takeUntil()

返回包含来自此Iterable的条目的相同类型的新Iterable，只要`predicate`返回false即可。

```javascript
takeUntil(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### Inherited from

```
Iterable#takeUntil
```

#### Example

```javascript
Seq.of('dog','frog','cat','hat','god').takeUntil(x => x.match(/at/))
// ['dog', 'frog']
```

## Combination

### Map#concat()

用其他值返回一个具有相同类型的新Iterable，并将其连接到此类。

```javascript
concat(...valuesOrIterables: any[]): Iterable<K, V>
```

#### Inherited from

```
Iterable#concat
```

#### Discussion

对于Seqs，即使它们具有相同的密钥，所有条目也会出现在所得到的迭代中。

### Map#flatten()

压扁嵌套的Iterables。

```javascript
flatten(depth?: number): Iterable<any, any>
flatten(shallow?: boolean): Iterable<any, any>
```

#### Inherited from

```
Iterable#flatten
```

#### Discussion

默认情况下会严格地将Iterable扁平化，返回一个相同类型的Iterable，但`depth`可以以数字或布尔值的形式提供（其中true表示浅层扁平化）。深度为0（或者浅：假）将会变得很平坦。

仅将其他的Iterable变为Flattens，而不是阵列或对象。

注意：`flatten(true)`在Iterable>上运行并返回Iterable

### Map#flatMap()

平面映射Iterable，返回相同类型的Iterable。

```javascript
flatMap<MK, MV>(mapper: (value?: V, key?: K, iter?: Iterable<K, V>) => Iterable<MK, MV>,context?: any): Iterable<MK, MV>
flatMap<MK, MV>(mapper: (value?: V, key?: K, iter?: Iterable<K, V>) => any,context?: any): Iterable<MK, MV>
```

#### Inherited from

```
Iterable#flatMap
```

#### Discussion

Similar to `iter.map(...).flatten(true)`.

## Reducing a value

### Map#reduce()

通过调用Iterable中的`reducer`每个条目并传递缩小的值，将Iterable减少为一个值。

```javascript
reduce<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### Inherited from

```
Iterable#reduce
```

#### see

[`Array#reduce`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce).

#### Discussion

如果`initialReduction`未提供，或者为空，则将使用Iterable中的第一项。

### Map#reduceRight()

反向（从右侧）减少Iterable。

```javascript
reduceRight<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### Inherited from

```
Iterable#reduceRight
```

#### Discussion

注意：类似于this.reverse()。reduce()，并提供与奇偶校验[`Array#reduceRight`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight)。

### Map#every()

如果`predicate`对Iterable中的所有条目返回true，则返回true。

```javascript
every(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### Inherited from

```
Iterable#every
```

### Map#some()

如果`predicate`对Iterable中的任何条目返回true，则返回true。

```javascript
some(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### Inherited from

```
Iterable#some
```

### Map#join()

将值作为字符串连接在一起，在每个值之间插入一个分隔符。默认分隔符是`","`。

```javascript
join(separator?: string): string
```

#### Inherited from

```
Iterable#join
```

### Map#isEmpty()

如果此Iterable不包含任何值，则返回true。

```javascript
isEmpty(): boolean
```

#### Inherited from

```
Iterable#isEmpty
```

#### Discussion

对于一些懒惰的人`Seq`，`isEmpty`可能需要迭代以确定空虚。至多会发生一次迭代。

### Map#count()

返回此Iterable的大小。

```javascript
count(): number
count(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): number
```

#### Inherited from

```
Iterable#count
```

#### Discussion

不管这个Iterable是否可以懒惰地描述它的大小（有些Seqs不能），这个方法总是会返回正确的大小。例如，`Seq`如果需要，它会评估一个懒惰。

如果`predicate`提供，则返回Iterable中`predicate`返回值为true 的条目的计数。

### Map#countBy()

返回一个`Seq.Keyed`计数，按`grouper`函数的返回值分组。

```javascript
countBy<G>(grouper: (value?: V, key?: K, iter?: Iterable<K, V>) => G,context?: any): Map<G, number>
```

#### Inherited from

```
Iterable#countBy
```

#### Discussion

注意：这不是一个懒惰的操作。

## Search for value

### Map#find()

返回`predicate`返回true 的第一个值。

```javascript
find(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### Inherited from

```
Iterable#find
```

### Map#findLast()

返回返回值为`predicate`true 的最后一个值。

```javascript
findLast(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### Inherited from

```
Iterable#findLast
```

#### Discussion

注意：`predicate`每个条目都会被调用。

### Map#findEntry()

返回返回值为true的第一个键值对`predicate`。

```javascript
findEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

#### Inherited from

```
Iterable#findEntry
```

### Map#findLastEntry()

返回返回值为true的最后一个键值对`predicate`。

```javascript
findLastEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

#### Inherited from

```
Iterable#findLastEntry
```

#### Discussion

注意：`predicate`每个条目都会被调用。

### Map#findKey()

返回`predicate`返回true 键。

```javascript
findKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

#### Inherited from

```
Iterable#findKey
```

### Map#findLastKey()

返回`predicate`返回true 的最后一个键。

```javascript
findLastKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

#### Inherited from

```
Iterable#findLastKey
```

#### Discussion

注意：`predicate`每个条目都会被调用。

### Map#keyOf()

返回与搜索值关联的键，或者未定义。

```javascript
keyOf(searchValue: V): K
```

#### Inherited from

```
Iterable#keyOf
```

### Map#lastKeyOf()

返回与搜索值关联的最后一个键，或者未定义。

```javascript
lastKeyOf(searchValue: V): K
```

#### Inherited from

```
Iterable#lastKeyOf
```

### Map#max()

返回此集合中的最大值。如果任何值相当相等，则找到的第一个将被返回。

```javascript
max(comparator?: (valueA: V, valueB: V) => number): V
```

#### Inherited from

```
Iterable#max
```

#### Discussion

在`comparator`以同样的方式使用`Iterable#sort`。如果未提供，则默认比较器为`>`。

当两个值被认为是等价的，遇到的第一个将被返回。否则，`max`只要比较器是可交换的，将独立于输入的顺序进行操作。默认比较器*只有*在类型不相同*时才*`>`可以交换。

如果`comparator`返回0，且其中任一值为NaN，undefined或null，则将返回该值。

### Map#maxBy()

`就像max`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

```javascript
maxBy<C>(comparatorValueMapper: (value?: V, key?: K, iter?: Iterable<K, V>) => C,comparator?: (valueA: C, valueB: C) => number): V
```

#### Inherited from

```
Iterable#maxBy
```

#### Example

```javascript
hitters.maxBy(hitter => hitter.avgHits);
```

### Map#min()

返回此集合中的最小值。如果任何值相当相等，则找到的第一个将被返回。

```javascript
min(comparator?: (valueA: V, valueB: V) => number): V
```

#### Inherited from

```
Iterable#min
```

#### Discussion

在`comparator`以同样的方式使用`Iterable#sort`。如果未提供，则默认比较器为`<`。

当两个值被认为是等价的，遇到的第一个将被返回。否则，`min`只要比较器是可交换的，将独立于输入的顺序进行操作。默认比较器*只有*在类型不相同*时才*`<`可以交换。

如果`comparator`返回0，且其中任一值为NaN，undefined或null，则将返回该值。

### Map#minBy()

`就像min`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

```javascript
minBy<C>(comparatorValueMapper: (value?: V, key?: K, iter?: Iterable<K, V>) => C,comparator?: (valueA: C, valueB: C) => number): V
```

#### Inherited from

```
Iterable#minBy
```

#### Example

```javascript
hitters.minBy(hitter => hitter.avgHits);
```

## Comparison

### Map#isSubset()

如果`iter`包含此Iterable中的每个值，则为真。

```javascript
isSubset(iter: Iterable<any, V>): boolean
isSubset(iter: Array<V>): boolean
```

#### Inherited from

```
Iterable#isSubset
```

### Map#isSuperset()

如果此Iterable包含每个值，则为真`iter`。

```javascript
isSuperset(iter: Iterable<any, V>): boolean
isSuperset(iter: Array<V>): boolean
```

#### Inherited from

```
Iterable#isSuperset
```

## Sequence functions

### Map#flip()

返回键和值已翻转的同一类型的新Iterable.Keyed。

```javascript
flip(): Iterable.Keyed<V, K>
```

#### Inherited from

```
Iterable.Keyed#flip
```

#### Example

```javascript
Seq({ a: 'z', b: 'y' }).flip() // { z: 'a', y: 'b' }
```

### mapKeys()

使用通过`mapper`函数传递的键返回相同类型的新Iterable.Keyed 。

```javascript
mapKeys<M>(mapper: (key?: K, value?: V, iter?: Iterable.Keyed<K, V>) => M,context?: any): Iterable.Keyed<M, V>
```

#### Inherited from

```
Iterable.Keyed#mapKeys
```

#### Example

```javascript
Seq({ a: 1, b: 2 })
  .mapKeys(x => x.toUpperCase())
// Seq { A: 1, B: 2 }
```

### mapEntries()

通过`mapper`函数传递条目（键，值元组），返回相同类型的新Iterable.Keyed 。

```javascript
mapEntries<KM, VM>(mapper: (entry?: Array<any>,index?: number,iter?: Iterable.Keyed<K, V>) => Array<any>,context?: any): Iterable.Keyed<KM, VM>
```

#### Inherited from

```
Iterable.Keyed#mapEntries
```

#### Example

```javascript
Seq({ a: 1, b: 2 })
  .mapEntries(([k, v]) => [k.toUpperCase(), v * 2])
// Seq { A: 2, B: 4 }
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-04-11