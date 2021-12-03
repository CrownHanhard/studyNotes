### 第二节 ES 新特性与TypeScript、JS性能优化

#### ECMAScript与JavaScript关系

- Javascript是ECMAScript的扩展语言
- es6是称Es2015后的统称 如Async其实是Es2017 但是还是叫做es6
- 安装nodemon自动执行我们的文件 会热更新文件 重新执行脚本
- let var const 作用域
- 解构赋值：
    - 数组 [...Arr]
    - 对象 [...Obj]
- 模板字符串 \` \`
- 字符串判断位置
    - startWith('') 判断是否以‘ ’开头
    - endWith('') 判断是否以‘ ’结尾
    - include('') 判断是否包含' '
- 参数默认值 可定义未传递参数时候 默认为啥值
- ...操作符
    - rest
    - spread

#### 箭头函数

- ()=>{}
- 没有this this指向上级一层 不会改变this指向
- 可以和延时函数并用以解决this指向问题

#### 对象

- 可以用[fun] 作为属性名 fun为表达式结果
- Object.assign() 合并对象
- Object.is() 用来判断两个值是否相等
- Object.defineProperty 监听数据响应
    - es2015中Proxy也是为了监听对象中的数据变化
    - 通过 生成一个proxy实例对象 如let a=proxy('第一个参数是需要代理对象',{第二个参数是操作函数 对代理对象哪种方法或改变进行监听})

#### Reflect

- 统一的操作对象API 属于静态类 它拥有一些静态方法
- has() 传入两个参数 一个是对象 另一个是属性 判断对象是否有这个属性
- deleteProperty 对象删除某个属性
- ownKeys 取对象的所有属性 可以取到Symbol属性

#### Promise

- resolve 返回一个promise对象
- reject 返回一个错误
- then 返回promise执行结果
- all 返回promise数组执行结果集合
- race 返回promise数组执行第一个的结果
- catch 捕获异常
- finally 无论执行状态如何返回用户传入的回调函数

#### Class

- class关键词声明 类对象
- constructor方法为当前类的构造函数 （可以使用this访问当前构造函数的数据）
- static用于声明静态方法 方法可以通过类名.方法名直接使用
- extends是class的继承 是es6中专门用来继承的关键字
    - 需要在constructor中通过super对象并传入父类需要的参数

#### Set

- 新的数据结构
- 构造的实例可以存放数据
- 允许链式调用
- 数据唯一性
- 可以用实例的forEach 或者 for of 方法
- 实例的size属性可以获取集合的长度
- has方法判断是否存在某个值
- delete方法用来删除某个值
- clear方法清除全部值
- 最常见场景 数组去重

#### Map

- 新的数据解构
- 与对象解构类似 键值对集合
- 键只能是字符串类型
- 生成一个map实例
- 实例的set方法存数据
- get方法取数据
- has方法判断是否存在
- clear清除
- delete删除某个方法
- forEach方法遍历

#### Symbol

- 新的原始数据类型 唯一类型
- 业务例如给库扩展新的方法时候 方法名定义 不影响原先库的使用
- 通过给对象中加入symbol的属性 使得无法直接访问 间接实现了属性私有化
- for方法 维护的字符串和Symbol见的对应关系 可以全等
    - 如：var a=Symbol.for('a') var b=Symbol.for('a') a===b
- toStringTag 自定义toString方法返回的值 内置的一个string
    - 如：
    - `let obj = {}; obj.toString()==='[object object]'`
    - `obj={[Symbol.toStringTag]:'Xobject'} //重新赋值`
    - `obj.toString='[object Xobject]'`
- iterator 自定义对象实现js中的内置方法 可被for of 遍历 添加迭代器
- hasInstance
- 通过Symbol声明的属性 for in 和obj.keys 拿不到 JSON化的话也拿不到
- 可以通过 Reflect.ownKeys 拿到 也可以通过Object.getOwnPropertySymbol获取 （但只能拿到Symbol）

#### for of遍历

- 可以动态给对象或者数组添加这个方法
- 相比较forEach for of方法可以随时终止遍历
- 之前需要终止 我们需要通过 some 或者 every （some中return true every中 return false）
- map 实例化 然后用for of 去遍历这个map实例
- 遍历数组 => 键值对形式
- 遍历对象 默认不可被迭代 需要给对象手动添加迭代器

#### 可迭代接口

- System.iterator方法就是可迭代接口
- 给对象加[Symbol.iterator]属性 该属性是一个方法
- 实现迭代器的目的
    - 提供给使用者一个迭代接口 不需要担心内部变化
    - 可以统一遍历
    - 核心 对外提供遍历接口 不需要在考虑内部结构
        - 只对当前的解构
        - es6的Generator 适用于任何 只需要写生成器

