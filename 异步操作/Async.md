https://blog.csdn.net/weixin_42358083/article/details/126618433
https://hackernoon.com/lang/zh/%E7%BB%9F%E4%B8%80%E5%BC%80%E5%8F%91%E7%9A%84%E5%AE%8C%E6%95%B4%E5%BC%82%E6%AD%A5%E7%BC%96%E7%A8%8B%E5%85%A5%E9%97%A8
https://blog.zhulegend.com/blog/unity%E4%B8%AD%E5%AF%B9await%E7%9A%84%E6%94%AF%E6%8C%81?utm_source=chatgpt.com
async await
协程在一些情况下已经足够好了，但是，通过协程执行函数存在一个较大的问题——无法获取异步的返回值。而在部分情况下，返回值是必要的，例如，从 API 异步的获取数据、从数据库载入数据等。**使用 `Await` ，可以运行你获取异步执行的返回值，同时以近似同步的方式编写异步代码，自由且简单的在主线程和后台线程间切换任务**。

`async/await` 的使用可能会引入额外的内存开销，因为每个异步方法都会生成一个状态机对象

只要方法内部要等待别的异步任务，就一定要 `await`，那么这个方法也必须加 `async`。例如：
```cs
async Awaitable<List<Achievement>> GetAchivementsAsync()
{
    var apiResult = await SomeMethodReturningATask(); // 或者其他可等待类型
    JsonConvert.DeserializeObject<List<Achievement>>(apiResult);
}

async Awaitable ShowAchievementsView()
{
    ShowLoadingOverlay();
    var achievements = await GetAchievementsAsync();
    HideLoadingOverlay();
    ShowAchivementsList(achievements);
}
```
这里GetAchivementsAsync()也必须是async，因为使用了`await SomeMethodReturningATask()`，而如果不加await：
```cs
Awaitable<List<Achievement>> GetAchivementsAsync()
{
    var apiResult = SomeMethodReturningATask(); // 或者其他可等待类型
    JsonConvert.DeserializeObject<List<Achievement>>(apiResult);
}
```
则会出问题：`SomeMethodReturningATask()` 返回的是一个**Task**（或者 `Awaitable`），如果不用await等待完成的话`apiResult` 实际上拿到的是一个「未完成的异步任务对象」，而不是最终的结果（比如字符串或JSON）。

Awaitable
Task