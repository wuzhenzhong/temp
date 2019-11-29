# Stack

栈是索引的集合，支持使用`unshift(v)`和从前端非常高效地添加和删除O（1）`shift()`。

```javascript
class Stack<T> extends Collection.Indexed<T>
```

#### 讨论

为了熟悉起见，Stack还提供了`push(v)`，`pop()`和`peek()`，但是请注意，它们也可以在列表的前面进行操作，而不像List或JavaScript Array。

注意：`reverse()`或任何固有的反向遍历（`reduceRight`，`lastIndexOf`等）是无效率用栈。

堆栈是通过单链表实现的。

## 命令

### Stack()

创建一个新的不可变的Stack，其中包含提供的类似iterable的值。

```javascript
Stack<T>(): Stack<T>
Stack<T>(iter: Iterable.Indexed<T>): Stack<T>
Stack<T>(iter: Iterable.Set<T>): Stack<T>
Stack<K, V>(iter: Iterable.Keyed<K, V>): Stack<any>
Stack<T>(array: Array<T>): Stack<T>
Stack<T>(iterator: Iterator<T>): Stack<T>
Stack<T>(iterable: Object): Stack<T>
```

#### 讨论

提供的迭代的迭代顺序保存在结果`Stack`中。

## 静态方法

### Stack.isStack()

如果提供的值是堆栈，则为真

```javascript
Stack.isStack(maybeStack: any): boolean
```

### Stack.of()

创建一个包含`values`的新Stack 。

```javascript
Stack.of<T>(...values: T[]): Stack<T>
```

## 成员

### Stack#size

```javascript
size: number
```

#### 继承

```
Collection#size
```

## 读取值

### Stack#peek()

别名`Stack.first()`。

```javascript
peek(): T
```

### Stack#get()

返回与提供的键相关联的值，如果Iterable不包含此键，则返回notSetValue。

```javascript
get(key: number, notSetValue?: T): T
```

#### 继承

```
Iterable#get
```

#### 讨论

注意：一个键可能与一个`undefined`值相关联，所以如果`notSetValue`没有提供并且该方法返回`undefined`，那么不能保证没有找到该键。

### Stack#has()

如果此关键字存在`Iterable`，则为真，`Immutable.is`用于确定相等性

```javascript
has(key: number): boolean
```

#### 继承

```
Iterable#has
```

### Stack#includes()

如果此值中存在值`Iterable`，则为true ，`Immutable.is`用于确定相等性

```javascript
includes(value: T): boolean
```

#### 继承

```
Iterable#includes
```

#### 别称

```
contains()
```

### Stack#first()

Iterable中的第一个值。

```javascript
first(): T
```

#### 继承

```
Iterable#first
```

### Stack#last()

Iterable中的最后一个值。

```javascript
last(): T
```

#### 继承

```
Iterable#last
```

## 持续变化

### Stack#clear()

返回一个具有0大小且没有值的新堆栈。

```javascript
clear(): Stack<T>
```

### Stack#unshift()

返回一个新的Stack，`values`前面提供了其他值，将其他值前移到较高的索引处。

```javascript
unshift(...values: T[]): Stack<T>
```

#### 讨论

这对Stack非常有效。

### Stack#unshiftAll()

例如`Stack#unshift`，但接受可迭代而不是可变参数。

```javascript
unshiftAll(iter: Iterable<any, T>): Stack<T>
unshiftAll(iter: Array<T>): Stack<T>
```

### Stack#shift()

返回一个尺寸小于此堆栈的新堆栈，不包括此堆栈中的第一个项目，将所有其他值移至较低的索引。

```javascript
shift(): Stack<T>
```

#### 讨论

注意：它返回一个新的堆栈而不是被删除的值。使用`first()`或`peek()`获取此堆栈中的第一个值。

### Stack#push()

别名为`Stack#unshift`且不等于`List#push`。

```javascript
push(...values: T[]): Stack<T>
```