#### Generator生成器

- 目的：
    - 在复杂的异步代码中减少回调函数嵌套产生的问题
    - 提供更好的异步编程解决方案
    - 惰性执行 不调用不执行
- 实现iterator方法 不断调用yield 直到 done=true

#### 展望Es2016 Es2017

- Es2016
    - 数组实例对象的includes方法（检测是否包含某个方法，在此之前是数组.indexOf方法并返回下标且不能查找NaN）
    - 指数运算符 2**2 => Math.power(2,2)
- Es2017
    - Object 对象的扩展方法
        - values Object.values返回的是值数组
        - entries 以数组形式返回键值对 可以通过for of 遍历 对象的entries
        - getOwnPropertyDescriptors 返回对象的描述信息
    - String的扩展方法
        - padStart 给定字符串头部填充目标字符串的长度 直到我们设置的值
        - padEnd 给定字符串尾部填充目标字符串的长度 直到我们设置的值
    - Async/Await
        - 解决异步函数中回调函数嵌套过深

### 第二节 TypeScript和@flow

#### 类型系统

- 强类型和弱类型 （类型安全维度）
    - 强类型不许有随意的隐式类型转换
    - 弱类型允许随意的隐式类型转换
- 静态类型与动态类型（类型检查维度）
    - 静态类型 一个变量声明时 类型就是明确的 声明后无法更改
    - 动态类型 变量在运行时候才有自己的类型 并且允许更改类型

#### JavaScript弱类型产生的问题

- 1、异常需要等到运行时候才能被发现
- 2、函数的功能可能因参数类型发生变化
- 3、对象索引器的错误用法

#### 强类型的优势

- 1、强类型代码错误更早暴露
- 2、强类型的编码更准确，有效
- 3、重构更牢靠
- 4、减少代码中不必要的代码判断

#### Flow

- 两种方案去移除flow注解
    - 1、flow-remove-types
    - 2、babel-preset-flow
- 安装开发工具的插件 flow

#### TypeScript

- 功能更为强大 生态更加健全 完善 渐进式
- 缺点：
    - 本身多了很多概念 接口 类型 泛型 枚举

> 可以通过加export{} 将文件模块化从而不会和其他文件声明的产生冲突

- Object 类型
    - 可以是任意的非原始类型的值 如 object={} =（） =function（）{}
- 数组类型
    - 两种方式
        - Array<number>
        - number[]
- 元组类型
    - `[string,number]=['123',123]`
- 枚举类型
    - `enum add{one=1,two,three}`
    - 如果是数字类型 会自增 如果是 字符串必须声明完
    - 可以加const 声明 打包后就可以省略 枚举部分
- 函数类型
    - `function（a:number,b:string,...rest:number[]）`
- 任意类型
    - any 可以是任意类型
- 隐式类型推断
    - 定义时候的类型 就是 实际类型
    - `let age=18`  age=>number类型
    - 如果 声明时候未去声明 则可以改变类型
- 类型断言
    - as关键词  `const number as number`
    - <>尖括号  `const number=<number>res` 尖括号 容易和标签发生冲突
- 接口
    - `interface post{x:number,y:string }`
    - `function a(post:post){}`
    - 可选成员
        - `interface post{x:number,y?:string }`?:可有可无
        - `interface post{x:number,readonly y:string }` readonly只读成员
        - `interface post{[prop:string]:string }` [prop:string] 任意成员
- Class类
    - 用来描述一些抽象的成员
    - 基本使用
        - `class person{ a:string; constructor(a:string){ this.a=a}}` 必须先去声明
    - 访问修饰符
        - private 私有属性 可以在声明时候加入
            - 不允许继承
        - public 公开属性
        - protected 受保护的修饰符 不能在外部访问
            - 只允许在子类中访问成员 可以通过extends继承 super录入后访问
        - static 静态方法
    - 只读属性
        - readonly 初始化后不能被修改了
    - 类和接口
        - `interface eat{ eat(food:string):void }`
        - `interface run{ eat(food:string):void }`
        - `implement class a implement eat,run {eat(food:string):void{console.log(food)};eat(food:string):void{console.log(food)}}`
        - 协议 也叫做接口 在类中定义的接口
        - 必须要有对应的成员 不然就会报错
        - 多个接口时候 可以用,隔开
    - 抽象类
        - 通过 abstract关键词去声明抽象类
        - 声明后无法用new关键词创建实例对象
        - 必须通过继承方式
        - 抽象类的方法也可以用abstract去抽象方法
    - 泛型

