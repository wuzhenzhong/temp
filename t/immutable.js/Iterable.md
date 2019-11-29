# Iterable

这`Iterable`是一组可以迭代的（键，值）条目，并且是所有集合的基类`immutable`，允许它们使用所有Iterable方法（如`map`和`filter`）。

```javascript
class Iterable<K, V>
```

#### 讨论

注：可迭代总是在重复相同的顺序，但是这个顺序可以不总是明确定义，因为对于情况`Map`和`Set`。

**结构**



### Iterable()

创建一个 Iterable。

```javascript
Iterable<K, V>(iterable: Iterable<K, V>): Iterable<K, V>
Iterable<T>(array: Array<T>): Iterable.Indexed<T>
Iterable<V>(obj: {[key: string]: V}): Iterable.Keyed<string, V>
Iterable<T>(iterator: Iterator<T>): Iterable.Indexed<T>
Iterable<T>(iterable: Object): Iterable.Indexed<T>
Iterable<V>(value: V): Iterable.Indexed<V>
```

#### 讨论

所创建的Iterable类型基于输入。

- 如果一个`Iterable`，那一样`Iterable`。
- 如果一个数组，一个`Iterable.Indexed`。
- 如果一个对象有一个迭代器，一个`Iterable.Indexed`。
- 如果一个迭代器，一个`Iterable.Indexed`。
- 如果一个对象，一个`Iterable.Keyed`。

此方法强制将对象和字符串转换为可执行文件。如果您想确保返回一个项目的 Iterable，请使用`Seq.of`。

## 静态方法

### Iterable.isIterable()

如果`maybeIterable`是 Iterable 或其任何子类，则为 true 。

```javascript
Iterable.isIterable(maybeIterable: any): boolean
```

### Iterable.isKeyed()

如果`maybeKeyed`是 Iterable.Keyed 或其任何子类，则为 true 。

```javascript
Iterable.isKeyed(maybeKeyed: any): boolean
```

### Iterable.isIndexed()

如果`maybeIndexed`是 Iterable.Indexed 或其任何子类，则为真。

```javascript
Iterable.isIndexed(maybeIndexed: any): boolean
```

### Iterable.isAssociative()

如果`maybeAssociative`是键控或索引 Iterable，则为真。

```javascript
Iterable.isAssociative(maybeAssociative: any): boolean
```

### Iterable.isOrdered()

如果`maybeOrdered`是迭代顺序定义良好的 Iterable，则为真。True 对于 Iterable.Indexed 以及 OrderedMap 和 OrderedSet。

```javascript
Iterable.isOrdered(maybeOrdered: any): boolean
```

## 类型

Iterable.Keyed

## 价值平等

### Iterable#equals()

如果这和另一个 Iterable 具有值相等性，则为真，如下定义`Immutable.is()`。

```javascript
equals(other: Iterable<K, V>): boolean
```

#### 讨论

注意：这相当于`Immutable.is(this, other)`，但提供允许链式表达式。

### Iterable#hashCode()

计算并返回此 Iterable 的散列标识。

