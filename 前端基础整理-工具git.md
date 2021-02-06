## 工具-Git
###### 1.git 和 svn的区别
> 1. git 和 svn 最大的区别在于 git 是分布式的，而 svn 是集中式的。因此我们不能再离线的情况下使用 svn。`如果服务器出现问题，我们就没有办法使用 svn 来提交我们的代码`
> 2. svn 中的分支是整个版本库的复制的一份完整目录，而 git 的分支是指针指向某次提交，因此` git 的分支创建更加开销更小`并且分支上的变化不会影响到其他人。svn 的分支变化会影响到
> 3. `svn`相对于 git 来说要`简单`一些，比 git 更容易上手
> 4. [git 和 svn 对比 - 掘金](https://juejin.cn/post/6844903702374023182)
> 5. [git 学习小计 - 分支原理](https://www.jianshu.com/p/e8ad60710017)

###### 2.常见git 命令
```
git init // 新建 git 代码库
git add // 添加指定文件到暂存区
git rm // 删除工作区文件，并且将这次删除放入暂存区
git commit -m [message] // 提交暂存区到仓库区
git branch // 列出所有分支
git checkout -b [branch] // 新建一个分支，并切换到该分支
git status // 显示有变更的文件
```
>[常见 git 命令清单](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)

###### 3.git pull 和 git fetch 的区别   (`从远程仓库更新到本地仓库`)
>1. git fetch 只是将远程仓库的变化下载下来，并没有和本地分支合并。
>2. git pull 会将远程仓库的变化下载下来，并和当前分支合并
>3. `git pull = git fetch + git merge`
>4. [详解git pull 和 git fetch 的区别](https://blog.csdn.net/weixin_41975655/article/details/82887273)

###### 4.git rebase 和 git merge 的区别
>1. git merge 和 git rebase 都是用于分支合并，关键在 commit 记录的处理上不同。
>2. git merge 会新建一个`新的 commit 对象`，然后两个分支以前的 commit 记录都指向这个新 commit记录。这种方法会`保留之前每个分支的 commit 历史。`
>3. git rebase 会先找到两个分支的第一个共同的 commit 祖先记录，然后将提取当前分支这之后的所有commit 记录，然后将这个 commit 记录添加到目标分支的最新提交后面。经过这个合并后，两个分支合并后的 `commit 记录就变为了线性的记录了(线性树--对两个分支的历史有破坏)`
>4. [ 《git merge 与 git rebase 的区别》
>](https://blog.csdn.net/liuxiaoheng1992/article/details/79108233)

(更新 `day-1`，`2021/2/6` )



## 面经
#### 1 浏览器存储缓存机制

> 缓存的数据类型都为**字符串**，通常需将对象**序列化**成字符串。

**一、JS中的三种数据存储方式**

cookie、sessionStorage、localStorage

**二、cookie**   ` 服务端生成，客户端进行维护和存储。(最大 4k)`

**1、cookie的定义：**

cookie是存储在浏览器上的一小段数据，用来记录某些当页面关闭或者刷新后仍然需要记录的信息。在控制台用`document.cookie`可以查看当前正在浏览网站的cookie。

**2、cookie存在安全问题：**

cookie虽然很方便，但是使用cookie有一个很大的弊端，cookie中的所有数据在客户端就可以被修改，数据非常容易被伪造，那么一些重要的数据就不能放在cookie中了，而且如果cookie中数据字段太多会影响传输效率

**3、cookie的作用**

- `保存用户登录状态。`例如将用户id存储于一个cookie内，这样当用户下次访问该页面时就不需要重新登录了，现在很多论坛和社区都提供这样的功能。 `cookie还可以设置过期时间，`当超过时间期限后，cookie就会自动消失。因此，系统往往可以提示用户保持登录状态的时间：常见选项有一个月、三个 月、一年等。
- `跟踪用户行为。`例如一个天气预报网站，能够根据用户选择的地区显示当地的天气情况。如果每次都需要选择所在地是烦琐的，当利用了 cookie后就会显得很人性化了，系统能够记住上一次访问的地区，`当下次再打开该页面时，它就会自动显示上次用户所在地区的天气情况。`因为一切都是在后 台完成，所以这样的页面就像为某个用户所定制的一样，使用起来非常方便
- `定制页面。`如果网站提供了换肤或更换布局的功能，那么可以使用cookie来记录用户的选项，例如：`背景色、分辨率等。`当用户下次访问时，仍然可以保存上一次访问的界面风格。

**手写cookie**

```js
let cookie = {
    //一个cookie的格式为键值对‘key=value; key=value; key=value’
    // 每个cookie以'; '分隔（分号+空格）

    //设置cookie
    // time为有效时间
    set: function(key, val, time) {
        let date = new Date();
        let expiresDays = time;
        date.setTime(date.getTime() + expiresDays * 24 * 3600 * 1000); //计算过期时间
        document.cookie = key + '=' + val + ';expires=' + date.toGMTString();
    },
    // 获取cookie
    get: function(key) {
        let getCookie = document.cookie.replace(/[ ]/g, ""); //去除不同cookie之间的分界空格
        let arrCookie = getCookie.split(";"); //将cookie保存到arrCookie数组中
        let res;
        for (let i = 0; i < arrCookie.length; i++) {
            // 格式： key=value
            let arr = arrCookie[i].split('=');
            if (key === arr[0]) {
                res = arr[1];
                break;
            }
        }
        return res;
    }
}
```



**三、sessionStorage**

当用户用账号和密码登录某个网站后，刷新页面仍然保持登录的状态，服务器如何分辨这次发起请求的用户是刚才登录过的用户呢？这里就是用session保存状态。

用户在输入用户名密码提交给服务器，服务端验证通过后会创建一个session用于记录用户的相关信息，这个session可保存在服务器内存中也可保存在数据库中。

- 创建session后，会把关联的`session_id`通过`setCookie`添加到`http相应头部`
- 浏览器在加载页面时发现响应头部有set-cookie字段，就把这个cookie种到浏览器指定域名下
- 当下次刷新页面时，发送的请求会带上这条cookie，服务端在接收到后根据这个`session_id`来识别用户。

**四、localStorage**

- localStorage是H5本地存储web storage特性的API之一，用于将大量数据`（最大5M）`保存在浏览器中，保存后数据永远存在不会失效过期，`除非用js手动清除。`
- 不参与网络传输
- 一般用于性能优化，可以保存图片、js、css、html模板、大量数据

**五、cookie、sessionStorage、localStorage之间的区别**

**相同点：**都是存储在浏览器端。并且是同源的

**不同点：**

**1、存储大小**

一般浏览器存储`cookie最大容量为4kb`，很多浏览器都限制一个站点最多保存`20个cookie`。

sessionStorage和localStorage虽然也有存储大小的限制，但是要比cookie大得多。

**`2、存储有效期：`**

cookie保存的数据在过期时间之前一直有效，即使关闭浏览器

`sessionStorage`保存的数据用于浏览器的一次会话（session），当会话结束（通常是窗口关闭），数据被清空；

`localStorage`保存的数据长期存在，因为localStorage的数据是写入磁盘中，每次读取数据时，实际上是从硬盘驱动器上读取这些字节。

**3、安全性**

cookie中的所有数据在客户端就可以被修改，数据非常容易被伪造，所以对于用户信息来讲，应该将与登录相关的重要信息存储在session中。

**4、性能**

如果cookie中数据字段太多的话，会影响传输效率，此时需要将重要的信息存放在session中。
但是，session会在一定的时间内保存在服务器上，当访问增多时，会影响服务器的性能。为了减轻服务器的负担，应当使用cookie。

(更新 `day-1`，`2021/2/6` )