````typescript 
    function creatArray(length:number,value:number):number[]{
        let arr=Array<number>(value).fill(value)
    }
````

 ````typescript 
    function creatArray<T>(length:number,value:T):T[]{
        let arr=Array<T>(value).fill(value)
    }
 ````

 ````typescript 
 <T> <number> <string> //泛型 在函数名后面 声明以及结果一致 可以声明任意类型数组
 ````

- 类型声明
    - 当引入其他库时候 可能没有函数的一些声明类型
    - declare
        - 可以通过declare去声明

````typescript 
declare function camelCase(input:string):string
````

- 声明后就有类型的限制了
- 为了兼容一下类型声明模块

### 第三节 JavaScript的性能优化

#### 内存管理

- 内存 由可读写单元组成 表示一片可操作的空间
- 管理 人为的 去操作一片空间的申请 使用和释放
- 内存管理 开发者可以主动的申请空间 使用空间 释放空间

#### 垃圾回收机制

- JavaScript内存管理是自动的 后续执行完毕会自动销毁
- 对象不再被引用时候是垃圾
- 对象不能从根上访问到的时是垃圾
- JavaScript可达对象
    - 可以访问到的对象（引用 作用域链）
    - 可达的标准是是否能从根出发找得到
    - JavaScript的根可以理解为是全局的变量对象

#### GC定义与作用

- GC就是垃圾回收机制的简写
- GC可以找到内存中的垃圾 并释放空间
- 什么样的东西被GC当作垃圾
    - 程序中不再需要使用的对象
    - 程序中不能再访问的对象
- GC算法是什么
    - GC是一种机制 垃圾回收器完成具体的工作
    - 工作的内容就是查找垃圾释放空间 回收空间
    - 算法就是工作时查找和回收所遵循的规则
    - 引用计数
        - 引用计数算法原理
            - 核心思想 设置引用数 判断当前引用数是否为0
        - 引用计数器
          - 引用关系发生改变时候修改引用数字
          - 引用数字为0时立即回收
        - 引用计数算法的优缺点介绍
            - 优点
                - 发现垃圾时候立即回收
                - 最大限度减少程序骤停
            - 缺点
                - 无法回收循环引用的对象
                - 时间开销大
    - 标记清除
        - 核心思想分标记和清除两步完成
        - 遍历所有对象找标记活动对象
        - 遍历所有对象清楚没有标记对象
        - 优点：
            - 可以清除循环引用的对象
        - 缺点：
            - 空间碎片化 清楚后的空间 可能不等于新产生空间大小 因此 会造成 坑坑洼洼的
            - 不会立即清除垃圾回收对象
    - 标记整理
        - 标记整理算法是标记清除的增强操作 第一阶段和标记清除一样
        - 清除阶段会先执行整理操作 整理后 就不会坑坑洼洼的
    - 分代回收

#### 认识V8

- V8引擎是目前主流的JavaScript执行引擎
- V8采用及时编译
- 内存设有上限
    - 64位操作系统 不超过1.5G
    - 32位操作系统 不超过800M
- V8垃圾回收策略
    - 采用分代回收思想
        - 内存分为 新生代 老生代
        - 针对不同对象采用不同回收算法
    - V8中常用的GC算法
        - 分代回收
        - 标记清除
        - 标记增量
        - 标记整理
        - 空间复制
    - V8内存分配
        - V8内存空间一分为二
        - 小空间用于存储新生代的对象
        - 新生代对象是指存活时间较短的对象
    - 新生代对象回收
        - 回收过程采用复制算法+标记整理
        - 新生代内存区分为两个等大小的空间
        - 使用空间为From状态 空闲空间为To状态
        - 活动对象储存在From状态
        - 标记整理将活动的对象拷贝到To状态
        - From与To交换空间完成释放
        - 回收细节
            - 拷贝过程中可能出现晋升
                - 晋升就是指新生代移动到老生代
            - 一轮GC还存活的新生代需要晋升
            - To空间使用率超过25%
    - 老生代对象回收
        - 老生代存放右侧老生代区域
        - 64 1.4G 32 70M
        - 存活时间较长的对象
        - 主要采用 标记清除 标记整理 和 增量标记
        - 标记清除完成垃圾回收
        - 标记整理算法进行空间优化
            - 当发现新生代移动到老生代（晋升）并且发现空间不足
            - 此时会采取标记整理
        - 增量标记进行效率优化
    - 细节对比
        - 新生代使用空间换时间
        - 老生代不适合复制 空间比较多