```javascript
hashCode(): number
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

如果两个值相同`hashCode`，则不能保证相等（**http://en.wikipedia.org/wiki/Collision_（computer_science％29**）。如果两个值有不同的`hashCode`s，则它们不能相等。

## 读取值

### Iterable#get()

返回与提供的键相关联的值，如果 Iterable 不包含此键，则返回 notSetValue。

```javascript
get(key: K, notSetValue?: V): V
```

#### 讨论

注意：一个键可能与一个`undefined`值相关联，所以如果`notSetValue`没有提供并且该方法返回`undefined`，那么不能保证没有找到该键。

### Iterable#has()

如果此关键字存在`Iterable`，则为真，`Immutable.is`用于确定相等性

```javascript
has(key: K): boolean
```

### Iterable#includes()

如果此值中存在值`Iterable`，则为 true ，`Immutable.is`用于确定相等性

```javascript
includes(value: V): boolean
```

#### 别号

```
contains()
```

### Iterable#first()

Iterable 中的第一个值。

```javascript
first(): V
```

### Iterable#last()

Iterable 中的最后一个值。

```javascript
last(): V
```

## 读 **deep values**



### Iterable#getIn()

通过嵌套的 Iterables 返回键或索引路径的值。

```javascript
getIn(searchKeyPath: Array<any>, notSetValue?: any): any
getIn(searchKeyPath: Iterable<any, any>, notSetValue?: any): any
```

### Iterable#hasIn()

如果通过嵌套的 Iterables 跟随键或索引路径的结果导致设置值，则返回 true。

```javascript
hasIn(searchKeyPath: Array<any>): boolean
hasIn(searchKeyPath: Iterable<any, any>): boolean
```

## 转换为 JavaScript 类型

### Iterable#toJS()

将此 Iterable 深度转换为等效的 JS。

```javascript
toJS(): any
```

#### 别号

```
toJSON()
```

#### 讨论

`Iterable.Indexeds`，并且`Iterable.Sets`成为阵列，同时`Iterable.Keyeds`成为物体。

### Iterable#toArray()

浅显地将这个迭代器转换为一个 Array，丢弃键。

```javascript
toArray(): Array<V>
```

### Iterable#toObject()

将此 Iterable 浅转换为 Object。

```javascript
toObject(): {[key: string]: V}
```

#### 讨论

如果键不是字符串，则抛出。

## 转换为集合

### Iterable#toMap()

将此 Iterable 转换为 Map，如果键不可哈希则抛出。

```javascript
toMap(): Map<K, V>
```

#### 讨论

注意：这相当于`Map(this.toKeyedSeq())`，但为方便起见并允许链接表达式。

### Iterable#toOrderedMap()

将此 Iterable 转换为 Map，并保持迭代顺序。

```javascript
toOrderedMap(): OrderedMap<K, V>
```

#### 讨论

注意：这相当于`OrderedMap(this.toKeyedSeq())`，但为方便起见并允许链接表达式。

### Iterable#toSet()

将此 Iterable 转换为 Set，放弃键。如果值不可哈希则抛出。

```javascript
toSet(): Set<V>
```

#### 讨论

注意：这相当于`Set(this)`，但提供允许链式表达式。

### Iterable#toOrderedSet()

将此 Iterable 转换为 Set，保持迭代顺序并丢弃键。

```javascript
toOrderedSet(): OrderedSet<V>
```

#### 讨论

注意：这相当于`OrderedSet(this.valueSeq())`，但为方便起见并允许链接表达式。

### Iterable#toList()

将此 Iterable 转换为 List，放弃键。

```javascript
toList(): List<V>
```

#### 讨论

注意：这相当于`List(this)`，但提供允许链式表达式。

### Iterable#toStack()

将此 Iterable 转换为堆栈，丢弃键。如果值不可哈希则抛出。

```javascript
toStack(): Stack<V>
```

#### 讨论

注意：这相当于`Stack(this)`，但提供允许链式表达式。

## 转换为 Seq

### Iterable#toSeq()

将此 Iterable 转换为相同类型的 Seq（索引，键控或设置）。

```javascript
toSeq(): Seq<K, V>
```

### Iterable#toKeyedSeq()

从此 Iterable 返回一个 Seq.Keyed，其索引被视为键。

```javascript
toKeyedSeq(): Seq.Keyed<K, V>
```

#### 讨论

如果您想要对 Iterable.Indexed 进行操作并保留索引，值对，这非常有用。

返回的 Seq 将具有与此 Iterable 相同的迭代顺序。

示例：

```javascript
var indexedSeq = Immutable.Seq.of('A', 'B', 'C');
indexedSeq.filter(v => v === 'B').toString() // Seq [ 'B' ]
var keyedSeq = indexedSeq.toKeyedSeq();
keyedSeq.filter(v => v === 'B').toString() // Seq { 1: 'B' }
```

### Iterable#toIndexedSeq()

返回一个 Seq.Indexed 这个 Iterable 的值，丢弃键。

```javascript
toIndexedSeq(): Seq.Indexed<V>
```

### Iterable#toSetSeq()

返回一个 Seq.Set 这个 Iterable 的值，丢弃键。

```javascript
toSetSeq(): Seq.Set<V>
```

## 迭代器

### Iterable#keys()

这个`Iterable`键的迭代器。

```javascript
keys(): Iterator<K>
```

#### 讨论

注意：这将返回一个不支持 Immutable JS 序列算法的 ES6 迭代器。使用`keySeq`替代，如果这是你想要的。

### Iterable#values()

这个`Iterable`值的迭代器。

```javascript
values(): Iterator<V>
```

#### 讨论

注意：这将返回一个不支持 Immutable JS 序列算法的 ES6 迭代器。使用`valueSeq`替代，如果这是你想要的。

### Iterable#entries()

这个`Iterable`条目的迭代器作为`[key, value]`元组。

```javascript
entries(): Iterator<Array<any>>
```

#### 讨论

注意：这将返回一个不支持 Immutable JS 序列算法的 ES6 迭代器。使用`entrySeq`替代，如果这是你想要的。

## 失败（Seq）

### Iterable#keySeq()

返回此 Iterable 的新键的 Seq.Indexed，放弃值。

```javascript
keySeq(): Seq.Indexed<K>
```

### Iterable#valueSeq()

返回一个 Seq.Indexed 这个 Iterable 的值，丢弃键。

```javascript
valueSeq(): Seq.Indexed<V>
```

### Iterable#entrySeq()

返回一个新的 Seq.Indexed 键值值元组。

```javascript
entrySeq(): Seq.Indexed<Array<any>>
```

## 序列算法

### Iterable#map()

使用通过`mapper`函数传递的值返回相同类型的新 Iterable 。

```javascript
map<M>(mapper: (value?: V, key?: K, iter?: Iterable<K, V>) => M,context?: any): Iterable<K, M>
```

#### 示例

```javascript
Seq({ a: 1, b: 2 }).map(x => 10 * x)
// Seq { a: 10, b: 20 }
```

### Iterable#filter()

仅`predicate`返回函数返回 true 的条目返回相同类型的新 Iterable 。

```javascript
filter(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### 示例

```javascript
Seq({a:1,b:2,c:3,d:4}).filter(x => x % 2 === 0)
// Seq { b: 2, d: 4 }
```

### Iterable#filterNot()

仅`predicate`返回函数返回 false 的条目返回相同类型的新 Iterable 。

```javascript
filterNot(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### 示例

```javascript
Seq({a:1,b:2,c:3,d:4}).filterNot(x => x % 2 === 0)
// Seq { a: 1, c: 3 }
```

### Iterable#reverse()

按相反顺序返回相同类型的新 Iterable。

```javascript
reverse(): Iterable<K, V>
```

### Iterable#sort()

返回包含相同条目的相同类型的新 Iterable，通过使用`comparator`进行稳定排序。

```javascript
sort(comparator?: (valueA: V, valueB: V) => number): Iterable<K, V>
```

#### 讨论

如果`comparator`没有提供，默认比较器使用`<`和`>`。

`comparator(valueA, valueB)`:

- 返回`0`元素不应该交换的情况。
- 返回`-1`（或任何负数）如果`valueA`之前`valueB`
- 返回`1`（或任何正数）如果`valueA`后来`valueB`
- 是纯粹的，即它必须始终为同一对值返回相同的值。

排序没有定义顺序的集合时，它们的顺序等价物将被返回。例如`map.sort()`返回 OrderedMap。

### Iterable#sortBy()

如`sort`，但也接受一个`comparatorValueMapper`允许更复杂的手段进行排序的一个：

```javascript
sortBy<C>(comparatorValueMapper: (value?: V, key?: K, iter?: Iterable<K, V>) => C,comparator?: (valueA: C, valueB: C) => number): Iterable<K, V>
```

#### 示例

```javascript
hitters.sortBy(hitter => hitter.avgHits);
```

### Iterable#groupBy()

返回`Iterable.Keyed`的`Iterable.Keyeds`，由返回值分组`grouper`功能。

```javascript
groupBy<G>(grouper: (value?: V, key?: K, iter?: Iterable<K, V>) => G,context?: any): Seq.Keyed<G, Iterable<K, V>>
```

#### 讨论

注意：这总是一个急切的操作。

## 副作用

### Iterable#forEach()

`sideEffect`是在可迭代的每个条目执行。

```javascript
forEach(sideEffect: (value?: V, key?: K, iter?: Iterable<K, V>) => any,context?: any): number
```

#### 讨论

不同**Array#forEach**的是，如果有任何`sideEffect`回报的话`false`，迭代将停止。返回迭代的条目数（包括返回false的最后一次迭代）。

## 创建子集

### Iterable#slice()

返回一个新的 Iterable，其类型代表这个 Iterable 从开始到结束的一部分。

```javascript
slice(begin?: number, end?: number): Iterable<K, V>
```

#### 讨论

如果 begin（开始） 是负数，它将从 Iterable 的末尾偏移。例如`slice(-2)`返回最后两个条目的 Iterable。如果没有提供，则新的 Iterable 将在此 Iterable 开始时开始。

如果 end（最后）是负数，它将从 Iterable 的末尾偏移。例如`slice(0, -1)`返回除最后一项之外的所有内容的 Iterable。如果没有提供，那么新的 Iterable 将会持续到这个 Iterable 的结尾。

如果所请求的分片等同于当前的 Iterable，那么它将自行返回。

### Iterable#rest()

返回包含除第一个以外的所有条目的同一类型的新 Iterable。

```javascript
rest(): Iterable<K, V>
```

### Iterable#butLast()

返回包含除最后一个以外的所有条目的同一类型的新 Iterable。

```javascript
butLast(): Iterable<K, V>
```

### Iterable#skip()

返回`amount`从此 Iterable 中排除第一个条目的同一类型的新 Iterable。

```javascript
skip(amount: number): Iterable<K, V>
```

### Iterable#skipLast()

返回`amount`从此Iterable中排除最后一个条目的同一类型的新 Iterable。

```javascript
skipLast(amount: number): Iterable<K, V>
```

### Iterable#skipWhile()

返回包含从`predicate`第一个返回 false 时开始的相同类型的新 Iterable 。

```javascript
skipWhile(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### 示例

```javascript
Seq.of('dog','frog','cat','hat','god')
  .skipWhile(x => x.match(/g/))
// Seq [ 'cat', 'hat', 'god' ]
```

### Iterable#skipUntil()

返回包含从`predicate`第一个返回 true 时开始的相同类型的新 Iterable 。

```javascript
skipUntil(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### 示例

```javascript
Seq.of('dog','frog','cat','hat','god')
  .skipUntil(x => x.match(/hat/))
// Seq [ 'hat', 'god' ]
```

### Iterable#take()

返回包含`amount`此 Iterable 中第一个条目的相同类型的新 Iterable。

```javascript
take(amount: number): Iterable<K, V>
```

### Iterable#takeLast()

返回包含`amount`此 Iterable 中最后一个条目的相同类型的新 Iterable。

```javascript
takeLast(amount: number): Iterable<K, V>
```

### Iterable#takeWhile()

返回包含来自此 Iterable 的条目的相同类型的新 Iterable，只要`predicate`返回值为 true 即可。

```javascript
takeWhile(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### 示例

```javascript
Seq.of('dog','frog','cat','hat','god')
  .takeWhile(x => x.match(/o/))
// Seq [ 'dog', 'frog' ]
```

### Iterable#takeUntil()

返回包含来自此 Iterable 的条目的相同类型的新 Iterable，只要`predicate`返回 false 即可。

```javascript
takeUntil(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### 示例

```javascript
Seq.of('dog','frog','cat','hat','god').takeUntil(x => x.match(/at/))
// ['dog', 'frog']
```

## 组合

### Iterable#concat()

用其他值返回一个具有相同类型的新 Iterable，并将其连接到此类。

```javascript
concat(...valuesOrIterables: any[]): Iterable<K, V>
```

#### 讨论

对于 Seqs，即使它们具有相同的密钥，所有条目也会出现在所得到的迭代中。

### Iterable#flatten()

压扁嵌套的 Iterables。

```javascript
flatten(depth?: number): Iterable<any, any>
flatten(shallow?: boolean): Iterable<any, any>
```

#### 讨论

默认情况下会严格地将 Iterable 扁平化，返回一个相同类型的 Iterable，但`depth`可以以数字或布尔值的形式提供（其中 true 表示浅层扁平化）。深度为0（或者浅：假）将会变得很平坦。

仅将其他的 Iterable 变为 Flattens，而不是阵列或对象。

注意：`flatten(true)`在 Iterable> 上运行并返回 Iterable

### Iterable#flatMap()

平面映射 Iterable，返回相同类型的 Iterable。

```javascript
flatMap<MK, MV>(mapper: (value?: V, key?: K, iter?: Iterable<K, V>) => Iterable<MK, MV>,context?: any): Iterable<MK, MV>
flatMap<MK, MV>(mapper: (value?: V, key?: K, iter?: Iterable<K, V>) => any,context?: any): Iterable<MK, MV>
```

#### 讨论

类似于`iter.map(...).flatten(true)`。

## 减少值

### Iterable#reduce()

通过调用 Iterable 中的`reducer`每个条目并传递缩小的值，将 Iterable 减少为一个值。

```javascript
reduce<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### 参阅

`**Array#reduce**`.

#### 讨论

如果`initialReduction`未提供，或者为空，则将使用 Iterable 中的第一项。

### Iterable#reduceRight()

反向（从右侧）减少 Iterable。

```javascript
reduceRight<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### 讨论

注意：类似于 this.reverse（）。reduce（），并提供与奇偶校验`**Array#reduceRight**`。

### Iterable#every()

如果`predicate`对 Iterable 中的所有条目返回 true，则返回 true。

```javascript
every(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

### Iterable#some()

如果`predicate`对Iterable中的任何条目返回 true，则返回 true。

```javascript
some(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

### Iterable#join()

将值作为字符串连接在一起，在每个值之间插入一个分隔符。默认分隔符是`","`。

```javascript
join(separator?: string): string
```

### Iterable#isEmpty()

如果此 Iterable 不包含任何值，则返回true。

```javascript
isEmpty(): boolean
```

#### 讨论

对于一些惰性`Seq`，`isEmpty`可能需要迭代以确定空虚。至多会发生一次迭代。

### Iterable#count()

返回此 Iterable 的大小。

```javascript
count(): number
count(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): number
```

#### 讨论

不管这个Iterable是否可以懒惰地描述它的大小（有些Seqs不能），这个方法总是会返回正确的大小。例如，`Seq`如果需要，它会评估一个懒惰。

如果`predicate`提供，则返回Iterable中`predicate`返回值为true 的条目的计数。

### Iterable#countBy()

返回一个`Seq.Keyed`计数，按`grouper`函数的返回值分组。

```javascript
countBy<G>(grouper: (value?: V, key?: K, iter?: Iterable<K, V>) => G,context?: any): Map<G, number>
```

#### 讨论

注意：这不是一个惰性操作。

## 搜索值

### Iterable#find()

返回`predicate`返回true 的第一个值。

```javascript
find(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

### Iterable#findLast()

返回返回值为`predicate`true 的最后一个值。

```javascript
findLast(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### Iterable#findEntry()

返回返回值为true的第一个键值对`predicate`。

```javascript
findEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

### Iterable#findLastEntry()

返回返回值为true的最后一个键值对`predicate`。

```javascript
findLastEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### Iterable#findKey()

返回`predicate`返回true 的键。

```javascript
findKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

### Iterable#findLastKey()

返回`predicate`返回true 的最后一个键。

```javascript
findLastKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### Iterable#keyOf()

返回与搜索值关联的键，或者未定义。

```javascript
keyOf(searchValue: V): K
```

### Iterable#lastKeyOf()

返回与搜索值关联的最后一个键，或者未定义。

```javascript
lastKeyOf(searchValue: V): K
```

### Iterable#max()

返回此集合中的最大值。如果任何值相当相等，则找到的第一个将被返回。

```javascript
max(comparator?: (valueA: V, valueB: V) => number): V
```

#### 讨论

在`comparator`以同样的方式使用`Iterable#sort`。如果未提供，则默认比较器为`>`。

当两个值被认为是等价的，遇到的第一个将被返回。否则，`max`只要比较器是可交换的，将独立于输入的顺序进行操作。默认比较器*只有*在类型不相同*时才*`>`可以交换。

如果`comparator`返回0，且其中任一值为NaN，undefined或null，则将返回该值。

### Iterable#maxBy()

喜欢`max`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

```javascript
maxBy<C>(comparatorValueMapper: (value?: V, key?: K, iter?: Iterable<K, V>) => C,comparator?: (valueA: C, valueB: C) => number): V
```

#### 例

```javascript
hitters.maxBy(hitter => hitter.avgHits);
```

### Iterable#min()

返回此集合中的最小值。如果任何值相当相等，则找到的第一个将被返回。

```javascript
min(comparator?: (valueA: V, valueB: V) => number): V
```

#### 讨论

在`comparator`以同样的方式使用`Iterable#sort`。如果未提供，则默认比较器为`<`。

当两个值被认为是等价的，遇到的第一个将被返回。否则，`min`只要比较器是可交换的，将独立于输入的顺序进行操作。默认比较器*只有*在类型不相同*时才*`<`可以交换。

如果`comparator`返回0，且其中任一值为NaN，undefined或null，则将返回该值。

### Iterable#minBy()

喜欢`min`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

```javascript
minBy<C>(comparatorValueMapper: (value?: V, key?: K, iter?: Iterable<K, V>) => C,comparator?: (valueA: C, valueB: C) => number): V
```

#### 例

```javascript
hitters.minBy(hitter => hitter.avgHits);
```

## 对照

### Iterable#isSubset()

如果`iter`包含此Iterable中的每个值，则为真。

```javascript
isSubset(iter: Iterable<any, V>): boolean
isSubset(iter: Array<V>): boolean
```

### Iterable#isSuperset()

如果此Iterable包含每个值，则为真`iter`。

```javascript
isSuperset(iter: Iterable<any, V>): boolean
isSuperset(iter: Array<V>): boolean
```

键控的可计算元件具有与每个值相关的离散键。

```javascript
class Iterable.Keyed<K, V> extends Iterable<K, V>
```

#### 讨论

迭代时`Iterable.Keyed`，每次迭代都会产生一个`[K, V]`元组，换句话说，`Iterable#entries`是Keyed Iterables的默认迭代器。

## 结构

### Iterable.Keyed()

创建一个Iterable.Keyed

```javascript
Iterable.Keyed<K, V>(iter: Iterable.Keyed<K, V>): Iterable.Keyed<K, V>
Iterable.Keyed<K, V>(iter: Iterable<any, any>): Iterable.Keyed<K, V>
Iterable.Keyed<K, V>(array: Array<any>): Iterable.Keyed<K, V>
Iterable.Keyed<V>(obj: {[key: string]: V}): Iterable.Keyed<string, V>
Iterable.Keyed<K, V>(iterator: Iterator<any>): Iterable.Keyed<K, V>
Iterable.Keyed<K, V>(iterable: Object): Iterable.Keyed<K, V>
```

#### 讨论

类似于`Iterable()`，但是它期望如果不是从Iterable.Keyed或JS对象构造的K，V元组的迭代式喜欢。

## 转换为Seq

### Iterable.Keyed#toSeq()

返回Seq.Keyed。

```javascript
toSeq(): Seq.Keyed<K, V>
```

#### 覆盖

```
Iterable#toSeq
```

### Iterable.Keyed#toKeyedSeq()

从此Iterable返回一个Seq.Keyed，其索引被视为键。

```javascript
toKeyedSeq(): Seq.Keyed<K, V>
```

#### 继承

```
Iterable#toKeyedSeq
```

#### 讨论

如果您想要对Iterable.Indexed进行操作并保留索引，值对，这非常有用。

返回的Seq将具有与此Iterable相同的迭代顺序。

例：

```javascript
var indexedSeq = Immutable.Seq.of('A', 'B', 'C');
indexedSeq.filter(v => v === 'B').toString() // Seq [ 'B' ]
var keyedSeq = indexedSeq.toKeyedSeq();
keyedSeq.filter(v => v === 'B').toString() // Seq { 1: 'B' }
```

### Iterable.Keyed#toIndexedSeq()

返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
toIndexedSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#toIndexedSeq
```

### Iterable.Keyed#toSetSeq()

返回一个Seq.Set这个Iterable的值，丢弃键。

```javascript
toSetSeq(): Seq.Set<V>
```

#### 继承

```
Iterable#toSetSeq
```

## 序列功能

### Iterable.Keyed#flip()

返回键和值已翻转的同一类型的新Iterable.Keyed。

```javascript
flip(): Iterable.Keyed<V, K>
```

#### 例

```javascript
Seq({ a: 'z', b: 'y' }).flip() // { z: 'a', y: 'b' }
```

### Iterable.Keyed#mapKeys()

使用通过`mapper`函数传递的键返回相同类型的新Iterable.Keyed 。

```javascript
mapKeys<M>(mapper: (key?: K, value?: V, iter?: Iterable.Keyed<K, V>) => M,context?: any): Iterable.Keyed<M, V>
```

#### 例

```javascript
Seq({ a: 1, b: 2 })
  .mapKeys(x => x.toUpperCase())
// Seq { A: 1, B: 2 }
```

### Iterable.Keyed#mapEntries()

通过`mapper`函数传递条目（键，值元组），返回相同类型的新Iterable.Keyed 。

```javascript
mapEntries<KM, VM>(mapper: (entry?: Array<any>,index?: number,iter?: Iterable.Keyed<K, V>) => Array<any>,context?: any): Iterable.Keyed<KM, VM>
```

#### 例

```javascript
Seq({ a: 1, b: 2 })
  .mapEntries(([k, v]) => [k.toUpperCase(), v * 2])
// Seq { A: 2, B: 4 }
```

## 价值平等

### Iterable.Keyed#equals()

如果这和另一个Iterable具有值相等性，则为真，如下定义`Immutable.is()`。

```javascript
equals(other: Iterable<K, V>): boolean
```

#### 继承

```
Iterable#equals
```

#### 讨论

注意：这相当于`Immutable.is(this, other)`，但提供允许链式表达式。

### Iterable.Keyed#hashCode()

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

### Iterable.Keyed#get()

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

### Iterable.Keyed#has()

如果此关键字存在`Iterable`，则为真，`Immutable.is`用于确定相等性

```javascript
has(key: K): boolean
```

#### 继承

```
Iterable#has
```

### Iterable.Keyed#includes()

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

### Iterable.Keyed#first()

Iterable中的第一个值。

```javascript
first(): V
```

#### 继承

```
Iterable#first
```

### Iterable.Keyed#last()

Iterable中的最后一个值。

```javascript
last(): V
```

#### 继承

```
Iterable#last
```

## 读**deep values**

### Iterable.Keyed#getIn()

通过嵌套的Iterables返回键或索引路径的值。

```javascript
getIn(searchKeyPath: Array<any>, notSetValue?: any): any
getIn(searchKeyPath: Iterable<any, any>, notSetValue?: any): any
```

#### 继承

```
Iterable#getIn
```

### Iterable.Keyed#hasIn()

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

### Iterable.Keyed#toJS()

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

### Iterable.Keyed#toArray()

浅显地将这个迭代器转换为一个Array，丢弃键。

```javascript
toArray(): Array<V>
```

#### 继承

```
Iterable#toArray
```

### Iterable.Keyed#toObject()

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

### Iterable.Keyed#toMap()

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

### Iterable.Keyed#toOrderedMap()

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

### Iterable.Keyed#toSet()

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

### Iterable.Keyed#toOrderedSet()

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

### Iterable.Keyed#toList()

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

### Iterable.Keyed#toStack()

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

### Iterable.Keyed#keys()

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

### Iterable.Keyed#values()

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

### Iterable.Keyed#entries()

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

## 失败（Seq）

### Iterable.Keyed#keySeq()

返回此Iterable的新键的Seq.Indexed，放弃值。

```javascript
keySeq(): Seq.Indexed<K>
```

#### 继承

```
Iterable#keySeq
```

### Iterable.Keyed#valueSeq()

返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
valueSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#valueSeq
```

### Iterable.Keyed#entrySeq()

返回一个新的Seq.Indexed键值值元组。

```javascript
entrySeq(): Seq.Indexed<Array<any>>
```

#### 继承

```
Iterable#entrySeq
```

## 序列算法

### Iterable.Keyed#map()

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

### Iterable.Keyed#filter()

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

### Iterable.Keyed#filterNot()

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

### Iterable.Keyed#reverse()

按相反顺序返回相同类型的新Iterable。

```javascript
reverse(): Iterable<K, V>
```

#### 继承

```
Iterable#reverse
```

### Iterable.Keyed#sort()

返回包含相同条目的相同类型的新Iterable，通过使用a进行稳定排序`comparator`。

```javascript
sort(comparator?: (valueA: V, valueB: V) => number): Iterable<K, V>
```

#### 继承

```
Iterable#sort
```

#### 讨论

如果`comparator`没有提供a，默认比较器使用`<`和`>`。

`comparator(valueA, valueB)`:

- 返回`0`元素不应该交换的情况。
- 返回`-1`（或任何负数）如果`valueA`之前`valueB`
- 返回`1`（或任何正数）如果`valueA`后来`valueB`
- 是纯粹的，即它必须始终为同一对值返回相同的值。

排序没有定义顺序的集合时，它们的顺序等价物将被返回。例如`map.sort()`返回OrderedMap。

### Iterable.Keyed#sortBy()

喜欢`sort`，但也接受一个`comparatorValueMapper`允许更复杂的手段进行排序的一个：

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

### Iterable.Keyed#groupBy()

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

### Iterable.Keyed#forEach()

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

### Iterable.Keyed#slice()

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

### Iterable.Keyed#rest()

返回包含除第一个以外的所有条目的同一类型的新Iterable。

```javascript
rest(): Iterable<K, V>
```

#### 继承

```
Iterable#rest
```

### Iterable.Keyed#butLast()

返回包含除最后一个以外的所有条目的同一类型的新Iterable。

```javascript
butLast(): Iterable<K, V>
```

#### 继承

```
Iterable#butLast
```

### Iterable.Keyed#skip()

返回`amount`从此Iterable中排除第一个条目的同一类型的新Iterable。

```javascript
skip(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skip
```

### Iterable.Keyed#skipLast()

返回`amount`从此Iterable中排除最后一个条目的同一类型的新Iterable。

```javascript
skipLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skipLast
```

### Iterable.Keyed#skipWhile()

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

### Iterable.Keyed#skipUntil()

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

### Iterable.Keyed#take()

返回包含`amount`此Iterable中第一个条目的相同类型的新Iterable。

```javascript
take(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#take
```

### Iterable.Keyed#takeLast()

返回包含`amount`此Iterable中最后一个条目的相同类型的新Iterable。

```javascript
takeLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#takeLast
```

### Iterable.Keyed#takeWhile()

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

### Iterable.Keyed#takeUntil()

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

### Iterable.Keyed#concat()

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

### Iterable.Keyed#flatten()

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

### Iterable.Keyed#flatMap()

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

Similar to `iter.map(...).flatten(true)`.

## 减少值

### Iterable.Keyed#reduce()

通过调用Iterable中的`reducer`每个条目并传递缩小的值，将Iterable减少为一个值。

```javascript
reduce<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### 继承

```
Iterable#reduce
```

#### 参阅

[`Array#reduce`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce).

#### 讨论

如果`initialReduction`未提供，或者为空，则将使用Iterable中的第一项。

### Iterable.Keyed#reduceRight()

反向（从右侧）减少Iterable。

```javascript
reduceRight<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### 继承

```
Iterable#reduceRight
```

#### 讨论

注意：类似于this.reverse（）。reduce（），并提供与奇偶校验[`Array#reduceRight`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight)。

### Iterable.Keyed#every()

如果`predicate`对Iterable中的所有条目返回true，则返回true。

```javascript
every(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#every
```

### Iterable.Keyed#some()

如果`predicate`对Iterable中的任何条目返回true，则返回true。

```javascript
some(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#some
```

### Iterable.Keyed#join()

将值作为字符串连接在一起，在每个值之间插入一个分隔符。默认分隔符是`","`。

```javascript
join(separator?: string): string
```

#### 继承

```
Iterable#join
```

### Iterable.Keyed#isEmpty()

如果此Iterable不包含任何值，则返回true。

```javascript
isEmpty(): boolean
```

#### 继承

```
Iterable#isEmpty
```

#### 讨论

对于一些惰性`Seq`，`isEmpty`可能需要迭代以确定空虚。至多会发生一次迭代。

### Iterable.Keyed#count()

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

### Iterable.Keyed#countBy()

返回一个`Seq.Keyed`计数，按`grouper`函数的返回值分组。

```javascript
countBy<G>(grouper: (value?: V, key?: K, iter?: Iterable<K, V>) => G,context?: any): Map<G, number>
```

#### 继承

```
Iterable#countBy
```

#### 讨论

注意：这不是一个惰性操作。

## 搜索值

### Iterable.Keyed#find()

返回`predicate`返回true 的第一个值。

```javascript
find(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### 继承

```
Iterable#find
```

### Iterable.Keyed#findLast()

返回返回值为`predicate`true 的最后一个值。

```javascript
findLast(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### 继承

```
Iterable#findLast
```

#### 讨论

注意：每个条目都会被调用`predicate`。

### Iterable.Keyed#findEntry()

返回返回值为true的第一个键值对`predicate`。

```javascript
findEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

#### 继承

```
Iterable#findEntry
```

### Iterable.Keyed#findLastEntry()

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

### Iterable.Keyed#findKey()

返回`predicate`返回true 的键。

```javascript
findKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

#### 继承

```
Iterable#findKey
```

### Iterable.Keyed#findLastKey()

返回`predicate`返回true 的最后一个键。

```javascript
findLastKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

#### Inherited from

```
Iterable#findLastKey
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### Iterable.Keyed#keyOf()

返回与搜索值关联的键，或者未定义。

```javascript
keyOf(searchValue: V): K
```

#### 继承

```
Iterable#keyOf
```

### Iterable.Keyed#lastKeyOf()

返回与搜索值关联的最后一个键，或者未定义。

```javascript
lastKeyOf(searchValue: V): K
```

#### Inherited from

```
Iterable#lastKeyOf
```

### Iterable.Keyed#max()

返回此集合中的最大值。如果任何值相当相等，则找到的第一个将被返回。

```javascript
max(comparator?: (valueA: V, valueB: V) => number): V
```

#### Inherited from

```
Iterable#max
```

#### 讨论

在`comparator`以同样的方式使用`Iterable#sort`。如果未提供，则默认比较器为`>`。

当两个值被认为是等价的，遇到的第一个将被返回。否则，`max`只要比较器是可交换的，将独立于输入的顺序进行操作。默认比较器*只有*在类型不相同*时才*`>`可以交换。

如果`comparator`返回0，且其中任一值为NaN，undefined或null，则将返回该值。

### Iterable.Keyed#maxBy()

像`max`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

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

### Iterable.Keyed#min()

返回此集合中的最小值。如果任何值相当相等，则找到的第一个将被返回。

```javascript
min(comparator?: (valueA: V, valueB: V) => number): V
```

#### Inherited from

```
Iterable#min
```

#### 讨论

在`comparator`以同样的方式使用`Iterable#sort`。如果未提供，则默认比较器为`<`。

当两个值被认为是等价的，遇到的第一个将被返回。否则，`min`只要比较器是可交换的，将独立于输入的顺序进行操作。默认比较器*只有*在类型不相同*时才*`<`可以交换。

如果`comparator`返回0，且其中任一值为NaN，undefined或null，则将返回该值。

### Iterable.Keyed#minBy()

像`min`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

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

### Iterable.Keyed#isSubset()

如果`iter`包含此Iterable中的每个值，则为真。

```javascript
isSubset(iter: Iterable<any, V>): boolean
isSubset(iter: Array<V>): boolean
```

#### 继承

```
Iterable#isSubset
```

### Iterable.Keyed#isSuperset()

如果此Iterable包含每个值，则为真`iter`。

```javascript
isSuperset(iter: Iterable<any, V>): boolean
isSuperset(iter: Array<V>): boolean
```

#### Inherited from

```
Iterable#isSuperset
```

键控的可计算元件具有与每个值相关的离散键。

```javascript
class Iterable.Keyed<K, V> extends Iterable<K, V>
```

#### 讨论

迭代时`Iterable.Keyed`，每次迭代都会产生一个`[K, V]`元组，换句话说，`Iterable#entries`是Keyed Iterables的默认迭代器。

## 架构

### Iterable.Keyed()

创建一个Iterable.Keyed

```javascript
Iterable.Keyed<K, V>(iter: Iterable.Keyed<K, V>): Iterable.Keyed<K, V>
Iterable.Keyed<K, V>(iter: Iterable<any, any>): Iterable.Keyed<K, V>
Iterable.Keyed<K, V>(array: Array<any>): Iterable.Keyed<K, V>
Iterable.Keyed<V>(obj: {[key: string]: V}): Iterable.Keyed<string, V>
Iterable.Keyed<K, V>(iterator: Iterator<any>): Iterable.Keyed<K, V>
Iterable.Keyed<K, V>(iterable: Object): Iterable.Keyed<K, V>
```

#### 讨论

类似于`Iterable()`，但是它期望如果不是从Iterable.Keyed或JS对象构造的K，V元组的迭代式喜欢。

## Conversion to Seq

### Iterable.Keyed#toSeq()

返回Seq.Keyed。

```javascript
toSeq(): Seq.Keyed<K, V>
```

#### Overrides

```
Iterable#toSeq
```

### Iterable.Keyed#toKeyedSeq()

从此Iterable返回一个Seq.Keyed，其索引被视为键。

```javascript
toKeyedSeq(): Seq.Keyed<K, V>
```

#### Inherited from

```
Iterable#toKeyedSeq
```

#### Discussion

如果您想要对Iterable.Indexed进行操作并保留索引值对，这非常有用。

返回的Seq将具有与此Iterable相同的迭代顺序。

例：

```javascript
var indexedSeq = Immutable.Seq.of('A', 'B', 'C');
indexedSeq.filter(v => v === 'B').toString() // Seq [ 'B' ]
var keyedSeq = indexedSeq.toKeyedSeq();
keyedSeq.filter(v => v === 'B').toString() // Seq { 1: 'B' }
```

### Iterable.Keyed#toIndexedSeq()

返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
toIndexedSeq(): Seq.Indexed<V>
```

#### Inherited from

```
Iterable#toIndexedSeq
```

### Iterable.Keyed#toSetSeq()

返回一个Seq.Set这个Iterable的值，丢弃键。

```javascript
toSetSeq(): Seq.Set<V>
```

#### Inherited from

```
Iterable#toSetSeq
```

## 序列功能

### Iterable.Keyed#flip()

返回键和值已翻转的同一类型的新Iterable.Keyed。

```javascript
flip(): Iterable.Keyed<V, K>
```

#### 例

```javascript
Seq({ a: 'z', b: 'y' }).flip() // { z: 'a', y: 'b' }
```

### Iterable.Keyed#mapKeys()

使用通过`mapper`函数传递的键返回相同类型的新Iterable.Keyed 。

```javascript
mapKeys<M>(mapper: (key?: K, value?: V, iter?: Iterable.Keyed<K, V>) => M,context?: any): Iterable.Keyed<M, V>
```

#### 例

```javascript
Seq({ a: 1, b: 2 })
  .mapKeys(x => x.toUpperCase())
// Seq { A: 1, B: 2 }
```

### Iterable.Keyed#mapEntries()

通过`mapper`函数传递条目（key, value tuples），返回相同类型的新Iterable.Keyed 。

```javascript
mapEntries<KM, VM>(mapper: (entry?: Array<any>,index?: number,iter?: Iterable.Keyed<K, V>) => Array<any>,context?: any): Iterable.Keyed<KM, VM>
```

#### 例

```javascript
Seq({ a: 1, b: 2 })
  .mapEntries(([k, v]) => [k.toUpperCase(), v * 2])
// Seq { A: 2, B: 4 }
```

## 价值平等

### Iterable.Keyed#equals()

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

### Iterable.Keyed#hashCode()

计算并返回此Iterable的散列标识。

```javascript
hashCode(): number
```

#### Inherited from

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

### Iterable.Keyed#get()

返回与提供的键关联的值，如果Iterable不包含此键，则返回notSetValue。

```javascript
get(key: K, notSetValue?: V): V
```

#### Inherited from

```
Iterable#get
```

#### 讨论

注意：一个键可能与一个`undefined`值相关联，所以如果`notSetValue`没有提供并且该方法返回`undefined`，那么不能保证没有找到该键。

### Iterable.Keyed#has()

如果此关键字存在`Iterable`，则为真，`Immutable.is`用于确定相等性

```javascript
has(key: K): boolean
```

#### Inherited from

```
Iterable#has
```

### Iterable.Keyed#includes()

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

### Iterable.Keyed#first()

Iterable中的第一个值。

```javascript
first(): V
```

#### Inherited from

```
Iterable#first
```

### Iterable.Keyed#last()

Iterable中的最后一个值。

```javascript
last(): V
```

#### Inherited from

```
Iterable#last
```

## 读深刻的价值

### Iterable.Keyed#getIn()

通过嵌套的Iterables返回键或索引路径的值。

```javascript
getIn(searchKeyPath: Array<any>, notSetValue?: any): any
getIn(searchKeyPath: Iterable<any, any>, notSetValue?: any): any
```

#### Inherited from

```
Iterable#getIn
```

### Iterable.Keyed#hasIn()

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

### Iterable.Keyed#toJS()

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

#### 讨论

`Iterable.Indexeds`，并`Iterable.Sets`成为阵列，同时`Iterable.Keyeds`成为物体。

### Iterable.Keyed#toArray()

浅显地将这个迭代器转换为一个Array，丢弃键。

```javascript
toArray(): Array<V>
```

#### 继承

```
Iterable#toArray
```

### Iterable.Keyed#toObject()

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

### Iterable.Keyed#toMap()

将此Iterable转换为Map，如果键不可哈希则抛出。

```javascript
toMap(): Map<K, V>
```

#### Inherited from

```
Iterable#toMap
```

#### 讨论

注意：这相当于`Map(this.toKeyedSeq())`，但为方便起见并允许链接表达式。

### Iterable.Keyed#toOrderedMap()

将此Iterable转换为Map，并保持迭代的顺序。

```javascript
toOrderedMap(): OrderedMap<K, V>
```

#### Inherited from

```
Iterable#toOrderedMap
```

#### 讨论

注意：这相当于`OrderedMap(this.toKeyedSeq())`，但为方便起见并允许链接表达式。

### Iterable.Keyed#toSet()

将此Iterable转换为Set，放弃键。如果值不可哈希则抛出。

```javascript
toSet(): Set<V>
```

#### Inherited from

```
Iterable#toSet
```

#### 讨论

注意：这相当于`Set(this)`，但提供允许链式表达式。

### Iterable.Keyed#toOrderedSet()

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

### Iterable.Keyed#toList()

将此Iterable转换为List，放弃键。

```javascript
toList(): List<V>
```

#### Inherited from

```
Iterable#toList
```

#### 讨论

注意：这相当于`List(this)`，但提供允许链式表达式。

### Iterable.Keyed#toStack()

将此Iterable转换为堆栈，丢弃键。如果值不可哈希则抛出。

```javascript
toStack(): Stack<V>
```

#### 继承

```
Iterable#toStack
```

#### Discussion

注意：这相当于`Stack(this)`，但提供允许链式表达式。

## 迭代器

### Iterable.Keyed#keys()

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

### Iterable.Keyed#values()

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

### Iterable.Keyed#entries()

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

### Iterable.Keyed#keySeq()

返回此Iterable的新键的Seq.Indexed，放弃值。

```javascript
keySeq(): Seq.Indexed<K>
```

#### Inherited from

```
Iterable#keySeq
```

### Iterable.Keyed#valueSeq()

返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
valueSeq(): Seq.Indexed<V>
```

#### Inherited from

```
Iterable#valueSeq
```

### Iterable.Keyed#entrySeq()

返回一个新的Seq.Indexed键值值元组。

```javascript
entrySeq(): Seq.Indexed<Array<any>>
```

#### Inherited from

```
Iterable#entrySeq
```

## 序列算法

### Iterable.Keyed#map()

使用通过`mapper`函数传递的值返回相同类型的新Iterable 。

```javascript
map<M>(mapper: (value?: V, key?: K, iter?: Iterable<K, V>) => M,context?: any): Iterable<K, M>
```

#### Inherited from

```
Iterable#map
```

#### 例

```javascript
Seq({ a: 1, b: 2 }).map(x => 10 * x)
// Seq { a: 10, b: 20 }
```

### Iterable.Keyed#filter()

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

### Iterable.Keyed#filterNot()

仅`predicate`返回函数返回false 的条目返回相同类型的新Iterable 。

```javascript
filterNot(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### Inherited from

```
Iterable#filterNot
```

#### 例

```javascript
Seq({a:1,b:2,c:3,d:4}).filterNot(x => x % 2 === 0)
// Seq { a: 1, c: 3 }
```

### Iterable.Keyed#reverse()

按相反顺序返回相同类型的新Iterable。

```javascript
reverse(): Iterable<K, V>
```

#### Inherited from

```
Iterable#reverse
```

### Iterable.Keyed#sort()

返回包含相同条目的相同类型的新Iterable，通过使用`comparator`进行稳定排序。

```javascript
sort(comparator?: (valueA: V, valueB: V) => number): Iterable<K, V>
```

#### Inherited from

```
Iterable#sort
```

#### 讨论

如果`comparator`没有提供，默认比较器使用`<`和`>`。

`comparator(valueA, valueB)`:

- 返回`0`元素不应该交换的情况。
- 返回`-1`（或任何负数）如果`valueA`之前`valueB`
- 返回`1`（或任何正数）如果`valueA`后来`valueB`
- 是纯粹的，即它必须始终为同一对值返回相同的值。

排序没有定义顺序的集合时，它们的顺序等价物将被返回。例如`map.sort()`返回OrderedMap。

### Iterable.Keyed#sortBy()

像`sort`，但也接受一个`comparatorValueMapper`允许更复杂的手段进行排序的一个：

```javascript
sortBy<C>(comparatorValueMapper: (value?: V, key?: K, iter?: Iterable<K, V>) => C,comparator?: (valueA: C, valueB: C) => number): Iterable<K, V>
```

#### Inherited from

```
Iterable#sortBy
```

#### 例

```javascript
hitters.sortBy(hitter => hitter.avgHits);
```

### Iterable.Keyed#groupBy()

返回`Iterable.Keyed`的`Iterable.Keyeds`，由返回值分组`grouper`功能。

```javascript
groupBy<G>(grouper: (value?: V, key?: K, iter?: Iterable<K, V>) => G,context?: any): Seq.Keyed<G, Iterable<K, V>>
```

#### Inherited from

```
Iterable#groupBy
```

#### 讨论

注意：这总是一个急切的操作。

## 副作用

### Iterable.Keyed#forEach()

`sideEffect`是在可迭代的每个条目执行。

```javascript
forEach(sideEffect: (value?: V, key?: K, iter?: Iterable<K, V>) => any,context?: any): number
```

#### 继承

```
Iterable#forEach
```

#### 讨论

不同于[`Array#forEach`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)，如果有任何`sideEffect`回报的话`false`，迭代将停止。返回迭代的条目数（包括返回false的最后一次迭代）。

## 创建子集

### Iterable.Keyed#slice()

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

### Iterable.Keyed#rest()

返回包含除第一个以外的所有条目的同一类型的新Iterable。

```javascript
rest(): Iterable<K, V>
```

#### Inherited from

```
Iterable#rest
```

### Iterable.Keyed#butLast()

返回包含除最后一个以外的所有条目的同一类型的新Iterable。

```javascript
butLast(): Iterable<K, V>
```

#### Inherited from

```
Iterable#butLast
```

### Iterable.Keyed#skip()

返回`amount`从此Iterable中排除第一个条目的同一类型的新Iterable。

```javascript
skip(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skip
```

### Iterable.Keyed#skipLast()

返回`amount`从此Iterable中排除最后一个条目的同一类型的新Iterable。

```javascript
skipLast(amount: number): Iterable<K, V>
```

#### Inherited from

```
Iterable#skipLast
```

### Iterable.Keyed#skipWhile()

返回包含从`predicate`第一个返回false 时开始的相同类型的新Iterable 。

```javascript
skipWhile(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### Inherited from

```
Iterable#skipWhile
```

#### 例

```javascript
Seq.of('dog','frog','cat','hat','god')
  .skipWhile(x => x.match(/g/))
// Seq [ 'cat', 'hat', 'god' ]
```

### Iterable.Keyed#skipUntil()

返回包含从`predicate`第一个返回true 时开始的相同类型的新Iterable 。

```javascript
skipUntil(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### Inherited from

```
Iterable#skipUntil
```

#### 例

```javascript
Seq.of('dog','frog','cat','hat','god')
  .skipUntil(x => x.match(/hat/))
// Seq [ 'hat', 'god' ]
```

### Iterable.Keyed#take()

返回包含`amount`此Iterable中第一个条目的相同类型的新Iterable。

```javascript
take(amount: number): Iterable<K, V>
```

#### Inherited from

```
Iterable#take
```

### Iterable.Keyed#takeLast()

返回包含`amount`此Iterable中最后一个条目的相同类型的新Iterable。

```javascript
takeLast(amount: number): Iterable<K, V>
```

#### Inherited from

```
Iterable#takeLast
```

### Iterable.Keyed#takeWhile()

返回包含来自此Iterable的条目的相同类型的新Iterable，只要`predicate`返回值为true即可。

```javascript
takeWhile(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### Inherited from

```
Iterable#takeWhile
```

#### 例

```javascript
Seq.of('dog','frog','cat','hat','god')
  .takeWhile(x => x.match(/o/))
// Seq [ 'dog', 'frog' ]
```

### Iterable.Keyed#takeUntil()

返回包含来自此Iterable的条目的相同类型的新Iterable，只要`predicate`返回false即可。

```javascript
takeUntil(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): Iterable<K, V>
```

#### Inherited from

```
Iterable#takeUntil
```

#### 例

```javascript
Seq.of('dog','frog','cat','hat','god').takeUntil(x => x.match(/at/))
// ['dog', 'frog']
```

## 组合

### Iterable.Keyed#concat()

用其他值返回一个具有相同类型的新Iterable，并将其连接到此类。

```javascript
concat(...valuesOrIterables: any[]): Iterable<K, V>
```

#### Inherited from

```
Iterable#concat
```

#### 讨论

对于Seqs，即使它们具有相同的密钥，所有条目也会出现在所得到的迭代中。

### Iterable.Keyed#flatten()

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

### Iterable.Keyed#flatMap()

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

Similar to `iter.map(...).flatten(true)`.

## 降低价值

### Iterable.Keyed#reduce()

通过调用Iterable中的`reducer`每个条目并传递减小的值，将Iterable减少为一个值。

```javascript
reduce<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### Inherited from

```
Iterable#reduce
```

#### see

[`Array#reduce`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce).

#### 讨论

如果`initialReduction`未提供，或者为空，则将使用Iterable中的第一项。

### Iterable.Keyed#reduceRight()

反向（从右侧）减少Iterable。

```javascript
reduceRight<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### Inherited from

```
Iterable#reduceRight
```

#### 讨论

注意：类似于this.reverse()。reduce()，并提供与奇偶校验[`Array#reduceRight`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight)。

### Iterable.Keyed#every()

如果`predicate`对Iterable中的所有条目返回true，则返回true。

```javascript
every(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### Inherited from

```
Iterable#every
```

### Iterable.Keyed#some()

如果`predicate`对Iterable中的任何条目返回true，则返回true。

```javascript
some(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### Inherited from

```
Iterable#some
```

### Iterable.Keyed#join()

将值作为字符串连接在一起，在每个值之间插入一个分隔符。默认分隔符是`","`。

```javascript
join(separator?: string): string
```

#### Inherited from

```
Iterable#join
```

### Iterable.Keyed#isEmpty()

如果此Iterable不包含任何值，则返回true。

```javascript
isEmpty(): boolean
```

#### 继承

```
Iterable#isEmpty
```

#### 讨论

对于一些懒惰的`Seq`，`isEmpty`可能需要迭代以确定空虚。至多会发生一次迭代。

### Iterable.Keyed#count()

返回此Iterable的大小。

```javascript
count(): number
count(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): number
```

#### Inherited from

```
Iterable#count
```

#### 讨论

不管这个Iterable是否可以懒惰地描述它的大小（有些Seqs不能），这个方法总是会返回正确的大小。例如，`Seq`如果需要，它会评估一个懒惰。

如果`predicate`提供，则返回Iterable中`predicate`返回值为true 的条目的计数。

### Iterable.Keyed#countBy()

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

### Iterable.Keyed#find()

返回`predicate`返回true 的第一个值。

```javascript
find(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### Inherited from

```
Iterable#find
```

### Iterable.Keyed#findLast()

返回返回值为`predicate`true 的最后一个值。

```javascript
findLast(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### Inherited from

```
Iterable#findLast
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### Iterable.Keyed#findEntry()

返回返回值为true的第一个键值对`predicate`。

```javascript
findEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

#### Inherited from

```
Iterable#findEntry
```

### Iterable.Keyed#findLastEntry()

返回返回值为true的最后一个键值对`predicate`。

```javascript
findLastEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

#### Inherited from

```
Iterable#findLastEntry
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### Iterable.Keyed#findKey()

返回`predicate`返回true 的键。

```javascript
findKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

#### Inherited from

```
Iterable#findKey
```

### Iterable.Keyed#findLastKey()

返回`predicate`返回true 的最后一个键。

```javascript
findLastKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

#### Inherited from

```
Iterable#findLastKey
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### Iterable.Keyed#keyOf()

返回与搜索值关联的键，或者未定义。

```javascript
keyOf(searchValue: V): K
```

#### 继承

```
Iterable#keyOf
```

### Iterable.Keyed#lastKeyOf()

返回与搜索值关联的最后一个键，或者未定义。

```javascript
lastKeyOf(searchValue: V): K
```

#### Inherited from

```
Iterable#lastKeyOf
```

### Iterable.Keyed#max()

返回此集合中的最大值。如果任何值相当相等，则找到的第一个将被返回。

```javascript
max(comparator?: (valueA: V, valueB: V) => number): V
```

#### Inherited from

```
Iterable#max
```

#### 讨论

在`comparator`以同样的方式使用`Iterable#sort`。如果未提供，则默认比较器为`>`。

当两个值被认为是等价的，遇到的第一个将被返回。否则，`max`只要比较器是可交换的，将独立于输入的顺序进行操作。默认比较器*只有*在类型不相同*时才*`>`可以交换。

如果`comparator`返回0，且其中任一值为NaN，undefined或null，则将返回该值。

### Iterable.Keyed#maxBy()

像`max`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

```javascript
maxBy<C>(comparatorValueMapper: (value?: V, key?: K, iter?: Iterable<K, V>) => C,comparator?: (valueA: C, valueB: C) => number): V
```

#### Inherited from

```
Iterable#maxBy
```

#### 例

```javascript
hitters.maxBy(hitter => hitter.avgHits);
```

### Iterable.Keyed#min()

返回此集合中的最小值。如果任何值相当相等，则找到的第一个将被返回。

```javascript
min(comparator?: (valueA: V, valueB: V) => number): V
```

#### Inherited from

```
Iterable#min
```

#### 讨论

在`comparator`以同样的方式使用`Iterable#sort`。如果未提供，则默认比较器为`<`。

当两个值被认为是等价的，遇到的第一个将被返回。否则，`min`只要比较器是可交换的，将独立于输入的顺序进行操作。默认比较器*只有*在类型不相同*时才*`<`可以交换。

如果`comparator`返回0，且其中任一值为NaN，undefined或null，则将返回该值。

### Iterable.Keyed#minBy()

像`min`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

```javascript
minBy<C>(comparatorValueMapper: (value?: V, key?: K, iter?: Iterable<K, V>) => C,comparator?: (valueA: C, valueB: C) => number): V
```

#### Inherited from

```
Iterable#minBy
```

#### 例

```javascript
hitters.minBy(hitter => hitter.avgHits);
```

## 对照

### Iterable.Keyed#isSubset()

如果`iter`包含此Iterable中的每个值，则为真。

```javascript
isSubset(iter: Iterable<any, V>): boolean
isSubset(iter: Array<V>): boolean
```

#### Inherited from

```
Iterable#isSubset
```

### Iterable.Keyed#isSuperset()

如果此Iterable包含每个值，则为真`iter`。

```javascript
isSuperset(iter: Iterable<any, V>): boolean
isSuperset(iter: Array<V>): boolean
```

#### Inherited from

```
Iterable#isSuperset
```

键控的可计算元件具有与每个值相关的离散键。

```javascript
class Iterable.Keyed<K, V> extends Iterable<K, V>
```

#### 讨论

迭代时`Iterable.Keyed`，每次迭代都会产生一个`[K, V]`元组，换句话说，`Iterable#entries`是Keyed Iterables的默认迭代器。

## Construction

### Iterable.Keyed()

创建一个Iterable.Keyed

```javascript
Iterable.Keyed<K, V>(iter: Iterable.Keyed<K, V>): Iterable.Keyed<K, V>
Iterable.Keyed<K, V>(iter: Iterable<any, any>): Iterable.Keyed<K, V>
Iterable.Keyed<K, V>(array: Array<any>): Iterable.Keyed<K, V>
Iterable.Keyed<V>(obj: {[key: string]: V}): Iterable.Keyed<string, V>
Iterable.Keyed<K, V>(iterator: Iterator<any>): Iterable.Keyed<K, V>
Iterable.Keyed<K, V>(iterable: Object): Iterable.Keyed<K, V>
```

#### Discussion

类似于`Iterable()`，但是它期望如果不是从Iterable.Keyed或JS对象构造的K，V元组的迭代式喜欢。

## 转换为Seq

### Iterable.Keyed#toSeq()

返回Seq.Keyed。

```javascript
toSeq(): Seq.Keyed<K, V>
```

#### 覆盖

```
Iterable#toSeq
```

### Iterable.Keyed#toKeyedSeq()

从此Iterable返回一个Seq.Keyed，其索引被视为键。

```javascript
toKeyedSeq(): Seq.Keyed<K, V>
```

#### 继承

```
Iterable#toKeyedSeq
```

#### 讨论

如果您想要对Iterable.Indexed进行操作并保留索引，值对，这非常有用。

返回的Seq将具有与此Iterable相同的迭代顺序。

例：

```javascript
var indexedSeq = Immutable.Seq.of('A', 'B', 'C');
indexedSeq.filter(v => v === 'B').toString() // Seq [ 'B' ]
var keyedSeq = indexedSeq.toKeyedSeq();
keyedSeq.filter(v => v === 'B').toString() // Seq { 1: 'B' }
```

### Iterable.Keyed#toIndexedSeq()

返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
toIndexedSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#toIndexedSeq
```

### Iterable.Keyed#toSetSeq()

返回一个Seq.Set这个Iterable的值，丢弃键。

```javascript
toSetSeq(): Seq.Set<V>
```

#### 继承

```
Iterable#toSetSeq
```

## 序列功能

### Iterable.Keyed#flip()

返回键和值已翻转的同一类型的新Iterable.Keyed。

```javascript
flip(): Iterable.Keyed<V, K>
```

#### 例

```javascript
Seq({ a: 'z', b: 'y' }).flip() // { z: 'a', y: 'b' }
```

### Iterable.Keyed#mapKeys()

使用通过`mapper`函数传递的键返回相同类型的新Iterable.Keyed 。

```javascript
mapKeys<M>(mapper: (key?: K, value?: V, iter?: Iterable.Keyed<K, V>) => M,context?: any): Iterable.Keyed<M, V>
```

#### 例

```javascript
Seq({ a: 1, b: 2 })
  .mapKeys(x => x.toUpperCase())
// Seq { A: 1, B: 2 }
```

### Iterable.Keyed#mapEntries()

通过`mapper`函数传递条目（键，值元组），返回相同类型的新Iterable.Keyed 。

```javascript
mapEntries<KM, VM>(mapper: (entry?: Array<any>,index?: number,iter?: Iterable.Keyed<K, V>) => Array<any>,context?: any): Iterable.Keyed<KM, VM>
```

#### 例

```javascript
Seq({ a: 1, b: 2 })
  .mapEntries(([k, v]) => [k.toUpperCase(), v * 2])
// Seq { A: 2, B: 4 }
```

## 价值平等

### Iterable.Keyed#equals()

如果这和另一个Iterable具有值相等性，则为真，如下定义`Immutable.is()`。

```javascript
equals(other: Iterable<K, V>): boolean
```

#### 继承

```
Iterable#equals
```

#### 讨论

注意：这相当于`Immutable.is(this, other)`，但提供允许链式表达式。

### Iterable.Keyed#hashCode()

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

### Iterable.Keyed#get()

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

### Iterable.Keyed#has()

如果此关键字存在`Iterable`，则为真，`Immutable.is`用于确定相等性

```javascript
has(key: K): boolean
```

#### 继承

```
Iterable#has
```

### Iterable.Keyed#includes()

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

### Iterable.Keyed#first()

Iterable中的第一个值。

```javascript
first(): V
```

#### 继承

```
Iterable#first
```

### Iterable.Keyed#last()

Iterable中的最后一个值。

```javascript
last(): V
```

#### 继承

```
Iterable#last
```

## 读深刻的价值

### Iterable.Keyed#getIn()

通过嵌套的Iterables返回键或索引路径的值。

```javascript
getIn(searchKeyPath: Array<any>, notSetValue?: any): any
getIn(searchKeyPath: Iterable<any, any>, notSetValue?: any): any
```

#### 继承

```
Iterable#getIn
```

### Iterable.Keyed#hasIn()

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

### Iterable.Keyed#toJS()

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

### Iterable.Keyed#toArray()

浅显地将这个迭代器转换为一个Array，丢弃键。

```javascript
toArray(): Array<V>
```

#### 继承

```
Iterable#toArray
```

### Iterable.Keyed#toObject()

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

### Iterable.Keyed#toMap()

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

### Iterable.Keyed#toOrderedMap()

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

### Iterable.Keyed#toSet()

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

### Iterable.Keyed#toOrderedSet()

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

### Iterable.Keyed#toList()

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

### Iterable.Keyed#toStack()

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

### Iterable.Keyed#keys()

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

### Iterable.Keyed#values()

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

### Iterable.Keyed#entries()

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

### Iterable.Keyed#keySeq()

返回此Iterable的新键的Seq.Indexed，放弃值。

```javascript
keySeq(): Seq.Indexed<K>
```

#### 继承

```
Iterable#keySeq
```

### Iterable.Keyed#valueSeq()

返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
valueSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#valueSeq
```

### Iterable.Keyed#entrySeq()

返回一个新的Seq.Indexed键值值元组。

```javascript
entrySeq(): Seq.Indexed<Array<any>>
```

#### 继承

```
Iterable#entrySeq
```

## 序列算法

### Iterable.Keyed#map()

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

### Iterable.Keyed#filter()

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

### Iterable.Keyed#filterNot()

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

### Iterable.Keyed#reverse()

按相反顺序返回相同类型的新Iterable。

```javascript
reverse(): Iterable<K, V>
```

#### 继承

```
Iterable#reverse
```

### Iterable.Keyed#sort()

返回包含相同条目的相同类型的新Iterable，通过使用a进行稳定排序`comparator`。

```javascript
sort(comparator?: (valueA: V, valueB: V) => number): Iterable<K, V>
```

#### 继承

```
Iterable#sort
```

#### 讨论

如果`comparator`没有提供a，默认比较器使用`<`和`>`。

`comparator(valueA, valueB)`:

- 返回`0`元素不应该交换的情况。
- 返回`-1`（或任何负数）如果`valueA`之前`valueB`
- 返回`1`（或任何正数）如果`valueA`后来`valueB`
- 是纯粹的，即它必须始终为同一对值返回相同的值。

排序没有定义顺序的集合时，它们的顺序等价物将被返回。例如`map.sort()`返回OrderedMap。

### Iterable.Keyed#sortBy()

喜欢`sort`，但也接受一个`comparatorValueMapper`允许更复杂的手段进行排序的一个：

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

### Iterable.Keyed#groupBy()

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

### Iterable.Keyed#forEach()

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

### Iterable.Keyed#slice()

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

### Iterable.Keyed#rest()

返回包含除第一个以外的所有条目的同一类型的新Iterable。

```javascript
rest(): Iterable<K, V>
```

#### 继承

```
Iterable#rest
```

### Iterable.Keyed#butLast()

返回包含除最后一个以外的所有条目的同一类型的新Iterable。

```javascript
butLast(): Iterable<K, V>
```

#### 继承

```
Iterable#butLast
```

### Iterable.Keyed#skip()

返回`amount`从此Iterable中排除第一个条目的同一类型的新Iterable。

```javascript
skip(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skip
```

### Iterable.Keyed#skipLast()

返回`amount`从此Iterable中排除最后一个条目的同一类型的新Iterable。

```javascript
skipLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skipLast
```

### Iterable.Keyed#skipWhile()

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

### Iterable.Keyed#skipUntil()

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

### Iterable.Keyed#take()

返回包含`amount`此Iterable中第一个条目的相同类型的新Iterable。

```javascript
take(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#take
```

### Iterable.Keyed#takeLast()

返回包含`amount`此Iterable中最后一个条目的相同类型的新Iterable。

```javascript
takeLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#takeLast
```

### Iterable.Keyed#takeWhile()

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

### Iterable.Keyed#takeUntil()

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

### Iterable.Keyed#concat()

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

### Iterable.Keyed#flatten()

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

### Iterable.Keyed#flatMap()

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

Similar to `iter.map(...).flatten(true)`.

## 降低价值

### Iterable.Keyed#reduce()

通过调用Iterable中的`reducer`每个条目并传递缩小的值，将Iterable减少为一个值。

```javascript
reduce<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### 继承

```
Iterable#reduce
```

#### see

[`Array#reduce`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce).

#### 讨论

如果`initialReduction`未提供，或者为空，则将使用Iterable中的第一项。

### Iterable.Keyed#reduceRight()

反向（从右侧）减少Iterable。

```javascript
reduceRight<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### 继承

```
Iterable#reduceRight
```

#### 讨论

注意：类似于this.reverse（）。reduce（），并提供与奇偶校验[`Array#reduceRight`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight)。

### Iterable.Keyed#every()

如果`predicate`对Iterable中的所有条目返回true，则返回true。

```javascript
every(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#every
```

### Iterable.Keyed#some()

如果`predicate`对Iterable中的任何条目返回true，则返回true。

```javascript
some(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#some
```

### Iterable.Keyed#join()

将值作为字符串连接在一起，在每个值之间插入一个分隔符。默认分隔符是`","`。

```javascript
join(separator?: string): string
```

#### 继承

```
Iterable#join
```

### Iterable.Keyed#isEmpty()

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

### Iterable.Keyed#count()

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

### Iterable.Keyed#countBy()

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

## 搜索值

### Iterable.Keyed#find()

返回`predicate`返回true 的第一个值。

```javascript
find(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### 继承

```
Iterable#find
```

### Iterable.Keyed#findLast()

返回返回值为`predicate`true 的最后一个值。

```javascript
findLast(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### 继承

```
Iterable#findLast
```

#### Discussion

注意：`predicate`每个条目都会被调用。

### Iterable.Keyed#findEntry()

返回返回值为true的第一个键值对`predicate`。

```javascript
findEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

#### 继承

```
Iterable#findEntry
```

### Iterable.Keyed#findLastEntry()

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

### Iterable.Keyed#findKey()

返回`predicate`返回true 的键。

```javascript
findKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

#### 继承

```
Iterable#findKey
```

### Iterable.Keyed#findLastKey()

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

### Iterable.Keyed#keyOf()

返回与搜索值关联的键，或者未定义。

```javascript
keyOf(searchValue: V): K
```

#### 继承

```
Iterable#keyOf
```

### Iterable.Keyed#lastKeyOf()

返回与搜索值关联的最后一个键，或者未定义。

```javascript
lastKeyOf(searchValue: V): K
```

#### 继承

```
Iterable#lastKeyOf
```

### Iterable.Keyed#max()

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

### Iterable.Keyed#maxBy()

喜欢`max`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

```javascript
maxBy<C>(comparatorValueMapper: (value?: V, key?: K, iter?: Iterable<K, V>) => C,comparator?: (valueA: C, valueB: C) => number): V
```

继承于

```
Iterable#maxBy
```

#### 例

```javascript
hitters.maxBy(hitter => hitter.avgHits);
```

### Iterable.Keyed#min()

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

### Iterable.Keyed#minBy()

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

### Iterable.Keyed#isSubset()

如果`iter`包含此Iterable中的每个值，则为真。

```javascript
isSubset(iter: Iterable<any, V>): boolean
isSubset(iter: Array<V>): boolean
```

#### 继承

```
Iterable#isSubset
```

### Iterable.Keyed#isSuperset()

如果此Iterable包含每个值，则为真`iter`。

```javascript
isSuperset(iter: Iterable<any, V>): boolean
isSuperset(iter: Array<V>): boolean
```

#### 继承

```
Iterable#isSuperset
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-04-11