### Stack#pushAll()

别名为`Stack#unshiftAll`。

```javascript
pushAll(iter: Iterable<any, T>): Stack<T>
pushAll(iter: Array<T>): Stack<T>
```

### Stack#pop()

别名`Stack#shift`且不等于`List#pop`。

```javascript
pop(): Stack<T>
```

## 瞬态变化

### Stack#withMutations()

注意：并非所有方法都可用于可变集合或内部`withMutations`！只有`set`，`push`与`pop`可以被突变地使用。

```javascript
withMutations(mutator: (mutable: Stack<T>) => any): Stack<T>
```

#### see

```
Map#withMutations
```

### Stack#asMutable()

```javascript
asMutable(): Stack<T>
```

#### see

```
Map#asMutable
```

### Stack#asImmutable()

```javascript
asImmutable(): Stack<T>
```

#### see

```
Map#asImmutable
```

## 转换为Seq

### Stack#toSeq()

返回Seq.Indexed。

```javascript
toSeq(): Seq.Indexed<T>
```

#### 继承

```
Collection.Indexed#toSeq
```

### Stack#toKeyedSeq()

从这个Iterable返回一个Seq.Keyed，其索引被视为键。

```javascript
toKeyedSeq(): Seq.Keyed<number, T>
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

### Stack#toIndexedSeq()

返回一个Iterable的值的Seq.Indexed，丢弃键。

```javascript
toIndexedSeq(): Seq.Indexed<T>
```

#### 继承

```
Iterable#toIndexedSeq
```

### Stack#toSetSeq()

返回一个Iterable的值的Seq.Set，丢弃键。

```javascript
toSetSeq(): Seq.Set<T>
```

#### 继承

```
Iterable#toSetSeq
```

### Stack#fromEntrySeq()

如果这是一个可迭代的键值元组，它将返回这些条目的Seq.Keyed。

```javascript
fromEntrySeq(): Seq.Keyed<any, any>
```

#### 继承

```
Iterable.Indexed#fromEntrySeq
```

## 值相等

### Stack#equals()

如果这和另一个Iterable具有值相等性，则为真，如下定义`Immutable.is()`。

```javascript
equals(other: Iterable<number, T>): boolean
```

#### 继承

```
Iterable#equals
```

#### 讨论

注意：这相当于`Immutable.is(this, other)`，但提供允许链式表达式。

### Stack#hashCode()

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

如果两个值相同`hashCode`，则不能保证相等。如果两个值有不同的`hashCode`s，则它们不能相等。

## 读深式的值

### Stack#getIn()

通过嵌套的Iterables返回键或索引路径的值。

```javascript
getIn(searchKeyPath: Array<any>, notSetValue?: any): any
getIn(searchKeyPath: Iterable<any, any>, notSetValue?: any): any
```

#### 继承

```
Iterable#getIn
```

### Stack#hasIn()

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

### Stack#toJS()

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

Iterable.Indexeds和Iterable.Sets成为数组，而Iterable.Keyeds成为对象。

### Stack#toArray()

浅显地将这个迭代器转换为一个Array，丢弃键。

```javascript
toArray(): Array<T>
```

#### 继承

```
Iterable#toArray
```

### Stack#toObject()

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

### Stack#toMap()

将此Iterable转换为Map，如果键不可哈希则抛出。

```javascript
toMap(): Map<number, T>
```

#### 继承

```
Iterable#toMap
```

#### 讨论

注意：这相当于`Map(this.toKeyedSeq())`，但为方便起见并允许链接表达式。

### Stack#toOrderedMap()

将此Iterable转换为Map，并保持迭代顺序。

```javascript
toOrderedMap(): OrderedMap<number, T>
```

#### 继承

```
Iterable#toOrderedMap
```

#### 讨论

注意：这相当于`OrderedMap(this.toKeyedSeq())`，但为方便起见并允许链接表达式。

### Stack#toSet()

将此Iterable转换为Set，放弃键。如果值不可哈希则抛出。

```javascript
toSet(): Set<T>
```

#### 继承

```
Iterable#toSet
```

#### 讨论

注意：这相当于`Set(this)`，但提供允许链式表达式。

### Stack#toOrderedSet()

将此Iterable转换为Set，保持迭代顺序并丢弃键。

```javascript
toOrderedSet(): OrderedSet<T>
```

#### 继承

```
Iterable#toOrderedSet
```

#### 讨论

注意：这相当于`OrderedSet(this.valueSeq())`，但为方便起见并允许链接表达式。

### Stack#toList()

将此Iterable转换为List，放弃键。

```javascript
toList(): List<T>
```

#### 继承

```
Iterable#toList
```

#### 讨论

注意：这相当于`List(this)`，但提供允许链式表达式。

### Stack#toStack()

将此Iterable转换为堆栈，丢弃键。如果值不可哈希则抛出。

```javascript
toStack(): Stack<T>
```

#### 继承

```
Iterable#toStack
```

#### 讨论

注意：这相当于`Stack(this)`，但提供允许链式表达式。

## 迭代器

### Stack#keys()

这个`Iterable`键的迭代器。

```javascript
keys(): Iterator<number>
```

#### 继承

```
Iterable#keys
```

#### 讨论

注意：这将返回一个不支持Immutable JS序列算法的ES6迭代器。使用`keySeq`替代，如果这是你想要的。

### Stack#values()

这个`Iterable`值的迭代器。

```javascript
values(): Iterator<T>
```

#### 继承

```
Iterable#values
```

#### 讨论

注意：这将返回一个不支持Immutable JS序列算法的ES6迭代器。使用`valueSeq`替代，如果这是你想要的。

### Stack#entries()

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

### Stack#keySeq()

返回此Iterable的新键的Seq.Indexed，放弃值。

```javascript
keySeq(): Seq.Indexed<number>
```

#### 继承

```
Iterable#keySeq
```

### Stack#valueSeq()

返回一个Seq.Indexed这个Iterable的值，丢弃键。

```javascript
valueSeq(): Seq.Indexed<T>
```

#### 继承

```
Iterable#valueSeq
```

### Stack#entrySeq()

返回一个新的Seq.Indexed键值值元组。

```javascript
entrySeq(): Seq.Indexed<Array<any>>
```

#### 继承

```
Iterable#entrySeq
```

## 序列算法

### Stack#map()

使用通过`mapper`函数传递的值返回相同类型的新Iterable 。

```javascript
map<M>(mapper: (value?: T, key?: number, iter?: Iterable<number, T>) => M,context?: any): Iterable<number, M>
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

