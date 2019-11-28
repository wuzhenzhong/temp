# 管理规范化数据( Managing Normalized Data )

- 贡献者1人

  

正如标准化状态形状中所述，Normalizr 库经常用于将嵌套响应数据转换为适合集成到商店中的标准化形状。但是，这并没有解决执行对该规范化数据的进一步更新的问题，因为它正在应用程序的其他地方使用。根据自己的喜好，可以使用各种不同的方法。我们将使用向帖子添加新评论的示例。

## 标准方法

### 简单合并

一种方法是将动作的内容合并到现有状态中。在这种情况下，我们需要进行深度递归合并，而不仅仅是浅拷贝。Lodash 的 `merge`函数可以为我们处理这个问题：

```javascript
import merge from "lodash/merge";

function commentsById(state = {}, action) {
    switch(action.type) {
        default : {
           if(action.entities && action.entities.comments) {
               return merge({}, state, action.entities.comments.byId);
           }
           return state;
        }
    }
}
```

这需要 reducer 方面的工作量最少，但确实需要动作创建者可能会做相当多的工作，以在动作分派之前将数据组织为正确的形状。它也不处理试图删除一个项目。

### 切片**Reducer**组成

如果我们有切片缩减器的嵌套树，则每个切片 reducer 都需要知道如何适当地响应此操作。我们需要在行动中包含所有相关数据。我们需要用评论 ID 更新正确的 Post 对象，使用该 ID 作为关键字创建一个新的评论对象，并将评论 ID 包含在所有评论 ID 列表中。以下是关于这件作品可能如何组合在一起的方法：

```javascript
// actions.js
function addComment(postId, commentText) {
    // Generate a unique ID for this comment
    const commentId = generateId("comment");

    return {
        type : "ADD_COMMENT",
        payload : {
            postId,
            commentId,
            commentText
        }
    };
}


// reducers/posts.js
function addComment(state, action) {
    const {payload} = action;
    const {postId, commentId} = payload;

    // Look up the correct post, to simplify the rest of the code
    const post = state[postId];

    return {
        ...state,
        // Update our Post object with a new "comments" array
        [postId] : {
             ...post,
             comments : post.comments.concat(commentId)             
        }
    };
}

function postsById(state = {}, action) {
    switch(action.type) {
        case "ADD_COMMENT" : return addComment(state, action);
        default : return state;
    }
}

function allPosts(state = [], action) {
    // omitted - no work to be done for this example
}

const postsReducer = combineReducers({
    byId : postsById,
    allIds : allPosts
});


// reducers/comments.js
function addCommentEntry(state, action) {
    const {payload} = action;
    const {commentId, commentText} = payload;

    // Create our new Comment object
    const comment = {id : commentId, text : commentText};

    // Insert the new Comment object into the updated lookup table
    return {
        ...state,
        [commentId] : comment
    };
}

function commentsById(state = {}, action) {
    switch(action.type) {
        case "ADD_COMMENT" : return addCommentEntry(state, action);
        default : return state;
    }
}


function addCommentId(state, action) {
    const {payload} = action;
    const {commentId} = payload;
    // Just append the new Comment's ID to the list of all IDs
    return state.concat(commentId);
}

function allComments(state = [], action) {
    switch(action.type) {
        case "ADD_COMMENT" : return addCommentId(state, action);
        default : return state;
    }
}

const commentsReducer = combineReducers({
    byId : commentsById,
    allIds : allComments
});
```

这个例子有点长，因为它展示了所有不同的切片减速器和 reducer 如何配合在一起。请注意这里涉及的代表团。`postsById`切片 reducer 为代表这种情况下工作`addComment`，这将插入新的评论的 ID 为正确的 Post 物品。同时，`commentsById`reducer 和`allComments`reducer 都有自己的大小写 reducer，它们会更新评论查找表和所有评论ID的列表。

## 其他方法

### 基于任务的更新

