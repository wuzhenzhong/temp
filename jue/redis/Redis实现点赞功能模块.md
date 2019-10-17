# Redis实现点赞功能模块

> 之前看了一篇文章，讲redis的应用场景，其中一个应用场景就是实现点赞功能，纸上得来恐觉浅，必须实战一波

### 功能点设计

比如我喜欢发文章的掘金网站就有点赞的功能，统计文章点赞的总数，用户所有文章的点赞数，因此设计的点赞功能模块具有以下功能点：

- 某篇文章的点赞数
- 用户所有文章的点赞数
- 用户点赞的文章
- 持久化到`MySQL`数据库

### 数据库设计

- Redis数据库设计 `Redis`是`K-V`数据库，没有统一的数据结构，针对不同的功能点设计了不同的`K-V`存储结构

  - 用户某篇文章的点赞数 使用`HashMap`数据结构，`HashMap`中的`key`为`articleId`，`value`为`Set`，`Set`中的值为用户`ID`，即`HashMap<String, Set<String>>`
  - 用户总的点赞数 使用`HashMap`数据结构，`HashMap`中的`key`为`userId`，`value`为`String`记录总的点赞数
  - 用户点赞的文章 使用`HashMap`数据结构，`HashMap`中的`key`为`userId`，`value`为`Set`，`Set`中的值为文章`ID`，即`HashMap<String, Set<String>>`

- MySQL数据库设计 最主要的两张表，`article`表和`user_like_article`

  - `article`表结构

  |      字段值      | 字段类型 |     说明     |
  | :--------------: | :------: | :----------: |
  |   article_name   | varchar  |   文章名字   |
  |     content      |   blob   |   文章内容   |
  | total_like_count |  bigint  | 文章总点赞数 |

  文章总的点赞数需要和`Redis`中的点赞数进行同步

  - `user_like_article`表结构

  |   字段值   | 字段类型 |  说明  |
  | :--------: | :------: | :----: |
  |  user_id   |  bigint  | 用户ID |
  | article_id |  bigint  | 文章ID |

  记录用户点赞文章的信息，是一张中间表

说明：表结构设计省略了`id`、`deleted`、`gmt_create`、`gmt_modified`字段

### 流程图