### Stack#filter()

仅返回谓词函数返回true的条目的同一类型的新Iterable。

```javascript
filter(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any): Iterable<number, T>
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

### Stack#filterNot()

仅`predicate`返回函数返回false 的条目返回相同类型的新Iterable 。

```javascript
filterNot(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any): Iterable<number, T>
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

### Stack#reverse()

按相反顺序返回相同类型的新Iterable。

```javascript
reverse(): Iterable<number, T>
```

#### 继承

```
Iterable#reverse
```

### Stack#sort()

返回包含相同条目的相同类型的新Iterable，通过使用a进行稳定排序`comparator`。

```javascript
sort(comparator?: (valueA: T, valueB: T) => number): Iterable<number, T>
```

#### 继承

```
Iterable#sort
```

#### 讨论

如果`comparator`没有提供，默认比较器使用`<`和`>`。

`comparator(valueA, valueB)`:

- 返回`0`如果元素属于不应该交换的情况。
- 返回`-1`（或任何负数），如果`valueA`在`valueB` 之前
- 返回`1`（或任何正数），如果`valueA`在`valueB` 之后
- 是纯粹的，即它必须始终为同一对值返回相同的值。

排序没有定义顺序的集合时，它们的顺序等价物将被返回。例如`map.sort()`返回OrderedMap。

