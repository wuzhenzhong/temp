# List

列表是有序索引的密集集合，很像JavaScript数组。

```javascript
class List<T> extends Collection.Indexed<T>
```

#### Discussion

列表是不可变的，并且完全持久化，O（log32 N）获取和设置，以及O（1）推送和弹出。

列出实现Deque，并从end（`push`，`pop`）和beginning（`unshift`，`shift`）中有效地添加和删除。

与JavaScript数组不同，“未设置”索引与设置为的索引之间没有区别`undefined`。`List#forEach`不管它们是否被明确定义，都会访问从0到大小的所有索引。

## Construction

### List()

创建一个包含所提供的iterable-like的值的新的不可变列表。

```javascript
List<T>(): List<T>
List<T>(iter: Iterable.Indexed<T>): List<T>
List<T>(iter: Iterable.Set<T>): List<T>
List<K, V>(iter: Iterable.Keyed<K, V>): List<any>
List<T>(array: Array<T>): List<T>
List<T>(iterator: Iterator<T>): List<T>
List<T>(iterable: Object): List<T>
```

## Static methods

### List.isList()

如果提供的值是List，则为true

```javascript
List.isList(maybeList: any): boolean
```

### List.of()

创建一个包含的新列表`values`。

```javascript
List.of<T>(...values: T[]): List<T>
```

## Members

### List#size

```javascript
size: number
```

#### Inherited from

```
Collection#size
```

## Persistent changes

### List#set()

返回包含`value` 的新列表`index`。如果`index`已经存在于此列表中，它将被替换。

```javascript
set(index: number, value: T): List<T>
```

#### Discussion

`index`可能是一个负数，从列表末尾开始索引。`v.set(-1, "value")`设置列表中的最后一个项目。

如果`index`大于`size`，返回的List `size`将会足够大以包含`index`。

### List#delete()

返回一个新的列表，它排除了这个`index`，并且大小小于这个列表。上述指数的值`index`向下移动1以填补位置。

```javascript
delete(index: number): List<T>
```

#### alias

```
remove()
```

#### Discussion

这是同义词`list.splice(index, 1)`。

`index`可能是一个负数，从列表末尾开始索引。`v.delete(-1)`删除列表中的最后一个项目。

注意：`delete`不能在IE8中安全使用

### List#insert()

返回带有`value`at 的新列表，`index`其大小为1，大于此列表。上述指数的值`index`移动了1。

```javascript
insert(index: number, value: T): List<T>
```

#### Discussion

这与`list.splice(index,0,value)是同义词，

### List#clear()

返回一个具有0大小且没有值的新列表。

```javascript
clear(): List<T>
```

### List#push()

从列表中返回一个新的列表，并`values`附带提供的`size`。

```javascript
push(...values: T[]): List<T>
```

### List#pop()

返回一个新列表，其大小小于此列表，不包括此列表中的最后一个索引。

```javascript
pop(): List<T>
```

#### Discussion

注意：这不同于[`Array#pop`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)因为它返回一个新的列表而不是删除的值。使用`last()`获得此列表中最后一个值。

### List#unshift()

返回一个新的列表，并提供`values`预先提供的值，将其他值前移到较高的索引处。

```javascript
unshift(...values: T[]): List<T>
```

### List#shift()

返回一个新列表，其大小小于此列表，不包括此列表中的第一个索引，将所有其他值移至较低索引。

```javascript
shift(): List<T>
```

#### Discussion

注意：这不同于[`Array#shift`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)因为它返回一个新的列表而不是删除的值。使用`first()`获得此列表中第一个值。

### List#update()

返回一个新的List，其中更新值为`index`，返回值是调用`updater`现有值，或者`notSetValue`是否`index`未设置。如果使用单个参数调用，`updater`则使用List自身调用。

```javascript
update(updater: (value: List<T>) => List<T>): List<T>
update(index: number, updater: (value: T) => T): List<T>
update(index: number, notSetValue: T, updater: (value: T) => T): List<T>
```

#### see

```
Map#update
```

#### Discussion

`index`可能是一个负数，从列表末尾开始索引。`v.update(-1)`更新列表中的最后一个项目。

### List#merge()

