### [函数式编程总结](#1)
- [认识函数式编程](#1.1)
- [函数相关复习](#1.2)
    - [函数是一等公民](#1.2.1)
    - [高阶函数](#1.2.2)
    - [闭包](#1.2.3)
- [函数式编程基础](#1.3)
    - [lodash](#1.3.1)
    - [纯函数](#1.3.2)
    - [柯里化](#1.3.3)
    - [管道](#1.3.4)
    - [函数组合](#1.3.5)
- [函子](#1.4)
    - [Functor](#1.4.1)
    - [MayBe](#1.4.2)
    - [Either](#1.4.3)
    - [IO](#1.4.4)
    - [folktale 函子库](#1.4.5)
        - [Task](#1.4.5.1)
    - [Monad](#1.4.6)
### <a id="1">第一节 函数式编程范式</a>
#### <a id="1.1">拥抱函数式编程</a>
- 可以抛弃this指针
- 方便测试、方便处理
#### 什么是函数式编程
- 给定一个因得到一个果
- 将这份因果关系映射到我们的代码中 做好关联
- 关联做好后，这个函数存在我们代码中
#### <a id="1.2.1">函数是一等公民</a>
- 函数可以存储在变量中
- 函数作为参数
- 函数作为返回值
- 把函数赋值给变量 
#### <a id="1.2.2">高阶函数</a>
- 可以在函数的参数里面传入一个函数（函数作为参数）
   - 优点：
     - 函数有实际意义不需要考虑里面的实现
- 也可以将函数作为返回的结果 
   - 优点：
     - 可以包装原有函数 为原来函数加一些新的筛选功能
类似用装饰器的效果
#### 高阶函数的意义：
- 抽象帮我们屏蔽了一些细节 只需要关注目标（节流，防抖）
- 高阶函数是用来抽象通用的问题 
- 通过一个函数传个另一个函数 可以变得更灵活
#### <a id="1.2.3">闭包：</a>
- 可以在一个函数中访问另外一个函数的作用域内的变量
#### 闭包的本质
- 函数在执行时候会放在执行栈上，执行完就会在被移除。但是堆上的作用域
 成员因为受到外部的引用所以不会移除 因此内部函数还是可以访问得到
#### <a id="1.3.1">Lodash相关：</a>
- 是一个纯函数的库
#### <a id="1.3.2">纯函数：</a>
- 相同的输入只会得到相同的输出 例如lodash就是纯函数库
#### 不纯的函数：
- 相同输入不一定是相同输出
#### 纯函数好处：
- 可以写缓存 也就是记忆函数 从而去 缓存结果 以达到优化
#### 副作用：
- 纯函数：
  - 有硬编码 后续可以柯里化解决
  - 对于相同的输入永远会得到相同的输出 而且没有任何观察的副作用
- 不纯函数：
  - 副作用来源
    - 配置文件
    - 数据库
    - 获取用户的输入
  - 所有的外部的交互都会产生副作用 会导致方法的安全隐患以及不确定性，尽可能在可控范围内发生
#### <a id="1.3.3">柯里化</a>
- 运用闭包的形式将函数的参数分隔开 从而达到参数互不影响
- 当一个函数有多个参数需要传入时候可以先传入一部分从而产生一个新的函数 传入的参数将永远不变
- 返回的函数将等待剩余参数 并返回结果
#### 柯里化的实现
- 实现函数的参数需要传入一个参数 这个参数是一个函数
- 返回值是一个新的函数 新的函数参数为...args 解构arguments
- 在其调用时候需要接收参数 并判断参数是否已经传入完毕
- 参数等于形参个数的话 返回执行函数
- 参数不等于形参个数的话 返回一个新函数 并把本次函数参数传进 并且链接好下次函数参数的传入
#### 函数柯里化总结
- 柯里化可以让我们给一个函数传递若干数量的参数并得到一个记住这些参数的函数
- 对函数参数的变相 缓存
- 让函数变得灵活 粒度变小
- 可以把多元函数转换为一元函数 并使函数组合实现新的功能
#### <a id="1.3.5">函数组合</a>
- 将若干个函数组合到一起 这样有利于分析 是具体哪一步出现了问题 也称之为洋葱函数
#### Point Free
- 把数据处理的过程定义为运算 不需要用到的代表数据的那个参数
  - 不需要指明处理的数据
  - 只需要合成运算的过程
  - 需要定义一些基本的运算函数
#### <a id="1.4">函子</a>
  - 容器：包含值以及值的变形关系 变形关系->函数
  - <a id="1.4.1">函子</a>：是一个特殊容器 通过一个普通的对象来实现 该对象有map方法 map方法可以运行一个函数对值进行处理
  - Container函子：普通函子
  - <a id="1.4.2">MayBe函子</a>：判断值是否为空
  - <a id="1.4.3">Either函子</a>：用于捕捉执行中可能出现的异常
  - <a id="1.4.4">IO函子</a>：
    - IO函子中的储值是一个函数，这里把函数作为值去处理
    - IO函子可以把不纯的动作存储到储值中，延迟执行这个不纯的动作（惰性执行），保证当前的操作纯
    - 把不纯的操作交给调用者去处理
#### <a id="1.4.5">FolkTale</a>
  - 一个函子库，包含了异步函数（Task ）等若干操作
    - Task函子：异步
    - Pointed函子：实现of静态方法的函子
      - of方法是把值包裹到一个新的函子里面并且返回
    - <a id="1.4.6">Monad函子</a>：防止出现嵌套函子 对调用时不方便 
      - 如果有join和of两个方法并遵守一定的定律 那么就是Monad函子
### <a id="2">第二节 异步编程</a>
#### 内容概要：
- 同步模式与异步模式
- 事件循环与消息队列
- 异步编程的几种方式
- Promise异步方案、宏任务/微任务队列
- Generator异步方案、Async/Await语法糖
#### 同步模式：
- 代码按照顺序依次执行 没有执行到相关函数时候会产生阻塞
#### 异步模式：
- 不会等待栈中任务的结束再去执行下一个函数 会通过回调函数进行后续操作 不会产生阻塞可以继续执行下去
- 因为Js为单线程 所以异步执行时候代码执行顺序会显得比较乱
#### 回调函数
- 回调函数可以理解成为一件你想要做的事情
- 想要做->需要什么物料配合->需要购买哪些材料->等待购买完毕继续去做
- 由调用者定义 交给执行者执行的函数
#### Promise
- 最开始是CommonJs提出的一种规范
- 规范了回调函数的最终执行状态究竟是成功还是失败
- 一旦出现了结果 往往是不可改变的
- Promise使用方法
    - new Promise
    - 接收一个函数 函数的参数为 resolve 和 reject 即是成功和失败的执行
    - 函数有一个then方法可以用来处理resolve和reject后的操作
- Promise常见误区
    - 嵌套使用Promise 解决方案 尽量使用Promise的then方法去链式调用
- Promise链式调用 then 方法 会返回一个全新的Promise对象 且当前的结果将作为下一个then方法的参数  
- unhandleRejection 用于捕获未被Promise捕获的异常
- Promise静态方法
  - Promise.resolve() 快速把一个值转换为Promise对象
  - Promise.reject() Promise失败的对象
- Promise并行执行
  - Promise.all 接受一个Promise对象数组 当全部完成后 才会返回结果 如果有失败就会返回失败                     
  - Promise.race 也是接收一个Promise对象数组 但是只要有一个任务完成了 就会返回结果 
- Promise 执行时序/宏任务vs微任务
  - 宏任务全部执行完毕后执行微任务然后执行下一波宏任务
  - 回调队列中的任务叫做宏任务 这些宏任务执行中所产生的新的需求 也会加到宏任务队尾
  - Promise的回调作为微任务执行
#### Generator
  - 函数声明处多了一个*  `function * fun(){}`
  - 通过yield去执行下一个yield的位置 
  - 异步方案：
    - 通过调用函数的next()去访问函数里面执行到yield时候的执行结果 使函数继续执行到下一步所需的结果
    - 通过yield得到一个近乎于同步函数的体验
    - 不断调用next 通过判断返回结果的done属性是否为true 为true的时候就是生成器执行结束 完成了函数流程
    - 递归执行生成器函数
      - while判断res的done属性是否为false(为true代表生成器已经结束)
      - 不断调用next 并保存 next返回的结果 将结果进行整合使用
      - 拓展 GitHub社区中有个完善的库 co
#### Async/Await 语法糖
- 生成器函数的更方便的语法糖
- 使用：
  - 给函数加上async标识 内部通过await去给某些操作注册等待策略
  - 返回的是Promise对象 可以使我们更好的控制结果 
  - 只能在函数内部 不能再顶层作用域中使用
#### Promise的实现
- Promise 是一个类
- 在执行这个类的时候 需要传递一个执行器进去 执行器会立即执行
- 回调有两个参数 resolve reject 是两个函数用于修改Promise的状态
- 总共有三种状态 Pending(等待) Fulfilled(成功) Rejected(失败)