由于 reducer 只是函数，所以有无数的方法来分解这个逻辑。虽然使用切片 reducer 显然是最常见的，但也可以在更多以任务为导向的结构中组织行为。因为这通常会涉及更多的嵌套更新，所以您可能需要使用不可变更新实用程序库（如[dot-prop-immutable](https://github.com/debitoor/dot-prop-immutable)或[object-path-immutable）](https://github.com/mariocasciaro/object-path-immutable)来简化更新语句。以下是可能的样例：

```javascript
import posts from "./postsReducer";
import comments from "./commentsReducer";
import dotProp from "dot-prop-immutable";
import {combineReducers} from "redux";
import reduceReducers from "reduce-reducers";

const combinedReducer = combineReducers({
    posts,
    comments
});


function addComment(state, action) {
    const {payload} = action;
    const {postId, commentId, commentText} = payload;

    // State here is the entire combined state
    const updatedWithPostState = dotProp.set(
        state, 
        `posts.byId.${postId}.comments`, 
        comments => comments.concat(commentId)
    );

    const updatedWithCommentsTable = dotProp.set(
        updatedWithPostState, 
        `comments.byId.${commentId}`,
        {id : commentId, text : commentText}
    );

    const updatedWithCommentsList = dotProp.set(
        updatedWithCommentsTable,
        `comments.allIds`,
        allIds => allIds.concat(commentId);
    );

    return updatedWithCommentsList;
}

const featureReducers = createReducer({}, {
    ADD_COMMENT : addComment,
};

const rootReducer = reduceReducers(
    combinedReducer,
    featureReducers
);
```

这种方法非常清楚这种情况发生了什么`"ADD_COMMENTS"`，但它确实需要嵌套的更新逻辑以及状态树形状的一些特定知识。根据你想要编写你的 reducer 逻辑的方式，这可能会也可能不需要。

### 终极版-ORM

[终极版-ORM ](https://github.com/tommikaikkonen/redux-orm)库提供用于在终极版商店管理标准化数据一个非常有用的抽象层。它允许你声明 Model 类并定义它们之间的关系。然后，它可以为您的数据类型生成空的“表”，作为查找数据的专用选择器工具，并对该数据执行不可变的更新。

有几种方法可以使用 Redux-ORM 执行更新。首先，Redux-ORM 文档建议在每个 Model 子类上定义 Reducer 函数，然后在自己的商店中包含自动生成的组合 reducer 函数：

```javascript
// models.js
import {Model, many, Schema} from "redux-orm";

export class Post extends Model {
  static get fields() {
    return {
      // Define a many-sided relation - one Post can have many Comments, 
      // at a field named "comments"
      comments : many("Comment") 
    };
  }

  static reducer(state, action, Post) {
    switch(action.type) {
      case "CREATE_POST" : {
        // Queue up the creation of a Post instance
        Post.create(action.payload);
        break;
      }
      case "ADD_COMMENT" : {
        const {payload} = action;
        const {postId, commentId} = payload;
        // Queue up the addition of a relation between this Comment ID 
        // and this Post instance
        Post.withId(postId).comments.add(commentId);
        break;
      }
    }

    // Redux-ORM will automatically apply queued updates after this returns
  }
}
Post.modelName = "Post";

export class Comment extends Model {
  static get fields() {
    return {};
  }

  static reducer(state, action, Comment) {
    switch(action.type) {
      case "ADD_COMMENT" : {
        const {payload} = action;
        const {commentId, commentText} = payload;

        // Queue up the creation of a Comment instance
        Comment.create({id : commentId, text : commentText});
        break;
      }   
    }

    // Redux-ORM will automatically apply queued updates after this returns
  }
}
Comment.modelName = "Comment";

// Create a Schema instance, and hook up the Post and Comment models
export const schema = new Schema();
schema.register(Post, Comment);


// main.js
import { createStore, combineReducers } from 'redux'
import {schema} from "./models";

const rootReducer = combineReducers({
  // Insert the auto-generated Redux-ORM reducer.  This will
  // initialize our model "tables", and hook up the reducer
  // logic we defined on each Model subclass
  entities : schema.reducer()
});

// Dispatch an action to create a Post instance
store.dispatch({
  type : "CREATE_POST",
  payload : {
    id : 1,
    name : "Test Post Please Ignore" 
  }
});

// Dispatch an action to create a Comment instance as a child of that Post
store.dispatch({
  type : "ADD_COMMENT",
  payload : {
    postId : 1,
    commentId : 123,
    commentText : "This is a comment"
  }
});
```

Redux-ORM 库维护要应用的内部更新队列。这些更新然后不断地应用，简化了更新过程。

另一个变体是在单个案例缩减器中使用 Redux-ORM 作为抽象层：

```javascript
import {schema} from "./models";

// Assume this case reducer is being used in our "entities" slice reducer, 
// and we do not have reducers defined on our Redux-ORM Model subclasses
function addComment(entitiesState, action) {
    const session = schema.from(entitiesState);
    const {Post, Comment} = session;
    const {payload} = action;
    const {postId, commentId, commentText} = payload;

    const post = Post.withId(postId);
    post.comments.add(commentId);

    Comment.create({id : commentId, text : commentText});

    return session.reduce();
}
```

总的来说，Redux-ORM 提供了一组非常有用的抽象概念，用于定义数据类型之间的关系，在我们的状态下创建“表”，检索和反规范化关系数据以及将不可变更新应用于关系数据。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18