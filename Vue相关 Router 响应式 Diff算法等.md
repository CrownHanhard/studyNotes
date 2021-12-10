### Vue.js

#### 基础回顾

1. render方法
    1. h函数 h函数创建虚拟dom
    2. render函数将虚拟dom返回
2. Vue的生命周期
    1. beforeCreate
    2. Created
    3. beforeMound
    4. Mounted
    5. beforeUpdate
    6. Updated
    7. beforeDestroy
    8. Destroyed
3. Vue语法和概念
    1. 差值表达式{{}}
    2. 指令 v-directive
    3. 计算属性和侦听器
    4. Class和Style绑定
    5. 条件渲染/列表渲染
    6. 表单输入绑定
    7. 组件
    8. 插槽
    9. 插件
    10. 混入mixin
    11. 深入响应式原理
    12. 不同构建版本的Vue

### VueRouter的实现原理

1. 使用步骤
    1. 注册路由插件 Vue.use就是注册插件的
    2. 路由规则
    3. 创建路由对象
    4. 在main.js中注册router 这个对象
        1. 注册的话 会在实例里面看到$route 和 $router这两个属性
2. 动态路由
    1. 可以开启props 为true
    2. 在组件中就可以通过props来接收URL参数
    3. 两种获取参数方式
        1. $router.params.参数名
        2. 通过开启props 并通过组件中props:\[参数名] 接收参数 不在依赖路由规则
3. 嵌套路由
    1. 在使用 会把外部路径 和 内部路径加载后 合在一起
    2. 例如 封装的模板里面 具有相同的首尾 那么只需要加载中间的组件就可以完成更新
4. 编程式导航
    1. 场景 处理登录时候
    2. push 方法
        1. {'地址'} 跳转具体地址
        2. {name:''} 通过name跳转
    3. replace 方法
        1. 与push方法类似都可以跳转
        2. 不会记录本次历史 会将当前的历史记录替换为跳转地址 无法返回上一页
    4. go 方法
        1. 跳转到历史中的某一个
        2. 可以是负数 为返回效果
5. hash模式和history模式的区别
    1. 表现形式
        1. hash带有# #后面带有我们路由地址
        2. history模式中 是正常的路由地址
    2. 原理的区别
        1. hash模式是基于锚点 以及onchange事件
        2. history模式是基于Html5中的historyAPI
            1. history.pushState IE10以后才支持
            2. history.replaceState 不会发送请求 只会改变地址 并把地址放进历史记录里面
6. History模式
    1. History模式需要服务器的支持
    2. 单页应用中 服务端不存在www.test.com/test这样的地址 会返回找不到页面
    3. 在服务器中应该除了静态页面资源外都返回单页应用的html
7. History模式在NodeJS中
    1. 在node服务器中开启服务
    2. app.use(history) 开启history模式
8. History模式 nginx配置
    1. start nginx 启动nginx
    2. nginx -s reload 重启
    3. 为了支持history模式
        1. 在conf里面去配置
        2. 在 location 中 添加 try_files $uri \$uri/ /index.html;
9. VueRouter原理
    1. vue的前置知识
        1. 插件
        2. 混入
        3. Vue.observable()
        4. 插槽
        5. render函数
        6. 运行时和完整版的Vue
    2. Hash模式
        1. URL中的#后面的内容作为路径地址
        2. 监听hashChange事件
        3. 根据当前路由找到对应组件重新渲染
    3. History模式
        1. 通过history.pushState()方法改变地址栏
        2. 监听popState事件
        3. 根据当前路由找到对应组件重新渲染
10. VueRouter模拟实现分析
    1. 分析如何使用
        1. 首先要导入VueRouter模块
        2. 创建实例
            1. 需要有个install方法
            2. 需要接受对象
        3. 创建Vue实例 注册router对象