### Stack#sortBy()

例如`sort`，但也接受一个`comparatorValueMapper`允许更复杂的手段进行排序的一个：

```javascript
sortBy<C>(comparatorValueMapper: (value?: T,key?: number,iter?: Iterable<number, T>) => C,comparator?: (valueA: C, valueB: C) => number): Iterable<number, T>
```

#### 继承

```
Iterable#sortBy
```

#### 例

```javascript
hitters.sortBy(hitter => hitter.avgHits);
```

### Stack#groupBy()

返回`Iterable.Keyed`的`Iterable.Keyeds`，由返回值分组`grouper`功能。

```javascript
groupBy<G>(grouper: (value?: T, key?: number, iter?: Iterable<number, T>) => G,context?: any): Seq.Keyed<G, Iterable<number, T>>
```

#### 继承

```
Iterable#groupBy
```

#### 讨论

注意：这总是一个急切的操作。

## 副作用

### Stack#forEach()

该`sideEffect`是在可迭代的每个条目执行。

```javascript
forEach(sideEffect: (value?: T, key?: number, iter?: Iterable<number, T>) => any,context?: any): number
```

#### 继承

```
Iterable#forEach
```

#### 讨论

与Array＃forEach不同，如果任何sideEffect调用返回false，则迭代将停止。 返回迭代的条目数（包括返回false的最后一次迭代）。

## 创建子集

### Stack#slice()

返回一个新的Iterable，其类型代表这个Iterable从开始到结束的一部分。

```javascript
slice(begin?: number, end?: number): Iterable<number, T>
```

#### 继承

```
Iterable#slice
```

#### 讨论

如果begin是负数，它将从Iterable的末尾偏移。例如`slice(-2)`返回最后两个条目的Iterable。如果没有提供，则新的Iterable将在此Iterable开始时开始。

如果end是负数，它将从Iterable的末尾偏移。例如`slice(0, -1)`返回除最后一项之外的所有内容的Iterable。如果没有提供，那么新的Iterable将会持续到这个Iterable的结尾。

如果所请求的分片等同于当前的Iterable，那么它将自行返回。

### Stack#rest()

返回包含除第一个以外的所有条目的同一类型的新Iterable。

```javascript
rest(): Iterable<number, T>
```

#### 继承

```
Iterable#rest
```

### Stack#butLast()

返回包含除最后一个以外的所有条目的同一类型的新Iterable。

```javascript
butLast(): Iterable<number, T>
```

#### 继承

```
Iterable#butLast
```

### Stack#skip()

返回从此Iterable中排除第一个数量条目的同一类型的新Iterable。

```javascript
skip(amount: number): Iterable<number, T>
```

#### 继承

```
Iterable#skip
```

### Stack#skipLast()

返回从此Iterable中排除最后一个条目的相同类型的新Iterable。

```javascript
skipLast(amount: number): Iterable<number, T>
```

#### 继承

```
Iterable#skipLast
```

### Stack#skipWhile()

返回相同类型的新Iterable，其中包含从谓词第一次返回false时开始的条目。

```javascript
skipWhile(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any): Iterable<number, T>
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

### Stack#skipUntil()

返回相同类型的新Iterable，其中包含从谓词第一次返回true时开始的条目。

```javascript
skipUntil(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any): Iterable<number, T>
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

### Stack#take()

返回包含此Iterable中第一个条目的相同类型的新Iterable。

```javascript
take(amount: number): Iterable<number, T>
```

#### 继承

```
Iterable#take
```

### Stack#takeLast()

返回包含此Iterable中最后一个条目的相同类型的新Iterable。

```javascript
takeLast(amount: number): Iterable<number, T>
```

#### 继承

```
Iterable#takeLast
```

### Stack#takeWhile()

只要谓词返回true，就返回包含来自此Iterable的条目的相同类型的新Iterable。