![img](https://user-gold-cdn.xitu.io/2019/10/15/16dcf30ae94fdb6b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



流程图比较简单，点赞和取消点赞基本实现步骤相同

- 参数校验 对传入的参数进行`null`值判断
- 逻辑校验 对于用户点赞，用户不能重复点赞相同的文章 对于取消点赞，用户不能取消未点赞的文章
- 存入`Redis` 存入的数据主要有所有文章的点赞数、某篇文章的点赞数、用户点赞的文章
- 定时任务 通过定时【1小时执行一次】，从`Redis`读取数据持久化到`MySQL`中

### 代码功能实现

- 点赞

```
public void likeArticle(Long articleId, Long likedUserId, Long likedPostId) {
    validateParam(articleId, likedUserId, likedPostId);  //参数验证

    logger.info("点赞数据存入redis开始，articleId:{}，likedUserId:{}，likedPostId:{}", articleId, likedUserId, likedPostId);
    synchronized (this) {
        //只有未点赞的用户才可以进行点赞
        likeArticleLogicValidate(articleId, likedUserId, likedPostId);
        //1.用户总点赞数+1
        redisTemplate.opsForHash().increment(TOTAL_LIKE_COUNT_KEY, String.valueOf(likedUserId), 1);

        //2.用户喜欢的文章+1
        String userLikeResult = (String) redisTemplate.opsForHash().get(USER_LIKE_ARTICLE_KEY, String.valueOf(likedPostId));
        Set<Long> articleIdSet = userLikeResult == null ? new HashSet<>() : FastjsonUtil.deserializeToSet(userLikeResult, Long.class);
            articleIdSet.add(articleId);
        redisTemplate.opsForHash().put(USER_LIKE_ARTICLE_KEY, String.valueOf(likedPostId), FastjsonUtil.serialize(articleIdSet));

        //3.文章点赞数+1
        String articleLikedResult = (String) redisTemplate.opsForHash().get(ARTICLE_LIKED_USER_KEY, String.valueOf(articleId));
        Set<Long> likePostIdSet = articleLikedResult == null ? new HashSet<>() : FastjsonUtil.deserializeToSet(articleLikedResult, Long.class);
        likePostIdSet.add(likedPostId);
        redisTemplate.opsForHash().put(ARTICLE_LIKED_USER_KEY, String.valueOf(articleId), FastjsonUtil.serialize(likePostIdSet));
        logger.info("取消点赞数据存入redis结束，articleId:{}，likedUserId:{}，likedPostId:{}", articleId, likedUserId, likedPostId);
    }
}
复制代码
```

- 取消点赞

```
public void unlikeArticle(Long articleId, Long likedUserId, Long likedPostId) {
    validateParam(articleId, likedUserId, likedPostId);  //参数校验

    logger.info("取消点赞数据存入redis开始，articleId:{}，likedUserId:{}，likedPostId:{}", articleId, likedUserId, likedPostId);
    //1.用户总点赞数-1
    synchronized (this) {
        //只有点赞的用户才可以取消点赞
        unlikeArticleLogicValidate(articleId, likedUserId, likedPostId);
        Long totalLikeCount = Long.parseLong((String)redisTemplate.opsForHash().get(TOTAL_LIKE_COUNT_KEY, String.valueOf(likedUserId)));
         redisTemplate.opsForHash().put(TOTAL_LIKE_COUNT_KEY, String.valueOf(likedUserId), String.valueOf(--totalLikeCount));

        //2.用户喜欢的文章-1
        String userLikeResult = (String) redisTemplate.opsForHash().get(USER_LIKE_ARTICLE_KEY, String.valueOf(likedPostId));
        Set<Long> articleIdSet = FastjsonUtil.deserializeToSet(userLikeResult, Long.class);
        articleIdSet.remove(articleId);
        redisTemplate.opsForHash().put(USER_LIKE_ARTICLE_KEY, String.valueOf(likedPostId), FastjsonUtil.serialize(articleIdSet));

        //3.取消用户某篇文章的点赞数
        String articleLikedResult = (String) redisTemplate.opsForHash().get(ARTICLE_LIKED_USER_KEY, String.valueOf(articleId));
        Set<Long> likePostIdSet = FastjsonUtil.deserializeToSet(articleLikedResult, Long.class);
        likePostIdSet.remove(likedPostId);
        redisTemplate.opsForHash().put(ARTICLE_LIKED_USER_KEY, String.valueOf(articleId), FastjsonUtil.serialize(likePostIdSet));
    }

    logger.info("取消点赞数据存入redis结束，articleId:{}，likedUserId:{}，likedPostId:{}", articleId, likedUserId, likedPostId);
}
复制代码
```

- 异步落库

```
@Scheduled(cron = "0 0 0/1 * * ? ")
public void redisDataToMySQL() {
    logger.info("time:{}，开始执行Redis数据持久化到MySQL任务", LocalDateTime.now().format(formatter));
    //1.更新文章总的点赞数
    Map<String, String> articleCountMap = redisTemplate.opsForHash().entries(ARTICLE_LIKED_USER_KEY);
    for (Map.Entry<String, String> entry : articleCountMap.entrySet()) {
        String articleId = entry.getKey();
        Set<Long> userIdSet = FastjsonUtil.deserializeToSet(entry.getValue(), Long.class);
        //1.同步某篇文章总的点赞数到MySQL
        synchronizeTotalLikeCount(articleId, userIdSet);
        //2.同步用户喜欢的文章
        synchronizeUserLikeArticle(articleId, userIdSet);
    }
    logger.info("time:{}，结束执行Redis数据持久化到MySQL任务", LocalDateTime.now().format(formatter));
}
复制代码
```

说明：

- 针对存在并发的问题，通过添加`synchronize`关键字实现
- 另外还有获取某篇文章的点赞数、用户所有文章的点赞数、用户点赞的文章方法实现，方法实现比较简单不说明，可以在[完整代码](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2FTiantianUpup%2Flike-article)中找到

### 目前存在的不足

- 用户点赞\取消点赞方法中，`Redis`事务没有保证
- 该应用只适用于单机环境，分布式环境下存在并发问题，分布式锁待完成