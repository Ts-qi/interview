### 一 从输入 url 到浏览器显示页面，中间经历了什么过程？

>  会先查找当前的 URL 是否存在缓存（涉及到浏览器缓存，路由缓存，DNS 缓存等），并比较当缓存是否过期（用本地Last-modified时间与if-modified-since时间比较，如果不一致则认为缓存已过期并返回新资源给浏览器；如果时间一致则发送304状态码，让浏览器继续使用缓存），在利用 DNS 进行 URL 所对应的 IP，在根据 IP 建立TCP 连接（涉及到三次握手），连接中HTTP进行资源请求，服务器得到相应的请求，并返回相应的数据，浏览器接收服务器返回的数据，在利用浏览器相应的引擎进行页面的渲染，最后在关闭 TCP 连接（四次挥手!

##### 1 涉及到的优化的点？

> http:  1 减少请求的次数；2 减少单次请求所花费的时间；

> 缓存： 保证时间不会失效---失效重新请求资源，否则利用缓存；

> 浏览器解析：1  重绘，重排 -- IFC ，BFC 等;2 js 相关优化

​						

##### 2 服务器返回html字符串后，浏览器解析经历了哪些事件/周期？

> 浏览器解析原理:  1 处理 HTML 构建 DOM 树；   
>
> ​								2 处理测css ，构建 cssDom 树
>
> ​								3 将CSSDOM 树+ DOM 树 = 渲染树；
>
> ​								4 根据渲染树布局，计算每个节点的位置，
>
> ​								5 调用 GPU 绘制，合成图层，显示在屏幕上；

​							

​						



##### 3上述事件触发时机/顺序，以及哪些事件可以用于优化指标和性能监控？

> HTTP 请求中进行相应的优化；TCP 的连接可以进行一个性能检测

##### 4 中间涉及到哪些缓存环节？强缓存和协商缓存是什么？试用场景？如何启用？

> 1  进行 HTTP 请求时： 涉及HTTP cache ：

> 2 强缓存： 利用 HTTP 头中的 Expires 和 Cache-Control 两个字段控制，强缓存中，当请求再次发出的时候，浏览器会根据这两个字段判断目标资源是否命中强缓存； 若有，自己取出，反之 进行服务端通信；

> 协商缓存： 向服务器发送请求，会根据这个请求中的request  header 的一些参数判断是否命中协商缓存，如果命中，则返回 304 状态码并带上新的 response header 通知浏览器才从缓存中读取资源；需要与 cache-control 共同使用；

> 注意： 优先级：强缓存 >  协商缓存；
>
> ​			共同点：都是从客户端缓存中读取资源；
>
> ​			不同点： 强缓存不会发送请求，协商缓存会发送请求；



常见用户行为对浏览器缓存的影响： 

> 1 地址栏访问：将会触发浏览器的缓存；

> 2 F5 刷新，浏览器会设置 max-age = 0; 以此跳过强缓存，会进行协商缓存；

> 3 ctrl+F5 ,跳过强缓存和协商缓存，直接从服务器拉取资源；

> 适合场景： 。。。。？

> 启用：。？

## 二 浏览器的HTML/CSS/JS解析过程是怎样的？

#### 1 解析过程

#####  浏览器内核中主要线程：

#####  1 GUI 渲染线程

> a   主要负责页面的渲染，解析HTML，css, 构建 DOM 树，布局和绘制等；

> B 当界面有重绘 或者有某种操作引发回流时，执行该线程；

> c 该线程和 js 引擎线程是相互排斥的，当该线程执行时，js 线程会被挂起；



###### 2 js 引擎线程； 

> a 处理 js 脚本；

> b 也主要负责执行准备好待执行的事件，即当定时器计数结束，或者异步请求成功正确返回时，将以此进入任务队列，等待 js 引擎线程的执行；

> c 该线程和 GUI 线程互斥；当 js 引擎线程执行 JavaScript 脚本时间过长，将导致页面的阻塞；

###### 3 事件触发线程；

> 事主要负责将准备好的时间交给 js 引擎线程执行； 比如定时器，ajax 等异步请求成功并触发的回调函数，或者点击事件，该线程会将这些事件以此加入到任务队列的队尾，等待 js 引擎线程的执行；

###### 4 定时器触发线程

> 1 负责执行异步定时器一类的函数的线程： 比如： setTimeout , setInterval.

> 2 主线程一次执行代码时，遇到定时器，会将定时器交给该线程处理，当计数完毕后，事件触发线程会将计数完毕后的事件加入到任务队列的尾部，等待 js 引擎线程的执行；

###### 5 HTTP 请求线程；

> 1 负责执行异步请求一类的函数的线程： 比如： Promise , axios,ajax 等.

> 2 主线程一次执行代码时，遇到异步请求，会将定函数交给该线程处理，当监听状态码变更，事如果有回调函数，事件触发线程加入到任务队列的尾部，等待 js 引擎线程的执行；

###### 



浏览器解析过程: 先利用 GUI 渲染线程进行 HTML ，css解析，并构建 DOM 树，在根据渲染树布局，计算节点位置，最后渲染到页面上；在利用 js引擎线程 ，进行脚本解析，在解析后会将其转换为 AST，基于 AST，解释器便可以开始工作并产生字节码，此时就开始执行 JavaScript 代码，最后加载出来；

![clipboard.png](https://segmentfault.com/img/bVRm39?w=624&h=289)

#### 

同行另一个大佬整理：https://www.zybuluo.com/kodo/note/1619267