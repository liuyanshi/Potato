
##项目相关
1. 启动方式: node app.js 或 supervisor app.js
2. 数据库信息

----
##学习过程
###对express-session 和 connect-mongo的解释
> 1. 对极客学院Wiki-PDF(Page30)添加更多注释
> 2. 一部分有来自CNode社区

1. connect-mongo和connect-redis都是为express-session写的扩展;
2. express-session是express中负责处理session的中间件,本来session是存储在内存中,现在需要将其持久化存储在数据库或redis或者memcache中
3. 存储在不同的持久化介质中就需要有对应不同的扩展,就像java-connector-mysql,java-connector-mongoDB,java-connector-Sql Server一样的.相当于是桥梁一样的角色
4. express-session的store实例有
>1. store.destory(sessionId, callback);销毁
 2. store.get(sessionId, callback); 取
 3. store.set(sessionId, session, callback);设置(也就是存储)
   - 不同的是connect-mongo把session存储在了MongoDB中.connect-redis把session存储在了redis中
   - 把session存储在持久化介质中可以让session更加稳定,持久化一些.默认express-session是把session存储在内存中的,后果是程序一旦崩溃,session就丢失了
5. TODO 2016年01月31日23:01:54 存储在内存中,在程序崩溃后session丢失会有什么后果,举例说明
   1. 猜测: 程序若是突然崩溃,那么用户立马就会掉线,用户刷新页面后需要重新登录,因此此时客户端发送到服务器端的cookie没有办法找到其唯一标识了,因此需要重新登录
   2. 猜测: 扇贝的Android客户端每过一段时间都会掉线,账户密码被要求重新输入进行登录是否是因为session失效的原因?
   3. 猜测: 比如浏览器在浏览淘宝时候,半小时内不操作,再次操作会要求输入密码重新登录是因为cookie失效还是session失效?(session和cookie的关系是什么?)
 
 
##应用场景
### `||` 
1.res.status(err.status || 500);
> 在`routes/error.js`中分别对404和500错误进行了处理,如果没有出现404错误直接出现500错误,那么err.status为undefined,则`res.status(err.status || 500)`为500

    ```javascript
    /*
    || 操作符号表示或者符号,当前面一个变量没有赋值,也就是undefined的时候,其值为false,采用后面一个值
    举例:
    var x = undefined || 12; //x = 12;
    var y = 12 || 13; // x= 13
    var z = 0 || 13;  // x=13;
    */
    // 具体场景:
    res.status(err.status || 500);
    ```
    
    
##QA
1. Page35 中的flash模块实现页面通知是什么机制和socket.io有关系么?
2. 登录注册成功之后,怎么执行之前未完成的动作?
3. session 怎么利用express-session以及connect-mongo两个中间件存储到数据库?
>1. 以及注册登录之后的session怎么存储到数据库中
>2. app.use(session{secret,key,cookie,store});以及req.session.user=user;是怎么把session存储到数据库中的!
