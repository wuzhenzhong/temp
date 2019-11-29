# Collection

Collection是具体数据结构的抽象基类。它不能直接构建。

```javascript
class Collection<K, V> extends Iterable<K, V>
```

#### 讨论

实现应该扩展子类，一种`Collection.Keyed`，`Collection.Indexed`或`Collection.Set`。

## 类型

Collection.Keyed

## 会员

### **Collection#size**



```javascript
size: number
```

## 价值平等

### **Collection#equals()**



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

### **Collection#hashCode()**



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

### **Collection#get()**



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

### **Collection#has()**



如果此关键字存在`Iterable`，则为真，`Immutable.is`用于确定相等性

```javascript
has(key: K): boolean
```

#### 继承

```
Iterable#has
```

### **Collection#includes()**



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

### **Collection#first()**



Iterable中的第一个值。

```javascript
first(): V
```

#### 继承

```
Iterable#first
```

### **Collection#last()**



Iterable中的最后一个值。

```javascript
last(): V
```

#### 继承

```
Iterable#last
```

## 读 **deep values**



### **Collection#getIn()**



通过嵌套的Iterables返回键或索引路径的值。

```javascript
getIn(searchKeyPath: Array<any>, notSetValue?: any): any
getIn(searchKeyPath: Iterable<any, any>, notSetValue?: any): any
```

#### 继承

```
Iterable#getIn
```

### **Collection#hasIn()**



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

### **Collection#toJS()**



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

`Iterable.Indexeds`, and `Iterable.Sets` become Arrays, while `Iterable.Keyeds` become Objects.

### **Collection#toArray()**



浅显地将这个迭代器转换为一个Array，丢弃键。

```javascript
toArray(): Array<V>
```

#### 继承

```
Iterable#toArray
```

### **Collection#toObject()**



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

### **Collection#toMap()**



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

### **Collection#toOrderedMap()**



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

### **Collection#toSet()**



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

### **Collection#toOrderedSet()**



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

### **Collection#toList()**



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

### **Collection#toStack()**



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

### **Collection#toSeq()**



将此Iterable转换为相同类型的Seq（索引，键控或设置）。

```javascript
toSeq(): Seq<K, V>
```

#### 继承

```
Iterable#toSeq
```

### **Collection#toKeyedSeq()**



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

### **Collection#toIndexedSeq()**



返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
toIndexedSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#toIndexedSeq
```

### **Collection#toSetSeq()**



返回一个Seq.Set这个Iterable的值，丢弃键。

```javascript
toSetSeq(): Seq.Set<V>
```

#### 继承

```
Iterable#toSetSeq
```

## 迭代器

### **Collection#keys()**



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

### **Collection#values()**



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

### **Collection#entries()**



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

### **Collection#keySeq()**



返回此Iterable的新键的Seq.Indexed，放弃值。

```javascript
keySeq(): Seq.Indexed<K>
```

#### 继承

```
Iterable#keySeq
```

### **Collection#valueSeq()**



返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
valueSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#valueSeq
```

### **Collection#entrySeq()**



返回一个新的Seq.Indexed键值值元组。

```javascript
entrySeq(): Seq.Indexed<Array<any>>
```

#### 继承

```
Iterable#entrySeq
```

## 序列算法

### **Collection#map()**



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

### **Collection#filter()**



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

### **Collection#filterNot()**



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

### **Collection#reverse()**



按相反顺序返回相同类型的新Iterable。

```javascript
reverse(): Iterable<K, V>
```

#### 继承

```
Iterable#reverse
```

### **Collection#sort()**



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

### **Collection#sortBy()**



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

### **Collection#groupBy()**



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

### **Collection#forEach()**



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

### **Collection#slice()**



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

### **Collection#rest()**



返回包含除第一个以外的所有条目的同一类型的新Iterable。

```javascript
rest(): Iterable<K, V>
```

#### 继承

```
Iterable#rest
```

### **Collection#butLast()**



返回包含除最后一个以外的所有条目的同一类型的新Iterable。

```javascript
butLast(): Iterable<K, V>
```

#### 继承

