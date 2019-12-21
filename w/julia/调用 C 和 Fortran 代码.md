# Julia 调用 C 和 Fortran 代码

## 调用 C 和 Fortran 代码

## Julia 调用 C 和 Fortran 的函数，既简单又高效。

被调用的代码应该是共享库的格式。大多数 C 和 Fortran 库都已经被编译为共享库。如果自己使用 GCC （或 Clang ）编译代码，需要添加 `-shared` 和 `-fPIC` 选项。Julia 调用这些库的开销与本地 C 语言相同。

调用共享库和函数时使用多元组形式： `(:function, "library")` 或 `("function", "library")` ，其中 `function` 是 C 的导出函数名， `library` 是共享库名。共享库依据名字来解析，路径由环境变量来确定，有时需要直接指明。

多元组内有时仅有函数名（仅 `:function` 或 `"function"` ）。此时，函数名由当前进程解析。这种形式可以用来调用 C 库函数， Julia 运行时函数，及链接到 Julia 的应用中的函数。

使用 `ccall` 来生成库函数调用。 `ccall` 的参数如下:

1. (:function, "library") 多元组对儿（必须为常量，详见下面）
2. 返回类型，可以为任意的位类型，包括 `Int32` ， `Int64` ， `Float64` ，或者指向任意类型参数 `T` 的指针 `Ptr{T}` ，或者仅仅是指向无类型指针 `void*` 的 `Ptr`
3. 输入的类型的多元组，与上述的返回类型的要求类似。输入必须是多元组，而不是值为多元组的变量或表达式
4. 后面的参数，如果有的话，都是被调用函数的实参

下例调用标准 C 库中的 `clock` ：

```
    julia> t = ccall( (:clock, "libc"), Int32, ())
    2292761

    julia> t
    2292761

    julia> typeof(ans)
    Int32
```

`clock` 函数没有参数，返回 `Int32` 类型。输入的类型如果只有一个，常写成一元多元组，在后面跟一逗号。例如要调用 `getenv` 函数取得指向一个环境变量的指针，应这样调用：

```
    julia> path = ccall( (:getenv, "libc"), Ptr{Uint8}, (Ptr{Uint8},), "SHELL")
    Ptr{Uint8} @0x00007fff5fbffc45

    julia> bytestring(path)
    "/bin/bash"
```

注意，类型多元组的参数必须写成 `(Ptr{Uint8},)` ，而不是 `(Ptr{Uint8})` 。这是因为 `(Ptr{Uint8})` 等价于 `Ptr{Uint8}` ，它并不是一个包含 `Ptr{Uint8}` 的一元多元组：

```
    julia> (Ptr{Uint8})
    Ptr{Uint8}

    julia> (Ptr{Uint8},)
    (Ptr{Uint8},)
```

