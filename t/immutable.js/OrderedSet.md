# OrderedSet

一种类型的Set，它具有额外的保证，即值的迭代顺序将是它们被`add`编辑的顺序。

```javascript
class OrderedSet<T> extends Set<T>
```

#### Discussion

OrderedSet的迭代行为与原生ES6 Set相同。

请注意，这`OrderedSet`比无序更昂贵`Set`，可能会消耗更多的内存。`OrderedSet#add`分期付款O（log32 N），但不稳定。

## Construction

### OrderedSet()

创建一个包含所提供的类似iterable的值的新的不可变OrderedSet。

```javascript
OrderedSet<T>(): OrderedSet<T>
OrderedSet<T>(iter: Iterable.Set<T>): OrderedSet<T>
OrderedSet<T>(iter: Iterable.Indexed<T>): OrderedSet<T>
OrderedSet<K, V>(iter: Iterable.Keyed<K, V>): OrderedSet<any>
OrderedSet<T>(array: Array<T>): OrderedSet<T>
OrderedSet<T>(iterator: Iterator<T>): OrderedSet<T>
OrderedSet<T>(iterable: Object): OrderedSet<T>
```

## Static methods

### OrderedSet.isOrderedSet()

如果提供的值是OrderedSet，则为true。

```javascript
OrderedSet.isOrderedSet(maybeOrderedSet: any): boolean
```

### OrderedSet.of()

创建一个新的OrderedSet `values`。

```javascript
OrderedSet.of<T>(...values: T[]): OrderedSet<T>
```

### OrderedSet.fromKeys()

`OrderedSet.fromKeys()` 创建一个新的不可变OrderedSet，其中包含来自此Iterable或JavaScript Object的键。

```javascript
OrderedSet.fromKeys<T>(iter: Iterable<T, any>): OrderedSet<T>
OrderedSet.fromKeys(obj: {[key: string]: any}): OrderedSet<string>
```

## Members

### OrderedSet#size

```javascript
size: number
```

#### Inherited from

```
Collection#size
```

## Persistent changes

### OrderedSet#add()

返回一个新的Set，它也包含这个值。

```javascript
add(value: T): Set<T>
```

#### Inherited from

```
Set#add
```

### OrderedSet#delete()

返回排除此值的新Set。

```javascript
delete(value: T): Set<T>
```

#### Inherited from

```
Set#delete
```

#### alias

```
remove()
```

#### Discussion

注意：`delete`不能在IE8中安全使用

### OrderedSet#clear()

返回一个不包含值的新Set。

```javascript
clear(): Set<T>
```

#### Inherited from

```
Set#clear
```

### OrderedSet#union()

返回包含`iterables`该集合中尚不存在的任何值的集合。

```javascript
union(...iterables: Iterable<any, T>[]): Set<T>
union(...iterables: Array<T>[]): Set<T>
```

#### Inherited from

```
Set#union
```

#### alias

```
merge()
```

### OrderedSet#intersect()

返回一个Set，它删除了不包含在其中的任何值`iterables`。

```javascript
intersect(...iterables: Iterable<any, T>[]): Set<T>
intersect(...iterables: Array<T>[]): Set<T>
```

#### Inherited from

```
Set#intersect
```

### OrderedSet#subtract()

返回排除其中包含的值设置`iterables`。

```javascript
subtract(...iterables: Iterable<any, T>[]): Set<T>
subtract(...iterables: Array<T>[]): Set<T>
```

#### Inherited from

```
Set#subtract
```

## Transient changes

### OrderedSet#withMutations()

注意：并非所有方法都可用于可变集合或内部`withMutations`！只能`add`用于变异。

```javascript
withMutations(mutator: (mutable: Set<T>) => any): Set<T>
```

#### Inherited from

```
Set#withMutations
```

#### see

```
Map#withMutations
```

### OrderedSet#asMutable()

```javascript
asMutable(): Set<T>
```

#### Inherited from

```
Set#asMutable
```

#### see

```
Map#asMutable
```

### OrderedSet#asImmutable()

```javascript
asImmutable(): Set<T>
```

#### Inherited from

```
Set#asImmutable
```

#### see

```
Map#asImmutable
```

## Conversion to Seq

### OrderedSet#toSeq()

返回Seq.Set。

```javascript
toSeq(): Seq.Set<T>
```

#### Inherited from

```
Collection.Set#toSeq
```

### OrderedSet#toKeyedSeq()

从此Iterable返回一个Seq.Keyed，其索引被视为键。

```javascript
toKeyedSeq(): Seq.Keyed<T, T>
```

