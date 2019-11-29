# Seq

这表示一系列值，但不能由具体的数据结构支持。

```javascript
class Seq<K, V> extends Iterable<K, V>
```

#### 讨论

**Seq是不可变的** - 一旦创建了Seq，它就不能被更改，附加到，重新排列或以其他方式修改。相反，任何在a上调用的变化方法`Seq`都会返回一个新的`Seq`。

**Seq是懒惰的** - Seq只需要做很少的工作来响应任何方法调用。值通常在迭代过程中创建，包括在减少或转换为具体数据结构（如`List`JavaScript或JavaScript）时的隐式迭代[`Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)。

例如，以下内容不会执行任何操作，因为生成的Seq值永远不会被迭代：

```javascript
var oddSquares = Immutable.Seq.of(1,2,3,4,5,6,7,8)
  .filter(x => x % 2).map(x => x * x);
```

一旦Seq被使用，它只执行必要的工作。在这个例子中，没有创建任何中间数据结构，过滤器只被调用三次，map只被调用一次：

```javascript
console.log(oddSquares.get(1)); // 9
```

Seq允许操作的高效链接，从而可以表达逻辑，否则这些逻辑可能非常单调：

```javascript
Immutable.Seq({a:1, b:1, c:1})
  .flip().map(key => key.toUpperCase()).flip().toObject();
// Map { A: 1, B: 1, C: 1 }
```

以及表达逻辑，否则将是内存或时间限制：

```javascript
Immutable.Range(1, Infinity)
  .skip(1000)
  .map(n => -n)
  .filter(n => n % 2 === 0)
  .take(2)
  .reduce((r, n) => r * n, 1);
// 1006008
```

Seq通常用于为JavaScript Object提供丰富的集合API。

```javascript
Immutable.Seq({ x: 0, y: 1, z: 2 }).map(v => v * 2).toObject();
// { x: 0, y: 2, z: 4 }
```

## 命令指示

### Seq()

创建一个Seq。

```javascript
Seq<K, V>(): Seq<K, V>
Seq<K, V>(seq: Seq<K, V>): Seq<K, V>
Seq<K, V>(iterable: Iterable<K, V>): Seq<K, V>
Seq<T>(array: Array<T>): Seq.Indexed<T>
Seq<V>(obj: {[key: string]: V}): Seq.Keyed<string, V>
Seq<T>(iterator: Iterator<T>): Seq.Indexed<T>
Seq<T>(iterable: Object): Seq.Indexed<T>
```

#### 讨论

`Seq`根据输入返回一种特定类型。

- 如果是一个`Seq`，那返回一样的`Seq`。
- 如果是`Iterable`，返回一个同种（键控，索引，或设置）`Seq`。
- 如果是一个数组，返回一个`Seq.Indexed`。
- 如果是一个有一个迭代器的对象，返回一个`Seq.Indexed`。
- 如果是一个迭代器，返回一个`Seq.Indexed`。
- 如果是一个对象，则返回一个`Seq.Keyed`。

## 静态方法

### Seq.isSeq()

如果`maybeSeq`是Seq，则为真，它不受具体结构（如Map，List或Set）的支持。

```javascript
Seq.isSeq(maybeSeq: any): boolean
```

### Seq.of()

返回提供的值的Seq。别名为`Seq.Indexed.of()`。

```javascript
Seq.of<T>(...values: T[]): Seq.Indexed<T>
```

## 类型

Seq.Keyed

## 成员

### Seq#size

```javascript
size: number
```

## 强制评价(Force evaluation)

### Seq#cacheResult()

因为序列是懒惰的并且被设计为链接在一起，所以它们不会缓存它们的结果。例如，这个映射函数总共被称为6次，因为每个`join`都迭代三个值的Seq。

```javascript
cacheResult(): Seq<K, V>
```

#### 例

```javascript
var squares = Seq.of(1,2,3).map(x => x * x);
squares.join() + squares.join();
```

如果您知道一个`Seq`将被多次使用，首先将其缓存在内存中可能会更有效。在这里，地图功能只被调用3次。

```javascript
var squares = Seq.of(1,2,3).map(x => x * x).cacheResult();
squares.join() + squares.join();
```

明智地使用这种方法，因为它必须充分评估一个可能成为内存和可能性能负担的Seq。

注意：调用后`cacheResult`，Seq将始终有一个`size`。

## 值相等

### Seq#equals()

如果这和另一个Iterable具有值相等性，则为真，如下定义`Immutable.is()`。

```javascript
equals(other: Iterable<K, V>): boolean
```

#### 继承

```
Iterable#equals
```

#### 讨论

注意：这相当于`Immutable.is(this, other)`，但允许提供链式表达式。

### Seq#hashCode()

计算并返回此Iterable的散列标识。

```javascript
hashCode(): number
```

#### 继承

```
Iterable#hashCode
```

#### 讨论

Iterable的hashCode用于确定潜在的相等性，在将其添加到Set或Map中的键时使用，可以通过不同的实例进行查找。

```javascript
var a = List.of(1, 2, 3);
var b = List.of(1, 2, 3);
assert(a !== b); // different instances
var set = Set.of(a);
assert(set.has(b) === true);
```

如果两个值相同`hashCode`，则不能保证相等；如果两个值有不同的`hashCode`s，则它们不能相等。

## 读取值

### Seq#get()

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

### Seq#has()

如果此关键字存在`Iterable`，则为真，`Immutable.is`用于确定相等性

```javascript
has(key: K): boolean
```

#### 继承

```
Iterable#has
```

### Seq#includes()

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

### Seq#first()

Iterable中的第一个值。

```javascript
first(): V
```

#### 继承

```
Iterable#first
```

### Seq#last()

Iterable中的最后一个值。

```javascript
last(): V
```

#### 继承

```
Iterable#last
```

## 读更深的值

### Seq#getIn()

通过嵌套的Iterables返回键或索引路径的值。

```javascript
getIn(searchKeyPath: Array<any>, notSetValue?: any): any
getIn(searchKeyPath: Iterable<any, any>, notSetValue?: any): any
```

#### 继承

```
Iterable#getIn
```

### Seq#hasIn()

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

### Seq#toJS()

将此Iterable深度转换为等效的JS。

```javascript
toJS(): any
```

#### 继承

```
Iterable#toJS
```

#### 别称

```
toJSON()
```

#### 讨论

Iterable.Indexeds和Iterable.Sets成为数组，而Iterable.Keyeds成为对象。

### Seq#toArray()

在浅显层面上将这个迭代器转换为一个Array，丢弃键。

```javascript
toArray(): Array<V>
```

#### 继承

```
Iterable#toArray
```

### Seq#toObject()

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

### Seq#toMap()

将此Iterable转换为Map，如果键不可哈希则抛出。

```javascript
toMap(): Map<K, V>
```

#### 继承

```
Iterable#toMap
```

#### 讨论

注意：这相当于`Map(this.toKeyedSeq())`，但为方便起见而允许链接表达式。

### Seq#toOrderedMap()

将此Iterable转换为Map，并保持迭代顺序。

```javascript
toOrderedMap(): OrderedMap<K, V>
```

#### 继承

```
Iterable#toOrderedMap
```

#### 讨论

注意：这相当于`OrderedMap(this.toKeyedSeq())`，但为方便起见而允许链接表达式。

### Seq#toSet()

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

### Seq#toOrderedSet()

将此Iterable转换为Set，保持迭代顺序并丢弃键。

```javascript
toOrderedSet(): OrderedSet<V>
```

#### 继承

```
Iterable#toOrderedSet
```

#### 讨论

注意：这相当于`OrderedSet(this.valueSeq())`，但为方便起见而允许链接表达式。

### Seq#toList()

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

### Seq#toStack()

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

## 转换为Seq

### Seq#toSeq()

将此Iterable转换为相同类型的Seq（索引，键控或设置）。

```javascript
toSeq(): Seq<K, V>
```

#### 继承

```
Iterable#toSeq
```

### Seq#toKeyedSeq()

从此Iterable返回一个Seq.Keyed，其索引被视为键。

```javascript
toKeyedSeq(): Seq.Keyed<K, V>
```

#### 继承

```
Iterable#toKeyedSeq
```

#### 讨论

如果您想要对Iterable.Indexed进行操作并保留索引、值对，这会非常有用。

返回的Seq将具有与此Iterable相同的迭代顺序。

例：

```javascript
var indexedSeq = Immutable.Seq.of('A', 'B', 'C');
indexedSeq.filter(v => v === 'B').toString() // Seq [ 'B' ]
var keyedSeq = indexedSeq.toKeyedSeq();
keyedSeq.filter(v => v === 'B').toString() // Seq { 1: 'B' }
```

### Seq#toIndexedSeq()

返回一个Seq.Indexed此Iterable的值，丢弃键。

```javascript
toIndexedSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#toIndexedSeq
```

### Seq#toSetSeq()

返回一个Seq.Set此Iterable的值，丢弃键。

```javascript
toSetSeq(): Seq.Set<V>
```

#### 继承

```
Iterable#toSetSeq
```

## 迭代器

### Seq#keys()

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

### Seq#values()

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

### Seq#entries()

将这个`Iterable`条目的迭代器作为`[key, value]`元组。

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

### Seq#keySeq()

返回此Iterable的新键的Seq.Indexed，放弃值。

```javascript
keySeq(): Seq.Indexed<K>
```

#### 继承

```
Iterable#keySeq
```

### Seq#valueSeq()

返回一个此Iterable的值的Seq.Indexed，丢弃键。

```javascript
valueSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#valueSeq
```

### Seq#entrySeq()

返回一个新的Seq.Indexed键值值元组。

```javascript
entrySeq(): Seq.Indexed<Array<any>>
```

#### 继承

```
Iterable#entrySeq
```

## 序列算法

### Seq#map()

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

### Seq#filter()

仅返回谓词函数返回true的条目的同一类型的新Iterable。

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

### Seq#filterNot()

仅返回谓词函数返回false的条目返回相同类型的新Iterable。

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

### Seq#reverse()

按相反顺序返回相同类型的新Iterable。

```javascript
reverse(): Iterable<K, V>
```

#### 继承

```
Iterable#reverse
```

### Seq#sort()

返回包含相同条目的相同类型的新Iterable，并使用比较器进行稳定排序。

```javascript
sort(comparator?: (valueA: V, valueB: V) => number): Iterable<K, V>
```

#### 继承

```
Iterable#sort
```

#### 讨论

如果`comparator`没有被提供，则默认比较器使用`<`和`>`。

`comparator(valueA, valueB)`:

- 返回`0 `如果元素不应该交换的情况。
- 返回`-1`（或任何负数），如果valueA在valueB 之前
- 返回`1`（或任何正数），如果valueA在valueB 之后
- 是纯粹的，即它必须始终为同一对值返回相同的值。

排序没有定义顺序的集合时，它们的顺序等价物将被返回。例如`map.sort()`返回OrderedMap。

### Seq#sortBy()

比如`sort`，但也接受一个`comparatorValueMapper`允许更复杂的手段进行排序的一个：

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

### Seq#groupBy()

返回Iterable.Keyeds的Iterable.Keyed，按照grouper函数的返回值进行分组。

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

### Seq#forEach()

该`sideEffect`是在可迭代的每个条目执行的。

```javascript
forEach(sideEffect: (value?: V, key?: K, iter?: Iterable<K, V>) => any,context?: any): number
```

#### 继承

```
Iterable#forEach
```

#### 讨论

不同的是[`Array#forEach`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)，如果有任何`sideEffect`回报的话返回`false`，迭代将停止。返回迭代的条目数（包括返回false的最后一次迭代）。

## 创建子集

### Seq#slice()

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

### Seq#rest()

返回包含除第一个以外的所有条目的同一类型的新Iterable。

```javascript
rest(): Iterable<K, V>
```

#### 继承

```
Iterable#rest
```

### Seq#butLast()

返回包含除最后一个以外的所有条目的同一类型的新Iterable。

```javascript
butLast(): Iterable<K, V>
```

#### 继承

```
Iterable#butLast
```

### Seq#skip()

返回`amount`，从此Iterable中排除第一个条目的同一类型的新Iterable。

```javascript
skip(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skip
```

### Seq#skipLast()

返回从此Iterable中排除最后一个条目的相同类型的新Iterable。

```javascript
skipLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skipLast
```

### Seq#skipWhile()

返回相同类型的新Iterable，其中包含从谓词第一次返回false时开始的条目。

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

### Seq#skipUntil()

返回相同类型的新Iterable，其中包含从谓词第一次返回true时开始的条目。

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

### Seq#take()

返回包含此Iterable中第一个条目的相同类型的新Iterable。

```javascript
take(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#take
```

### Seq#takeLast()

返回包含此Iterable中最后一个条目的相同类型的新Iterable。

```javascript
takeLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#takeLast
```

### Seq#takeWhile()

只要谓词返回true，就返回包含来自此Iterable的条目的相同类型的新Iterable。

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

### Seq#takeUntil()

只要谓词返回false，就返回包含来自此Iterable的条目的相同类型的新Iterable。

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

### Seq#concat()

用其他值返回一个具有相同类型的新Iterable，并将其连接到此。

```javascript
concat(...valuesOrIterables: any[]): Iterable<K, V>
```

#### 继承

```
Iterable#concat
```

#### 讨论

对于Seqs，即使它们具有相同的密钥，所有条目也会出现在所得到的迭代中。

### Seq#flatten()

平化嵌套Iterables。

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

仅扁平化其他的Iterable，而不是阵列或对象。

注意：`flatten(true)`在<Iterable>上运行并返回Iterable

### Seq#flatMap()

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

## 降低值

### Seq#reduce()

通过调用Iterable中的`reducer`每个条目并传递缩小的值，将Iterable减少为一个值。

```javascript
reduce<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### 继承

```
Iterable#reduce
```

#### 讨论

如果`initialReduction`未提供，或者为空，则将使用Iterable中的第一项。

### Seq#reduceRight()

反向（从右侧）减少Iterable。

```javascript
reduceRight<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### 继承

```
Iterable#reduceRight
```

#### 讨论

注意：类似于this.reverse（）。reduce（），并提供奇偶校验。

### Seq#every()

如果`predicate`对Iterable中的所有条目都返回true，则返回true。

```javascript
every(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#every
```

### Seq#some()

如果`predicate`对Iterable中的任何条目返回true，则返回true。

```javascript
some(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#some
```

### Seq#join()

将值作为字符串连接在一起，在每个值之间插入一个分隔符。默认分隔符是`","`。

```javascript
join(separator?: string): string
```

#### 继承

```
Iterable#join
```

### Seq#isEmpty()

如果此Iterable不包含任何值，则返回true。

```javascript
isEmpty(): boolean
```

#### 继承

```
Iterable#isEmpty
```

#### 讨论

对于一些空闲的`Seq`，`isEmpty`可能需要迭代以确定是否为空。至多会发生一次迭代。

### Seq#count()

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

不管这个Iterable是否可以描述它的大小（有些Seqs不能），这个方法总是会返回正确的大小。例如，如果需要，它会评估一个懒惰型`Seq`

如果提供谓词，则返回谓词返回true的Iterable中条目的数目。

### Seq#countBy()

返回计数的Seq.Keyed，按照grouper函数的返回值分组。

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

### Seq#find()

返回`predicate`返回true 的第一个值。

```javascript
find(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### 继承

```
Iterable#find
```

### Seq#findLast()

返回谓词返回true的最后一个值。

```javascript
findLast(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### 继承

```
Iterable#findLast
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### Seq#findEntry()

返回谓词返回true的第一个键值对。

```javascript
findEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

#### 继承

```
Iterable#findEntry
```

### Seq#findLastEntry()

返回谓词返回true的最后一个键值对。

```javascript
findLastEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

#### 继承

```
Iterable#findLastEntry
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### Seq#findKey()

返回`predicate`返回true 的键。

```javascript
findKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

#### 继承

```
Iterable#findKey
```

### Seq#findLastKey()

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

### Seq#keyOf()

返回与搜索值关联的键，或者未定义。

```javascript
keyOf(searchValue: V): K
```

#### 继承

```
Iterable#keyOf
```

### Seq#lastKeyOf()

返回与搜索值关联的最后一个键，或者未定义。

```javascript
lastKeyOf(searchValue: V): K
```

#### 继承

```
Iterable#lastKeyOf
```

### Seq#max()

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

当两个值被认为是等价的，遇到的第一个将被返回。否则，`max`只要比较器是可交换的，就将独立于输入的顺序进行操作。默认比较器*只有*在类型不相同*时才*`>`可以交换。

如果`comparator`返回0，且其中任一值为NaN、undefined或null，则将返回该值。

### Seq#maxBy()

例如`max`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

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

### Seq#min()

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

如果`comparator`返回0，且其中任一值为NaN、undefined或null，则将返回该值。

### Seq#minBy()

例如`min`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

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

### Seq#isSubset()

如果`iter`包含此Iterable中的每个值，则为真。

```javascript
isSubset(iter: Iterable<any, V>): boolean
isSubset(iter: Array<V>): boolean
```

#### 继承

```
Iterable#isSubset
```

### Seq#isSuperset()

如果此Iterable包含iter的每个值，则为真。

```javascript
isSuperset(iter: Iterable<any, V>): boolean
isSuperset(iter: Array<V>): boolean
```

#### 继承

```
Iterable#isSuperset
```

`Seq` 代表键值对。

```javascript
class Seq.Keyed<K, V> extends Seq<K, V>, Iterable.Keyed<K, V>
```

## 命令

### Seq.Keyed()

总是返回一个Seq.Keyed，如果输入未被键入，则需要一个可迭代的K，V元组。

```javascript
Seq.Keyed<K, V>(): Seq.Keyed<K, V>
Seq.Keyed<K, V>(seq: Iterable.Keyed<K, V>): Seq.Keyed<K, V>
Seq.Keyed<K, V>(seq: Iterable<any, any>): Seq.Keyed<K, V>
Seq.Keyed<K, V>(array: Array<any>): Seq.Keyed<K, V>
Seq.Keyed<V>(obj: {[key: string]: V}): Seq.Keyed<string, V>
Seq.Keyed<K, V>(iterator: Iterator<any>): Seq.Keyed<K, V>
Seq.Keyed<K, V>(iterable: Object): Seq.Keyed<K, V>
```

## 成员

### Seq.Keyed#size

```javascript
size: number
```

#### 继承

```
Seq#size
```

## 转换为Seq

### Seq.Keyed#toSeq()

返回自己

```javascript
toSeq(): Seq.Keyed<K, V>
```

#### 覆盖

```
Iterable#toSeq
```

### Seq.Keyed#toKeyedSeq()

从此Iterable返回一个Seq.Keyed，其索引被视为键。

```javascript
toKeyedSeq(): Seq.Keyed<K, V>
```

#### 继承

```
Iterable#toKeyedSeq
```

#### 讨论

如果您想要对Iterable.Indexed进行操作并保留索引、值对，这非常有用。

返回的Seq将具有与此Iterable相同的迭代顺序。

例：

```javascript
var indexedSeq = Immutable.Seq.of('A', 'B', 'C');
indexedSeq.filter(v => v === 'B').toString() // Seq [ 'B' ]
var keyedSeq = indexedSeq.toKeyedSeq();
keyedSeq.filter(v => v === 'B').toString() // Seq { 1: 'B' }
```

### Seq.Keyed#toIndexedSeq()

返回一个Iterable的值的Seq.Indexed，丢弃键。

```javascript
toIndexedSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#toIndexedSeq
```

### Seq.Keyed#toSetSeq()

返回一个Iterable的值的Seq.Set，丢弃键。

```javascript
toSetSeq(): Seq.Set<V>
```

#### 继承

```
Iterable#toSetSeq
```

## 强制评估

### Seq.Keyed#cacheResult()

因为序列是懒惰的并且被设计为链接在一起，所以它们不会缓存它们的结果。例如，这个映射函数总共被称为6次，因为每个`join`迭代三个值的Seq。

```javascript
cacheResult(): Seq<K, V>
```

#### 继承

```
Seq#cacheResult
```

#### 例

```javascript
var squares = Seq.of(1,2,3).map(x => x * x);
squares.join() + squares.join();
```

如果您知道一个`Seq`将被多次使用，首先将其缓存在内存中可能会更有效。在这里，地图功能只被调用3次。

```javascript
var squares = Seq.of(1,2,3).map(x => x * x).cacheResult();
squares.join() + squares.join();
```

明智地使用这种方法，因为它必须充分评估一个可能成为内存和可能性能负担的Seq。

注意：调用后`cacheResult`，Seq将始终有一个`size`。

## 值相等

### Seq.Keyed#equals()

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

### Seq.Keyed#hashCode()

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

如果两个值`hashCode`相同，则不能保证相等。如果两个值有不同的`hashCode`s，则它们不能相等。

## 读取值

### Seq.Keyed#get()

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

### Seq.Keyed#has()

如果此关键字存在`Iterable`，则为真，`Immutable.is`用于确定相等性

```javascript
has(key: K): boolean
```

#### 继承

```
Iterable#has
```

### Seq.Keyed#includes()

如果此值中存在值`Iterable`，则为true ，`Immutable.is`用于确定相等性

```javascript
includes(value: V): boolean
```

#### 继承

```
Iterable#includes
```

#### 别称

```
contains()
```

### Seq.Keyed#first()

Iterable中的第一个值。

```javascript
first(): V
```

#### 继承

```
Iterable#first
```

### Seq.Keyed#last()

Iterable中的最后一个值。

```javascript
last(): V
```

#### 继承

```
Iterable#last
```

## 读深式的值

### Seq.Keyed#getIn()

通过嵌套的Iterables返回键或索引路径的值。

```javascript
getIn(searchKeyPath: Array<any>, notSetValue?: any): any
getIn(searchKeyPath: Iterable<any, any>, notSetValue?: any): any
```

#### 继承

```
Iterable#getIn
```

### Seq.Keyed#hasIn()

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

### Seq.Keyed#toJS()

将此Iterable深度转换为等效的JS。

```javascript
toJS(): any
```

#### 继承

```
Iterable#toJS
```

#### 别称

```
toJSON()
```

#### 讨论

Iterable.Indexeds和Iterable.Sets成为数组，而Iterable.Keyeds成为对象。

### Seq.Keyed#toArray()

浅显地将这个迭代器转换为一个Array，丢弃键。

```javascript
toArray(): Array<V>
```

#### 继承

```
Iterable#toArray
```

### Seq.Keyed#toObject()

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

### Seq.Keyed#toMap()

将此Iterable转换为Map，如果键不可哈希则抛出。

```javascript
toMap(): Map<K, V>
```

#### 继承

```
Iterable#toMap
```

#### 讨论

注意：这相当于`Map(this.toKeyedSeq())`，但为方便起见而允许链接表达式。

### Seq.Keyed#toOrderedMap()

将此Iterable转换为Map，并保持迭代顺序。

```javascript
toOrderedMap(): OrderedMap<K, V>
```

#### 继承

```
Iterable#toOrderedMap
```

#### 讨论

注意：这与OrderedMap（this.toKeyedSeq（））等价，但为方便起见并允许链接表达式。

### Seq.Keyed#toSet()

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

### Seq.Keyed#toOrderedSet()

将此Iterable转换为Set，保持迭代顺序并丢弃键。

```javascript
toOrderedSet(): OrderedSet<V>
```

#### 继承

```
Iterable#toOrderedSet
```

#### 讨论

注意：这相当于`OrderedSet(this.valueSeq())`，但为方便起见而允许链接表达式。

### Seq.Keyed#toList()

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

### Seq.Keyed#toStack()

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

### Seq.Keyed#keys()

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

### Seq.Keyed#values()

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

### Seq.Keyed#entries()

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

### Seq.Keyed#keySeq()

返回此Iterable的新键的Seq.Indexed，放弃值。

```javascript
keySeq(): Seq.Indexed<K>
```

#### 继承

```
Iterable#keySeq
```

### Seq.Keyed#valueSeq()

返回一个Iterable的值的Seq.Indexed，丢弃键。

```javascript
valueSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#valueSeq
```

### Seq.Keyed#entrySeq()

返回一个新的Seq.Indexed键值值元组。

```javascript
entrySeq(): Seq.Indexed<Array<any>>
```

#### 继承

```
Iterable#entrySeq
```

## 序列算法

### Seq.Keyed#map()

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

### Seq.Keyed#filter()

仅返回谓词函数返回true的条目的同一类型的新Iterable。

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

### Seq.Keyed#filterNot()

仅返回谓词函数返回false的条目返回相同类型的新Iterable。

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

### Seq.Keyed#reverse()

按相反顺序返回相同类型的新Iterable。

```javascript
reverse(): Iterable<K, V>
```

#### 继承

```
Iterable#reverse
```

### Seq.Keyed#sort()

返回包含相同条目的相同类型的新Iterable，并使用比较器进行稳定排序。

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

- 返回`0 `若元素属于不应该交换的情况。
- 返回`-1`（或任何负数），如果`valueA`在`valueB` 之前
- 返回`1`（或任何正数），如果`valueA`在`valueB` 之后
- 是纯粹的，即它必须始终为同一对值返回相同的值。

排序没有定义顺序的集合时，它们的顺序等价物将被返回。例如`map.sort()`返回OrderedMap。

### Seq.Keyed#sortBy()

例如`sort`，但也接受一个`comparatorValueMapper`允许更复杂的手段进行排序的一个：

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

### Seq.Keyed#groupBy()

返回Iterable.Keyeds的Iterable.Keyed，按照grouper函数的返回值进行分组。

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

### Seq.Keyed#forEach()

该`sideEffect`是在可迭代的每个条目执行的。

```javascript
forEach(sideEffect: (value?: V, key?: K, iter?: Iterable<K, V>) => any,context?: any): number
```

#### 继承

```
Iterable#forEach
```

#### 讨论

与Array＃forEach不同，如果任何sideEffect调用返回false，则迭代将停止。 返回迭代的条目数（包括返回false的最后一次迭代）。

## 创建子集

### Seq.Keyed#slice()

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

### Seq.Keyed#rest()

返回包含除第一个以外的所有条目的同一类型的新Iterable。

```javascript
rest(): Iterable<K, V>
```

#### 继承

```
Iterable#rest
```

### Seq.Keyed#butLast()

返回包含除最后一个以外的所有条目的同一类型的新Iterable。

```javascript
butLast(): Iterable<K, V>
```

#### 继承

```
Iterable#butLast
```

### Seq.Keyed#skip()

返回从此Iterable中排除第一个数量条目的同一类型的新Iterable。

```javascript
skip(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skip
```

### Seq.Keyed#skipLast()

返回从此Iterable中排除最后一个条目的相同类型的新Iterable。

```javascript
skipLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skipLast
```

### Seq.Keyed#skipWhile()

返回相同类型的新Iterable，其中包含从谓词第一次返回false时开始的条目。

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

### Seq.Keyed#skipUntil()

返回相同类型的新Iterable，其中包含从谓词第一次返回true时开始的条目。

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

### Seq.Keyed#take()

返回包含此Iterable中第一个条目的相同类型的新Iterable。

```javascript
take(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#take
```

### Seq.Keyed#takeLast()

返回包含此Iterable中最后一个条目的相同类型的新Iterable。

```javascript
takeLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#takeLast
```

### Seq.Keyed#takeWhile()

只要谓词返回true，就返回包含来自此Iterable的条目的相同类型的新Iterable。

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

### Seq.Keyed#takeUntil()

只要谓词返回false，就返回包含来自此Iterable的条目的相同类型的新Iterable。

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

### Seq.Keyed#concat()

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

### Seq.Keyed#flatten()

扁平嵌套的Iterables。

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

仅扁平化其他的Iterable，而不是阵列或对象。

注意：`flatten(true)`在<Iterable>上运行并返回Iterable

### Seq.Keyed#flatMap()

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

类似于`iter.map(...).flatten(true)`.

## 降低值

### Seq.Keyed#reduce()

通过调用Iterable中的`reducer`每个条目并传递缩小的值，将Iterable减少为一个值。

```javascript
reduce<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### 继承

```
Iterable#reduce
```

#### 讨论

如果`initialReduction`未提供，或者为空，则将使用Iterable中的第一项。

### Seq.Keyed#reduceRight()

反向（从右侧）减少Iterable。

```javascript
reduceRight<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### 继承

```
Iterable#reduceRight
```

#### 讨论

注意：类似于this.reverse（）.reduce（），并提供奇偶校验。

### Seq.Keyed#every()

如果`predicate`对Iterable中的所有条目返回true，则返回true。

```javascript
every(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#every
```

### Seq.Keyed#some()

如果`predicate`对Iterable中的任何条目返回true，则返回true。

```javascript
some(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#some
```

### Seq.Keyed#join()

将值作为字符串连接在一起，在每个值之间插入一个分隔符。默认分隔符是`","`。

```javascript
join(separator?: string): string
```

#### 继承

```
Iterable#join
```

### Seq.Keyed#isEmpty()

如果此Iterable不包含任何值，则返回true。

```javascript
isEmpty(): boolean
```

#### 继承

```
Iterable#isEmpty
```

#### 讨论

对于一些空闲的`Seq`，`isEmpty`可能需要迭代以确定是否为空。至多会发生一次迭代。

### Seq.Keyed#count()

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

不管这个Iterable是否可以描述它的大小（有些Seqs不能），这个方法总是会返回正确的大小。例如，如果需要，它会评估一个懒惰型`Seq`

如果提供`predicate`，则返回Iterable中`predicate`返回值为true 的条目的计数。

### Seq.Keyed#countBy()

返回计数的Seq.Keyed，按照grouper函数的返回值分组。

```javascript
countBy<G>(grouper: (value?: V, key?: K, iter?: Iterable<K, V>) => G,context?: any): Map<G, number>
```

#### 继承

```
Iterable#countBy
```

#### 讨论

注意：这不是一个懒惰型操作。

## 搜索值

### Seq.Keyed#find()

返回`predicate`返回true 的第一个值。

```javascript
find(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### 继承

```
Iterable#find
```

### Seq.Keyed#findLast()

返回谓词返回true的最后一个值。

```javascript
findLast(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### 继承

```
Iterable#findLast
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### Seq.Keyed#findEntry()

返回谓词返回true的第一个键值对。

```javascript
findEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

#### 继承

```
Iterable#findEntry
```

### Seq.Keyed#findLastEntry()

返回谓词返回true的最后一个键值对。

```javascript
findLastEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

#### 继承

```
Iterable#findLastEntry
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### Seq.Keyed#findKey()

返回`predicate`返回true 的键。

```javascript
findKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

#### 继承

```
Iterable#findKey
```

### Seq.Keyed#findLastKey()

返回谓词返回true的最后一个键。

```javascript
findLastKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

#### 继承

```
Iterable#findLastKey
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### Seq.Keyed#keyOf()

返回与搜索值关联的键，或者未定义。

```javascript
keyOf(searchValue: V): K
```

#### 继承

```
Iterable#keyOf
```

### Seq.Keyed#lastKeyOf()

返回与搜索值关联的最后一个键，或者未定义。

```javascript
lastKeyOf(searchValue: V): K
```

#### 继承

```
Iterable#lastKeyOf
```

### Seq.Keyed#max()

返回此集合中的最大值。如果任何值相当相等，则找到的第一个将被返回。

```javascript
max(comparator?: (valueA: V, valueB: V) => number): V
```

#### 继承

```
Iterable#max
```

#### 讨论

比较器的使用方式与Iterable＃sort相同。 如果没有提供，默认比较器是>。

当两个值被认为是等价的，遇到的第一个将被返回。否则，`max`只要比较器是可交换的，将独立于输入的顺序进行操作。默认比较器*只有*在类型不相同*时才*`>`可以交换。

如果`comparator`返回0，且其中任一值为NaN，undefined或null，则将返回该值。

### Seq.Keyed#maxBy()

例如`max`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

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

### Seq.Keyed#min()

返回此集合中的最小值。如果任何值相当相等，则找到的第一个将被返回。

```javascript
min(comparator?: (valueA: V, valueB: V) => number): V
```

#### 继承

```
Iterable#min
```

#### 讨论

比较器的使用方式与Iterable＃排序相同。 如果未提供，则默认比较值为<。

当两个值被认为是等价的，遇到的第一个将被返回。否则，`min`只要比较器是可交换的，将独立于输入的顺序进行操作。默认比较器*只有*在类型不相同*时才*`<`可以交换。

如果`comparator`返回0，且其中任一值为NaN、undefined或null，则将返回该值。

### Seq.Keyed#minBy()

例如`min`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

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

### Seq.Keyed#isSubset()

如果`iter`包含此Iterable中的每个值，则为真。

```javascript
isSubset(iter: Iterable<any, V>): boolean
isSubset(iter: Array<V>): boolean
```

#### 继承

```
Iterable#isSubset
```

### Seq.Keyed#isSuperset()

如果此Iterable包含iter中的每个值，则为true。

```javascript
isSuperset(iter: Iterable<any, V>): boolean
isSuperset(iter: Array<V>): boolean
```

#### 继承

```
Iterable#isSuperset
```

## 序列功能

### Seq.Keyed#flip()

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

### Seq.Keyed#mapKeys()

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

### Seq.Keyed#mapEntries()

通过`mapper`函数传递条目（键，值元组），返回相同类型的新Iterable.Keyed 。

```javascript
mapEntries<KM, VM>(mapper: (entry?: Array<any>,index?: number,iter?: Iterable.Keyed<K, V>) => Array<any>,context?: any): Iterable.Keyed<KM, VM>
```

#### 继承

```
Iterable.Keyed#mapEntries
```

#### 例

```javascript
Seq({ a: 1, b: 2 })
  .mapEntries(([k, v]) => [k.toUpperCase(), v * 2])
// Seq { A: 2, B: 4 }
```

`Seq` 代表键值对。

```javascript
class Seq.Keyed<K, V> extends Seq<K, V>, Iterable.Keyed<K, V>
```

## 施工

### Seq.Keyed()

总是返回一个Seq.Keyed，如果输入未被键入，则需要一个可迭代的K，V元组。

```javascript
Seq.Keyed<K, V>(): Seq.Keyed<K, V>
Seq.Keyed<K, V>(seq: Iterable.Keyed<K, V>): Seq.Keyed<K, V>
Seq.Keyed<K, V>(seq: Iterable<any, any>): Seq.Keyed<K, V>
Seq.Keyed<K, V>(array: Array<any>): Seq.Keyed<K, V>
Seq.Keyed<V>(obj: {[key: string]: V}): Seq.Keyed<string, V>
Seq.Keyed<K, V>(iterator: Iterator<any>): Seq.Keyed<K, V>
Seq.Keyed<K, V>(iterable: Object): Seq.Keyed<K, V>
```

## 成员

### Seq.Keyed#size

```javascript
size: number
```

#### 继承

```
Seq#size
```

## 转换为Seq

### Seq.Keyed#toSeq()

返回自己

```javascript
toSeq(): Seq.Keyed<K, V>
```

#### 覆盖

```
Iterable#toSeq
```

### Seq.Keyed#toKeyedSeq()

从此Iterable返回一个Seq.Keyed，其索引被视为键。

```javascript
toKeyedSeq(): Seq.Keyed<K, V>
```

#### 继承

```
Iterable#toKeyedSeq
```

#### 讨论

如果您想要对Iterable.Indexed进行操作并保留索引、值对，这非常有用。

返回的Seq将具有与此Iterable相同的迭代顺序。

例：

```javascript
var indexedSeq = Immutable.Seq.of('A', 'B', 'C');
indexedSeq.filter(v => v === 'B').toString() // Seq [ 'B' ]
var keyedSeq = indexedSeq.toKeyedSeq();
keyedSeq.filter(v => v === 'B').toString() // Seq { 1: 'B' }
```

### Seq.Keyed#toIndexedSeq()

返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
toIndexedSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#toIndexedSeq
```

### Seq.Keyed#toSetSeq()

返回一个Seq.Set这个Iterable的值，丢弃键。

```javascript
toSetSeq(): Seq.Set<V>
```

#### 继承

```
Iterable#toSetSeq
```

## 强制型评估

### Seq.Keyed#cacheResult()

因为序列是懒惰的并且被设计为链接在一起，所以它们不会缓存它们的结果。例如，这个映射函数总共被称为6次，因为每个`join`迭代三个值的Seq。

```javascript
cacheResult(): Seq<K, V>
```

#### 继承

```
Seq#cacheResult
```

#### 例

```javascript
var squares = Seq.of(1,2,3).map(x => x * x);
squares.join() + squares.join();
```

如果您知道一个`Seq`将被多次使用，首先将其缓存在内存中可能会更有效。在这里，地图功能只被调用3次。

```javascript
var squares = Seq.of(1,2,3).map(x => x * x).cacheResult();
squares.join() + squares.join();
```

明智地使用这种方法，因为它必须充分评估一个可能成为内存和可能性能负担的Seq。

注意：在调用cacheResult之后，Seq将始终有一个size。

## 值相等

### Seq.Keyed#equals()

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

### Seq.Keyed#hashCode()

计算并返回此Iterable的散列标识。

```javascript
hashCode(): number
```

#### 继承

```
Iterable#hashCode
```

#### 讨论

Iterable的hashCode用于确定潜在的相等性，在将其添加到Set或Map中的键时使用，可以通过不同的实例进行查找。

```javascript
var a = List.of(1, 2, 3);
var b = List.of(1, 2, 3);
assert(a !== b); // different instances
var set = Set.of(a);
assert(set.has(b) === true);
```

如果两个值有相同`hashCode`，也不能保证相等。如果两个值有不同的`hashCode`s，则它们不能相等。

## 读取值

### Seq.Keyed#get()

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

### Seq.Keyed#has()

如果此关键字存在`Iterable`，则为真，`Immutable.is`用于确定相等性。

```javascript
has(key: K): boolean
```

#### 继承

```
Iterable#has
```

### Seq.Keyed#includes()

如果此值中存在值`Iterable`，则为true ，`Immutable.is`用于确定相等性

```javascript
includes(value: V): boolean
```

#### 继承

```
Iterable#includes
```

#### 别称

```
contains()
```

### Seq.Keyed#first()

Iterable中的第一个值。

```javascript
first(): V
```

#### 继承

```
Iterable#first
```

### Seq.Keyed#last()

Iterable中的最后一个值。

```javascript
last(): V
```

#### 继承

```
Iterable#last
```

## 读深式的值

### Seq.Keyed#getIn()

通过嵌套的Iterables返回键或索引路径的值。

```javascript
getIn(searchKeyPath: Array<any>, notSetValue?: any): any
getIn(searchKeyPath: Iterable<any, any>, notSetValue?: any): any
```

#### 继承

```
Iterable#getIn
```

### Seq.Keyed#hasIn()

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

### Seq.Keyed#toJS()

将此Iterable深度转换为等效的JS。

```javascript
toJS(): any
```

#### 继承

```
Iterable#toJS
```

#### 别称

```
toJSON()
```

#### 讨论

Iterable.Indexeds和Iterable.Sets成为数组，而Iterable.Keyeds成为对象。

### Seq.Keyed#toArray()

浅显地将这个迭代器转换为一个Array，丢弃键。

```javascript
toArray(): Array<V>
```

#### 继承

```
Iterable#toArray
```

### Seq.Keyed#toObject()

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

### Seq.Keyed#toMap()

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

### Seq.Keyed#toOrderedMap()

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

### Seq.Keyed#toSet()

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

### Seq.Keyed#toOrderedSet()

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

### Seq.Keyed#toList()

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

### Seq.Keyed#toStack()

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

### Seq.Keyed#keys()

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

### Seq.Keyed#values()

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

### Seq.Keyed#entries()

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

### Seq.Keyed#keySeq()

返回此Iterable的新键的Seq.Indexed，放弃值。

```javascript
keySeq(): Seq.Indexed<K>
```

#### 继承

```
Iterable#keySeq
```

### Seq.Keyed#valueSeq()

返回一个Iterable的值的Seq.Indexed，丢弃键。

```javascript
valueSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#valueSeq
```

### Seq.Keyed#entrySeq()

返回一个新的Seq.Indexed键值值元组。

```javascript
entrySeq(): Seq.Indexed<Array<any>>
```

#### 继承

```
Iterable#entrySeq
```

## 序列算法

### Seq.Keyed#map()

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

### Seq.Keyed#filter()

仅返回谓词函数返回true的条目的同一类型的新Iterable。

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

### Seq.Keyed#filterNot()

仅返回谓词函数返回false的条目返回相同类型的新Iterable。

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

### Seq.Keyed#reverse()

按相反顺序返回相同类型的新Iterable。

```javascript
reverse(): Iterable<K, V>
```

#### 继承

```
Iterable#reverse
```

### Seq.Keyed#sort()

返回包含相同条目的相同类型的新Iterable，并使用比较器进行稳定排序。

```javascript
sort(comparator?: (valueA: V, valueB: V) => number): Iterable<K, V>
```

#### 继承

```
Iterable#sort
```

#### 讨论

如果没有提供`comparator`，默认比较器使用`<`和`>`。

`comparator(valueA, valueB)`:

- 返回`0`如果元素属于不应该交换的情况。
- 返回`-1`（或任何负数），如果`valueA`在`valueB`之前
- 返回`1`（或任何正数），如果`valueA`在`valueB` 之后
- 是纯粹的，即它必须始终为同一对值返回相同的值。

排序没有定义顺序的集合时，它们的顺序等价物将被返回。例如`map.sort()`返回OrderedMap。

### Seq.Keyed#sortBy()

例如`sort`，但也接受一个`comparatorValueMapper`允许更复杂的手段进行排序的一个：

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

### Seq.Keyed#groupBy()

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

### Seq.Keyed#forEach()

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

### Seq.Keyed#slice()

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

### Seq.Keyed#rest()

返回包含除第一个以外的所有条目的同一类型的新Iterable。

```javascript
rest(): Iterable<K, V>
```

#### 继承

```
Iterable#rest
```

### Seq.Keyed#butLast()

返回包含除最后一个以外的所有条目的同一类型的新Iterable。

```javascript
butLast(): Iterable<K, V>
```

#### 继承

```
Iterable#butLast
```

### Seq.Keyed#skip()

返回从此Iterable中排除第一个数量条目的同一类型的新Iterable。

```javascript
skip(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skip
```

### Seq.Keyed#skipLast()

返回从此Iterable中排除最后一个条目的相同类型的新Iterable。

```javascript
skipLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skipLast
```

### Seq.Keyed#skipWhile()

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

### Seq.Keyed#skipUntil()

返回相同类型的新Iterable，其中包含从谓词第一次返回true时开始的条目。

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

### Seq.Keyed#take()

返回包含此Iterable中第一个条目的相同类型的新Iterable。

```javascript
take(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#take
```

### Seq.Keyed#takeLast()

返回包含此Iterable中最后一个条目的相同类型的新Iterable。

```javascript
takeLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#takeLast
```

### Seq.Keyed#takeWhile()

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

### Seq.Keyed#takeUntil()

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

### Seq.Keyed#concat()

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

### Seq.Keyed#flatten()

扁平化嵌套的Iterables。

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

仅扁平化其他的Iterable，而不是阵列或对象。

注意：`flatten(true)`在<Iterable>上运行并返回Iterable

### Seq.Keyed#flatMap()

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

## 降低值

### Seq.Keyed#reduce()

通过调用Iterable中的`reducer`每个条目并传递缩小的值，将Iterable减少为一个值。

```javascript
reduce<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### 继承

```
Iterable#reduce
```

#### 讨论

如果`initialReduction`未提供，或者为空，则将使用Iterable中的第一项。

### Seq.Keyed#reduceRight()

反向（从右侧）减少Iterable。

```javascript
reduceRight<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### 继承

```
Iterable#reduceRight
```

#### 讨论

注意：类似于this.reverse（）、reduce（），并提供奇偶校验。

### Seq.Keyed#every()

如果`predicate`对Iterable中的所有条目返回true，则返回true。

```javascript
every(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#every
```

### Seq.Keyed#some()

如果`predicate`对Iterable中的任何条目返回true，则返回true。

```javascript
some(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#some
```

### Seq.Keyed#join()

将值作为字符串连接在一起，在每个值之间插入一个分隔符。默认分隔符是`","`。

```javascript
join(separator?: string): string
```

#### 继承

```
Iterable#join
```

### Seq.Keyed#isEmpty()

如果此Iterable不包含任何值，则返回true。

```javascript
isEmpty(): boolean
```

#### 继承

```
Iterable#isEmpty
```

#### 讨论

对于一些懒惰的`Seq`，`isEmpty`可能需要迭代以确定是否空虚。至多会发生一次迭代。

### Seq.Keyed#count()

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

不管这个Iterable是否可以描述它的大小（有些Seqs不能），这个方法总是会返回正确的大小。例如，如果需要，它会评估一个懒惰型`Seq`

如果提供谓词，则返回谓词返回true的Iterable中条目的计数。

### Seq.Keyed#countBy()

返回计数的Seq.Keyed，按照grouper函数的返回值分组。

```javascript
countBy<G>(grouper: (value?: V, key?: K, iter?: Iterable<K, V>) => G,context?: any): Map<G, number>
```

#### 继承

```
Iterable#countBy
```

#### 讨论

注意：这不是一个懒惰型操作。

## 搜索值

### Seq.Keyed#find()

返回谓词返回true的第一个值。

```javascript
find(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### 继承

```
Iterable#find
```

### Seq.Keyed#findLast()

返回谓词返回true的最后一个值。

```javascript
findLast(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### 继承

```
Iterable#findLast
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### Seq.Keyed#findEntry()

返回谓词返回true的第一个键值对。

```javascript
findEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

#### 继承

```
Iterable#findEntry
```

### Seq.Keyed#findLastEntry()

返回谓词返回true的最后一个键值对。

```javascript
findLastEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

#### 继承

```
Iterable#findLastEntry
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### Seq.Keyed#findKey()

返回谓词返回true的键。

```javascript
findKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

#### 继承

```
Iterable#findKey
```

### Seq.Keyed#findLastKey()

返回谓词返回true的最后一个键。

```javascript
findLastKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

#### 继承

```
Iterable#findLastKey
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### Seq.Keyed#keyOf()

返回与搜索值关联的键，或者未定义。

```javascript
keyOf(searchValue: V): K
```

#### 继承

```
Iterable#keyOf
```

### Seq.Keyed#lastKeyOf()

返回与搜索值关联的最后一个键，或者未定义。

```javascript
lastKeyOf(searchValue: V): K
```

#### 继承

```
Iterable#lastKeyOf
```

### Seq.Keyed#max()

返回此集合中的最大值。如果任何值相当相等，则找到的第一个将被返回。

```javascript
max(comparator?: (valueA: V, valueB: V) => number): V
```

#### 继承

```
Iterable#max
```

#### 讨论

比较器的使用方式与Iterable＃排序相同。 如果没有提供，默认比较器是>。

当两个值被认为是等价的，遇到的第一个将被返回。 否则，只要比较器是可交换的，max将独立于输入的顺序进行操作。 默认的比较器>只有在类型不相同时才可以交换。

如果`comparator`返回0，且其中任一值为NaN、undefined或null，则将返回该值。

### Seq.Keyed#maxBy()

例如`max`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

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

### Seq.Keyed#min()

返回此集合中的最小值。如果任何值相当相等，则找到的第一个将被返回。

```javascript
min(comparator?: (valueA: V, valueB: V) => number): V
```

#### 继承

```
Iterable#min
```

#### 讨论

比较器的使用方式与Iterable＃排序相同。 如果未提供，则默认比较值为<。

当两个值被认为是等价的，遇到的第一个将被返回。否则，`min`只要比较器是可交换的，将独立于输入的顺序进行操作。默认比较器*只有*在类型不相同*时才*`<`可以交换。

如果`comparator`返回0，且其中任一值为NaN、undefined或null，则将返回该值。

### Seq.Keyed#minBy()

例如`min`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

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

### Seq.Keyed#isSubset()

如果`iter`包含此Iterable中的每个值，则为真。

```javascript
isSubset(iter: Iterable<any, V>): boolean
isSubset(iter: Array<V>): boolean
```

#### 继承

```
Iterable#isSubset
```

### Seq.Keyed#isSuperset()

如果此Iterable包含iter中的每个值，则为true。

```javascript
isSuperset(iter: Iterable<any, V>): boolean
isSuperset(iter: Array<V>): boolean
```

#### 继承

```
Iterable#isSuperset
```

## 序列功能

### Seq.Keyed#flip()

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

### Seq.Keyed#mapKeys()

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

### Seq.Keyed#mapEntries()

通过`mapper`函数传递条目（键，值元组），返回相同类型的新Iterable.Keyed 。

```javascript
mapEntries<KM, VM>(mapper: (entry?: Array<any>,index?: number,iter?: Iterable.Keyed<K, V>) => Array<any>,context?: any): Iterable.Keyed<KM, VM>
```

#### 继承

```
Iterable.Keyed#mapEntries
```

#### 例

```javascript
Seq({ a: 1, b: 2 })
  .mapEntries(([k, v]) => [k.toUpperCase(), v * 2])
// Seq { A: 2, B: 4 }
```

`Seq` 代表键值对。

```javascript
class Seq.Keyed<K, V> extends Seq<K, V>, Iterable.Keyed<K, V>
```

## 命令

### Seq.Keyed()

总是返回一个Seq.Keyed，如果输入未被键入，则需要一个可迭代的K，V元组。

```javascript
Seq.Keyed<K, V>(): Seq.Keyed<K, V>
Seq.Keyed<K, V>(seq: Iterable.Keyed<K, V>): Seq.Keyed<K, V>
Seq.Keyed<K, V>(seq: Iterable<any, any>): Seq.Keyed<K, V>
Seq.Keyed<K, V>(array: Array<any>): Seq.Keyed<K, V>
Seq.Keyed<V>(obj: {[key: string]: V}): Seq.Keyed<string, V>
Seq.Keyed<K, V>(iterator: Iterator<any>): Seq.Keyed<K, V>
Seq.Keyed<K, V>(iterable: Object): Seq.Keyed<K, V>
```

## 成员

### Seq.Keyed#size

```javascript
size: number
```

#### 继承

```
Seq#size
```

## 转换为Seq

### Seq.Keyed#toSeq()

返回自己

```javascript
toSeq(): Seq.Keyed<K, V>
```

#### 覆盖

```
Iterable#toSeq
```

### Seq.Keyed#toKeyedSeq()

从此Iterable返回一个Seq.Keyed，其索引被视为键。

```javascript
toKeyedSeq(): Seq.Keyed<K, V>
```

#### 继承

```
Iterable#toKeyedSeq
```

#### 讨论

如果您想要对Iterable.Indexed进行操作并保留索引、值对，这非常有用。

返回的Seq将具有与此Iterable相同的迭代顺序。

例：

```javascript
var indexedSeq = Immutable.Seq.of('A', 'B', 'C');
indexedSeq.filter(v => v === 'B').toString() // Seq [ 'B' ]
var keyedSeq = indexedSeq.toKeyedSeq();
keyedSeq.filter(v => v === 'B').toString() // Seq { 1: 'B' }
```

### Seq.Keyed#toIndexedSeq()

返回一个Iterable的值的Seq.Indexed，丢弃键。

```javascript
toIndexedSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#toIndexedSeq
```

### Seq.Keyed#toSetSeq()

返回一个Iterable的值的Seq.Set，丢弃键。

```javascript
toSetSeq(): Seq.Set<V>
```

#### 继承

```
Iterable#toSetSeq
```

## 强制评估

### Seq.Keyed#cacheResult()

因为序列是懒惰的并且被设计为链接在一起，所以它们不会缓存它们的结果。例如，这个映射函数总共被称为6次，因为每个`join`迭代三个值的Seq。

```javascript
cacheResult(): Seq<K, V>
```

#### 继承

```
Seq#cacheResult
```

#### 例

```javascript
var squares = Seq.of(1,2,3).map(x => x * x);
squares.join() + squares.join();
```

如果您知道一个`Seq`将被多次使用，首先将其缓存在内存中可能会更有效。在这里，地图功能只被调用3次。

```javascript
var squares = Seq.of(1,2,3).map(x => x * x).cacheResult();
squares.join() + squares.join();
```

明智地使用这种方法，因为它必须充分评估一个可能成为内存和可能性能负担的Seq。

注意：调用`cacheResult`后，Seq将始终有一个`size`。

## 值相等

### Seq.Keyed#equals()

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

### Seq.Keyed#hashCode()

计算并返回此Iterable的散列标识。

```javascript
hashCode(): number
```

#### 继承

```
Iterable#hashCode
```

#### 讨论

Iterable的hashCode用于确定潜在的相等性，在将其添加到Set或Map中的键时使用，可以通过不同的实例进行查找。

```javascript
var a = List.of(1, 2, 3);
var b = List.of(1, 2, 3);
assert(a !== b); // different instances
var set = Set.of(a);
assert(set.has(b) === true);
```

如果两个值具有相同的hashCode，它们也不能保证相等。如果两个值具有不同的hashCode，则它们不能相等。

## 读取值

### Seq.Keyed#get()

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

### Seq.Keyed#has()

如果此关键字存在`Iterable`，则为真，`Immutable.is`用于确定相等性

```javascript
has(key: K): boolean
```

#### 继承

```
Iterable#has
```

### Seq.Keyed#includes()

如果此值中存在值`Iterable`，则为true ，`Immutable.is`用于确定相等性

```javascript
includes(value: V): boolean
```

#### 继承

```
Iterable#includes
```

#### 别称

```
contains()
```

### Seq.Keyed#first()

Iterable中的第一个值。

```javascript
first(): V
```

#### 继承

```
Iterable#first
```

### Seq.Keyed#last()

Iterable中的最后一个值。

```javascript
last(): V
```

#### 继承

```
Iterable#last
```

## 读深式的值

### Seq.Keyed#getIn()

通过嵌套的Iterables返回键或索引路径的值。

```javascript
getIn(searchKeyPath: Array<any>, notSetValue?: any): any
getIn(searchKeyPath: Iterable<any, any>, notSetValue?: any): any
```

#### 继承

```
Iterable#getIn
```

### Seq.Keyed#hasIn()

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

### Seq.Keyed#toJS()

将此Iterable深度转换为等效的JS。

```javascript
toJS(): any
```

#### 继承

```
Iterable#toJS
```

#### 别称

```
toJSON()
```

#### 讨论

Iterable.Indexeds和Iterable.Sets成为数组，而Iterable.Keyeds成为对象。

### Seq.Keyed#toArray()

浅显地将这个迭代器转换为一个Array，丢弃键。

```javascript
toArray(): Array<V>
```

#### 继承

```
Iterable#toArray
```

### Seq.Keyed#toObject()

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

### Seq.Keyed#toMap()

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

### Seq.Keyed#toOrderedMap()

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

### Seq.Keyed#toSet()

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

### Seq.Keyed#toOrderedSet()

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

### Seq.Keyed#toList()

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

### Seq.Keyed#toStack()

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

### Seq.Keyed#keys()

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

### Seq.Keyed#values()

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

### Seq.Keyed#entries()

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

### Seq.Keyed#keySeq()

返回此Iterable的新键的Seq.Indexed，放弃值。

```javascript
keySeq(): Seq.Indexed<K>
```

#### 继承

```
Iterable#keySeq
```

### Seq.Keyed#valueSeq()

返回一个Iterable的值的Seq.Indexed，丢弃键。

```javascript
valueSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#valueSeq
```

### Seq.Keyed#entrySeq()

返回一个新的Seq.Indexed键值值元组。

```javascript
entrySeq(): Seq.Indexed<Array<any>>
```

#### 继承

```
Iterable#entrySeq
```

## 序列算法

### Seq.Keyed#map()

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

### Seq.Keyed#filter()

仅返回谓词函数返回true的条目的同一类型的新Iterable。

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

### Seq.Keyed#filterNot()

仅返回谓词函数返回false的条目返回相同类型的新Iterable。

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

### Seq.Keyed#reverse()

按相反顺序返回相同类型的新Iterable。

```javascript
reverse(): Iterable<K, V>
```

#### 继承

```
Iterable#reverse
```

### Seq.Keyed#sort()

返回包含相同条目的相同类型的新Iterable，并使用比较器进行稳定排序。

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

- 返回`0`若元素属于不应该交换的情况。
- 返回`-1`（或任何负数），如果`valueA`在`valueB` 之前
- 返回`1`（或任何正数），如果`valueA`在`valueB` 之后
- 是纯粹的，即它必须始终为同一对值返回相同的值。

排序没有定义顺序的集合时，它们的顺序等价物将被返回。例如`map.sort()`返回OrderedMap。

### Seq.Keyed#sortBy()

例如`sort`，但也接受一个`comparatorValueMapper`允许更复杂的手段进行排序的一个：

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

### Seq.Keyed#groupBy()

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

### Seq.Keyed#forEach()

该`sideEffect`是在可迭代的每个条目执行。

```javascript
forEach(sideEffect: (value?: V, key?: K, iter?: Iterable<K, V>) => any,context?: any): number
```

#### 继承

```
Iterable#forEach
```

#### 讨论

不同的是[`Array#forEach`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)，如果有任何`sideEffect`返回`false`的话，迭代将停止。返回迭代的条目数（包括返回false的最后一次迭代）。

## 创建子集

### Seq.Keyed#slice()

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

### Seq.Keyed#rest()

返回包含除第一个以外的所有条目的同一类型的新Iterable。

```javascript
rest(): Iterable<K, V>
```

#### 继承

```
Iterable#rest
```

### Seq.Keyed#butLast()

返回包含除最后一个以外的所有条目的同一类型的新Iterable。

```javascript
butLast(): Iterable<K, V>
```

#### 继承

```
Iterable#butLast
```

### Seq.Keyed#skip()

返回`amount`从此Iterable中排除第一个条目的同一类型的新Iterable。

```javascript
skip(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skip
```

### Seq.Keyed#skipLast()

返回从此Iterable中排除最后一个条目的相同类型的新Iterable。

```javascript
skipLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skipLast
```

### Seq.Keyed#skipWhile()

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

### Seq.Keyed#skipUntil()

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

### Seq.Keyed#take()

返回包含此Iterable中第一个条目的相同类型的新Iterable。

```javascript
take(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#take
```

### Seq.Keyed#takeLast()

返回包含此Iterable中最后一个条目的相同类型的新Iterable。

```javascript
takeLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#takeLast
```

### Seq.Keyed#takeWhile()

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

### Seq.Keyed#takeUntil()

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

### Seq.Keyed#concat()

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

### Seq.Keyed#flatten()

扁平嵌套的Iterables。

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

仅扁平化其他的Iterable，而不是阵列或对象。

注意：`flatten(true)`在<Iterable>上运行并返回Iterable

### Seq.Keyed#flatMap()

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

## 降低值

### Seq.Keyed#reduce()

通过为Iterable中的每个条目调用reducer并传递缩小的值，将Iterable减少为一个值。

```javascript
reduce<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### 继承

```
Iterable#reduce
```

#### 讨论

如果`initialReduction`未提供，或者为空，则将使用Iterable中的第一项。

### Seq.Keyed#reduceRight()

反向（从右侧）减少Iterable。

```javascript
reduceRight<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### 继承

```
Iterable#reduceRight
```

#### 讨论

注意：类似于this.reverse（）.reduce（），并提供奇偶校验。

### Seq.Keyed#every()

如果`predicate`对Iterable中的所有条目返回true，则返回true。

```javascript
every(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#every
```

### Seq.Keyed#some()

如果`predicate`对Iterable中的任何条目返回true，则返回true。

```javascript
some(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#some
```

### Seq.Keyed#join()

将值作为字符串连接在一起，在每个值之间插入一个分隔符。默认分隔符是`","`。

```javascript
join(separator?: string): string
```

#### 继承

```
Iterable#join
```

### Seq.Keyed#isEmpty()

如果此Iterable不包含任何值，则返回true。

```javascript
isEmpty(): boolean
```

#### 继承

```
Iterable#isEmpty
```

#### 讨论

对于一些懒惰的`Seq`，`isEmpty`可能需要迭代以确定是否为空。至多会发生一次迭代。

### Seq.Keyed#count()

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

不管这个Iterable是否可以描述它的大小（有些Seqs不能），这个方法总是会返回正确的大小。例如，如果需要，它会评估一个懒惰型`Seq`

如果`predicate`提供，则返回Iterable中`predicate`返回值为true 的条目的计数。

### Seq.Keyed#countBy()

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

### Seq.Keyed#find()

返回谓词返回true的第一个值。

```javascript
find(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### 继承

```
Iterable#find
```

### Seq.Keyed#findLast()

返回谓词返回true的最后一个值。

```javascript
findLast(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### 继承

```
Iterable#findLast
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### Seq.Keyed#findEntry()

返回返回值为true的第一个键值对`predicate`。

```javascript
findEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

#### 继承

```
Iterable#findEntry
```

### Seq.Keyed#findLastEntry()

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

### Seq.Keyed#findKey()

返回`predicate`返回true 的键。

```javascript
findKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

#### 继承

```
Iterable#findKey
```

### Seq.Keyed#findLastKey()

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

### Seq.Keyed#keyOf()

返回与搜索值关联的键，或者未定义。

```javascript
keyOf(searchValue: V): K
```

#### 继承

```
Iterable#keyOf
```

### Seq.Keyed#lastKeyOf()

返回与搜索值关联的最后一个键，或者未定义。

```javascript
lastKeyOf(searchValue: V): K
```

#### 继承

```
Iterable#lastKeyOf
```

### Seq.Keyed#max()

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

当两个值被认为是等价的，遇到的第一个将被返回。否则，只要比较器是可交换的，min将独立于输入的顺序进行操作。默认比较器`>`*只有*在类型不相同*时才*可以交换。

如果`comparator`返回0，且其中任一值为NaN、undefined或null，则将返回该值。

### Seq.Keyed#maxBy()

例如`max`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

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

### Seq.Keyed#min()

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

当两个值被认为是等价的，遇到的第一个将被返回。否则，只要比较器是可交换的，min将独立于输入的顺序进行操作。默认比较器`<`*只有*在类型不相同*时才*可以交换。

如果`comparator`返回0，且其中任一值为NaN、undefined或null，则将返回该值。

### Seq.Keyed#minBy()

例如`min`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

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

### Seq.Keyed#isSubset()

如果`iter`包含此Iterable中的每个值，则为真。

```javascript
isSubset(iter: Iterable<any, V>): boolean
isSubset(iter: Array<V>): boolean
```

#### 继承

```
Iterable#isSubset
```

### Seq.Keyed#isSuperset()

如果此Iterable包含iter中的每个值，则为true。

```javascript
isSuperset(iter: Iterable<any, V>): boolean
isSuperset(iter: Array<V>): boolean
```

#### 继承

```
Iterable#isSuperset
```

## 序列功能

### Seq.Keyed#flip()

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

### Seq.Keyed#mapKeys()

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

### Seq.Keyed#mapEntries()

通过`mapper`函数传递条目（键，值元组），返回相同类型的新Iterable.Keyed 。

```javascript
mapEntries<KM, VM>(mapper: (entry?: Array<any>,index?: number,iter?: Iterable.Keyed<K, V>) => Array<any>,context?: any): Iterable.Keyed<KM, VM>
```

#### 继承

```
Iterable.Keyed#mapEntries
```

#### 例

```javascript
Seq({ a: 1, b: 2 })
  .mapEntries(([k, v]) => [k.toUpperCase(), v * 2])
// Seq { A: 2, B: 4 }
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-04-11