实际中要提供可复用代码时，通常要使用 Julia 的函数来封装 `ccall` ，设置参数，然后检查 C 或 Fortran 函数中可能出现的任何错误，将其作为异常传递给 Julia 的函数调用者。下例中， `getenv` C 库函数被封装在 [env.jl](ttps://github.com/JuliaLang/julia/blob/master/base/env.jl) 里的 Julia 函数中：

```
    function getenv(var::String)
      val = ccall( (:getenv, "libc"),
                  Ptr{Uint8}, (Ptr{Uint8},), var)
      if val == C_NULL
        error("getenv: undefined variable: ", var)
      end
      bytestring(val)
    end
```

上例中，如果函数调用者试图读取一个不存在的环境变量，封装将抛出异常：

```
    julia> getenv("SHELL")
    "/bin/bash"

    julia> getenv("FOOBAR")
    getenv: undefined variable: FOOBAR
```

下例稍复杂些，显示本地机器的主机名：

```
    function gethostname()
      hostname = Array(Uint8, 128)
      ccall( (:gethostname, "libc"), Int32,
            (Ptr{Uint8}, Uint),
            hostname, length(hostname))
      return bytestring(convert(Ptr{Uint8}, hostname))
    end
```

此例先分配出一个字节数组，然后调用 C 库函数 `gethostname` 向数组中填充主机名，取得指向主机名缓冲区的指针，在默认其为空结尾 C 字符串的前提下，将其转换为 Julia 字符串。 C 库函数一般都用这种方式从函数调用者那儿，将申请的内存传递给被调用者，然后填充。在 Julia 中分配内存，通常都需要通过构建非初始化数组，然后将指向数据的指针传递给 C 函数。

调用 Fortran 函数时，所有的输入都必须通过引用来传递。

`&` 前缀说明传递的是指向标量参数的指针，而不是标量值本身。下例使用 BLAS 函数计算点积：

```
    function compute_dot(DX::Vector{Float64}, DY::Vector{Float64})
      assert(length(DX) == length(DY))
      n = length(DX)
      incx = incy = 1
      product = ccall( (:ddot_, "libLAPACK"),
                      Float64,
                      (Ptr{Int32}, Ptr{Float64}, Ptr{Int32}, Ptr{Float64}, Ptr{Int32}),
                      &n, DX, &incx, DY, &incy)
      return product
    end
```

前缀 `&` 的意思与 C 中的不同。对引用的变量的任何更改，都是对 Julia 不可见的。 `&` 并不是真正的地址运算符，可以在任何语法中使用它，例如 `&0` 和 `&f(x)` 。

注意在处理过程中，C 的头文件可以放在任何地方。目前还不能将 Julia 的结构和其他非基础类型传递给 C 库。通过传递指针来生成、使用非透明结构类型的 C 函数，可以向 Julia 返回 `Ptr{Void}` 类型的值，这个值以 `Ptr{Void}` 的形式被其它 C 函数调用。可以像任何 C 程序一样，通过调用库中对应的程序，对对象进行内存分配和释放。

## 把 C 类型映射到 Julia

Julia 自动调用 `convert` 函数，将参数转换为指定类型。例如：

```
    ccall( (:foo, "libfoo"), Void, (Int32, Float64),
          x, y)
```

会按如下操作：

```
    ccall( (:foo, "libfoo"), Void, (Int32, Float64),
          convert(Int32, x), convert(Float64, y))
```

如果标量值与 `&` 一起被传递作为 `Ptr{T}` 类型的参数时，值首先会被转换为 `T` 类型。

### 数组转换

把数组作为一个 `Ptr{T}` 参数传递给 C 时，它不进行转换。Julia 仅检查元素类型是否为 `T` ，然后传递首元素的地址。这样做可以避免不必要的复制整个数组。

因此，如果 `Array` 中的数据格式不对时，要使用显式转换，如 `int32(a)` 。

如果想把数组 *不经转换* 而作为一个不同类型的指针传递时，要么声明参数为 `Ptr{Void}` 类型，要么显式调用 `convert(Ptr{T}, pointer(A))` 。

### 类型相关

基础的 C/C++ 类型和 Julia 类型对照如下。每个 C 类型也有一个对应名称的 Julia 类型，不过冠以了前缀 C 。这有助于编写简便的代码（但 C 中的 int 与 Julia 中的 Int 不同）。

**与系统无关：**

| unsigned char                               | Cuchar                                        | Uint8           |
| :------------------------------------------ | :-------------------------------------------- | :-------------- |
| short                                       | Cshort                                        | Int16           |
| unsigned short                              | Cushort                                       | Uint16          |
| int                                         | Cint                                          | Int32           |
| unsigned int                                | Cuint                                         | Uint32          |
| long long                                   | Clonglong                                     | Int64           |
| unsigned long long                          | Culonglong                                    | Uint64          |
| intmax_t                                    | Cintmax_t                                     | Int64           |
| uintmax_t                                   | Cuintmax_t                                    | Uint64          |
| float                                       | Cfloat                                        | Float32         |
| double                                      | Cdouble                                       | Float64         |
| ptrdiff_t                                   | Cptrdiff_t                                    | Int             |
| ssize_t                                     | Cssize_t                                      | Int             |
| size_t                                      | Csize_t                                       | Uint            |
| void                                        |                                               | Void            |
| void*                                       | Ptr{Void}                                     |                 |
| char* (or char[], e.g. a string)            | Ptr{Uint8}                                    |                 |
| char* *(or* char[])                         |                                               | Ptr{Ptr{Uint8}} |
| struct T* (T 正确表示一个定义好的 bit 类型) | Ptr{T} (在参数列表中使用 &variable_name 调用) |                 |
| struct T (T 正确表示一个定义好的 bit 类型)  | T (在参数列表中使用 &variable_name 调用)      |                 |
| jl_value_t* (任何 Julia 类型)               |                                               | Ptr{Any}        |

对应于字符串参数（ `char*` ）的 Julia 类型为 `Ptr{Uint8}` ，而不是 `ASCIIString` 。参数中有 `char**` 类型的 C 函数，在 Julia 中调用时应使用 `Ptr{Ptr{Uint8}}` 类型。例如，C 函数：

```
    int main(int argc, char **argv);
```

在 Julia 中应该这样调用：

```
    argv = [ "a.out", "arg1", "arg2" ]
    ccall(:main, Int32, (Int32, Ptr{Ptr{Uint8}}), length(argv), argv)
```

对于 `wchar_t*` 参数，Julia 类型为 `Ptr{Wchar_t}`,并且数据可以通过 `wstring(s)` 方法转换为原始的 Julia 字符串(等同于 `utf16(s)`或`utf32(s)` ,这取决于 `Cwchar_t` 的宽度)。还要注意 ASCII, UTF-8, UTF-16, 和UTF-32 字符串数据在 Julia 内部是以 NUL 结尾的，所以它能够传递到 C 函数中以 NUL 为结尾的数据，而不用再做一个拷贝。

### 通过指针读取数据

下列方法是“不安全”的，因为坏指针或类型声明可能会导致意外终止或损坏任意进程内存。

指定 `Ptr{T}` ，常使用 `unsafe_ref(ptr, [index])` 方法，将类型为 `T` 的内容从所引用的内存复制到 Julia 对象中。 `index` 参数是可选的（默认为 1 ），它是从 1 开始的索引值。此函数类似于 `getindex()` 和 `setindex!()` 的行为（如 `[]` 语法）。

返回值是一个被初始化的新对象，它包含被引用内存内容的浅拷贝。被引用的内存可安全释放。

如果 `T` 是 `Any` 类型，被引用的内存会被认为包含对 Julia 对象 `jl_value_t*` 的引用，结果为这个对象的引用，且此对象不会被拷贝。需要谨慎确保对象始终对垃圾回收机制可见（指针不重要，重要的是新的引用），来确保内存不会过早释放。注意，如果内存原本不是由 Julia 申请的，新对象将永远不会被 Julia 的垃圾回收机制释放。如果 `Ptr` 本身就是 `jl_value_t*` ，可使用 `unsafe_pointer_to_objref(ptr)` 将其转换回 Julia 对象引用。（可通过调用 `pointer_from_objref(v)` 将Julia 值 `v` 转换为 `jl_value_t*` 指针 `Ptr{Void}` 。）

逆操作（向 Ptr{T} 写数据）可通过 `unsafe_store!(ptr, value, [index])` 来实现。目前，仅支持位类型和其它无指针（ `isbits` ）不可变类型。

现在任何抛出异常的操作，估摸着都是还没实现完呢。来写个帖子上报 bug 吧，就会有人来解决啦。

如果所关注的指针是（位类型或不可变）的目标数据数组， `pointer_to_array(ptr,dims,[own])` 函数就非常有用啦。如果想要 Julia “控制”底层缓冲区并在返回的 `Array` 被释放时调用 `free(ptr)` ，最后一个参数应该为真。如果省略 `own` 参数或它为假，则调用者需确保缓冲区一直存在，直至所有的读取都结束。

`Ptr` 的算术(比如 `+`) 和 C 的指针算术不同， 对 `Ptr` 加一个整数会将指针移动一段距离的 *字节* ， 而不是元素。这样从指针运算上得到的地址不会依赖指针类型。

### 用指针传递修改值

因为 C 不支持多返回值， 所以通常 C 函数会用指针来修改值。 在 `ccall` 里完成这些需要把值放在适当类型的数组里。当你用 `Ptr` 传递整个数组时， Julia 会自动传递一个 C 指针到被这个值:

```
    width = Cint[0]
    range = Cfloat[0]
    ccall(:foo, Void, (Ptr{Cint}, Ptr{Cfloat}), width, range)
```

这被广泛用在了 Julia 的 LAPACK 接口上， 其中整数类型的 `info` 被以引用的方式传到 LAPACK， 再返回是否成功。

### 垃圾回收机制的安全

给 ccall 传递数据时，最好避免使用 `pointer()` 函数。应当定义一个转换方法，将变量直接传递给 ccall 。ccall 会自动安排，使得在调用返回前，它的所有参数都不会被垃圾回收机制处理。如果 C API 要存储一个由 Julia 分配好的内存的引用，当 ccall 返回后，需要自己设置，使对象对垃圾回收机制保持可见。推荐的方法为，在一个类型为 `Array{Any,1}` 的全局变量中保存这些值，直到 C 接口通知它已经处理完了。

只要构造了指向 Julia 数据的指针，就必须保证原始数据直至指针使用完之前一直存在。Julia 中的许多方法，如 `unsafe_ref()` 和 `bytestring()` ，都复制数据而不是控制缓冲区，因此可以安全释放（或修改）原始数据，不会影响到 Julia 。有一个例外需要注意，由于性能的原因， `pointer_to_array()` 会共享（或控制）底层缓冲区。

垃圾回收并不能保证回收的顺序。例如，当 `a` 包含对 `b` 的引用，且两者都要被垃圾回收时，不能保证 `b` 在 `a` 之后被回收。这需要用其它方式来处理。

### 非常量函数说明

`(name, library)` 函数说明应为常量表达式。可以通过 `eval` ，将计算结果作为函数名：

```
    @eval ccall(($(string("a","b")),"lib"), ...
```

表达式用 `string` 构造名字，然后将名字代入 `ccall` 表达式进行计算。注意 `eval` 仅在顶层运行，因此在表达式之内，不能使用本地变量（除非本地变量的值使用 `$` 进行过内插）。 `eval` 通常用来作为顶层定义，例如，将包含多个相似函数的库封装在一起。

### 间接调用

`ccall` 的第一个参数可以是运行时求值的表达式。此时，表达式的值应为 `Ptr` 类型，指向要调用的原生函数的地址。这个特性用于 `ccall` 的第一参数包含对非常量（本地变量或函数参数）的引用时。

### 调用方式

`ccall` 的第二个（可选）参数指定调用方式（在返回值之前）。如果没指定，将会使用操作系统的默认 C 调用方式。其它支持的调用方式为: `stdcall` , `cdecl` , `fastcall` 和 `thiscall` 。例如 (来自 base/libc.jl)：

```
    hn = Array(Uint8, 256)
    err=ccall(:gethostname, stdcall, Int32, (Ptr{Uint8}, Uint32), hn, length(hn))
```

更多信息请参考 [LLVM Language Reference](http://llvm.org/docs/LangRef.html#calling-conventions)

### 访问全局变量

当全局变量导出到本地库时可以使用 `cglobal` 方法，通过名称进行访问。`cglobal`的参数和 `ccall` 的指定参数是相同的符号，并且其表述了存储在变量中的值类型：

```
    julia> cglobal((:errno,:libc), Int32)
    Ptr{Int32} @0x00007f418d0816b8
```

该结果是一个该值的地址的指针。可以通过这个指针对这个值进行操作，但需要使用 `unsafe_load` 和 `unsafe_store`。

### 将 Julia 的回调函数传递给 C

可以将 Julia 函数传递给本地的函数，只要该函数有指针参数。一个典型的例子为标准 C 库 `qsort` 函数，描述如下：

```
    void qsort(void *base, size_t nmemb, size_t size,
               int(*compare)(const void *a, const void *b));
```

`base` 参数是一个数组长度 `nmemb` 的指针，每个元素大小为 `size` 字节。`compare` 是一个回调函数，带有两个元素 `a` 和 `b` 的指针，并且如果 `a` 在 `b` 之前或之后出现，则返回一个大于或者小于 0 的整数（如果允许任意顺序的话，结果为 0）。现在假设我们在 Julia 值中有一个一维数组 `A`，我们想给这个数组进行排序，使用 `qsort` 函数（不用 Julia 的内置函数）。在我们调用 `qsort` 和传递参数之前，我们需要写一个比较函数，来适应任意类型 T：

```
    function mycompare{T}(a_::Ptr{T}, b_::Ptr{T})
        a = unsafe_load(a_)
        b = unsafe_load(b_)
        return convert(Cint, a < b ? -1 : a > b ? +1 : 0)
    end
```

请注意，我们必须注意返回值类型：`qsort` 需要的是 C 语言的 `int` 类型变量作为返回值，所以我们必须通过调用 `convert` 来确保返回 `Cint`。

为了能够传递这个函数给 C，我们要通过 `cfunction` 来得到它的地址:

```
    const mycompare_c = cfunction(mycompare, Cint, (Ptr{Cdouble}, Ptr{Cdouble}))
```

`cfunction` 接受三个参数：Julia 函数（`mycompare`），返回值类型 （`Cint`）,和一个参数类型的元组，在这种情况下对 `cdouble`（Float64）元素 的数组进行排序。

最终对 `qsort` 的调用如下：

```
    A = [1.3, -2.7, 4.4, 3.1]
    ccall(:qsort, Void, (Ptr{Cdouble}, Csize_t, Csize_t, Ptr{Void}),
          A, length(A), sizeof(eltype(A)), mycompare_c)
```

执行该操作之后， `A` 会更改为排序数组 `[ -2.7, 1.3, 3.1, 4.4]`。注意 Julia 知道如何去将数组转换为 `Ptr{Cdouble}`,如何计算字节大小（与 C 的 `sizeof` 是相同的）等等。如果你有兴趣，你可以尝试在 `mycompare` 插入一个 `println("mycompare($a,$b)")`，这将允许你以比较的方式去查看 `qsort`
（并且确认它的确调用了 你传递的 Julia 函数）。

### 线程安全

一些 C 从不同的线程中执行他们的回调函数，并且 Julia 不含有线程安全，你需要做一些额外的预防措施。特别是，你需要设置两层系统：C 的回调应该只调度（通过 Julia 的时间循环）你“真正”的回调函数的执行。你的回调需要两个输入（你很可能会忘记）并且通过 `SingleAsyncWork` 进行包装::

```
  cb = Base.SingleAsyncWork(data -> my_real_callback(args))
```

你传递给 C 的回调应该仅仅执行 `ccall` 到 `:uv_async_send`,传递 `cb.handle` 作为参数。

### 关于回调更多的内容

对于更多的如何传递回调到 C 库的细节，请参考 [blog post](http://julialang.org/blog/2013/05/callback/)。

### C++

[Cpp](https://github.com/timholy/Cpp.jl) 和 [Clang](https://github.com/ihnorton/Clang.jl) 扩展包提供了有限的 C++ 支持。

### 处理不同平台

当处理不同的平台库的时候，经常要针对特殊平台提供特殊函数。这时常用到变量 `OS_NAME` 。此外，还有一些常用的宏： `@windows`, `@unix`, `@linux`, 及 `@osx` 。注意， linux 和 osx 是 unix 的不相交的子集。宏的用法类似于三元条件运算符。

简单的调用：

```
    ccall( (@windows? :_fopen : :fopen), ...)
```

复杂的调用：

```
    @linux? (
             begin
                 some_complicated_thing(a)
             end
           : begin
                 some_different_thing(a)
             end
           )
```

链式调用（圆括号可以省略，但为了可读性，最好加上）：

```
    @windows? :a : (@osx? :b : :c)
```