#### Inherited from

```
Iterable#toKeyedSeq
```

#### Discussion

如果您想要对Iterable.Indexed进行操作并保留索引，这对值非常有用。

返回的Seq将具有与此Iterable相同的迭代顺序。

Example:

```javascript
var indexedSeq = Immutable.Seq.of('A', 'B', 'C');
indexedSeq.filter(v => v === 'B').toString() // Seq [ 'B' ]
var keyedSeq = indexedSeq.toKeyedSeq();
keyedSeq.filter(v => v === 'B').toString() // Seq { 1: 'B' }
```

### OrderedSet#toIndexedSeq()

返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
toIndexedSeq(): Seq.Indexed<T>
```

#### Inherited from

```
Iterable#toIndexedSeq
```

### OrderedSet#toSetSeq()

返回一个Seq.Set这个Iterable的值，丢弃键。

```javascript
toSetSeq(): Seq.Set<T>
```

#### Inherited from

```
Iterable#toSetSeq
```

## Value equality

### OrderedSet#equals()

如果这和另一个Iterable具有值相等性，则为真，如下定义`Immutable.is()`。

```javascript
equals(other: Iterable<T, T>): boolean
```

#### Inherited from

```
Iterable#equals
```

#### Discussion

注意：这相当于`Immutable.is(this, other)`，但提供允许链式表达式。

### OrderedSet#hashCode()

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

### OrderedSet#get()

返回与提供的键相关联的值，如果Iterable不包含此键，则返回notSetValue。

```javascript
get(key: T, notSetValue?: T): T
```

#### Inherited from

```
Iterable#get
```

#### Discussion

注意：一个键可能与一个`undefined`值相关联，所以如果`notSetValue`没有提供并且该方法返回`undefined`，那么不能保证没有找到该键。

### OrderedSet#has()

如果此关键字存在`Iterable`则为真，`Immutable.is`用于确定相等性

```javascript
has(key: T): boolean
```

#### Inherited from

```
Iterable#has
```

### OrderedSet#includes()

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

### OrderedSet#first()

Iterable中的第一个值。

```javascript
first(): T
```

#### Inherited from

```
Iterable#first
```

### OrderedSet#last()

Iterable中的最后一个值。

```javascript
last(): T
```

#### Inherited from

```
Iterable#last
```

## Reading deep values

### OrderedSet#getIn()

通过嵌套的Iterables返回键或索引路径的值。

```javascript
getIn(searchKeyPath: Array<any>, notSetValue?: any): any
getIn(searchKeyPath: Iterable<any, any>, notSetValue?: any): any
```

#### Inherited from

```
Iterable#getIn
```

### OrderedSet#hasIn()

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

### OrderedSet#toJS()

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

### OrderedSet#toArray()

浅显地将这个迭代器转换为一个Array，丢弃键。

```javascript
toArray(): Array<T>
```

#### Inherited from

```
Iterable#toArray
```

### OrderedSet#toObject()

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

### OrderedSet#toMap()

将此Iterable转换为Map，如果键不可哈希则抛出。

```javascript
toMap(): Map<T, T>
```

#### Inherited from

```
Iterable#toMap
```

#### Discussion

注意：这相当于`Map(this.toKeyedSeq())`，但为方便起见并允许链接表达式。

### OrderedSet#toOrderedMap()

将此Iterable转换为Map，并保持迭代顺序。

```javascript
toOrderedMap(): OrderedMap<T, T>
```

#### Inherited from

```
Iterable#toOrderedMap
```

#### Discussion

注意：这相当于`OrderedMap(this.toKeyedSeq())`，但为方便起见并允许链接表达式。

### OrderedSet#toSet()

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

### OrderedSet#toOrderedSet()

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

### OrderedSet#toList()

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

### OrderedSet#toStack()

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

### OrderedSet#keys()

这个`Iterable`键的迭代器。

```javascript
keys(): Iterator<T>
```

#### Inherited from

```
Iterable#keys
```

#### Discussion

注意：这将返回一个不支持Immutable JS序列算法的ES6迭代器。使用`keySeq`替代，如果这是你想要的。

### OrderedSet#values()

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

### OrderedSet#entries()

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

### OrderedSet#keySeq()

返回此Iterable的新键的Seq.Indexed，放弃值。

```javascript
keySeq(): Seq.Indexed<T>
```

#### Inherited from

```
Iterable#keySeq
```

### OrderedSet#valueSeq()

返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
valueSeq(): Seq.Indexed<T>
```

#### Inherited from

```
Iterable#valueSeq
```

### OrderedSet#entrySeq()