```javascript
takeWhile(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any): Iterable<number, T>
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

### Stack#takeUntil()

只要谓词返回false，就返回包含来自此Iterable的条目的相同类型的新Iterable。

```javascript
takeUntil(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any): Iterable<number, T>
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

### Stack#concat()

用其他值返回一个具有相同类型的新Iterable，并将其连接到此类。

```javascript
concat(...valuesOrIterables: any[]): Iterable<number, T>
```

#### 继承

```
Iterable#concat
```

#### 讨论

对于Seqs，即使它们具有相同的密钥，所有条目也会出现在所得到的迭代中。

### Stack#flatten()

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

### Stack#flatMap()

平面映射Iterable，返回相同类型的Iterable。

```javascript
flatMap<MK, MV>(mapper: (value?: T,key?: number,iter?: Iterable<number, T>) => Iterable<MK, MV>,context?: any): Iterable<MK, MV>
flatMap<MK, MV>(mapper: (value?: T, key?: number, iter?: Iterable<number, T>) => any,context?: any): Iterable<MK, MV>
```

#### 继承

```
Iterable#flatMap
```

#### 讨论

类似于`iter.map(...).flatten(true)`。

### Stack#interpose()

使用此Iterable中每个项目之间的分隔符返回相同类型的Iterable。

```javascript
interpose(separator: T): Iterable.Indexed<T>
```

#### 继承

```
Iterable.Indexed#interpose
```

### Stack#interleave()

使用提供的`iterables`交错迭代到此迭代中，返回相同类型的Iterable 。

```javascript
interleave(...iterables: Array<Iterable<any, T>>): Iterable.Indexed<T>
```

#### 继承

```
Iterable.Indexed#interleave
```

#### 讨论

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

### Stack#splice()

Splice通过用新值替换此Iterable的区域来返回新的索引Iterable。如果未提供值，则只跳过要删除的区域。

```javascript
splice(index: number, removeNum: number, ...values: any[]): Iterable.Indexed<T>
```

#### 继承

```
Iterable.Indexed#splice
```

#### 讨论

`index`可能是一个负数，从Iterable的末尾开始索引。`s.splice(-2)`在倒数第二项之后拼接。

```javascript
Seq(['a','b','c','d']).splice(1, 2, 'q', 'r', 's')
// Seq ['a', 'q', 'r', 's', 'd']
```

### Stack#zip()

使用提供的迭代器返回“压缩”相同类型的Iterable。

```javascript
zip(...iterables: Array<Iterable<any, any>>): Iterable.Indexed<any>
```

#### 继承

```
Iterable.Indexed#zip
```

#### 讨论