11. VueRouter
    1. 定义install方法
        1. 接收参数 Vue对象
        2. 需要做的事情
            1. 判断当前插件是否已经被安装
            2. 把Vue构造函数记录到全局变量中
            3. 把创建Vue实例时候传入的router对象 注入到Vue实例中
        3. 判断VueRouter.install.installed的值是否为true 判断是否安装
        4. 定义_vue变量 并把传入的Vue实例赋值给_vue
        5. 将_vue.prototype.$router=this.$options.router
            1. 需要混入 _vue.mixin({})
            2. 添加beforeCreate函数并把 _vue.prototype.$router=this.$options.router 放进去
            3. 只需要执行一次 所以需要判断一下
            4. 判断当前选项的$options.router是否存在 存在了就不去执行
    2. 实现构造函数
        1. 初始化三个属性
            1. options
            2. data
            3. routeMap
        2. 创建constructor
            1. 接收参数为options
            2. this.options=options
            3. this.routeMap={}
            4. this.data
                1. 需要创建响应式对象
                2. 调用_vue.observable({current:'/'})
        3. 实现createRouteMap方法
            1. 将构造函数中传过来的routes转化为键值对的形式并存储到routeMap中
            2. 遍历所有的规则并转换文键值对 并存储到routeMap
            3. 通过this.options.route.forEach去遍历
            4. this.routeMap\[item.path]=item.component 将键值对存储
        4. 创建initComponents方法 接收Vue实例
            1. 通过_vue.component 创建组件router-link
            2. props属性 默认有 to 属性值为String
            3. template 为 a标签 <a :href="to"></a>
            4. 创建init方法 并调用createRouteMap 与 initComponents （_vue）方法
        5. 导入我们自己定义的函数时候发现不能用 原因如下
            1. Vue的构建版本
                1. 运行时版 不支持template模板 需要打包的时候提前编译
                2. 完整版 包含运行时和编译器 体积比运行时版大了10k左右 程序运行的时候把模板转换为render函数
        6. 切换完整版的Vue
            1. 在config文件中配置
            2. runtimeCompile 设置完后可以使用template并自动编译
        7. 不使用编译器解决问题
            1. 之前注册文件为template模板 不支持
            2. 所以我们使用render函数
                1. 在创建component中加入render函数
                2. 传入h参数
                3. 是一个函数 第一个是标签名称
                4. 第二个是attr 默认属性
                5. 第三个是插槽 默认插槽 所以this.$slots.default
        8. 创建router-view组件
            1. 同样需要_vue.component函数
            2. 同样需要render函数 还需要储存this指向
            3. 给a标签注册事件 click this.clickHandler
            4. methods clickHandler
                1. history.pushState({},'',this.to)改变地址栏
                2. e.preventDefault()
                3. this.$route.data.current=this.to
        9. initEvent方法
            1. 前进后退按钮并没有变换地址栏
            2. 注册popState事件
            3. 通过window.addEventListener 监听popstate
            4. this.data.current=window.location.pathname

#### Vue响应原理

1. 准备工作
    1. 数据驱动
        1. 数据响应式 双向绑定 数据驱动
        2. 数据响应式
            1. 数据模型仅仅是普通的JavaScript对象 当我们改变数据的时候 视图就会更新 避免了繁琐的DOM操作 提高开发效率
        3. 双向绑定
            1. 数据改变 视图改变 视图改变 数据也随之改变
            2. 我们使用v-model在表单元素中实现双向数据绑定
        4. 数据驱动
            1. 开发工程中只需要关注数据本身 不需要关心数据如何渲染到DOM上
2. 数据响应式的原理
    1. 2.x中使用Object.defineProperty给 传入的data中的数据设置getter setter
    2. 通过defineProperty实现数据劫持
        1. 第一个属性是对象
        2. 第二个是给第一个对象添加的属性
        3. 第三个是对象包含 getter setter函数
            1. enumerable 是否可枚举 可遍历
            2. configurable 是否可配置 可以使用delete删除 或者通过defineProperty重新定义
        4. 多个属性 通过Object.keys().forEach去进行遍历
    3. Vue3中的响应式原理
        1. 直接监听对象 不需要遍历
        2. new Proxy(obj,{}) obj对应的就是我们需要监听的对象
3. 发布订阅模式
    1. 自定义事件都是基于发布订阅模式的
    2. $on设置 $emit触发
    3. 发布订阅实现原理
        1. 设置一个存放事件名称以及处理函数
            1. this.subs=Object.create(null)
        2. 设置注册事件函数
            1. $on(){}
            2. 两个参数 第一个是事件名称 key 第二个是处理函数 handle
            3. this.subs\[key]=this.subs\[key]||\[] 先去初始化数组
            4. this.subs\[key].push(handle)
        3. 设置发布事件函数
            1. $emit(){}
            2. 一个参数 事件名称key
            3. 通过this.subs\[key]先去判断是否有值 如果有的话
            4. 通过forEach方法遍历数组 并执行所有函数
            5. （如果需要传参的话 还需要设置一个参数）
4. 观察者模式
    1. 观察者 订阅者 Watcher
        1. update
    2. 目标 发布者
        1. subs 数组
        2. addSub 添加观察者
        3. notify 事件发生 调用所有观察者的update方法
        4.