返回一个新的Seq.Indexed键值值元组。

```javascript
entrySeq(): Seq.Indexed<Array<any>>
```

#### Inherited from

```
Iterable#entrySeq
```

## Sequence algorithms

### OrderedSet#map()

使用通过`mapper`函数传递的值返回相同类型的新Iterable 。

```javascript
map<M>(mapper: (value?: T, key?: T, iter?: Iterable<T, T>) => M,context?: any): Iterable<T, M>
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

### OrderedSet#filter()

仅`predicate`返回函数返回true 的条目返回相同类型的新Iterable 。

```javascript
filter(predicate: (value?: T, key?: T, iter?: Iterable<T, T>) => boolean,context?: any): Iterable<T, T>
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

### OrderedSet#filterNot()

仅`predicate`返回函数返回false 的条目返回相同类型的新Iterable 。

```javascript
filterNot(predicate: (value?: T, key?: T, iter?: Iterable<T, T>) => boolean,context?: any): Iterable<T, T>
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

### OrderedSet#reverse()

按相反顺序返回相同类型的新Iterable。

```javascript
reverse(): Iterable<T, T>
```

#### Inherited from

```
Iterable#reverse
```

### OrderedSet#sort()

返回包含相同条目的相同类型的新Iterable，通过使用a进行稳定排序`comparator`。

```javascript
sort(comparator?: (valueA: T, valueB: T) => number): Iterable<T, T>
```

#### Inherited from

```
Iterable#sort
```

#### Discussion

如果没有提供`comparator`，默认比较器使用`<`and`>`。

`comparator(valueA, valueB)`:

- 返回`0`元素不应该交换的情况。
- 返回`-1`（或任何负数）如果`valueA`之前`valueB`
- 返回`1`（或任何正数）如果`valueA`后来`valueB`
- 它是纯粹的，即i.e.它必须始终为同一对值返回相同的值。

排序没有定义顺序的集合时，它们的顺序等价物将被返回。例如`map.sort()`返回OrderedMap。

### OrderedSet#sortBy()

就像`sort`，但也接受一个`comparatorValueMapper`允许更复杂的手段进行排序的一个：

```javascript
sortBy<C>(comparatorValueMapper: (value?: T, key?: T, iter?: Iterable<T, T>) => C,comparator?: (valueA: C, valueB: C) => number): Iterable<T, T>
```

#### Inherited from

```
Iterable#sortBy
```

#### Example

```javascript
hitters.sortBy(hitter => hitter.avgHits);
```

### OrderedSet#groupBy()

返回`Iterable.Keyed`的`Iterable.Keyeds`，由返回值分组`grouper`功能。

```javascript
groupBy<G>(grouper: (value?: T, key?: T, iter?: Iterable<T, T>) => G,context?: any): Seq.Keyed<G, Iterable<T, T>>
```

#### Inherited from

```
Iterable#groupBy
```

#### Discussion

注意：这总是一个急切的操作。

## Side effects

### OrderedSet#forEach()

该`sideEffect`是在可迭代的每个条目执行。

```javascript
forEach(sideEffect: (value?: T, key?: T, iter?: Iterable<T, T>) => any,context?: any): number
```

#### Inherited from

```
Iterable#forEach
```

#### Discussion

不同的是[`Array#forEach`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)，如果有任何`sideEffect`回报的话`false`，迭代将停止。返回迭代的条目数（包括返回false的最后一次迭代）。

## Creating subsets

### OrderedSet#slice()

返回一个新的Iterable，其类型代表这个Iterable从开始到结束的一部分。

```javascript
slice(begin?: number, end?: number): Iterable<T, T>
```

#### Inherited from

```
Iterable#slice
```

#### Discussion

如果begin是负数，它将从Iterable的末尾偏移。例如`slice(-2)`返回最后两个条目的Iterable。如果没有提供，则新的Iterable将在此Iterable开始时开始。

如果end是负数，它将从Iterable的末尾偏移。例如`slice(0, -1)`返回除最后一项之外的所有内容的Iterable。如果没有提供，那么新的Iterable将会持续到这个Iterable的结尾。

如果所请求的分片等同于当前的Iterable，那么它将自行返回。

### OrderedSet#rest()

返回包含除第一个以外的所有条目的同一类型的新Iterable。

```javascript
rest(): Iterable<T, T>
```

#### Inherited from

```
Iterable#rest
```

### OrderedSet#butLast()

返回包含除最后一个以外的所有条目的同一类型的新Iterable。

```javascript
butLast(): Iterable<T, T>
```

#### Inherited from