例如`zipWith`，但使用默认值`zipper`：创建一个[`Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)。

```javascript
var a = Seq.of(1, 2, 3);
var b = Seq.of(4, 5, 6);
var c = a.zip(b); // Seq [ [ 1, 4 ], [ 2, 5 ], [ 3, 6 ] ]
```

### Stack#zipWith()

使用自定义`zipper`函数返回与提供的迭代“压缩”相同类型的Iterable 。

```javascript
zipWith<U, Z>(zipper: (value: T, otherValue: U) => Z,otherIterable: Iterable<any, U>): Iterable.Indexed<Z>
zipWith<U, V, Z>(zipper: (value: T, otherValue: U, thirdValue: V) => Z,otherIterable: Iterable<any, U>,thirdIterable: Iterable<any, V>): Iterable.Indexed<Z>
zipWith<Z>(zipper: (...any: Array<any>) => Z,...iterables: Array<Iterable<any, any>>): Iterable.Indexed<Z>
```

#### 继承

```
Iterable.Indexed#zipWith
```

#### 例

```javascript
var a = Seq.of(1, 2, 3);
var b = Seq.of(4, 5, 6);
var c = a.zipWith((a, b) => a + b, b); // Seq [ 5, 7, 9 ]
```

## 降低值

### Stack#reduce()

通过调用Iterable中的`reducer`条目并传递缩小的值，将Iterable减少为一个值。

```javascript
reduce<R>(reducer: (reduction?: R,value?: T,key?: number,iter?: Iterable<number, T>) => R,initialReduction?: R,context?: any): R
```

#### 继承

```
Iterable#reduce
```

#### see

[`Array#reduce`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce).

#### 讨论

如果`initialReduction`未提供，或者为空，则将使用Iterable中的第一项。

### Stack#reduceRight()

反向（从右侧）减少Iterable。

```javascript
reduceRight<R>(reducer: (reduction?: R,value?: T,key?: number,iter?: Iterable<number, T>) => R,initialReduction?: R,context?: any): R
```

#### 继承

```
Iterable#reduceRight
```

#### 讨论

注意：类似于this.reverse（）。reduce（），并提供与奇偶校验[`Array#reduceRight`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight)。

### Stack#every()

如果`predicate`对Iterable中的所有条目返回true，则返回true。

```javascript
every(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#every
```

### Stack#some()

如果`predicate`对Iterable中的任何条目返回true，则返回true。

```javascript
some(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any): boolean
```

#### 继承

```
Iterable#some
```

### Stack#join()

将值作为字符串连接在一起，在每个值之间插入一个分隔符。默认分隔符是`","`。

```javascript
join(separator?: string): string
```

#### 继承

```
Iterable#join
```

### Stack#isEmpty()

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

### Stack#count()

返回此Iterable的大小。

```javascript
count(): number
count(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any): number
```

#### 继承

```
Iterable#count
```

#### 讨论

不管这个Iterable是否可以描述它的大小（有些Seqs不能），这个方法总是会返回正确的大小。例如，如果需要，它会评估一个懒惰型`Seq`

如果`predicate`提供，则返回Iterable中`predicate`返回值为true 的条目的计数。

### Stack#countBy()

返回一个`Seq.Keyed`计数，按`grouper`函数的返回值分组。

```javascript
countBy<G>(grouper: (value?: T, key?: number, iter?: Iterable<number, T>) => G,context?: any): Map<G, number>
```

#### 继承

```
Iterable#countBy
```

#### 讨论

注意：这不是一个懒惰的操作。

## 搜索值

### Stack#find()

返回`predicate`返回true 的第一个值。

```javascript
find(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any,notSetValue?: T): T
```

#### 继承

```
Iterable#find
```

### Stack#findLast()

返回谓词返回true的最后一个值。

```javascript
findLast(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any,notSetValue?: T): T
```

#### 继承

```
Iterable#findLast
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### Stack#findEntry()

返回返回值为true的第一个键值对`predicate`。

```javascript
findEntry(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any,notSetValue?: T): Array<any>
```

#### 继承

```
Iterable#findEntry
```

### Stack#findLastEntry()

返回返回值为true的最后一个键值对`predicate`。

```javascript
findLastEntry(predicate: (value?: T, key?: number, iter?: Iterable<number, T>) => boolean,context?: any,notSetValue?: T): Array<any>
```

#### 继承

```
Iterable#findLastEntry
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### Stack#findKey()

返回`predicate`返回true 的键。

```javascript
findKey(predicate: (value?: T,key?: number,iter?: Iterable.Keyed<number, T>) => boolean,context?: any): number
```

#### 继承

```
Iterable#findKey
```

### Stack#findLastKey()

返回`predicate`返回true 的最后一个键。

```javascript
findLastKey(predicate: (value?: T,key?: number,iter?: Iterable.Keyed<number, T>) => boolean,context?: any): number
```

#### 继承

```
Iterable#findLastKey
```

#### 讨论

注意：`predicate`每个条目都会被调用。

### Stack#keyOf()

返回与搜索值关联的键，或者未定义。

```javascript
keyOf(searchValue: T): number
```

#### 继承

```
Iterable#keyOf
```

### Stack#lastKeyOf()

返回与搜索值关联的最后一个键，或者未定义。

```javascript
lastKeyOf(searchValue: T): number
```

#### 继承

```
Iterable#lastKeyOf
```

### Stack#max()

返回此集合中的最大值。如果任何值相当相等，则找到的第一个将被返回。

```javascript
max(comparator?: (valueA: T, valueB: T) => number): T
```

#### 继承

```
Iterable#max
```

#### 讨论

比较器的使用方式与Iterable＃排序相同。 如果没有提供，默认比较器是>。

当两个值被认为是等价的，遇到的第一个将被返回。否则，只要比较器是可交换的，max将独立于输入的顺序进行操作。默认比较器`>`*只有*在类型不相同*时才*可以交换。

如果`comparator`返回0，且其中任一值为NaN，undefined或null，则将返回该值。

### Stack#maxBy()

例如`max`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

```javascript
maxBy<C>(comparatorValueMapper: (value?: T,key?: number,iter?: Iterable<number, T>) => C,comparator?: (valueA: C, valueB: C) => number): T
```

#### 继承

```
Iterable#maxBy
```

#### 例

```javascript
hitters.maxBy(hitter => hitter.avgHits);
```

### Stack#min()

返回此集合中的最小值。如果任何值相当相等，则找到的第一个将被返回。

```javascript
min(comparator?: (valueA: T, valueB: T) => number): T
```

#### 继承

```
Iterable#min
```

#### 讨论

在`comparator`以同样的方式使用`Iterable#sort`。如果未提供，则默认比较器为`<`。

当两个值被认为是等价的，遇到的第一个将被返回。否则，只要比较器是可交换的，min将独立于输入的顺序进行操作。默认比较器`<`*只有*在类型不相同*时才*可以交换。

如果`comparator`返回0，且其中任一值为NaN，undefined或null，则将返回该值。

### Stack#minBy()

例如`min`，但也接受一个`comparatorValueMapper`允许通过更复杂的手段比较：

```javascript
minBy<C>(comparatorValueMapper: (value?: T,key?: number,iter?: Iterable<number, T>) => C,comparator?: (valueA: C, valueB: C) => number): T
```

#### 继承

```
Iterable#minBy
```

#### 例

```javascript
hitters.minBy(hitter => hitter.avgHits);
```

### Stack#indexOf()

返回可以在Iterable中找到给定值的第一个索引，如果不存在则返回-1。

```javascript
indexOf(searchValue: T): number
```

#### 继承

```
Iterable.Indexed#indexOf
```

### Stack#lastIndexOf()

返回可以在Iterable中找到给定值的最后一个索引，如果不存在则返回-1。

```javascript
lastIndexOf(searchValue: T): number
```

#### 继承

```
Iterable.Indexed#lastIndexOf
```

### Stack#findIndex()

返回Iterable中的第一个索引，其中的值满足提供的谓词函数。否则返回-1。

```javascript
findIndex(predicate: (value?: T, index?: number, iter?: Iterable.Indexed<T>) => boolean,context?: any): number
```

#### 继承

```
Iterable.Indexed#findIndex
```

### Stack#findLastIndex()

返回一个值满足提供的谓词函数的Iterable中的最后一个索引。否则返回-1。

```javascript
findLastIndex(predicate: (value?: T, index?: number, iter?: Iterable.Indexed<T>) => boolean,context?: any): number
```

#### 继承

```
Iterable.Indexed#findLastIndex
```

## 对照

### Stack#isSubset()

如果`iter`包含此Iterable中的每个值，则为真。

```javascript
isSubset(iter: Iterable<any, T>): boolean
isSubset(iter: Array<T>): boolean
```

#### 继承

```
Iterable#isSubset
```

### Stack#isSuperset()

如果此Iterable包含iter中的每个值，则为true。

```javascript
isSuperset(iter: Iterable<any, T>): boolean
isSuperset(iter: Array<T>): boolean
```

#### 继承

```
Iterable#isSuperset
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-04-11