```
Iterable#butLast
```

### **Collection#skip()**



返回`amount`从此Iterable中排除第一个条目的同一类型的新Iterable。

```javascript
skip(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skip
```

### **Collection#skipLast()**



返回`amount`从此Iterable中排除最后一个条目的同一类型的新Iterable。

```javascript
skipLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skipLast
```

### **Collection#skipWhile()**



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

### **Collection#skipUntil()**



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

### **Collection#take()**



返回包含`amount`此Iterable中第一个条目的相同类型的新Iterable。

```javascript
take(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#take
```

### **Collection#takeLast()**



返回包含`amount`此Iterable中最后一个条目的相同类型的新Iterable。

```javascript
takeLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#takeLast
```

### **Collection#takeWhile()**



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

### **Collection#takeUntil()**



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

### **Collection#concat()**



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

### **Collection#flatten()**



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

### **Collection#flatMap()**



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

## 减少值

### **Collection#reduce()**



通过调用Iterable中的`reducer`每个条目并传递减小的值，将Iterable减少为一个值。

```javascript
reduce<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### 继承

```
Iterable#reduce
```



**参考**

[`Array#reduce`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce).

#### 讨论

如果`initialReduction`未提供，或者为空，则将使用Iterable中的第一项。

### **Collection#reduceRight()**



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

### **Collection#every()**



如果`predicate`对Iterable中的所有条目返回true，则返回true。

```javascript
every(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#every
```

### **Collection#some()**



如果`predicate`对Iterable中的任何条目返回true，则返回true。

```javascript
some(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#some
```

### **Collection#join()**



将值作为字符串连接在一起，在每个值之间插入一个分隔符。默认分隔符是`","`。

```javascript
join(separator?: string): string
```

#### 继承

```
Iterable#join
```

### **Collection#isEmpty()**



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

### **Collection#count()**



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

### **Collection#countBy()**



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

## 搜索价值

### **Collection#find()**



返回`predicate`返回true 的第一个值。

```javascript
find(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### 继承

```
Iterable#find
```

### **Collection#findLast()**



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

### **Collection#findEntry()**



返回返回值为true的第一个键值对`predicate`。

```javascript
findEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

#### 继承

```
Iterable#findEntry
```

### **Collection#findLastEntry()**



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

### **Collection#findKey()**



返回`predicate`返回true 的键。

```javascript
findKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

#### 继承

```
Iterable#findKey
```

### **Collection#findLastKey()**



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

### **Collection#keyOf()**



返回与搜索值关联的键，或者未定义。

```javascript
keyOf(searchValue: V): K
```

#### 继承

```
Iterable#keyOf
```

### **Collection#lastKeyOf()**



返回与搜索值关联的最后一个键，或者未定义。

```javascript
lastKeyOf(searchValue: V): K
```

#### 继承

```
Iterable#lastKeyOf
```

### **Collection#max()**



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

### **Collection#maxBy()**



喜欢`max`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

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

### **Collection#min()**



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

### **Collection#minBy()**



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

### **Collection#isSubset()**



如果`iter`包含此Iterable中的每个值，则为真。

```javascript
isSubset(iter: Iterable<any, V>): boolean
isSubset(iter: Array<V>): boolean
```

#### 继承

```
Iterable#isSubset
```

### **Collection#isSuperset()**



如果此Iterable包含每个值，则为真`iter`。

```javascript
isSuperset(iter: Iterable<any, V>): boolean
isSuperset(iter: Array<V>): boolean
```

#### 继承

```
Iterable#isSuperset
```

`Collection` 代表键值对。

```javascript
class Collection.Keyed<K, V> extends Collection<K, V>, Iterable.Keyed<K, V>
```

## 会员

### Collection.Keyed＃大小

```javascript
size: number
```

#### 继承

```
Collection#size
```

## 转换为Seq

### **Collection.Keyed#toSeq()**



返回Seq.Keyed。

```javascript
toSeq(): Seq.Keyed<K, V>
```

#### 覆盖

```
Iterable#toSeq
```

### **Collection.Keyed#toKeyedSeq()**



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

### **Collection.Keyed#toIndexedSeq()**



返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
toIndexedSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#toIndexedSeq
```