```
Iterable#butLast
```

### OrderedSet#skip()

返回`amount`从此Iterable中排除第一个条目的同一类型的新Iterable。

```javascript
skip(amount: number): Iterable<T, T>
```

#### Inherited from

```
Iterable#skip
```

### OrderedSet#skipLast()

返回`amount`从此Iterable中排除最后一个条目的同一类型的新Iterable。

```javascript
skipLast(amount: number): Iterable<T, T>
```

#### Inherited from

```
Iterable#skipLast
```

### OrderedSet#skipWhile()

返回包含从`predicate`第一个返回false 时开始的相同类型的新Iterable 。

```javascript
skipWhile(predicate: (value?: T, key?: T, iter?: Iterable<T, T>) => boolean,context?: any): Iterable<T, T>
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

### OrderedSet#skipUntil()

返回包含从`predicate`第一个返回true 时开始的相同类型的新Iterable 。

```javascript
skipUntil(predicate: (value?: T, key?: T, iter?: Iterable<T, T>) => boolean,context?: any): Iterable<T, T>
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

### OrderedSet#take()

返回包含`amount`此Iterable中第一个条目的相同类型的新Iterable。

```javascript
take(amount: number): Iterable<T, T>
```

#### Inherited from

```
Iterable#take
```

### OrderedSet#takeLast()

返回包含`amount`此Iterable中最后一个条目的相同类型的新Iterable。

```javascript
takeLast(amount: number): Iterable<T, T>
```

#### Inherited from

```
Iterable#takeLast
```

### OrderedSet#takeWhile()

返回包含来自此Iterable的条目的相同类型的新Iterable，只要`predicate`返回值为true即可。

```javascript
takeWhile(predicate: (value?: T, key?: T, iter?: Iterable<T, T>) => boolean,context?: any): Iterable<T, T>
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

### OrderedSet#takeUntil()

返回包含来自此Iterable的条目的相同类型的新Iterable，只要`predicate`返回false即可。

```javascript
takeUntil(predicate: (value?: T, key?: T, iter?: Iterable<T, T>) => boolean,context?: any): Iterable<T, T>
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

### OrderedSet#concat()

用其他值返回一个具有相同类型的新Iterable，并将其连接到此类。

```javascript
concat(...valuesOrIterables: any[]): Iterable<T, T>
```

#### Inherited from

```
Iterable#concat
```

#### Discussion

对于Seqs，即使它们具有相同的密钥，所有条目也会出现在所得到的迭代中。

### OrderedSet#flatten()

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

### OrderedSet#flatMap()

平面映射Iterable，返回相同类型的Iterable。

```javascript
flatMap<MK, MV>(mapper: (value?: T, key?: T, iter?: Iterable<T, T>) => Iterable<MK, MV>,context?: any): Iterable<MK, MV>
flatMap<MK, MV>(mapper: (value?: T, key?: T, iter?: Iterable<T, T>) => any,context?: any): Iterable<MK, MV>
```

#### Inherited from

```
Iterable#flatMap
```

#### Discussion

类似于`iter.map(...).flatten(true)`。

## Reducing a value

### OrderedSet#reduce()

通过调用Iterable中的`reducer`每个条目并传递缩小的值，将Iterable减少为一个值。

```javascript
reduce<R>(reducer: (reduction?: R, value?: T, key?: T, iter?: Iterable<T, T>) => R,initialReduction?: R,context?: any): R
```

#### Inherited from

```
Iterable#reduce
```

#### see

[`Array#reduce`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce).

#### Discussion

如果`initialReduction`未提供，或者为空，则将使用Iterable中的第一项。

### OrderedSet#reduceRight()

反向（从右侧）减少Iterable。

```javascript
reduceRight<R>(reducer: (reduction?: R, value?: T, key?: T, iter?: Iterable<T, T>) => R,initialReduction?: R,context?: any): R
```

#### Inherited from

```
Iterable#reduceRight
```

#### Discussion

注意：类似于this.reverse()。reduce()，并提供与奇偶校验[`Array#reduceRight`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight)。

### OrderedSet#every()

如果`predicate`对Iterable中的所有条目返回true，则返回true。

```javascript
every(predicate: (value?: T, key?: T, iter?: Iterable<T, T>) => boolean,context?: any): boolean
```

#### Inherited from

```
Iterable#every
```

### OrderedSet#some()

如果`predicate`对Iterable中的任何条目返回true，则返回true。

```javascript
some(predicate: (value?: T, key?: T, iter?: Iterable<T, T>) => boolean,context?: any): boolean
```

#### Inherited from