```javascript
      // # 发布者
class Dep {
    constructor() {
        this.subs = []
    }

    addSub(sub) {
        if (sub && sub.update) {
            this.subs.push(sub)
        }
    }

    notify() {
        this.subs.forEach(sub => {
            sub.update()
        })
    }
}

// # 观察者
class Watcher {
    constructor() {
        update()
        {
            console.log('update')
        }
    }
}

let dep = new Dep()
let watch = new Watcher()
dep.addSub(watch)
dep.notify()  
```

> 观察者中是发布者和订阅者互相依赖 发布订阅中是切断了两者之间的依赖关系 通过消息中心去确定

5. Vue响应式原理
    1. Vue
        1. 功能
            1. 负责接收初始化参数
            2. 负责把data中的属性注入到Vue实例 转换成getter/setter
            3. 负责调用observer监听data中所有属性的变化
            4. 负责调用compiler解析指令/表达式
        2. 结构
            1. $options
            2. $el
            3. $data
            4. _proxyData()
        3. 实现

```javascript
    class Vue {
    constructor(options) {
        //1. 通过属性保存选项的数据
        this.$options = options
        this.$data = options.data || {}
        this.$el = typeof options.el === "string" ? document.querySelector(options.el) : options.el
        //2. 把data中的成员转换为getter setter 注入到vue实例中
        this._proxyData(this.$data)
        //3. 调用observer对象 监听数据的变化
        new Observer(this.$data)
        //4. 调用compiler对象 解析指令和插值表达式
        new Compiler(this)
    }

    _proxyData(data) {
        // 遍历data中所有的属性
        Object.keys(data).forEach(key => {
            // 把data的属性注入到vue实例中
            Object.defineProperty(this, key, {
                enumerable: true,
                configurable: true,
                get() {
                    return data[key]
                },
                set(newValue) {
                    if (newValue === data[key]) {
                        return
                    }
                    data[key] = newValue
                }
            })
        })
    }
}
```

5. 响应式原理
    2. observer
        1. 功能
            1. 负责把data选项中的属性转换为响应式数据
            2. data中的某个属性也是对象 把该属性转换为响应式数据
            3. 数据变化发送通知
            4. 结构
                1. walk
                2. defineReactive(data,key,value)
            5. 实现

```javascript
class Observer {
    constructor(data) {
        this.walk(data)
    }

    walk(data) {
        // 1. 判断data是否为对象
        if (!data || typeof data !== 'object') return
        // 2. 遍历data中的所有属性
        Object.keys(data).forEach(key => {
            this.defineReactive(data, key, data[key])
        })
    }

    defineReactive(data, key, value) {
        const self = this
        let dep = new Dep()
        this.walk(value)// 如果是属性的话 不会处理 如果是对象的话 会对对象也做监听
        Object.defineProperty(obj, key, {
            enumerable: true,
            configurable: true,
            get() {
                Dep.target && dep.addSub(Dep.target)
                return value
            },
            set(newVal) {
                if (newVal === value) {
                    return
                }
                value = newVal
                self.walk(newVal)
                // 发送通知
                dep.notify()
            }
        })
    }
}
```

5. 响应式原理
    3. Compiler
        1. 功能
            1. 负责编译模板 解析指令/差值表达式
            2. 负责页面的首次渲染
            3. 当数据变化后重新渲染视图
        2. 结构
            1. el
            2. vm
            3. compile
            4. compileElement
            5. compileText
            6. isDirective
            7. isTextNode
            8. isElementNode
        3. 实现