```javascript
merge(...iterables: Iterable.Indexed<T>[]): List<T>
merge(...iterables: Array<T>[]): List<T>
```

#### see

```
Map#merge
```

### List#mergeWith()

```javascript
mergeWith(merger: (previous?: T, next?: T, key?: number) => T,...iterables: Iterable.Indexed<T>[]): List<T>
mergeWith(merger: (previous?: T, next?: T, key?: number) => T,...iterables: Array<T>[]): List<T>
```

#### see

```
Map#mergeWith
```

### List#mergeDeep()

```javascript
mergeDeep(...iterables: Iterable.Indexed<T>[]): List<T>
mergeDeep(...iterables: Array<T>[]): List<T>
```

#### see

```
Map#mergeDeep
```

### List#mergeDeepWith()

```javascript
mergeDeepWith(merger: (previous?: T, next?: T, key?: number) => T,...iterables: Iterable.Indexed<T>[]): List<T>
mergeDeepWith(merger: (previous?: T, next?: T, key?: number) => T,...iterables: Array<T>[]): List<T>
```

#### see

```
Map#mergeDeepWith
```

### List#setSize()

返回一个带有大小的新列表`size`。如果`size`小于此列表的大小，则新列表将排除较高索引处的值。如果`size`大于此列表的大小，新列表将为新近可用的索引定义未定义的值。

```javascript
setSize(size: number): List<T>
```

#### Discussion

当建立一个新的列表并且最终大小被预先知道时，`setSize`与其一起使用`withMutations`可能导致更高性能的构造。

## Deep persistent changes

### List#setIn()

返回已经树立了新的列表`value`在此`keyPath`。如果任何键`keyPath`不存在，将在该键上创建一个新的不可变Map。

```javascript
setIn(keyPath: Array<any>, value: any): List<T>
setIn(keyPath: Iterable<any, any>, value: any): List<T>
```

#### Discussion

索引号被用作关键字来确定列表中的路径。

### List#deleteIn()

返回已删除此值的新列表`keyPath`。如果任何键`keyPath`不存在，则不会发生变化。

```javascript
deleteIn(keyPath: Array<any>): List<T>
deleteIn(keyPath: Iterable<any, any>): List<T>
```

#### alias

```
removeIn()
```

### List#updateIn()

```javascript
updateIn(keyPath: Array<any>, updater: (value: any) => any): List<T>
updateIn(keyPath: Array<any>,notSetValue: any,updater: (value: any) => any): List<T>
updateIn(keyPath: Iterable<any, any>, updater: (value: any) => any): List<T>
updateIn(keyPath: Iterable<any, any>,notSetValue: any,updater: (value: any) => any): List<T>
```

#### see

```
Map#updateIn
```

### List#mergeIn()

```javascript
mergeIn(keyPath: Iterable<any, any>,...iterables: Iterable.Indexed<T>[]): List<T>
mergeIn(keyPath: Array<any>, ...iterables: Iterable.Indexed<T>[]): List<T>
mergeIn(keyPath: Array<any>, ...iterables: Array<T>[]): List<T>
```

#### see

```
Map#mergeIn
```

### List#mergeDeepIn()

```javascript
mergeDeepIn(keyPath: Iterable<any, any>,...iterables: Iterable.Indexed<T>[]): List<T>
mergeDeepIn(keyPath: Array<any>, ...iterables: Iterable.Indexed<T>[]): List<T>
mergeDeepIn(keyPath: Array<any>, ...iterables: Array<T>[]): List<T>
```

#### see

```
Map#mergeDeepIn
```

## Transient changes

### List#withMutations()

注意：并非所有方法都可用于可变集合或内部`withMutations`！只有`set`，`push`，`pop`，`shift`，`unshift`和`merge`可变化的使用。

```javascript
withMutations(mutator: (mutable: List<T>) => any): List<T>
```

#### see

```
Map#withMutations
```

### List#asMutable()

```javascript
asMutable(): List<T>
```

#### see

```
Map#asMutable
```

### List#asImmutable()

```javascript
asImmutable(): List<T>
```

#### see

```
Map#asImmutable
```

## Conversion to Seq

### List#toSeq()

返回Seq.Indexed。