```
Iterable#some
```

### OrderedSet#join()

将值作为字符串连接在一起，在每个值之间插入一个分隔符。默认分隔符是`","`。

```javascript
join(separator?: string): string
```

#### Inherited from

```
Iterable#join
```

### OrderedSet#isEmpty()

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

### OrderedSet#count()

返回此Iterable的大小。

```javascript
count(): number
count(predicate: (value?: T, key?: T, iter?: Iterable<T, T>) => boolean,context?: any): number
```

#### Inherited from

```
Iterable#count
```

#### Discussion

不管这个Iterable是否可以懒惰地描述它的大小（有些Seqs不能），这个方法总是会返回正确的大小。例如，`Seq`如果需要，它会评估一个懒惰。

如果`predicate`提供，则返回Iterable中`predicate`返回值为true 的条目的计数。

### OrderedSet#countBy()

返回一个`Seq.Keyed`计数，按`grouper`函数的返回值分组。

```javascript
countBy<G>(grouper: (value?: T, key?: T, iter?: Iterable<T, T>) => G,context?: any): Map<G, number>
```

#### Inherited from

```
Iterable#countBy
```

#### Discussion

注意：这不是一个懒惰的操作。

## Search for value

### OrderedSet#find()

返回`predicate`返回true 的第一个值。

```javascript
find(predicate: (value?: T, key?: T, iter?: Iterable<T, T>) => boolean,context?: any,notSetValue?: T): T
```

#### Inherited from

```
Iterable#find
```

### OrderedSet#findLast()

返回返回值为`predicate`true 的最后一个值。

```javascript
findLast(predicate: (value?: T, key?: T, iter?: Iterable<T, T>) => boolean,context?: any,notSetValue?: T): T
```

#### Inherited from

```
Iterable#findLast
```

#### Discussion

注意：`predicate`每个条目都会被调用。

### OrderedSet#findEntry()

返回返回值为true的第一个键值`predicate`。

```javascript
findEntry(predicate: (value?: T, key?: T, iter?: Iterable<T, T>) => boolean,context?: any,notSetValue?: T): Array<any>
```

#### Inherited from

```
Iterable#findEntry
```

### OrderedSet#findLastEntry()

返回返回值为true的最后一个键值`predicate`。

```javascript
findLastEntry(predicate: (value?: T, key?: T, iter?: Iterable<T, T>) => boolean,context?: any,notSetValue?: T): Array<any>
```

#### Inherited from

```
Iterable#findLastEntry
```

#### Discussion

注意：`predicate`每个条目都会被调用。

### OrderedSet#findKey()

返回`predicate`返回true 的键。

```javascript
findKey(predicate: (value?: T, key?: T, iter?: Iterable.Keyed<T, T>) => boolean,context?: any): T
```

#### Inherited from

```
Iterable#findKey
```

### OrderedSet#findLastKey()

返回`predicate`返回true 的最后一个键。

```javascript
findLastKey(predicate: (value?: T, key?: T, iter?: Iterable.Keyed<T, T>) => boolean,context?: any): T
```

#### Inherited from

```
Iterable#findLastKey
```

#### Discussion

注意：`predicate`每个条目都会被调用。

### OrderedSet#keyOf()

返回与搜索值关联的键，或者未定义。

```javascript
keyOf(searchValue: T): T
```

#### Inherited from

```
Iterable#keyOf
```

### OrderedSet#lastKeyOf()

返回与搜索值关联的最后一个键，或者未定义。

```javascript
lastKeyOf(searchValue: T): T
```

#### Inherited from

```
Iterable#lastKeyOf
```

### OrderedSet#max()

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

### OrderedSet#maxBy()

就像`max`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

```javascript
maxBy<C>(comparatorValueMapper: (value?: T, key?: T, iter?: Iterable<T, T>) => C,comparator?: (valueA: C, valueB: C) => number): T
```

#### Inherited from

```
Iterable#maxBy
```

#### Example

```javascript
hitters.maxBy(hitter => hitter.avgHits);
```

### OrderedSet#min()

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

### OrderedSet#minBy()

就像`min`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

```javascript
minBy<C>(comparatorValueMapper: (value?: T, key?: T, iter?: Iterable<T, T>) => C,comparator?: (valueA: C, valueB: C) => number): T
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

### OrderedSet#isSubset()

如果`iter`包含此Iterable中的每个值，则为真。

```javascript
isSubset(iter: Iterable<any, T>): boolean
isSubset(iter: Array<T>): boolean
```

#### Inherited from

```
Iterable#isSubset
```

### OrderedSet#isSuperset()

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