```javascript
    class Compile {
    constructor(vm) {
        this.el = vm.$el
        this.vm = vm
    }

    // 编译模板 处理文本和元素节点
    compile(el) {
        let childNodes = el.childNodes
        Array.from(childNodes).forEach(node => {
            if (this.isTextNode(node)) {
                // 判断文本节点
                this.compileText(node)
            } else if (this.isElementNode(node)) {
                // 判断元素节点
                this.compileELement(node)
            }
            if (node.childNodes && node.childNodes.length) {
                this.compile(node)
            }
        })
    }

    // 编译元素节点 处理指令
    compileELement(node) {
        Array.from(node.attributes).forEach(attr => {
            let attrName = attr.name
            if (this.isDerictive(attrName)) {
                attrName = attrName.substr(2)
                let key = attr.value
                this.update(node, key, attrName)
            }
        })
        // 遍历所有的节点
        // 判断是否是指令
    }

    // 判断需要执行的方法
    update(node, key, attrName) {
        let fn = this[attrName + 'Update']
        fn && fn.call(this, node, this.vm[key], key)
    }

    // 处理v-text指令
    textUpdate(node, val, key) {
        node.textContent = val
        new Watcher(this.vm, key, (newVal) => {
            node.textContent = newVal
        })
    }

    // 处理v-model指令
    modelUpdate(node, val, key) {
        node.value = val
        new Watcher(this.vm, key, (newVal) => {
            node.value = newVal
        })
        // 双向绑定
        node.addEventListener('input', () => {
            this.vm[key] = node.value
        })
    }

    // 编译文本节点 处理插值表达式
    compileText(node) {
        let reg = /\{\{(.+?)\}\}/
        let val = node.textContent
        if (reg.test(val)) {
            let key = RegExp.$1.trim()
            node.textContent = val.replace(reg, this.vm[key])
            //    创建watcher对象 属性改变时候更新视图
            new Watcher(this.vm, key, (newVal) => {
                node.textContent = newVal
            })
        }
    }

    // 判断元素是否是指令
    isDerictive(attrName) {
        return attrName.startsWith('v-')
    }

    // 判断节点是否是文本节点
    isTextNode(node) {
        return node.nodeType === 3
    }

    // 判断是否为元素节点
    isElementNode(node) {
        return node.nodeType === 1
    }
}
```

5. 响应式原理
    4. Dep
        1. 功能
            1. 收集依赖 添加观察者
            2. 通知所有观察者
        2. 结构
            1. subs
            2. addSub
            3. notify
        3. 实现

```javascript
    class Dep {
    constructor() {
        this.subs = []
    }

    addSub(sub) {
        if (sub && sub.update) {
            this.subs.push(sub)
        }
    }

    notify() {
        this.subs.forEach(sub => {
            sub.update()
        })
    }
}
```

6. 响应式原理
    5. Watcher
        1. 功能
            1. 当数据变化时候触发依赖 dep通知Watcher实例更新视图
            2. 自身变化时候在dep中添加自己
        2. 结构
            1. vm
            2. key
            3. cb
            4. oldValue
            5. update
        3. 实现

```javascript
 class Watcher {
    constructor(vm, key, cb) {
        this.vm = vm
        this.key = key
        this.cb = cb
        // 当前的watcher记录到dep的target中
        // 触发get方法 在get中addSub
        Dep.target = this
        this.oldValue = vm[key]
        Dep.target = null
    }

    update() {
        let newValue = this.vm[this.key]
        if (this.oldValue === newValue) {
            return
        }
        this.cb(newValue)
    }

}
```
### Virtual DOM(虚拟DOM)
1. 什么是虚拟DOM
    1. 虚拟DOM 是用普通的js对象来描述DOM对象
2. 为什么使用虚拟DOM
    1. 前端刀耕火种时代 需要频繁操作DOM
    2. MVVM框架解决了视图和状态的同步问题
    3. 模板引擎可以简化视图操作 没办法跟踪状态
    4. 虚拟DOM跟踪状态变化
3. 虚拟DOM作用和虚拟DOM库
    1. 维护视图和状态的关系
    2. 复杂视图情况下才能提升渲染性能
    3. 跨平台
        1. 浏览器平台渲染DOM
        2. 服务器渲染SSR(Nuxt/Next)
        3. 原生应用(Weex/React Native)
        4. 小程序(mpvue/uni-app)
    4. Snabbdom
        1. Vue2中使用的虚拟DOM就是改造的Snaabbdom
        2. 大约200 SLOC
        3. 通过模块可扩展
        4. 源码使用TypeScript开发
        5. 最快的虚拟DOM之一
    5. virtual-dom
        1. 导入Snabbdom
            1. 看文档的意义
                1. 学习任何一个库都需要看文档
                2. 通过文档了解库的作用
                3. 看文档中提供的示例 自己快速实现一个demo
                4. 通过文档查看API的使用
        2. 导入init h 两个核心函数
        3. h函数
            1. 第一个参数字符串 描述标签选择器
            2. 第二个参数是标签的文本内容
        4. 获取页面中的div盒子
        5. patch=init([])
        6. 调用patch函数
            1. 第一个参数旧的VNode 可以是DOM元素
            2. 第二个参数新的VNode
            3. 会对比两个VNode差异 并把最终结果存储 以便下次作对比
            4. 返回新的VNode
        7. 创建空的节点在h函数里面放入一个 ! 号
    6. 模块的使用
        1. 模块的作用
            1. snabbdom的核心库并不能处理DOM元素的属性/样式/事件等
            2. 可以通过注册Snabbdom默认提供的模块来实现
            3. 模块可以用来扩展Snabbdom的功能
            4. 模块的实现是通过注册全局的钩子函数来实现的
        2. 官方提供的模块
            1. 提供了六个模块
            2. attributes
            3. props
            4. dataset
            5. class
            6. style
            7. eventListeners
        3. 模块的使用步骤
            1. 导入需要使用的模块
            2. init()注册模块 是一个数组 把要注册的模块 放到数组中
            3. h()函数的第二个参数使用模块
