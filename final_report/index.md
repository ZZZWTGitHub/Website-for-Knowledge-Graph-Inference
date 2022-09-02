## 小学期开发进度结题汇报

### 完成情况及效果

在上学期项目的基础上重点完成三方面的工作：

- 基于express后端调用子进程，实现胶水功能，以调用python的推荐算法、问答系统
- 在合适的地方，改为使用结构化数据库作为支持，包括数据库设计、具体实施、连接等工作，极大的提升了系统的速度。数据上看，速度提升几十倍左右
- 随着功能的增多，目录结构混乱，设计并调整目录结构，减少不必要的混乱并增强其复用性

另外也有一些零散的优化：

- 优化了原有的antiShake功能，有效防止数据外泄风险
- 修复了Chrome内核浏览器解析出错的问题
- 基于useCallback、useMemo的缓存优化，防止不必要的上层渲染更新所导致的下层组件的reflow
- 基于less后处理语言的CSS代码重构，有效减少了标签的查找次数，优化运行速度

未完成的部分：

- 详情页：此前部分数据保存不善，又由于时间有限，不想做没有什么技术含量的爬虫和静态web构建

### 具体实现

重点说基于express后端调用子进程，以实现胶水功能连接TransE模型这一功能：

问题核心：后端程序运行在Nodejs环境下，这与python编写的推荐算法并不兼容。

那么我们容易想到以下两种解决思路：

- 学习一种基于python的后端，再多开一个端口对推荐算法进行支持
  - 优点：对TransE算法模型的支持很完善
  - 缺点：时间不足，功能耦合
- 使用express的一些中间件，使js作为胶水使express后端链接到推荐算法
  - 优点：低耦合，单一出入口符合软件开发的逻辑
  - 缺点：没有规范，可借鉴资源不足

在诸多调研和尝试之后，我们选择了第二种实现方式，具体解决的思路如下：

- 使用nodejs的中间件spawn，分离出子进程，处理与python程序间的通信问题。
- 对TransE推荐算法包括学习出的模型进行封装，以便系统调用。
- 代码逻辑：
  - 在express程序中，监听到前端发来的axios请求后，获取其参数，
  - 分离出子进程，传入参数，并通过系统调用封装好的TransE算法。
  - 监听系统的标准输入输出流、错误信息流、文件关闭等信号，并分别作出响应和状态变换。

开发中遇到的问题及解决方案：

- 代码移植产生的路径问题，根据环境所需的格式重写部分python代码，用以适应系统调用。
- 系统调用、监听、传参这些通信之间的编码问题，尝试出编码的方式并进行解码与格式规范化处理

可能听起来解决这些问题的过程很轻松，但其实定位问题和不断的尝试需要耗费大量精力。

### 亮点和创新点

基于子进程并监听系统数据流的胶水实现

### 个人总结

可以说，开发对于我来说算是比较得心应手的，尤其是前端开发，这方面的经验算是比较丰富的。

这里面用到的一些前端方面的技术，如缓存优化、CSS后处理语言，都是当前互联网公司比较前沿的技术（有些是在分享会上听到，然后在项目中实践的）。

但是，于我而言更重要的是，当跳出熟悉的技术栈，去尝试解决那些几乎没有人遇到过的问题，没有现成的经验可以参考的时候，这件事就变得很有挑战性，思维活跃，甚至有一点浪漫主义色彩。