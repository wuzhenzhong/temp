# 规范化状态( Normalizing State Shape )

- 贡献者1人

  

许多应用程序处理嵌套或关系性质的数据。例如，博客编辑可以有许多帖子，每个帖子可以有很多评论，并且帖子和评论都将由用户编写。这种应用程序的数据可能如下所示：

```javascript
const blogPosts = [
    {
        id : "post1",
        author : {username : "user1", name : "User 1"},
        body : "......",
        comments : [
            {
                id : "comment1",
                author : {username : "user2", name : "User 2"},
                comment : ".....",
            },
            {
                id : "comment2",
                author : {username : "user3", name : "User 3"},
                comment : ".....",
            }
        ]    
    },
    {
        id : "post2",
        author : {username : "user2", name : "User 2"},
        body : "......",
        comments : [
            {
                id : "comment3",
                author : {username : "user3", name : "User 3"},
                comment : ".....",
            },
            {
                id : "comment4",
                author : {username : "user1", name : "User 1"},
                comment : ".....",
            },
            {
                id : "comment5",
                author : {username : "user3", name : "User 3"},
                comment : ".....",
            }
        ]    
    }
    // and repeat many times
]
```

请注意，数据的结构有点复杂，有些数据会重复。出于以下几个原因，这是一个问题：

- 当某个数据在多个地方复制时，确保更新得当的难度会更大。

- 嵌套数据意味着相应的还原器逻辑必须嵌套或更复杂。特别是，尝试更新深度嵌套的字段可能非常快速变得非常慢。

- 由于不可变数据更新要求复制和更新状态树中的所有先例，并且新对象引用将导致连接的UI组件重新呈现，因此对深度嵌套数据对象的更新可能会迫使完全不相关的UI组件重新呈现，即使他们正在显示的数据实际上没有改变，也会呈现。

因此，在 Redux 商店中管理关系数据或嵌套数据的推荐方法是将商店的一部分视为数据库，并将数据保持为**规范化**形式。

## 设计归一化状态

规范化数据的基本概念是：

- 每种类型的数据在状态中都有自己的“表格”。

- 每个“数据表”应该将各个项目存储在一个对象中，其中项目的ID作为键和项目本身作为值。

- 任何对单个项目的引用都应该通过存储该项目的ID来完成。

- 应使用ID数组来表示排序。

上述博客示例的规范化状态结构示例可能如下所示：

```javascript
{
    posts : {
        byId : {
            "post1" : {
                id : "post1",
                author : "user1",
                body : "......",
                comments : ["comment1", "comment2"]    
            },
            "post2" : {
                id : "post2",
                author : "user2",
                body : "......",
                comments : ["comment3", "comment4", "comment5"]    
            }
        }
        allIds : ["post1", "post2"]
    },
    comments : {
        byId : {
            "comment1" : {
                id : "comment1",
                author : "user2",
                comment : ".....",
            },
            "comment2" : {
                id : "comment2",
                author : "user3",
                comment : ".....",
            },
            "comment3" : {
                id : "comment3",
                author : "user3",
                comment : ".....",
            },
            "comment4" : {
                id : "comment4",
                author : "user1",
                comment : ".....",
            },
            "comment5" : {
                id : "comment5",
                author : "user3",
                comment : ".....",
            },
        },
        allIds : ["comment1", "comment2", "comment3", "commment4", "comment5"]
    },
    users : {
        byId : {
            "user1" : {
                username : "user1",
                name : "User 1",
            }
            "user2" : {
                username : "user2",
                name : "User 2",
            }
            "user3" : {
                username : "user3",
                name : "User 3",
            }
        },
        allIds : ["user1", "user2", "user3"]
    }
}
```

这种状态结构总体上更平坦。与原始嵌套格式相比，这在几方面有所改进：

- 因为每个项目只在一个地方定义，所以如果该项目更新，我们不必尝试在多个地方进行更改。

- Reducer 逻辑不必处理深层次的嵌套，所以它可能会简单得多。

- 检索或更新给定项目的逻辑现在相当简单且一致。给定一个项目的类型和ID，我们可以直接在几个简单的步骤中查找它，而无需挖掘其他对象来找到它。

- 由于每种数据类型都是分开的，因此像更改注释文本这样的更新只需要树的“注释> byId>注释”部分的新副本。这通常意味着需要更新的UI部分更少，因为它们的数据已更改。相比之下，更新原始嵌套形状中的注释需要更新注释对象，父级对象，所有后对象的数组，并可能导致UI中的**所有** Post组件和注释组件重新呈现他们自己。

请注意，规范化的状态结构通常意味着连接了更多的组件，并且每个组件都负责查找自己的数据，而不是一些连接的组件查找大量数据并向下传递所有数据。事实证明，连接父组件只需将项ID传递给连接的子项就是在 React Redux 应用程序中优化UI性能的一种很好的模式，因此保持状态规范化对提高性能起着关键作用。

## 在状态中组织规范化的数据

典型的应用程序可能会混合使用关系数据和非关系数据。虽然没有单一的规则来确定如何组织这些不同类型的数据，但一种常见的模式是将关系“表”置于共同的父key之下，例如“实体”。使用这种方法的状态结构可能如下所示：

```javascript
{
    simpleDomainData1: {....},
    simpleDomainData2: {....}
    entities : {
        entityType1 : {....},
        entityType2 : {....}
    }
    ui : {
        uiSection1 : {....},
        uiSection2 : {....}
    }
}
```

这可以通过多种方式进行扩展。例如，执行大量实体编辑的应用程序可能希望在状态中保留两组“表”，一个用于“当前”项目值，另一个用于“正在进行中”项目值。当一个项目被编辑时，它的值可以被复制到“进行中的工作”部分，并且任何更新它的动作都将被应用到“正在进行中的”副本中，从而允许编辑表单被控制该数据集合，而UI的另一部分仍然指向原始版本。“重置”编辑表格只需要从“正在进行中”部分移除项目并将原始数据从“当前”重新复制到“正在进行中”。

## 关系和表格

因为我们将 Redux 商店的一部分视为“数据库”，因此数据库设计的许多原理也适用于此。例如，如果我们有多对多关系，则可以使用存储相应项目（通常称为“连接表”或“关联表”）的ID的中间表进行建模。为了保持一致性，我们可能也想用同样的`byId`和`allIds`我们用实际的项目表中，这样的做法如下：

```javascript
{
    entities: {
        authors : { byId : {}, allIds : [] },
        books : { byId : {}, allIds : [] },
        authorBook : {
            byId : {
                1 : {
                    id : 1,
                    authorId : 5,
                    bookId : 22
                },
                2 : {
                    id : 2,
                    authorId : 5,
                    bookId : 15,
                }
                3 : {
                    id : 3,
                    authorId : 42,
                    bookId : 12
                }
            },
            allIds : [1, 2, 3]

        }
    }
}
```

像“查找本作者的所有书籍”这样的操作可以通过连接表上的单个循环来完成。鉴于客户端应用程序中的典型数据量和 JavaScript 引擎的速度，这种操作对于大多数使用情况来说可能具有足够快的性能。

## 规范化嵌套数据

由于API经常以嵌套形式发回数据，因此需要将数据转换为标准化形状，才能将其包含在状态树中。该 [Normalizr ](https://github.com/paularmstrong/normalizr)库通常用于此任务。您可以定义模式类型和关系，将模式和响应数据提供给 Normalizr，并且它将输出归一化的响应转换。该输出可以包含在一个动作中并用于更新商店。有关其用法的更多详细信息，请参阅 Normalizr 文档。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18