### **Collection.Keyed#toSetSeq()**



返回一个Seq.Set这个Iterable的值，丢弃键。

```javascript
toSetSeq(): Seq.Set<V>
```

#### 继承

```
Iterable#toSetSeq
```

## 价值平等

### **Collection.Keyed#equals()**



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

### **Collection.Keyed#hashCode()**



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

### **Collection.Keyed#get()**



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

### **Collection.Keyed#has()**



如果此关键字存在`Iterable`，则为真，`Immutable.is`用于确定相等性

```javascript
has(key: K): boolean
```

#### 继承

```
Iterable#has
```

### **Collection.Keyed#includes()**



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

### **Collection.Keyed#first()**



Iterable中的第一个值。

```javascript
first(): V
```

#### 继承

```
Iterable#first
```

### **Collection.Keyed#last()**



Iterable中的最后一个值。

```javascript
last(): V
```

#### 继承

```
Iterable#last
```

## 读 **deep values**



### **Collection.Keyed#getIn()**



通过嵌套的Iterables返回键或索引路径的值。

```javascript
getIn(searchKeyPath: Array<any>, notSetValue?: any): any
getIn(searchKeyPath: Iterable<any, any>, notSetValue?: any): any
```

#### 继承

```
Iterable#getIn
```

### **Collection.Keyed#hasIn()**



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

### **Collection.Keyed#toJS()**



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

### **Collection.Keyed#toArray()**



浅显地将这个迭代器转换为一个Array，丢弃键。

```javascript
toArray(): Array<V>
```

#### 继承

```
Iterable#toArray
```

### **Collection.Keyed#toObject()**



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

### **Collection.Keyed#toMap()**



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

### **Collection.Keyed#toOrderedMap()**



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

### **Collection.Keyed#toSet()**



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

### **Collection.Keyed#toOrderedSet()**



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

### **Collection.Keyed#toList()**



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

### **Collection.Keyed#toStack()**



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

### **Collection.Keyed#keys()**



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

### **Collection.Keyed#values()**



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

### **Collection.Keyed#entries()**



An iterator of this `Iterable`'s entries as `[key, value]` tuples.

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

### **Collection.Keyed#keySeq()**



返回此Iterable的新键的Seq.Indexed，放弃值。

```javascript
keySeq(): Seq.Indexed<K>
```

#### 继承

```
Iterable#keySeq
```

### **Collection.Keyed#valueSeq()**