- Performance工具介绍
    - GC目的是为了实现内存空间的良性循环
    - 良性循环是否合理
    - 时刻关注才能确定是否合理
    - Performance提供了多种监控方式
    - 使用步骤
        - 打开浏览器F12控制台
        - 选择性能选项
        - 开启录制功能 并访问具体页面
        - 执行用户行为 一段时间后停止录制
        - 分析界面中记录的信息
    - 内存问题的外在表现
        - 页面出现延迟加载或者经常性暂停
        - 页面出现持续性糟糕的性能表现
        - 页面的性能随着时间越来越差
        - 监控内存的几种常见的方式
            - 浏览器任务管理器
            - Timeline时序图记录
            - 堆快照查找分离DOM
            - 判断是否频繁垃圾回收
        - 界定内存问题的标准
            - 内存泄露：内存的持续升高
            - 内存膨胀：在多数设备上都存在性能问题
            - 频繁的垃圾回收：通过内存变化图进行分析
    - 任务管理器监控内存
        - Shift+Esc调出任务管理器
        - 内存
            - 表示原生内存 DOM节点所占据的内存
            - 不断增大说明不断创建新的DOM
        - JavaScript内存
            - JS堆的内存
            - 小括号值不断增多 说明当前界面创建新对象或者现有对象不断增长
    - Timeline记录内存
        - 判断内存图的升降
        - 城墙形的说明GC工作稳定和我们代码交替进行
        - 如果是直上形说明内存泄露 持续增高
    - 堆快照分离DOM
        - 界面元素存活在DOM树上
        - 垃圾对象时的DOM节点
        - 分离状态的DOM节点
        - 查找deta
    - 判断是否频繁的垃圾回收
        - Timeline中频繁的上升下降
        - 任务器中数据频繁的增加减少
    - Performance总结
        - Performance使用对内存进行分析
        - 内存问题的相关分析
        - Performance时序图监控内存变化
        - 任务管理器监控内存变化
        - 堆快照查找分离DOM

#### V8引擎工作流程

- Scanner是一个扫描器 扫描代码进行词法分析
- Parser是一个解析器 会把词法分析的结果转化为词法树 AST 同时也会抛出错误
    - 预解析
        - 跳过未被使用的代码
        - 不生成AST 创建无变量引用和声明的scopes
        - 依据规范抛出特定错误
        - 解析速度更快
    - 全量解析
        - 解析被使用的代码
        - 生成AST
        - 构建具体scopes信息 变量引用 声明
        - 抛出所有语法错误
- ignition是V8提供的一个解释器
- TurboFan是V8提供的编译器模块

#### 堆栈操作