```javascript
toSeq(): Seq.Indexed<T>
```

#### Inherited from

```
Collection.Indexed#toSeq
```

### List#toKeyedSeq()

从此Iterable返回一个Seq.Keyed，其索引被视为键。

```javascript
toKeyedSeq(): Seq.Keyed<number, T>
```

#### Inherited from

```
Iterable#toKeyedSeq
```

#### Discussion

如果您想要对Iterable.Indexed进行操作并保留索引那么这非常有用。

返回的Seq将具有与此Iterable相同的迭代顺序。

Example:

```javascript
var indexedSeq = Immutable.Seq.of('A', 'B', 'C');
indexedSeq.filter(v => v === 'B').toString() // Seq [ 'B' ]
var keyedSeq = indexedSeq.toKeyedSeq();
keyedSeq.filter(v => v === 'B').toString() // Seq { 1: 'B' }
```

### List#toIndexedSeq()

返回一个Seq.Indexed这个Iterable的值，这是丢弃键。

```javascript
toIndexedSeq(): Seq.Indexed<T>
```

#### Inherited from

```
Iterable#toIndexedSeq
```

### List#toSetSeq()

返回一个Seq.Set这个Iterable的值，这是丢弃键。

```javascript
toSetSeq(): Seq.Set<T>
```

#### Inherited from

```
Iterable#toSetSeq
```

### List#fromEntrySeq()

如果这是一个可迭代的键值元组，它将返回这些条目的Seq.Keyed。

```javascript
fromEntrySeq(): Seq.Keyed<any, any>
```

#### Inherited from

```
Iterable.Indexed#fromEntrySeq
```

## Value equality

### List#equals()

如果这和另一个Iterable具有值相等性，则为真，如下定义`Immutable.is()`。

```javascript
equals(other: Iterable<number, T>): boolean
```

#### Inherited from

```
Iterable#equals
```

#### Discussion

注意：这相当于`Immutable.is(this, other)`，但提供允许链式表达式。

### List#hashCode()

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

### List#get()

返回与提供的键相关联的值，如果Iterable不包含此键，则返回notSetValue。

```javascript
get(key: number, notSetValue?: T): T
```

#### Inherited from

```
Iterable#get
```

#### Discussion

注意：一个键可能与一个`undefined`值相关联，所以如果`notSetValue`没有提供并且该方法返回`undefined`，那么不能保证没有找到该键。

### List#has()

如果此关键字存在`Iterable`，则为真，`Immutable.is`用于确定相等性

```javascript
has(key: number): boolean
```

#### Inherited from

```
Iterable#has
```

### List#includes()

如果此值中存在值`Iterable`则为true ，`Immutable.is`用于确定相等性

```javascript
includes(value: T): boolean
```

#### Inherited from

```
Iterable#includes
```

#### alias

```
contains()
```

### List#first()

Iterable中的第一个值。

```javascript
first(): T
```

#### Inherited from

```
Iterable#first
```

### List#last()

Iterable中的最后一个值。

```javascript
last(): T
```

#### Inherited from

```
Iterable#last
```

## Reading deep values

### List#getIn()

通过嵌套的Iterables返回键或索引路径的值。

```javascript
getIn(searchKeyPath: Array<any>, notSetValue?: any): any
getIn(searchKeyPath: Iterable<any, any>, notSetValue?: any): any
```

#### Inherited from

```
Iterable#getIn
```

### List#hasIn()

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

### List#toJS()

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

`Iterable.Indexeds`，`Iterable.Sets`成为阵列，同时`Iterable.Keyeds`成为物体。

### List#toArray()

浅显地将这个迭代器转换为一个Array，这是丢弃键。

```javascript
toArray(): Array<T>
```

#### Inherited from

```
Iterable#toArray
```

### List#toObject()

将此Iterable浅转换为一个Object。

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

### List#toMap()

将此Iterable转换为Map，如果键不可哈希则抛出。

```javascript
toMap(): Map<number, T>
```

#### Inherited from

```
Iterable#toMap
```

#### Discussion

注意：这相当于`Map(this.toKeyedSeq())`，但为方便起见并允许链接表达式。

### List#toOrderedMap()

将此Iterable转换为Map，并保持迭代顺序。

