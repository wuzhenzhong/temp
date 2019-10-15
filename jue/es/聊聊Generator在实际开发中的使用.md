# 聊聊Generator在实际开发中的使用

一直都对生成器似懂非懂的感觉，知道生成器的特点：

- 可以在执行中暂停
- 执行生成器会返回迭代器

但是一直不明白生成器在实际开发的作用，下面一起来挖掘其可以解决哪些开发痛点。

# 先熟悉下迭代器

迭代器最常用的场景应该是for...of语法了：

```
for (let v of [1, 2, 3]) {
    console.log(v);
}
复制代码
```

这里我们用for...of语法去遍历数组每一项的值，数组之所以可以被for...of识别，是因为数组实现了可迭代协议：

**一个对象必须实现[Symbol.iterator]方法，其返回一个符合迭代器协议的对象**

什么是迭代器协议？迭代器协议要求一个对象实现next方法，其返回一个包含两个属性的对象：

- done: true/false，只是有没有超过可迭代次数
- value: 任何JavaScript值，done为true时可以省略

基于可迭代对象协议，我们也可以实现一个可迭代对象：

```
const obj = {
    [Symbol.iterator]() {
        const MAX_COUNT = 5;
        let count = 0;
        
        return {
            next() {
                return {
                    value: ++count,
                    done: count === MAX_COUNT
                };
            }
        };
    }
};
复制代码
```

然后使用for...of遍历这个可迭代对象：

```
for (let v of obj) {
    console.log(v);
}
复制代码
```

结果打印出：1 2 3 4，既然明白了for..of的本质其实就是对可迭代对象的封装，那么我们也可以实现一个for...of:

```
function forOf(obj, callback) {
    const iterator = obj[Symbol.iterator]();
    
    let { value, done } = iterator.next();
    
    while (!done) {
        callback(value);
        const o = iterator.next();
        value = o.value;
        done = o.done;
    }
}
复制代码
```

尝试使用下这个'for...of':

```
forOf([1, 2, 3], v => {
    console.log(v);
});
复制代码
```

结果正常打印出：1 2 3

# 使用迭代器替代回调

回顾forOf的实现，也就是说for...of语法其实是一个迭代器的语法糖，我们用回调的形式实现了for..of，那么反过来，是不是可以用for...of来简化回调呢？

比如说，对于一个树形结构：

```
const treeData = {
    value: 1,
    children: [
        {
            value: 2,
            children: null
        },
        {
            value: 3,
            children: [
                {
                    value: 4,
                    children: null
                }
            ]
        }
    ]
};
复制代码
```

想要去遍历这个树形结构，正常的想法就是去递归：

```
function traverse(tree, callback) {
    callback(tree.value);
    
    if (tree.children) {
        for (let child  of children) {
            traverse(child, callback);
        }
    }
}

traverse(treeData, v => {
    console.log(v);
});
复制代码
```

可以看到对于树结点的处理是通过回调完成的，是与遍历操作耦合在一起的，我们可以尝试用迭代器的方式去处理：

```
function* traverse(tree) {
    yield tree.value;
    
    if (tree.children) {
        for (let child of tree.children) {
            yield* traverse(child);
        }
    }
}

const iterator = traverse(treeData);
for (let v of iterator) {
    console.log(v);
}
复制代码
```

我们定义了一个生成器函数去遍历树结点，只是遍历，并没有包含其它操作。执行生成器函数会返回一个迭代器，在生成器内部是可以控制每次迭代的值的，然后迭代器是可以使用for...of语法的，对于结点的处理就放在for...of中进行。基于这种方式，可以很好地将遍历与处理的代码分隔开。

# 生成器与Promise结合

生成器在执行过程中是可以暂时挂起的，并且挂起状态是不会阻塞主线程的，后续可以用next方法让其被唤醒继续执行。Promise可以在未来触发某种条件的情况下得到事先承诺的值，本身也就是用来处理异步任务的，将其与生成器结合，可以更优雅地去处理异步任务。

```
function _async(generator) {
    const iterator = generator();
    
    function handle(iteratorResult) {
        const { value, done } = iteratorResult;
        
        if (done) {
            return;
        }
        
        if (value instanceof Promise) {
            value.then(v => handle(iterator.next(v)))
                    .catch(error => iterator.throw(error));
        }
    }
    
    try {
        handle(iterator.next());
    }
    catch (error) {
        iterator.throw(error);
    }
}
复制代码
```

_async函数接收一个生成器函数作为参数，在内部首先执行生成器函数返回迭代器iterator，然后执行内部函数handle，其参数为迭代器调用next方法的结果，并用try...catch捕获异常。我们知道，迭代器的next方法会唤醒生成器继续执行，并在下一个yield关键字处重新挂起，那么此处就非常适合等待异步任务的结果。

next方法的结果是一个对象，包含两个属性value与done，当done为true时，表示迭代完成，也就退出handle函数。否则，判断value是不是一个Promise，为什么呢？因为需要知道生成器再次被唤醒的时机，这个时机就是异步任务完成的时候，而Promise能很好地满足这个要求。Promise有三个状态：pending、fulfilled、rejected，当从pending变成fulfilled状态时，Promise会执行then方法注册的回调，参数为fulfilled的值。

这里，fulfilled状态就是异步任务完成的标记，而fulfilled的值就是异步任务的结果。所以这里，我们then方法的注册回调为：继续执行handle函数去处理下一个迭代值，也就是下一个异步任务，如果还有的话。

当然，异步任务失败，状态就从pending到rejected了，回去执行catch方法注册的回调，本质都是一样的。

我们模拟几个异步任务来测试下：

```
function getAsyncData(wait) {
    return new Promise(resolve => {
        setTimeout(() => {
            resolve(wait);
        }, wait * 1000);
    });
}

_async(function* () {
    const result1 = yield getAsyncData(1);
    console.log(result1);
    
    const result2 = yield getAsyncData(2);
    console.log(result2);
    
    const result3 = yield getAsyncData(3);
    console.log(result3);
});
复制代码
```

测试结果为：在第1s、3s、6s的时候打印了1 2 3，符合我们的预期。

其实，这就是我们平时常用的async...await语法的原理，即async函数就是生成器加Promise的语法糖。

# 结束

其实，平时开发中一直都在用生成器，只是没注意到而已。