- 堆栈准备
    - JS执行环境
    - 执行环境栈 ECStack
    - 执行上下文
    - VO（G） 全局变量对象
        1. 创建一个值100 由于100是基本数据类型 所以直接存放在栈区
        2. 声明一个变量 存放在VO（g）
        3. 建立变量和值之间的关系
    - 基本数据类型存储
        - 基本数据类型是按值进行操作的
        - 基本数据类型值是存放在栈区的
        - 无论我们当前看到的栈内存 还是后续引用数据类型会使用的堆内存都属于计算机内存
        - GO 全局对象
            - 与VO区别 不是VO（G）但是一个对象 因此也会有内存空间地址
            - 用来存储window对象 如setTimeout
            - 因为有内存地址可以对其访问 JS会在VO里面准备一个对象window 存的是引用地址
    - 引用类型存储
        - 栈区里面存放的是堆里面的地址
    - 函数堆栈处理
        - 创建函数时候与创建变量一样 函数名可以理解为一个变量
        - 单独开辟一个堆内存 去存储函数代码
        - 创建函数时候的scope作用域已经确定 是当前的EC（G）执行上下文
        - 创建函数之后会将函数名与函数体关联
        - 函数创建
            - 可以将函数名看作变量 存放在VO中 同时它的值就是当前函数对应的地址
            - 函数本身也是一个对象 创建一个内存地址 空间存放的就是函数体
        - 函数执行时做的事情
            - 确定作用域链
            - 确定this
            - 初始化arguments
            - 形参赋值 相当于变量声明 将声明变量存放在AO
            - 变量提升
            - 执行代码
        - 闭包堆栈处理
            - 闭包是一种机制，通过私有上下文来保护当中变量的机制
            - 我们也可以认为当我们创建的某一个执行上下文不被释放的时候就形成了闭包
            - 保护，存数据
        - 闭包 一种机制
            - 保护：当前上下文中的变量和其他上下文中的变量互不干扰
            - 保存：当前上下文中的数据（堆内存）被当前上下文中的变量所引用 这个数据保存下来
        - 闭包
            - 函数调用形成一个全新的私有上下文 在函数调用之后当前上下文不被释放就是闭包（临时不被释放）
        - 闭包与垃圾回收
            - 保存的不被释放
            - 没被保存的临时不被释放
            - 浏览器都有垃圾回收
            - 栈空间 堆空间
            - 堆：如果当前堆内存被占用，就能被释放掉，但是我们如果确认后续不再使用这个内存的数据，也可以主动置空 然后浏览器就会回收
            - 栈：当前上下文是否有内容 变量被其他上下文占用 有的话无法释放（闭包）
        - 循环事件添加
            - 自执行函数形成闭包拿到循环值 （var)
            - 也可以添加自定义属性 并赋值为i 通过this 访问自定义属性的值
            - 底层堆栈分析
                - var 声明变量在vo中会不断改变 所以最终拿到的值都是一样的
                - 用闭包的时候 有形参 匿名函数自调用生成私有上下文 作用域链中加上 声明 并保存
                - 循环+闭包时候 尽量在结束时候 通过xxx=null 去手动释放内存
            - 事件委托方式
                - 通过父级的 e.target.xxx找到子元素添加委托
                - 在内存上 或者 处理上 性能优于 循环闭包执行 使用空间少
    - JSBench使用
        - Setup 初始化
        - Teardown 收尾
        - ops 每秒执行多少次 越大越好
    - 变量局部化
        - 这样可以提高代码的执行效率 减少数据访问时的查找路径
        - 数据的存储和读取都会提升->离我们使用地方越近 更好
    - 缓存数据
        - 对于需要多次使用的数据提前进行保存 后续进行使用
        - 减少不必要的代码
        - 在函数里面通过定义某一个值 将堆空间的内容缓存到当前栈中 从而可以复用 空间换取时间
    - 减少访问层级
        - 构造函数中通过方法返回某个值 并没有 直接通过实例访问到这个属性 速度快 因为会创建新的作用域链 反而消耗了时间
    - 防抖与节流
        - 为什么需要防抖节流
            - 在一些高频率使用的场景下我们不希望对应的事件处理函数多次执行
        - 场景
            - 滚动事件
            - 输入的模糊匹配
            - 轮播图切换
            - 点击操作
            - ···
        - 浏览器默认情况下都会有自己的监听间隔

> 防抖：对于这个高频的操作来说 我们希望识别一次点击 可以是人为是第一次或是最后一次

- 设计时候 三个维度
    1. 形参1：需要监听的函数
    2. 形参2：事件触发多少次后开始执行
    3. 形参3：控制执行第一次还是最后一次

````javascript
  function deBounce(handle, wait, immediate) {
    if (typeof handle !== 'function') {
        throw new Error('handle must be an function')
    }
    if (typeof wait === 'undefined') wait = 300
    if (typeof wait === 'boolean') {
        immediate = wait
        wait = 300
    }
    if (typeof immediate !== 'boolean') immediate = false
    let timer = null
    return function proxy(...args) {
        let self = this,
            init = immediate && !timer
        // 如果想要传进来立即执行 则需要timer为null
        clearTimeout(timer)
        timer = setTimeout(() => {
            timer = null
            !init ? handle.call(self, ...args) : null
        }, wait)
        !init ? handle.call(self, ...args) : null
    }
}
````

> 节流：对于高频操作 我们可以设置频率 让本来会执行很多次的事件触发 按着我们定义的频率减少触发的次数

- 在自定义的一段时间内 让事件进行触发

````javascript
    function throttle(handle, wait) {
    if (typeof handle !== 'function') {
        throw new Error('handle must be an function')
    }
    if (typeof wait === 'undefined') wait = 400
    let oldTime = 0// 定义上一次时间
    let timer = null
    return function proxy(...args) {
        let self = this
        let newTime = new Date().getTime()// 定义当前时间
        if (newTime - oldTime >= wait) {
            clearTimeout(timer)
            timer = null
            handle.call(self, ...args)
            oldTime = new Date().getTime()
        } else if (!timer) {
            // 此时说明 这次操作发生在我们定义的频次时间范围外
            timer = setTimeout(() => {
                clearTimeout(timer)
                handle.call(self, ...args)
                oldTime = new Date().getTime()
            }, newTime - oldTime)
        }
    }
}
```` 
- 减少判断层级
  - 通过枚举includes判断 不通过if 进而减少层级
  - 避免多层嵌套去判断
- 减少循环体活动
  - 尽可能把循环体中常用的值提出去 从而提升执行效率
  - for循环改为while循环 会更快一些
- 字面量和构造式
  - 用new Object 声明 或者直接声明代替构造函数式声明
  - 直接声明会比构造式稍微快一些