```javascript
toOrderedMap(): OrderedMap<number, T>
```

#### Inherited from

```
Iterable#toOrderedMap
```

#### Discussion

注意：这相当于`OrderedMap(this.toKeyedSeq())`，但为方便起见并允许链接表达式。

### List#toSet()

将此Iterable转换为Set，放弃键。如果值不可哈希则抛出。

```javascript
toSet(): Set<T>
```

#### Inherited from

```
Iterable#toSet
```

#### Discussion

注意：这相当于`Set(this)`，但提供允许链式表达式。

### List#toOrderedSet()

将此Iterable转换为Set，保持迭代顺序并丢弃键。

```javascript
toOrderedSet(): OrderedSet<T>
```

#### Inherited from

```
Iterable#toOrderedSet
```

#### Discussion

注意：这相当于`OrderedSet(this.valueSeq())`，但为方便起见并允许链接表达式。

### List#toList()

将此Iterable转换为List，放弃键。

```javascript
toList(): List<T>
```

#### Inherited from

```
Iterable#toList
```

#### Discussion

注意：这相当于`List(this)`，但提供允许链式表达式。

### List#toStack()

将此Iterable转换为堆栈，丢弃键。如果值不可哈希则抛出。

```javascript
toStack(): Stack<T>
```

#### Inherited from

```
Iterable#toStack
```

#### Discussion

注意：这相当于`Stack(this)`，但提供允许链式表达式。

## Iterators

### List#keys()

这个`Iterable`键的迭代器。

```javascript
keys(): Iterator<number>
```

#### Inherited from

```
Iterable#keys
```

#### Discussion

注意：这将返回一个不支持Immutable JS序列算法的ES6迭代器。使用`keySeq`替代，如果这是你想要的。

### List#values()

这个`Iterable`值的迭代器。

```javascript
values(): Iterator<T>
```

#### Inherited from

```
Iterable#values
```

#### Discussion

注意：这将返回一个不支持Immutable JS序列算法的ES6迭代器。使用`valueSeq`替代，如果这是你想要的。

### List#entries()

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

### List#keySeq()

返回此Iterable的新键的Seq.Indexed，放弃值。

```javascript
keySeq(): Seq.Indexed<number>
```

#### Inherited from

```
Iterable#keySeq
```

### List#valueSeq()

返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
valueSeq(): Seq.Indexed<T>
```

#### Inherited from

```
Iterable#valueSeq
```

### List#entrySeq()

返回一个新的Seq.Indexed键值值元组。

```javascript
entrySeq(): Seq.Indexed<Array<any>>
```

#### Inherited from

```
Iterable#entrySeq
```

## Sequence algorithms

### List#map()

使用通过`mapper`函数传递的值返回相同类型的新Iterable 。

```javascript
map<M>(mapper: (value?: T, key?: number, iter?: Iterable<number, T>) => M,context?: any): Iterable<number, M>
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

### List#filter()

仅`predicate`返回函数返回true 的条目返回相同类型的新Iterable 。

```javascript
filter(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any): Iterable<number, T>
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

### List#filterNot()

仅`predicate`返回函数返回false 的条目返回相同类型的新Iterable 。

```javascript
filterNot(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any): Iterable<number, T>
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

### List#reverse()

按相反顺序返回相同类型的新Iterable。

```javascript
reverse(): Iterable<number, T>
```

#### Inherited from

```
Iterable#reverse
```

### List#sort()

返回包含相同条目的相同类型的新Iterable，通过使用a进行稳定排序`comparator`。

```javascript
sort(comparator?: (valueA: T, valueB: T) => number): Iterable<number, T>
```

#### Inherited from

```
Iterable#sort
```

#### Discussion

如果`comparator`没有提供默认比较器使用`<` and `>`。

`comparator(valueA, valueB)`:

- 返回`0`元素不应该交换的情况。
- 返回`-1`（或任何负数）如果`valueA`之前`valueB`
- 返回`1`（或任何正数）如果`valueA`后来`valueB`
- 它是纯粹的， i.e.即它必须始终为同一对值返回相同的值。

排序没有定义顺序的集合时，它们的顺序等价物将被返回。例如`map.sort()`返回OrderedMap。