返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
valueSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#valueSeq
```

### **Collection.Keyed#valueSeq()**



返回一个新的Seq.Indexed键值值元组。

```javascript
entrySeq(): Seq.Indexed<Array<any>>
```

#### 继承

```
Iterable#entrySeq
```

## 序列算法

### **Collection**.Keyed#map()

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

### **Collection.Keyed#filter()**



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

### **Collection.Keyed#filterNot()**



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

### **Collection.Keyed#reverse()**



按相反顺序返回相同类型的新Iterable。

```javascript
reverse(): Iterable<K, V>
```

#### 继承

```
Iterable#reverse
```

### **Collection.Keyed#sort()**



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

### **Collection.Keyed#sortBy()**



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

### **Collection.Keyed#groupBy()**



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

### Collection.Keyed#forEach()

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

### Collection.Keyed#slice()

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

### Collection.Keyed#rest()

返回包含除第一个以外的所有条目的同一类型的新Iterable。

```javascript
rest(): Iterable<K, V>
```

#### 继承

```
Iterable#rest
```

### Collection.Keyed#butLast()

返回包含除最后一个以外的所有条目的同一类型的新Iterable。

```javascript
butLast(): Iterable<K, V>
```

#### 继承

```
Iterable#butLast
```

### Collection.Keyed#skip()

返回`amount`从此Iterable中排除第一个条目的同一类型的新Iterable。

```javascript
skip(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skip
```

### Collection.Keyed#skipLast()

返回`amount`从此Iterable中排除最后一个条目的同一类型的新Iterable。

```javascript
skipLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skipLast
```

### Collection.Keyed#skipWhile()

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

### Collection.Keyed#skipUntil()

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

### Collection.Keyed#take()

返回包含`amount`此Iterable中第一个条目的相同类型的新Iterable。

```javascript
take(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#take
```

### Collection.Keyed#takeLast()

返回包含`amount`此Iterable中最后一个条目的相同类型的新Iterable。

```javascript
takeLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#takeLast
```

### Collection.Keyed#takeWhile()

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

### Collection.Keyed#takeUntil()

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

### Collection.Keyed#concat()

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

### Collection.Keyed#flatten()

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

### Collection.Keyed#flatMap()

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

### Collection.Keyed#reduce()

通过调用Iterable中的`reducer`每个条目并传递缩小的值，将Iterable减少为一个值。

```javascript
reduce<R>(reducer: (reduction?: R, value?: V, key?: K, iter?: Iterable<K, V>) => R,initialReduction?: R,context?: any): R
```

#### 继承

```
Iterable#reduce
```

**参阅**

[`Array#reduce`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce).

#### 讨论

如果`initialReduction`未提供，或者为空，则将使用Iterable中的第一项。

### Collection.Keyed#reduceRight()

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

### Collection.Keyed#every()

如果`predicate`对Iterable中的所有条目返回true，则返回true。

```javascript
every(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#every
```

### Collection.Keyed#some()

如果`predicate`对Iterable中的任何条目返回true，则返回true。

```javascript
some(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#some
```

### Collection.Keyed#join()

将值作为字符串连接在一起，在每个值之间插入一个分隔符。默认分隔符是`","`。

```javascript
join(separator?: string): string
```

#### 继承

```
Iterable#join
```

### Collection.Keyed#isEmpty()

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

### Collection.Keyed#count()

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

不管这个Iterable是否可以描述它的大小（有些Seqs不能），这个方法总是会返回正确的大小。例如，`Seq`如果需要，它会评估一个 lazy 。

如果`predicate`提供，则返回Iterable中`predicate`返回值为true 的条目的计数。

### Collection.Keyed#countBy()

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

## 搜索价值

### Collection.Keyed#find()

返回`predicate`返回true 的第一个值。

```javascript
find(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### 继承

```
Iterable#find
```

### Collection.Keyed#findLast()

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

### Collection.Keyed#findEntry()

Returns the first key, value entry for which the `predicate` returns true.

```javascript
findEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

#### 继承

```
Iterable#findEntry
```

### Collection.Keyed#findLastEntry()

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

### Collection.Keyed#findKey()

返回`predicate`返回true 的键。

```javascript
findKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

#### 继承

```
Iterable#findKey
```

### Collection.Keyed#findLastKey()

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

### Collection.Keyed#keyOf()

返回与搜索值关联的键，或者未定义。

```javascript
keyOf(searchValue: V): K
```

#### 继承

```
Iterable#keyOf
```

### Collection.Keyed#lastKeyOf()

返回与搜索值关联的最后一个键，或者未定义。

```javascript
lastKeyOf(searchValue: V): K
```

#### 继承

```
Iterable#lastKeyOf
```

### Collection.Keyed#max()

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

### Collection.Keyed#maxBy()

喜欢`max`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

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

### Collection.Keyed#min()

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

### Collection.Keyed#minBy()

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

### Collection.Keyed#isSubset()

如果`iter`包含此Iterable中的每个值，则为真。

```javascript
isSubset(iter: Iterable<any, V>): boolean
isSubset(iter: Array<V>): boolean
```

#### 继承

```
Iterable#isSubset
```

### Collection.Keyed#isSuperset()

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

### Collection.Keyed#flip()

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

### Collection.Keyed#mapKeys()

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

### Collection.Keyed#mapEntries()

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

`Collection` 代表键值对。

```javascript
class Collection.Keyed<K, V> extends Collection<K, V>, Iterable.Keyed<K, V>
```

## 会员

### Collection.Keyed#size

```javascript
size: number
```

#### 继承

```
Collection#size
```

## 转换为Seq

### Collection.Keyed#toSeq()

返回Seq.Keyed。

```javascript
toSeq(): Seq.Keyed<K, V>
```

#### 覆盖

```
Iterable#toSeq
```

### Collection.Keyed#toKeyedSeq()

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

### Collection.Keyed#toIndexedSeq()

返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
toIndexedSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#toIndexedSeq
```

### Collection.Keyed#toSetSeq()

返回一个Seq.Set这个Iterable的值，丢弃键。

```javascript
toSetSeq(): Seq.Set<V>
```

#### 继承

```
Iterable#toSetSeq
```

## 价值平等

### Collection.Keyed#equals()

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

### Collection.Keyed#hashCode()

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

### Collection.Keyed#get()

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

### Collection.Keyed#has()

如果此关键字存在`Iterable`，则为真，`Immutable.is`用于确定相等性

```javascript
has(key: K): boolean
```

#### 继承

```
Iterable#has
```

### Collection.Keyed#includes()

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

### Collection.Keyed#first()

Iterable中的第一个值。

```javascript
first(): V
```

#### 继承

```
Iterable#first
```

### Collection.Keyed#last()

Iterable中的最后一个值。

```javascript
last(): V
```

#### 继承

```
Iterable#last
```

## 读**deep values**



### Collection.Keyed#getIn()

通过嵌套的Iterables返回键或索引路径的值。

```javascript
getIn(searchKeyPath: Array<any>, notSetValue?: any): any
getIn(searchKeyPath: Iterable<any, any>, notSetValue?: any): any
```

#### 继承

```
Iterable#getIn
```

### Collection.Keyed#hasIn()

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

### Collection.Keyed#toJS()

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

### Collection.Keyed#toArray()

浅显地将这个迭代器转换为一个Array，丢弃键。

```javascript
toArray(): Array<V>
```

#### 继承

```
Iterable#toArray
```

### Collection.Keyed#toObject()

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

### Collection.Keyed#toMap()

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

### Collection.Keyed#toOrderedMap()

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

### Collection.Keyed#toSet()

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

### Collection.Keyed#toOrderedSet()

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

### Collection.Keyed#toList()

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

### Collection.Keyed#toStack()

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

### Collection.Keyed#keys()

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

### Collection.Keyed#values()

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

### Collection.Keyed#entries()

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

### Collection.Keyed#keySeq()

返回此Iterable的新键的Seq.Indexed，放弃值。

```javascript
keySeq(): Seq.Indexed<K>
```

#### 继承

```
Iterable#keySeq
```

### Collection.Keyed#valueSeq()

返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
valueSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#valueSeq
```

### Collection.Keyed#entrySeq()

返回一个新的Seq.Indexed键值值元组。

```javascript
entrySeq(): Seq.Indexed<Array<any>>
```

#### 继承

```
Iterable#entrySeq
```

## 序列算法

### Collection.Keyed#map()

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

### Collection.Keyed#filter()

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

### Collection.Keyed#filterNot()

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

### Collection.Keyed#reverse()

按相反顺序返回相同类型的新Iterable。

```javascript
reverse(): Iterable<K, V>
```

#### 继承

```
Iterable#reverse
```

### Collection.Keyed#sort()

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

### Collection.Keyed#sortBy()

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

### Collection.Keyed#groupBy()

返回`Iterable.Keyed`的`Iterable.Keyeds`，由返回值分组`grouper`函数。

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

### Collection.Keyed#forEach()

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

### Collection.Keyed#slice()

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

### Collection.Keyed#rest()

返回包含除第一个以外的所有条目的同一类型的新Iterable。

```javascript
rest(): Iterable<K, V>
```

#### 继承

```
Iterable#rest
```

### Collection.Keyed#butLast()

返回包含除最后一个以外的所有条目的同一类型的新Iterable。

```javascript
butLast(): Iterable<K, V>
```

#### 继承

```
Iterable#butLast
```

### Collection.Keyed#skip()

返回`amount`从此Iterable中排除第一个条目的同一类型的新Iterable。

```javascript
skip(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skip
```

### Collection.Keyed#skipLast()

返回`amount`从此Iterable中排除最后一个条目的同一类型的新Iterable。

```javascript
skipLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skipLast
```

### Collection.Keyed#skipWhile()

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

### Collection.Keyed#skipUntil()

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

### Collection.Keyed#take()

返回包含`amount`此Iterable中第一个条目的相同类型的新Iterable。

```javascript
take(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#take
```

### Collection.Keyed#takeLast()

返回包含`amount`此Iterable中最后一个条目的相同类型的新Iterable。

```javascript
takeLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#takeLast
```

### Collection.Keyed#takeWhile()

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

### Collection.Keyed#takeUntil()

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

### Collection.Keyed#concat()

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

### Collection.Keyed#flatten()

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

### Collection.Keyed#flatMap()

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

## 减少值

### Collection.Keyed#reduce()

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

### Collection.Keyed#reduceRight()

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

### Collection.Keyed#every()

如果`predicate`对Iterable中的所有条目返回true，则返回true。

```javascript
every(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#every
```

### Collection.Keyed#some()

如果`predicate`对Iterable中的任何条目返回true，则返回true。

```javascript
some(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#some
```

### Collection.Keyed#join()

将值作为字符串连接在一起，在每个值之间插入一个分隔符。默认分隔符是`","`。

```javascript
join(separator?: string): string
```

#### 继承

```
Iterable#join
```

### Collection.Keyed#isEmpty()

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

### Collection.Keyed#count()

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

不管这个Iterable是否可以描述它的大小（有些Seqs不能），这个方法总是会返回正确的大小。例如，`Seq`如果需要，它会评估一个 lazy。

如果`predicate`提供，则返回Iterable中`predicate`返回值为true 的条目的计数。

### Collection.Keyed#countBy()

返回一个`Seq.Keyed`计数，按`grouper`函数的返回值分组。

```javascript
countBy<G>(grouper: (value?: V, key?: K, iter?: Iterable<K, V>) => G,context?: any): Map<G, number>
```

#### 继承

```
Iterable#countBy
```

#### 讨论

注意：这不是一个惰性的操作。

## 搜索价值

### Collection.Keyed#find()

返回`predicate`返回true 的第一个值。

```javascript
find(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### 继承

```
Iterable#find
```

### Collection.Keyed#findLast()

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

### Collection.Keyed#findEntry()

返回返回值为true的第一个键值对`predicate`。

```javascript
findEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

#### 继承

```
Iterable#findEntry
```

### Collection.Keyed#findLastEntry()

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

### Collection.Keyed#findKey()

返回`predicate`返回true 的键。

```javascript
findKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

#### 继承

```
Iterable#findKey
```

### Collection.Keyed#findLastKey()

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

### Collection.Keyed#keyOf()

返回与搜索值关联的键，或者未定义。

```javascript
keyOf(searchValue: V): K
```

#### 继承

```
Iterable#keyOf
```

### Collection.Keyed#lastKeyOf()

返回与搜索值关联的最后一个键，或者未定义。

```javascript
lastKeyOf(searchValue: V): K
```

#### 继承

```
Iterable#lastKeyOf
```

### Collection.Keyed#max()

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

### Collection.Keyed#maxBy()

喜欢`max`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

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

### Collection.Keyed#min()

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

### Collection.Keyed#minBy()

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

### Collection.Keyed#isSubset()

如果`iter`包含此Iterable中的每个值，则为真。

```javascript
isSubset(iter: Iterable<any, V>): boolean
isSubset(iter: Array<V>): boolean
```

#### 继承

```
Iterable#isSubset
```

### Collection.Keyed#isSuperset()

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

### Collection.Keyed#flip()

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

### Collection.Keyed#mapKeys()

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

### Collection.Keyed#mapEntries()

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

`Collection` 代表键值对。

```javascript
class Collection.Keyed<K, V> extends Collection<K, V>, Iterable.Keyed<K, V>
```

## 成员

### Collection.Keyed#size

```javascript
size: number
```

#### 继承

```
Collection#size
```

## 转换为Seq

### Collection.Keyed#toSeq()

返回Seq.Keyed。

```javascript
toSeq(): Seq.Keyed<K, V>
```

#### 覆盖

```
Iterable#toSeq
```

### Collection.Keyed#toKeyedSeq()

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

### Collection.Keyed#toIndexedSeq()

返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
toIndexedSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#toIndexedSeq
```

### Collection.Keyed#toSetSeq()

返回一个Seq.Set这个Iterable的值，丢弃键。

```javascript
toSetSeq(): Seq.Set<V>
```

#### 继承

```
Iterable#toSetSeq
```

## 价值平等

### Collection.Keyed#equals()

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

### Collection.Keyed#hashCode()

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

### Collection.Keyed#get()

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

### Collection.Keyed#has()

如果此关键字存在`Iterable`，则为真，`Immutable.is`用于确定相等性

```javascript
has(key: K): boolean
```

#### 继承

```
Iterable#has
```

### Collection.Keyed#includes()

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

### Collection.Keyed#first()

Iterable中的第一个值。

```javascript
first(): V
```

#### 继承

```
Iterable#first
```

### Collection.Keyed#last()

Iterable中的最后一个值。

```javascript
last(): V
```

#### 继承

```
Iterable#last
```

## 读**deep values**



### Collection.Keyed#getIn()

通过嵌套的Iterables返回键或索引路径的值。

```javascript
getIn(searchKeyPath: Array<any>, notSetValue?: any): any
getIn(searchKeyPath: Iterable<any, any>, notSetValue?: any): any
```

#### 继承

```
Iterable#getIn
```

### Collection.Keyed#hasIn()

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

### Collection.Keyed#toJS()

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

### Collection.Keyed#toArray()

浅显地将这个迭代器转换为一个Array，丢弃键。

```javascript
toArray(): Array<V>
```

#### 继承

```
Iterable#toArray
```

### Collection.Keyed#toObject()

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

### Collection.Keyed#toMap()

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

### Collection.Keyed#toOrderedMap()

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

### Collection.Keyed#toSet()

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

### Collection.Keyed#toOrderedSet()

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

### Collection.Keyed#toList()

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

### Collection.Keyed#toStack()

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

### Collection.Keyed#keys()

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

### Collection.Keyed#values()

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

### Collection.Keyed#entries()

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

### Collection.Keyed#keySeq()

返回此Iterable的新键的Seq.Indexed，放弃值。

```javascript
keySeq(): Seq.Indexed<K>
```

#### 继承

```
Iterable#keySeq
```

### Collection.Keyed#valueSeq()

返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
valueSeq(): Seq.Indexed<V>
```

#### 继承

```
Iterable#valueSeq
```

### Collection.Keyed#entrySeq()

Returns a new Seq.Indexed of key, value tuples.

```javascript
entrySeq(): Seq.Indexed<Array<any>>
```

#### 继承

```
Iterable#entrySeq
```

## 序列算法

### Collection.Keyed#map()

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

### Collection.Keyed#filter()

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

### Collection.Keyed#filterNot()

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

### Collection.Keyed#reverse()

按相反顺序返回相同类型的新Iterable。

```javascript
reverse(): Iterable<K, V>
```

#### 继承

```
Iterable#reverse
```

### Collection.Keyed#sort()

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

### Collection.Keyed#sortBy()

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

### Collection.Keyed#groupBy()

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

### Collection.Keyed#forEach()

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

### Collection.Keyed#slice()

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

### Collection.Keyed#rest()

返回包含除第一个以外的所有条目的同一类型的新Iterable。

```javascript
rest(): Iterable<K, V>
```

#### 继承

```
Iterable#rest
```

### Collection.Keyed#butLast()

返回包含除最后一个以外的所有条目的同一类型的新Iterable。

```javascript
butLast(): Iterable<K, V>
```

#### 继承

```
Iterable#butLast
```

### Collection.Keyed#skip()

返回`amount`从此Iterable中排除第一个条目的同一类型的新Iterable。

```javascript
skip(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skip
```

### Collection.Keyed#skipLast()

返回`amount`从此Iterable中排除最后一个条目的同一类型的新Iterable。

```javascript
skipLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#skipLast
```

### Collection.Keyed#skipWhile()

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

### Collection.Keyed#skipUntil()

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

### Collection.Keyed#take()

返回包含`amount`此Iterable中第一个条目的相同类型的新Iterable。

```javascript
take(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#take
```

### Collection.Keyed#takeLast()

返回包含`amount`此Iterable中最后一个条目的相同类型的新Iterable。

```javascript
takeLast(amount: number): Iterable<K, V>
```

#### 继承

```
Iterable#takeLast
```

### Collection.Keyed#takeWhile()

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

### Collection.Keyed#takeUntil()

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

### Collection.Keyed#concat()

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

### Collection.Keyed#flatten()

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

### Collection.Keyed#flatMap()

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

## 减少值

### Collection.Keyed#reduce()

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

### Collection.Keyed#reduceRight()

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

### Collection.Keyed#every()

如果`predicate`对Iterable中的所有条目返回true，则返回true。

```javascript
every(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#every
```

### Collection.Keyed#some()

如果`predicate`对Iterable中的任何条目返回true，则返回true。

```javascript
some(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#some
```

### Collection.Keyed#join()

将值作为字符串连接在一起，在每个值之间插入一个分隔符。默认分隔符是`","`。

```javascript
join(separator?: string): string
```

#### 继承

```
Iterable#join
```

### Collection.Keyed#isEmpty()

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

### Collection.Keyed#count()

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

不管这个Iterable是否可以描述它的大小（有些Seqs不能），这个方法总是会返回正确的大小。例如，`Seq`如果需要，它会评估一个lazy。

如果`predicate`提供，则返回Iterable中`predicate`返回值为true 的条目的计数。

### Collection.Keyed#countBy()

返回一个`Seq.Keyed`计数，按`grouper`函数的返回值分组。

```javascript
countBy<G>(grouper: (value?: V, key?: K, iter?: Iterable<K, V>) => G,context?: any): Map<G, number>
```

#### 继承

```
Iterable#countBy
```

#### 讨论

注意：这不是一个惰性的操作。

## 搜索值

### Collection.Keyed#find()

返回`predicate`返回true 的第一个值。

```javascript
find(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): V
```

#### 继承

```
Iterable#find
```

### Collection.Keyed#findLast()

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

### Collection.Keyed#findEntry()

返回返回值为true的第一个键值对`predicate`。

```javascript
findEntry(predicate: (value?: V, key?: K, iter?: Iterable<K, V>) => boolean,context?: any,notSetValue?: V): Array<any>
```

#### 继承

```
Iterable#findEntry
```

### Collection.Keyed#findLastEntry()

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

### Collection.Keyed#findKey()

返回`predicate`返回true 的键。

```javascript
findKey(predicate: (value?: V, key?: K, iter?: Iterable.Keyed<K, V>) => boolean,context?: any): K
```

#### 继承

```
Iterable#findKey
```

### Collection.Keyed#findLastKey()

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

### Collection.Keyed#keyOf()

返回与搜索值关联的键，或者未定义。

```javascript
keyOf(searchValue: V): K
```

#### 继承

```
Iterable#keyOf
```

### Collection.Keyed#lastKeyOf()

返回与搜索值关联的最后一个键，或者未定义。

```javascript
lastKeyOf(searchValue: V): K
```

#### 继承

```
Iterable#lastKeyOf
```

### Collection.Keyed#max()

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

### Collection.Keyed#maxBy()

喜欢`max`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

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

### Collection.Keyed#min()

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

### Collection.Keyed#minBy()

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

### Collection.Keyed#isSubset()

如果`iter`包含此Iterable中的每个值，则为真。

```javascript
isSubset(iter: Iterable<any, V>): boolean
isSubset(iter: Array<V>): boolean
```

#### 继承

```
Iterable#isSubset
```

### Collection.Keyed#isSuperset()

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

### Collection.Keyed#flip()

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

### Collection.Keyed#mapKeys()

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

### Collection.Keyed#mapEntries()

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