4. 如何学习源码
    1. 宏观了解
    2. 带着目标看源码
    3. 看源码的过程要不求甚解
    4. 调试
    5. 参考资料
    6. Snabbdom的核心
        1. init函数设置模块 创建patch函数
        2. 使用h函数创建js对象VNode描述真实DOM
        3. patch比较两个新旧Vnode
        4. 把变化的内容更新到真实DOM树
    7. h函数
        1. 创建VNode对象
        2. Vue的h函数更强大一些 支持组件机制
        3. 函数重载
            1. 参数个数或参数类型不同的函数
            2. JavaScript中没有重载概念
            3. TypeScript中有 不过重载的概念还是通过代码调整参数
        4. h函数是可以重载的 根据传入参数个人以及类型不同执行不同函数 这些函数是用一个函数名的
    8. 源码必备的快捷键
        1. F12 光标放在某个变量身上 可以快速跳到定义位置 如果多个定义 会显示一个窗口
        2. 按Alt+方向键左 快速返回
        3. Ctrl+鼠标左键 跳到定义
    9. patch整体过程分析
        1. patch两个参数oldVNode newVNode
        2. 把新节点中变化的内容渲染到真实DOM 最后返回新节点作为下一次处理的旧节点
        3. 对比新旧VNode是否相同节点（节点的key和sel相同）
        4. 如果不是相同节点 删除之前的内容 重新渲染
        5. 如果是相同节点 在判断新的VNode是否有text 如果有并和oldVNode不同 直接更新文本内容
        6. 如果新的VNode节点有children 判断子节点是否有变化
    10. init函数
        1. 第二个参数是domAPI 如果传值是指定的环境 没有就是默认的DOM
        2. 初始化cbs 钩子函数 会在合适时期执行
    11. patch函数如何比较VNode
        1. 函数里面有一个常量用来储存 VNode队列
        2. patch触发前 首先触发了pre钩子函数
        3. 判断是否为dom还是VNode
            1. 通过判断VNode.sel是否是undefined
        4. 首先判断两个VNode是否相同
            1. 通过判断key 和 sel是否相同
        5. 遍历判断VNode
        6. 调试patch函数
            1. 首先判断是否为VNode对象 不是的话 会把真实dom转换为虚拟dom
        7. createElm
            1. 判断sel为！时候 创建注释节点
            2. 如果不是undefined 会创建dom节点
            3. 判断是否有子节点 有的话会递归调用
        8. removeVNodes
            1. 四个参数 第一个参数 父节点
            2. 第二个参数是一个数组 里面包含了需要删除的元素
            3. 第三个参数是删除数组中开始位置
            4. 第四个参数是删除数组中结束位置
        9. addVNodes
            1. 父元素
            2. 参考的节点
            3. 我们要添加的节点数组
            4. 数组开始
            5. 数组结束
            6. 存储刚才插入的具有VNode钩子函数的节点
        10. patchVNode
            1. 作用
                1. 对比新旧节点
            2. 运行步骤
                1. 触发prepatch钩子函数
                2. 触发update钩子函数
                3. 新节点有text属性且不等于旧节点的text
                    1. 如果老节点有children 移除老节点的children对应的dom元素
                    2. 设置新节点对应DOM元素的textContent
                4. 新老节点都有children 且不相等
                    1. 调用updateChildren
                    2. 对比子节点 并且更新子节点差异
                5. 只有新节点有children
                    1. 如果老节点有text 清空对应的dom的textContent
                    2. 添加所有子节点
                6. 只有老节点有children 移除所有老节点
                7. 只有老节点有text 清空对应dom元素中的textContent
                8. 触发postpatch钩子函数
5. Diff算法
    1. 虚拟DOM中的diff算法
        1. 查找两棵树每一个节点的差异
    2. Snabbdom中的diff算法 做了优化
        1. DOM操作时候很少跨级别操作节点
        2. 只比较同级别的节点