### List#sortBy()

就像`sort`，但也接受一个`comparatorValueMapper`允许更复杂的手段进行排序的一个方法：

```javascript
sortBy<C>(comparatorValueMapper: (value?: T,key?: number,iter?: Iterable<number, T>) => C,comparator?: (valueA: C, valueB: C) => number): Iterable<number, T>
```

#### Inherited from

```
Iterable#sortBy
```

#### Example

```javascript
hitters.sortBy(hitter => hitter.avgHits);
```

### List#groupBy()

返回`Iterable.Keyed`的`Iterable.Keyeds`，由返回值分组`grouper`功能。

```javascript
groupBy<G>(grouper: (value?: T, key?: number, iter?: Iterable<number, T>) => G,context?: any): Seq.Keyed<G, Iterable<number, T>>
```

#### Inherited from

```
Iterable#groupBy
```

#### Discussion

注意：这总是一个急切的操作。

## Side effects

### List#forEach()

该`sideEffect`是在可迭代的每个条目执行。

```javascript
forEach(sideEffect: (value?: T, key?: number, iter?: Iterable<number, T>) => any,context?: any): number
```

#### Inherited from

```
Iterable#forEach
```

#### Discussion

不同的是[`Array#forEach`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)，如果有任何`sideEffect`回报的话`false`，迭代将停止。返回迭代的条目数（包括返回false的最后一次迭代）。

## Creating subsets

### List#slice()

返回一个新的Iterable，其类型代表这个Iterable从开始到结束的一部分。

```javascript
slice(begin?: number, end?: number): Iterable<number, T>
```

#### Inherited from

```
Iterable#slice
```

#### Discussion

如果begin是负数，它将从Iterable的末尾偏移。例如`slice(-2)`返回最后两个条目的Iterable。如果没有提供，则新的Iterable将在此Iterable开始时开始。

如果end是负数，它将从Iterable的末尾偏移。例如`slice(0, -1)`返回除最后一项之外的所有内容的Iterable。如果没有提供，那么新的Iterable将会持续到这个Iterable的结尾。

如果所请求的分片等同于当前的Iterable，那么它将自行返回。

### List#rest()

返回包含除第一个以外的所有条目的同一类型的新Iterable。

```javascript
rest(): Iterable<number, T>
```

#### Inherited from

```
Iterable#rest
```

### List#butLast()

返回包含除最后一个以外的所有条目的同一类型的新Iterable。

```javascript
butLast(): Iterable<number, T>
```

#### Inherited from

```
Iterable#butLast
```

### List#skip()

返回`amount`从此Iterable中排除第一个条目的同一类型的新Iterable。

```javascript
skip(amount: number): Iterable<number, T>
```

#### Inherited from

```
Iterable#skip
```

### List#skipLast()

返回`amount`从此Iterable中排除最后一个条目的同一类型的新Iterable。

```javascript
skipLast(amount: number): Iterable<number, T>
```

#### Inherited from

```
Iterable#skipLast
```

### List#skipWhile()

返回包含从`predicate`第一个返回false 时开始的相同类型的新Iterable 。

```javascript
skipWhile(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any): Iterable<number, T>
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

### List#skipUntil()

返回包含从`predicate`第一个返回true 时开始的相同类型的新Iterable 。

```javascript
skipUntil(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any): Iterable<number, T>
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

### List#take()

返回包含`amount`此Iterable中第一个条目的相同类型的新Iterable。

```javascript
take(amount: number): Iterable<number, T>
```

#### Inherited from

```
Iterable#take
```

### List#takeLast()

返回包含`amount`此Iterable中最后一个条目的相同类型的新Iterable。

```javascript
takeLast(amount: number): Iterable<number, T>
```

#### Inherited from

```
Iterable#takeLast
```

### List#takeWhile()

返回包含来自此Iterable的条目的相同类型的新Iterable，只要`predicate`返回值为true即可。

```javascript
takeWhile(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any): Iterable<number, T>
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

### List#takeUntil()

返回包含来自此Iterable的条目的相同类型的新Iterable，只要`predicate`返回false即可。

```javascript
takeUntil(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any): Iterable<number, T>
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

