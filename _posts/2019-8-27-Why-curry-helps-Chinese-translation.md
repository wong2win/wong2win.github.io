# 柯里(咖喱)有啥用?

[原文链接](https://hughfdjackson.com/javascript/why-curry-helps/) - by Hugh FD Jackson

一个程序员的白日梦就是写一段代码, 然后毫不费劲地反复使用它. 这很生动, 因为你所写的表达了所需要的, 而复用则是因为... 你在复用. 而复用是坠吼的(你还有啥可指望的)...  

柯里可以帮上忙  

## 什么是柯里化, 它怎么就塔玛那么爽了?  

JavaScript中正常的函数调用都大致长这样:  

```Javascript
var add = function(a, b) { return a + b }
add(1, 2) //= 3
```

一个函数接受几个参数, 返回一个值. 我可以少传几个参数(得到一个古怪的结果), 也可以传多了(多的参数通常被忽略):  

```Javascript
add(1 ,2 ,'我没啥用') //= 3
add(1) //= NaN
```

而一个接受多个参数的柯里化的函数则相当于, 连续执行的多个只接受一个参数的函数. 例如, 柯里化的加法会像这样:  

```Javascript
var curry = require('curry')
var add = curry(function(a, b){ return a + b })
var add100 = add(100)
add100(1) //= 101
```

一个接受多个参数的柯里化的函数会被写成:  

```Javascript
var sum3 = curry(function(a, b, c){ return a + b + c })
sum3(1)(2)(3) //= 6
```

因为这么写在JavaScript的语法里有点丑, 柯里也允许你一次调用一个函数, 并传递多个参数:  
```Javascript
var sum3 = curry(function(a, b, c){ return a + b + c })
sum3(1, 2, 3) //= 6
sum3(1)(2, 3) //= 6
sum3(1, 2)(3) //= 6
```

## 那...那又如何?
如果你不熟悉一些日常搞柯里化的语言(立即想到了Haskell), 也许柯里的优势还不会太明显. 而对我而言, 两个主要好处是:  
1. 小的片段可以被很容易地配置和复用, 而且不会看起来乱七八糟的(译者注: 对我而言这里的乱七八糟是指=>);  
2. 从头到尾都在用函数.  

### 小片段
来看个很简单的例子; 用map遍历一个集合获取每一项的id组成的集合:  

```Javascript
var objects = [{ id: 1 }, { id: 2 }, { id: 3 }]
objects.map(function(o){ return o.id })
```

如果有人看不懂第二行的代码, 我就大发慈悲告诉你:  

```
MAP 遍历 OBJECTS 来获取 IDS
```

这短短的一行里有很多屑代码; 以函数定义的形式存在. 我们来清理一下:  

```Javascript
var get = curry(function(property, object){ return object[property] })
objects.map(get('id')) //= [1, 2, 3]
```

现在我们来讨论一下这个操作的真正逻辑 - map遍历这些对象, 获取id. 嘭, 我们在这个get函数中真正创建的是一个**可以被部分配置的函数**.  

那如果我们仍然想重用'从一个对象组成的的数组获取id数组'功能的话该咋整? 先用一种很奈芙的做法来实现一下:  

```Javascript
var getIDs = function(objects){
    return objects.map(get('id'))
}
getIDs(objects) //= [1, 2, 3]
```

emmmm, 我们好像又从简洁优雅回到了乱七八糟. 还能做点什么抢救一下吗? 啊 - 把map也变成部分可用一个函数配置, 而让集合参数再等等怎么样?  

```Javascript
var map = curry(function(fn, value){ return value.map(fn) })
var getIDs = map(get('id'))

getIDs(objects) //= [1, 2, 3]
```

我们可以看到, 如果基本的构造块是柯里化的函数, 我们就可以用它们轻松地创造出崭新的功能. 更令人兴奋的是, 代码读起来也就像你想表达的逻辑那样.  

### 彻头彻尾的函数  

这么写的另一个优势在于它鼓励创造函数; 而不是方法. 虽然方法也很有用 - 可以实现多态, 以及可读性不错的代码 - 可他们也不是银弹, 例如在异步代码中就不太合适.  

在这个简单的例子中, 我们从服务器get数据, 然后稍加处理. 数据大概长这样:  

```Javascript
{
    "user": "hughfdjackson",
    "posts": [
        { "title": "why curry?", "contents": "..." },
        { "title": "prototypes: the short(est possible) story", "contents": "..." }
    ]
}
```

而你的任务是获取每个post的title. GOGOGO!  

```Javascript
fetchFromServer()
    .then(JSON.parse)
    .then(function(data){ return data.posts })
    .then(function(posts){
        return posts.map(function(post){ return post.title })
    })
```

好吧这不太公平, 我有点仓促. (而且代表你写了这段代码, 也许你可以更优雅的解决它, 有点跑题了)  

因为promises( 或者回调, 随你喜欢 )链主要是在与函数打交道, 用map遍历一个服务器来的值, 而不先用一堆屑代码( 看上去和理解时 )把它包裹起来并不容易.

让我们再来一次, 这次用我们在上面已经定义过了的工具:  

```Javascript
fetchFromServer()
    .then(JSON.parse)
    .then(get('posts'))
    .then(map(get('title')))
```

这就是瘦逻辑, 轻松地表达意图; 不用柯里化的函数就没那么容易实现的.

---

其实这例子还不够复杂...  
俺寻思下面这个截取自[另一篇文章](https:fr.umio.us/favouring-curry/)的一个例子可能会更直观一些:  
### 乱七八糟版

```Javascript
getIncompleteTaskSummaries = function(membername) {
    return fetchData()
        .then(function(data) {
            return data.tasks;
        })
        .then(function(tasks) {
            var results = [];
            for (var i = 0, len = tasks.length; i < len; i++) {
                if (tasks[i].username == membername) {
                    results.push(tasks[i]);
                }
            }
            return results;
        })
        .then(function(tasks) {
            var results = [];
            for (var i = 0, len = tasks.length; i < len; i++) {
                if (!tasks[i].complete) {
                    results.push(tasks[i]);
                }
            }
            return results;
        })
        .then(function(tasks) {
            var results = [], task;
            for (var i = 0, len = tasks.length; i < len; i++) {
                task = tasks[i];
                results.push({
                    id: task.id,
                    dueDate: task.dueDate,
                    title: task.title,
                    priority: task.priority
                })
            }
            return results;
        })
        .then(function(tasks) {
            tasks.sort(function(first, second) {
                var a = first.dueDate, b = second.dueDate;
                return a < b ? -1 : a > b ? 1 : 0;
            });
            return tasks;
        });
};
```

=>

### ramda.js版

```Javascript
var getIncompleteTaskSummaries = function(membername) {
    return fetchData()
        .then(R.get('tasks'))
        .then(R.filter(R.propEq('username', membername)))
        .then(R.reject(R.propEq('complete', true)))
        .then(R.map(R.pick(['id', 'dueDate', 'title', 'priority'])))
        .then(R.sortBy(R.get('dueDate')));
};
```

这就很能说明问题了, 不是吗...