### List#concat()

用其他值返回一个具有相同类型的新Iterable，并将其连接到此类。

```javascript
concat(...valuesOrIterables: any[]): Iterable<number, T>
```

#### Inherited from

```
Iterable#concat
```

#### Discussion

对于Seqs，即使它们具有相同的密钥，所有条目也会出现在所得到的迭代中。

### List#flatten()

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

默认情况下会严格地将Iterable扁平化，返回一个相同类型的Iterable，但`depth`可以以数字或布尔值的形式提供（其中true表示浅层扁平化）。深度为0（或者显示：假）将会变得很平坦。

仅将其他的Iterable变为Flattens，而不是阵列或对象。

注意：`flatten(true)`在Iterable>上运行并返回Iterable

### List#flatMap()

平面映射Iterable，返回相同类型的Iterable。

```javascript
flatMap<MK, MV>(mapper: (value?: T,key?: number,iter?: Iterable<number, T>) => Iterable<MK, MV>,context?: any): Iterable<MK, MV>
flatMap<MK, MV>(mapper: (value?: T, key?: number, iter?: Iterable<number, T>) => any,context?: any): Iterable<MK, MV>
```

#### Inherited from

```
Iterable#flatMap
```

#### Discussion

Similar to `iter.map(...).flatten(true)`.

### List#interpose()

返回此Iterable中`separator`每个项目之间的相同类型的Iterable。

```javascript
interpose(separator: T): Iterable.Indexed<T>
```

#### Inherited from

```
Iterable.Indexed#interpose
```

### List#interleave()

使用提供的`iterables`交错迭代到此迭代中，返回相同类型的Iterable 。

```javascript
interleave(...iterables: Array<Iterable<any, T>>): Iterable.Indexed<T>
```

#### Inherited from

```
Iterable.Indexed#interleave
```

#### Discussion

由此产生的Iterable包含每个的第一个项目，然后是每个项目的第二个项目等。

```javascript
I.Seq.of(1,2,3).interleave(I.Seq.of('A','B','C'))
// Seq [ 1, 'A', 2, 'B', 3, 'C' ]
```

最短的Iterable停止交错。

```javascript
I.Seq.of(1,2,3).interleave(
  I.Seq.of('A','B'),
  I.Seq.of('X','Y','Z')
)
// Seq [ 1, 'A', 'X', 2, 'B', 'Y' ]
```

### List#splice()

Splice通过用新值替换此Iterable的区域来返回新的索引Iterable。如果未提供值，则只跳过要删除的区域。

```javascript
splice(index: number, removeNum: number, ...values: any[]): Iterable.Indexed<T>
```

#### Inherited from

```
Iterable.Indexed#splice
```

#### Discussion

`index`可能是一个负数，从Iterable的末尾开始索引。`s.splice(-2)`在倒数第二项之后拼接。

```javascript
Seq(['a','b','c','d']).splice(1, 2, 'q', 'r', 's')
// Seq ['a', 'q', 'r', 's', 'd']
```

### List#zip()

使用提供的迭代器返回“压缩”相同类型的Iterable。

```javascript
zip(...iterables: Array<Iterable<any, any>>): Iterable.Indexed<any>
```

#### Inherited from

```
Iterable.Indexed#zip
```

#### Discussion

喜欢`zipWith`，但使用默认值`zipper`：创建一个[`Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)。

```javascript
var a = Seq.of(1, 2, 3);
var b = Seq.of(4, 5, 6);
var c = a.zip(b); // Seq [ [ 1, 4 ], [ 2, 5 ], [ 3, 6 ] ]
```

### List#zipWith()

使用自定义`zipper`函数返回与提供的迭代“压缩”相同类型的Iterable 。

```javascript
zipWith<U, Z>(zipper: (value: T, otherValue: U) => Z,otherIterable: Iterable<any, U>): Iterable.Indexed<Z>
zipWith<U, V, Z>(zipper: (value: T, otherValue: U, thirdValue: V) => Z,otherIterable: Iterable<any, U>,thirdIterable: Iterable<any, V>): Iterable.Indexed<Z>
zipWith<Z>(zipper: (...any: Array<any>) => Z,...iterables: Array<Iterable<any, any>>): Iterable.Indexed<Z>
```

#### Inherited from

```
Iterable.Indexed#zipWith
```

#### Example

```javascript
var a = Seq.of(1, 2, 3);
var b = Seq.of(4, 5, 6);
var c = a.zipWith((a, b) => a + b, b); // Seq [ 5, 7, 9 ]
```

## Reducing a value

### List#reduce()

通过调用Iterable中的`reducer`每个条目并传递缩小的值，将Iterable减少为一个值。

```javascript
reduce<R>(reducer: (reduction?: R,value?: T,key?: number,iter?: Iterable<number, T>) => R,initialReduction?: R,context?: any): R
```

#### Inherited from

```
Iterable#reduce
```

#### see

[`Array#reduce`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce).

#### Discussion

如果`initialReduction`未提供，或者为空，则将使用Iterable中的第一项。

### List#reduceRight()

反向（从右侧）减少Iterable。

```javascript
reduceRight<R>(reducer: (reduction?: R,value?: T,key?: number,iter?: Iterable<number, T>) => R,initialReduction?: R,context?: any): R
```

#### Inherited from

```
Iterable#reduceRight
```

#### Discussion

注意：类似于this.reverse()。reduce()，并提供与奇偶校验[`Array#reduceRight`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight)。

### List#every()

如果`predicate`对Iterable中的所有条目返回true，则返回true。

```javascript
every(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any): boolean
```

#### Inherited from

```
Iterable#every
```

### List#some()

如果`predicate`对Iterable中的任何条目返回true，则返回true。

```javascript
some(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any): boolean
```

#### Inherited from

```
Iterable#some
```

### List#join()

将值作为字符串连接在一起，在每个值之间插入一个分隔符。默认分隔符是`","`。

```javascript
join(separator?: string): string
```

#### Inherited from

```
Iterable#join
```

### List#isEmpty()

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

### List#count()

返回此Iterable的大小。

```javascript
count(): number
count(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any): number
```

#### Inherited from

```
Iterable#count
```

#### Discussion

不管这个Iterable是否可以懒惰地描述它的大小（有些Seqs不能），这个方法总是会返回正确的大小。例如，`Seq`如果需要，它会评估一个懒惰。

如果`predicate`提供，则返回Iterable中`predicate`返回值为true 的条目的计数。

### List#countBy()

返回一个`Seq.Keyed`计数，按`grouper`函数的返回值分组。

```javascript
countBy<G>(grouper: (value?: T, key?: number, iter?: Iterable<number, T>) => G,context?: any): Map<G, number>
```

#### Inherited from

```
Iterable#countBy
```

#### Discussion

注意：这不是一个懒惰的操作。

## Search for value

### List#find()

返回`predicate`返回true 的第一个值。

```javascript
find(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any,notSetValue?: T): T
```

#### Inherited from

```
Iterable#find
```

### List#findLast()

返回返回值为`predicate`true 的最后一个值。

```javascript
findLast(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any,notSetValue?: T): T
```

#### Inherited from

```
Iterable#findLast
```

#### Discussion

注意：`predicate`每个条目都会被调用。

### List#findEntry()

返回返回值为true的第一个键值对`predicate`。

```javascript
findEntry(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any,notSetValue?: T): Array<any>
```

#### Inherited from

```
Iterable#findEntry
```

### List#findLastEntry()

返回返回值为true的最后一个键值对`predicate`。

```javascript
findLastEntry(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any,notSetValue?: T): Array<any>
```

#### Inherited from

```
Iterable#findLastEntry
```

#### Discussion

注意：`predicate`每个条目都会被调用。

### List#findKey()

返回`predicate`返回true 的键。

```javascript
findKey(predicate: (value?: T,key?: number,iter?: Iterable.Keyed<number, T>) => boolean,context?: any): number
```

#### Inherited from

```
Iterable#findKey
```

### List#findLastKey()

返回`predicate`返回true 的最后一个键。

```javascript
findLastKey(predicate: (value?: T,key?: number,iter?: Iterable.Keyed<number, T>) => boolean,context?: any): number
```

#### Inherited from

```
Iterable#findLastKey
```

#### Discussion

注意：`predicate`每个条目都会被调用。

### List#keyOf()

返回与搜索值关联的键，或者未定义。

```javascript
keyOf(searchValue: T): number
```

#### Inherited from

```
Iterable#keyOf
```

### List#lastKeyOf()

返回与搜索值关联的最后一个键，或者未定义。

```javascript
lastKeyOf(searchValue: T): number
```

#### Inherited from

```
Iterable#lastKeyOf
```

### List#max()

返回此集合中的最大值。如果任何值相当相等，则找到的第一个将被返回。

```javascript
max(comparator?: (valueA: T, valueB: T) => number): T
```

#### Inherited from

```
Iterable#max
```

#### Discussion

在`comparator`以同样的方式使用`Iterable#sort`。如果未提供，则默认比较器为`>`。

当两个值被认为是等价的，遇到的第一个将被返回。否则，`max`只要比较器是可交换的，将独立于输入的顺序进行操作。默认比较器*只有*在类型不相同*时才*`>`可以交换。

如果`comparator`返回0，且其中任一值为NaN，undefined或null，则将返回该值。

### List#maxBy()

喜欢`max`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

```javascript
maxBy<C>(comparatorValueMapper: (value?: T,key?: number,iter?: Iterable<number, T>) => C,comparator?: (valueA: C, valueB: C) => number): T
```

#### Inherited from

```
Iterable#maxBy
```

#### Example

```javascript
hitters.maxBy(hitter => hitter.avgHits);
```

### List#min()

返回此集合中的最小值。如果任何值相当相等，则找到的第一个将被返回。

```javascript
min(comparator?: (valueA: T, valueB: T) => number): T
```

#### Inherited from

```
Iterable#min
```

#### Discussion

在`comparator`以同样的方式使用`Iterable#sort`。如果未提供，则默认比较器为`<`。

当两个值被认为是等价的，遇到的第一个将被返回。否则，`min`只要比较器是可交换的，将独立于输入的顺序进行操作。默认比较器*只有*在类型不相同*时才*`<`可以交换。

如果`comparator`返回0，且其中任一值为NaN，undefined或null，则将返回该值。

### List#minBy()

喜欢`min`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

```javascript
minBy<C>(comparatorValueMapper: (value?: T,key?: number,iter?: Iterable<number, T>) => C,comparator?: (valueA: C, valueB: C) => number): T
```

#### Inherited from

```
Iterable#minBy
```

#### Example

```javascript
hitters.minBy(hitter => hitter.avgHits);
```

### List#indexOf()

返回可以在Iterable中找到给定值的第一个索引，如果不存在则返回-1。

```javascript
indexOf(searchValue: T): number
```

#### Inherited from

```
Iterable.Indexed#indexOf
```

### List#lastIndexOf()

返回可以在Iterable中找到给定值的最后一个索引，如果不存在则返回-1。

```javascript
lastIndexOf(searchValue: T): number
```

#### Inherited from

```
Iterable.Indexed#lastIndexOf
```

### List#findIndex()

返回Iterable中的第一个索引，其中的值满足提供的谓词函数。否则返回-1。

```javascript
findIndex(predicate: (value?: T, index?: number, iter?: Iterable.Indexed<T>) => boolean,context?: any): number
```

#### Inherited from

```
Iterable.Indexed#findIndex
```

### List#findLastIndex()

返回一个值满足提供的谓词函数的Iterable中的最后一个索引。否则返回-1。

```javascript
findLastIndex(predicate: (value?: T, index?: number, iter?: Iterable.Indexed<T>) => boolean,context?: any): number
```

#### Inherited from

```
Iterable.Indexed#findLastIndex
```

## Comparison

### List#isSubset()

如果`iter`包含此Iterable中的每个值，则为真。

```javascript
isSubset(iter: Iterable<any, T>): boolean
isSubset(iter: Array<T>): boolean
```

#### Inherited from

```
Iterable#isSubset
```

### List#isSuperset()

如果此Iterable包含每个值，则为真`iter`。

```javascript
isSuperset(iter: Iterable<any, T>): boolean
isSuperset(iter: Array<T>): boolean
```

#### Inherited from

```
Iterable#isSuperset
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-04-11