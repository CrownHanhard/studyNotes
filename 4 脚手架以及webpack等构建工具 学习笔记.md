### 模块化开发

#### 模块化只是思想

1. 模块化演变过程
    1. 文件划分方式 （每个功能以及相对应的数据放在一个模块中）
        1. 缺点
            1. 所有模块都在全局工作 没有私有空间
            2. 污染全局作用域
            3. 命名冲突
            4. 无法管理模块依赖关系
    2. 命名空间方式
        1. 将每个模块包裹到全局对象的方法去显示
        2. 可以减少命名冲突
        3. 仍然没有私有空间
    3. 立即执行函数 IIFE
        1. 通过闭包的方式去访问
2. 模块化规范
    1. 模块化标准+模块加载器
        1. CommonJS规范
            1. 一个文件就是一个模块
            2. 每个模块有单独作用域
            3. 通过module.exports导出成员
            4. 通过require函数载入模块
            5. 是以同步模式加载模块
        2. AMD
            1. 代表有：Require.js
            2. 目前绝大多数第三方库支持AMD规范
            3. 使用起来复杂
            4. 模块JS文件请求频繁
        3. CMD
            1. Sea.js
            2. 类似于CommonJS 使用上类似RequireJS
            3. 为了遵循CommonJS并用起来爽
    2. 模块化的最佳实践
        1. ES Modules 浏览器环境 CommonJS nodejs环境
        2. node中使用CommonJS
        3. 浏览器环境中用ES Modules
            1. 存在环境兼容问题
            2. 现如今随着WebPack等构建工具的使用 已经逐渐流行起来了
3. 常用的模块化打包工具
4. 基于模块化构建现代Web应用
5. 打包工具的优化技巧

### ES Modules特性

> 通过script标签 在type里面通过type=module去使用es modules特性

1. ESModules自动使用严格模式（全局下面不能直接使用this）
2. 每个ESModules都是运行在单独的私有作用域中
3. ESModules是通过CORS的方式请求外部JS模块的
4. ES的script标签会延迟执行

- ESModules导出

1. import export module.exports require
2. 每个模块都有自己的私有作用域 所以无法直接访问

- ESModules导入导出注意事项

1. export导出的不是字面量 是固定的语法 不是解构赋值 export default则是字面量等同于 var obj={a,b} 但是导出后就等于导出了一个对象
2. 导入时候的数据是引用关系暴漏给了外部 直接访问的存储地址 并不是拷贝了一个属性或者对象
3. 在外部导入一个成员过后 并不能去修改 它是一个只读的成员 我们可以利用这个特性做一些应用中常量 以及配置的问题

- ESModules导入

1. 必须要导入完整名称 不能导入js文件名 必须手动填写完整路径
2. 使用完整的URL去加载模块
3. 如果需要导入的特别多 我们可以用通配符方式全部提取出来 import * as obj from js
4. 动态导入 通过import函数 在.then中获取到module内容
5. 同时导出了命名和默认成员 再去导入时候先正常导入命名成员 然后在外层直接跟上default import a,{b,c} from js 或者 import {a,b,default as c} from js

- ESModules导入导出成员

1. 不同文件中导出组件时候 如 a中export default a b中export default b 我们可以新建个中间成员 导入完后全部导出出去 去简化主文件的导出编写
2. 针对默认模块需要进行重命名

- ESModules浏览器环境polyfill

1. 对于之前的环境不支持ESModules特性
2. 我们可以通过ESModuleLoader这个插件去支持
    1. 原理通过loader读取出来然后交给babel去转换语法
3. 浏览器不支持Promise 通过引入promise-polyfill去使浏览器支持
4. 如果浏览器支持时候 我们引入这些文件 则会导致浏览器执行重复 此时我们可以通过script中的nomodule标签从而使得浏览器判断该js是否加载 支持的浏览器就不会加载polyfill特性了
5. 我们一般是在生产阶段通过相关内容（babel等）去处理以确保兼容支持浏览器操作

- ESModules在Node环境

1. Node版本大于8.5就可以使用ESModules特性了
2. 更改文件后缀名 将.js改为.mjs
3. 启动时候带上`--experimental-modules`

- ESModules与CommonJS交互

1. CommonJS始终导出的是默认模块因此不能用import {a，b}方法去获取
2. 在node中不允许在CommonJS环境中引用ESModules模块

- Node环境中使用ESModules与CommonJS的差异

1. require module exports __filename __dirname 在ESModules中无法使用
2. 我们可以通过这种方法去替代__filename以及__dirname
    1. import {fileURLToPath} from 'url'
    2. const __filename=fileURLToPath(import.meta.url)
    3. import {dirname} from 'path'
    4. const __dirname=dirname(__filename)

- Node新版本进一步支持ESModules

1. 新建package.json 在type里面指定类型为module
2. 在使用CommonJS模块中则需要通过修改后缀名为.js=>.cjs

- Babel兼容方案

1. 安装babel/node babel/core babel/preset-env
2. preset-env是插件集合 集合中的某个插件用来转换新语法->浏览器理解的语法
3. 这时候需要
    1. yarn babel-node index.js --presets=@babel/preset-env
    2. 或者添加一个.babelrc 文件 在文件中添加presets:["@babel/preset-env"]
4. 用单一插件
    1. @babel/plugin-transform-modules-commonjs

### WebPack

- 什么是WebPack 为什么会有WebPack
    1. 前端发展
        1. web1.0
            1. 编写静态页面
            2. 表单验证和动效制
            3. 不存在太多工程化问题
        2. web2.0之AJAX
            1. 管理数据和用户交互
        3. 大前端开发 现代的Web开发
            1. 变得多样化和复杂化
            2. 不仅仅PC端 还有移动端 小程序 公众号 App
    2. 当前前端问题
        1. 采用模块化开发
        2. 使用新特性提高效率保证安全性
        3. 实时监听过程使用热更新
        4. 项目结果打包压缩优化
- WebPack功能
    - 打包：将不同资源按模块处理进行打包
    - 静态：打包后最终产出的静态资源
    - 模块：WebPack支持不同规范的模式开发
- WebPack配置文件
    - webpack.config.js 默认配置文件
    - 导出具体的配置项
        1. entry 入口
        2. output :{} 对象
            1. filename 打包后的结果名称
            2. path 打包后的地址 此处需要是绝对路径
- WebPack依赖图
    - 前端基于模块化的思想 打包
        - 如有 login.js login.css lg.png
        - 希望统一到一个静态资源里面
    - 在入口文件中 如果想处理某些模块资源 需要加到入口的模块关系中 这样就可以打包
    - 通过不同的loader对这些不同的内容进行转换
    - 在package.json里面对命令行中的script加上--config 自己的配置文件名 可以自定义设置配置文件
- loader相关 Css-loader

> webpack4中loader 但是在webpack5中cli里面的loader废弃了
>1. 行内loader
>2. 配置文件loader
>3. 命令行loader

1. 为什么需要loader
    1. 默认情况下WebPack中不会对其他非JS文件的模块进行处理 所以我们需要通过相关loader去处理
2. loader是什么
    1. loader是用来帮助WebPack处理其他非JS文件的模块
3. css-loader
    1. 行内loader
        1. 在引入资源模块更改导入
        2. import 'a.css' => import 'css-loader! a.css'
    2. 配置文件中写法
        1. 添加导出属性 modules 是一个对象
        2. modules:{rules:\['','','']}
        3. 可以有多个对象
            1. rules:\[{}]
            2. 第一个属性是test 一般是正则表达式 用来匹配我们需要处理的文件类型
            3. 第二个属性是use 是一个数组\[{}] 也可以是字符串内容
                1. 属性里面第一个参数是loader 可以添加相关loader
                2. 第二个参数是options 可以依据我们传进来的参数不同去进行不同的处理
                3. 第三个参数是query 在WebPack4中使用 5中大部分合并到options中了
4. style-loader的使用
    1. css-loader只会让webpack识别css语法 并不能直接使用因此需要style-loader
    2. 在rules的css里面去添加上style-loader
    3. loader的执行顺序默认是自右向左 因此需要先进行css-loader 其次去执行 style-loader
    4. 这样我们可以看到执行后的内容
5. less-loader的使用
    1. 同理我们使用less-loader的话
        1. 先用less-loader将我们的less转化为css
        2. 用我们的css-loader去处理我们转换过后的css
        3. 用style-loader去处理我们最终生成的css
6. Browserslist rc文件的书写
    1. 工程化
    2. 兼容性 CSS JS
    3. 如何实现兼容 到底兼容哪个平台 caniuse.com 网站可以看到浏览器占有率

    - 通过项目下面的browserslistrc的文件对相关浏览器进行处理
        - 如\>1% 就会找市面上大于1%的浏览器兼容
        - last 2 version 最新的是最后的两个版本
        - default 默认是 >0.5% 最新的2个版本
        - dead 最近2年没更新官网的浏览器就是默认为死掉的浏览器
    - 写在package.json里面 新增 browserslist 数组格式
    - 会返回相关符合配置的平台
7. postcss工具的使用
    1. postcss是什么
        1. JavaScript样式转换工具
        2. less(less-loader)=>(postcss)css(css-loader)
        3. 在命令行想要使用postcss时候需要安装postcss-cli
        4. 打包时候可以根据浏览器覆盖率参数自动生成适配于全部覆盖率的css
        5. 对项目中的css样式做兼容处理
        6. autoprefixer 对css兼容自动添加前缀
    2. postcss-loader的使用
        1. 先去在rules里面添加postcss-loader在css-loader之前执行
        2. postcss-loader单纯设置没有作用因此需要是个对象形式
        3. 对象里面需要配置options
        4. postcss-preset-env 插件的集合 集合了很多的现代的css插件
        5. 需要在less-loader后面 只能处理css的
        6. 也可以通过设置文件postcss.config.js去处理 可以作为复用
    ```js
    options:{
        postcssOptions:{
            plugins:[
                require('autoprefixer')
            ]
        }
    }
    ```
8. importLoaders属性
    1. 如果我们在css中导入了其他文件的css时候
    2. 我们这个css文件就直接交给了css-loader 而我们导入的没有被传入
    3. {loader:'css-loader',options:{importLoaders:1}}
    4. 通过importLoader可以处理css-loader处理前还需要处理多少个其他@import引入的css
9. file-loader处理图片
    1. \.(jpg|png|svg) 匹配规则
    2. 通过use file-loader 处理 webpack4 和 5 的处理结果不一样
    3. options：{esmModule：false} 是否将我们处理完的内容包装成一个ESModule对象
    4. 通过设置false后也可以正常的打包展示
    5. 可以在导入图片的文件里面直接import
    6. 获取require（img）.default
    7. css中的背景图片 url被处理时候 会自动写成require语法
        1. 这时候我们在css-loader的配置项里面通过设置esModule设置为false
    8. 处理file-loader打包后的结果
        1. 通过file-loader options来配置
        2. options:{name:''// 占位符}
            1. \[ext] 扩展名
            2. \[name] 文件名
            3. \[hash] 文件内容加上md4的算法生成hash值
            4. \[contentHash] 和hash结果一样
            5. \[hash:<length>] 可选hash长度
            6. \[path] 路径 相对于webpack
        3. outputPath 输入目录 简写也可以直接在name里面 'img/\[name]' 去确认输出目录
10. url-loader处理图片
    1. 默认情况下把所有图片以Base64URI方式加入
        1. 好处 减少请求次数 而file-loader是将资源拷贝到指定目录
        2. url-loader也会调用file-loader
            1. 根据limit属性 大于这个值就做拷贝没有超过就做URI
11. webpack5中的asset资源模块
    1. asset/resource 可以指定拷贝目录 相当于file-loader
    2. asset/inline 可以写成URI方式 相当于url-loader
    3. asset/source 类似于我们的row-loader
    4. asset/resource使用
        1. {test:'/\.jpg/',type:'asset/resource',generator:{fileName'/\.jpg/'}}
        2. 在output里面通过assetModuleFilename属性去控制输出目录
        3. 也可以写在规则里面写generator属性
    5. asset/inline使用
        1. {test:'/\.jpg/',type:'asset/inline'}
        2. 根据图片大小处理
            1. {test:'/\.jpg/',type:'asset',generator:{fileName'/\.jpg/'},parser:{dataUrlCondition:{max:30*1024}}}
    6. 处理图标字体
        1. test:'/\.woff/',type:'asset/resource',generator:{fileName'/font/\[name].\[hash:3]\[ext]/'}}
    7. webpack处理插件
        1. 打包过程中实用的插件
            1. loader : 对特定的模块类型转换
            2. plugin : 可以做更多事情 可以在任意打包时间去做事情
        2. clean-webpack-plugin 导入后new pluginName()
            1. plugin本质都是类 通过new生成实例对象
            2. 自动清除dist目录以及文件
    8. html-webpack-plugin
        1. 默认会创建一个html文件
        2. new HtmlWebpackPlugin({title:'own title',template:'./public/index.html'})
        3. DefinePlugin 内置的一个插件 用来定义常量
    9. copy-webpack-plugin
        1. 拷贝插件 可以把public里面的ico图标等无法处理的内容进行拷贝
        2. new CopyWebpackPlugin({patterns:{from:'public'}}) // to简写了 默认找output里面的输出目录
        3. 报错需要加一个globalOptions:{ignore:"**/index.html"} // 去设置不拷贝index.html
12. babel使用
    1. 为什么需要babel
        1. 例如 React中JSX Es6 Ts 都要转换后 给浏览器使用
        2. 类似PostCss做转换去兼容Css
        3. babel处理JS的兼容
        4. 默认Babel无法做转换
    2. 使用Babel
        1. 安装babel的核心 @babel/core
        2. 默认无法在命令行终端使用
            1. 需要安装@babel/cli以达到支持
        3. 安装@babel/plugin-transform-arrow-functions
            1. 转换箭头函数为普通函数
        4. 安装@babel/plugin-transform-block-scoping
            1. 处理块级作用域 把 const let 转换为var
        5. 安装@babel/preset-env
            1. 相当于插件集合 可以处理es6函数
            2. 预设的 默认有很多插件
    3. Babel-loader的使用
        1. 需要写规则 {test:'/\.js/',use:'babel-loader'}
        2. 默认啥事不做需要更改格式给定插件配置
        3. use:\[{loader:'babel-loader',options:{plugins:'@babel/plugin-transform-arrow-functions','
           @babel/plugin-transform-block-scoping'}]
        4. 这时候就会成功
        5. 也可以 options:{presets:\['@babel/preset-env']}
        6. 也可以再给个targets参数 {targets:'chrome 91' }
        7. babel-loader两种配置方式
            1. .babelrc.json babel7之前使用更多
            2. babel.config.js 现在是多包管理方式 7之后这个比较多
    4. babel-polyfill
        1. polyfill是什么
            1. preset-env可能处理的不够全面
            2. polyfill是填充 可以填充这些处理不全面的
            3. --save 生产环境下也是同样需要的
            4. 安装core-js regenerator-runtime
            5. \['@babel/preset-env',{useBuildIns:false}]
                1. false 不对当前的JS处理做polyfill填充
                2. core js:3 版本需要声明core js版本
                3. 设置usage 依据用户源代码当中所使用的新语法进行填充 不会管浏览器
                4. 如果给useBuildIns参数设置为entry 那么就会根据浏览器去填充
13. webpack-dev-server
    1. 热更到浏览器 可以在build中加入--watch
        1. 缺点 某一个发生改变 所有文件都要重新编译 时间消耗多
        2. 每次编译都需要进行文件对写
        3. live server
        4. 不能实现局部刷新
    2. dev-server会把操作卸载内存中 内存的操作比我们硬盘的操作快得多
14. webpack-dev-middleware
    1. 是一个容器可以把webpack处理后的文件传递给服务器
    2. 先去拿到配置文件 再将webpack函数执行返回一个compile
    3. 将compile交给middleware容器 然后服务端去启动服务
15. HMR热替换功能
    1. 当我们像更新局部某个模块变化而不影响其他部分更新的时候
    2. 找到配置文件
        1. 写在devServer属性里面 devServer:{hot:true}
        2. 加上target属性 改为target:'web'
    3. 需要加 if(module.hot){} 判断条件
        1. if(module.hot) module.hot.accept(\['./文件名.js'],()=>{回调}
        2. 这样设置后会给当前文件设置热更新
16. React组件支持热更新
    1. JSX语法 需要babel-loader处理
    2. babel-preset-react 处理react语法
17. Vue组件支持热更新
    1. .vue需要用vue-loader进行处理
    2. 其余的与module.hot 处理一样
18. output中的path
    1. 告知webpack该去哪里产出
    2. publicPath 默认是个空字符串
        1. 告知html将来去哪个地址找资源
19. devServer中的publicPath
    1. 指定本地服务所在的目录
    2. 如果打包后的资源依赖了其他资源
    3. 我们需要添加contentBase
        1. 我们打包后的资源依赖了其他资源 此时告知去哪找
        2. path.resolve(__dirname,'public')
        3. 添加上watchContentBase属性 值为true
20. devServer配置属性
    1. hotOnly 属性 true
    2. port 指定在哪个端口开启服务
    3. open 自动打开浏览器属性
    4. compress 压缩代码为gzip
21. proxy代理
    1. 在devServer里面设置proxy:{'/api':{target:'https:xxx.xx.com',}}
    2. pathRewrite:{'^/api':''}
    3. changeOrigin true
22. resolve模块解析
    1. 路径确定后确定后续文件
        1. 绝对路径
        2. 相对路径
        3. 模块 直接找node_modules
    2. resolve属性
        1. resolve:{extensions:\['.html','.jsx','.css','.js']}}
        2. alias 属性 别名设置
23. source-map的作用
    1. mode 属性 设置为development
    2. devtool控制是否生成source-map
    3. 报错后可以定位到具体代码中 避免定位到处理后的代码
24. devtool详细说明
    1. 属性 devtool:'source-map'
    2. 会生成一个.map的文件 会把当前文件和源码文件做映射
    3. 也可以设置cheap-source-map 可以提高效率 只提供行信息不提供列信息
    4. nosources-source-map 会生成.map 只有错误提示 并没有代码文件
25. ts-loader编译ts
    1. 和less-loader有点像
        1. 底层还是依赖了ts就和less-loader底层依赖less一样
    2. 在rules里面增加规则 需要安装typescript 默认不会安装
26. babel-loader编译TS
    1. 与ts-loader差异
        1. ts-loader处理时候并不会进行polyfill 当运用了es6新概念时候 不会进行转换 只会对ts语法进行转换为js 并不会转换新的特性
        2. babel/preset-typescript 对ts进行转换 并会通过polyfill进行填充
        3. 不会对数据类型进行校验 而ts-loader会对语法操作进行校验
    2. 通过ts-compile进行查找语法校验 加在package.json里面 tsc --noEmit
27. 加载.vue文件
    1. vue-template-compile 对模板语法进行解析
    2. vue-loader处理vue文件
28. 区分开发环境
    1. webpack.common.js
    2. webpack.prod.js
    3. webpack.dev.js
    4. 可以导出一个函数 接收参数为 env 返回值为一个对象
        1. 通过在package里面设置命令后缀--env development等去判断
29. 合并生产环境配置
    1. 新建一个paths.js用来处理路径问题 到处个reportPath的方法
    2. 导入webpack-merge 中的merge函数
        1. 通过判断导入环境信息打包相关的内容
        2. 通过merge函数 合并配置信息 如prod是对象形式 dev也是对象形式 通过合并后进行使用
30. 合并开发环境配置
    1. 根据打包的模式去决定plugin的值
    2. 根据打包模式决定打包需要内容
31. 代码拆分方式
    1. 多入口方式
        1. 在entry里面 设置多个js文件 多入口
        2. output中加入\[name]生成不同文件
    2. 模块引入方式
        1. 当前如果使用的是production的话会生成text文化 显示映射关联
            1. 不想要的话 在导出里面添加optimization属性在其下面通过 new TerSerWebpackPlugin里面设置属性 extractComments false去取消生成
        2. 在entry里面可以设置对象 entry:{main1:{import:'main1.js',dependOn:'lodash'},lodash:'lodash'}
            1. 第一个是我们需要依赖的第三方路径
            2. 第二个是我们打包后的名称
            3. 多次依赖也只会生成一个
        3. 如果依赖有多个的话
            1. 设置参数shared是一个数组\['jq','lodash']
            2. main1:{import:'main1.js',dependOn:'shared'}
    3. optimization中添加新属性SplitChunksPlugin
        1. chunks:async 异步方式可以检测出来
        2. chunks:initial 同步方式可以检测出来
        3. chunks:all 异步同步方式都可以检测出来
32. splitChunks参数设置
    1. chunks 默认是对异步进行处理 initial 对同步 all 对所有 做分包
    2. miniSize 最低分包后包的大小
    3. maxSize 包大于此值的 会进行分包 并且需要大于miniSize
    4. miniChunks 至少引用几次才可以拆分
    5. cacheGroups test 正则匹配
        1. name 和 filename区别 name可以用占位符 filename是写死
        2. 先去设置一个自定义key
            1. cacheGroups:{key:{test:/\[\\\\/]node_modules\[\\\\/]/,filename:'js/\[id]_vendor.js'}}
                1. 可以设置一个priority -10属性 优先级 谁的优先级大 都符合时候执行谁
            2. 还可以设置一个 default:{miniChunks:2,filename:'js/\[id].js'}
33. import动态导入
    1. 动态导入天生就被拆包了
    2. chunksId 以哪种算法进行打包
        1. natural 自然数 当前文件按照自然数进行排序 如果当前文件不再依赖将会重新打包
        2. named 以我们当前文件名称进行打包
        3. deterministic 用chunk_id作为文件名
34. runtimeChunk优化配置
    1. 默认为false
    2. 设置为true时候 会把引入的模块 单独的处理抽离打包
        1. 好处是优于浏览器的缓存
        2. 改变的文件 重新打包 没改变的文件保持不变
35. 代码懒加载
    1. 点击后才会请求相关js文件
    2. 首屏加载不需要的内容 在需要的地方动态导入方式来懒加载
        1. import(/* webpackChunkName:'打包文件名' */'文件路径').then(({ 其他文件的内容 })=> {})
36. prefetch和preload
    1. 通过魔法注释去标记需要预加载或预获取的文件
    2. prefetch 在浏览器空闲时候回去加载 /* webpackPreFetch */
    3. preload 并行加载 是立即下载的 中等优先级 /* webpackPreLoad */
37. 第三方扩展设置CDN
    1. 边缘节点 父节点 源节点
    2. 打包第三方包并引用到我们cdn服务器
        1. 在output里面设置publicPath 设置我们cdn服务器
    3. 不打包我们第三方包
        1. 配置externals {lodash:'_'}
        2. 第一个不希望打包的三方包 第二个是当前包对外暴漏的全局名称
        3. 在index.html里面通过script链接去引入cdn包
38. 打包dll库
    1. 类似cdn
    2. 使用webpack.DllPlugin
        1. name 将来生成文件名字
        2. path 生成地图文件的绝对路径manifest.json
        3. TerserWebpackPlugin清除txt文件
39. 使用dll库
    1. 通过配置webpack.DllReferencePlugin
        1. manifest 绝对路径 找到 manifest.json
        2. context 上下文找到我们相对文件
    2. 在打包过程中不在打包第三方包了
    3. 还需要安装插件add-asset-html-webpack-plugin
        1. 默认向html里面引入文件
        2. filepath:(路径文件)
        3. 如果添加了outputPath属性设置为js 那么就会输出到js文件夹下
        4. 还需要手动更改html 里面的生成文件 因为一直是显示的auto/目标文件
40. css抽离和压缩
    1. MiniCssExtractPlugin 对css进行单独的抽离
    2. new MiniCssExtractPlugin
        1. filename 设置产出的css名称
        2. MiniCssExtractPlugin.loader 替换 style-loader
        3. 第一步抽离已实现
    3. 判断环境 在生产环境下走当前的loader 在开发环境下 仍然走style-loader
    4. 使用CssMinimizerPlugin进行压缩
    5. new CssMinimizerPlugin
        1. 属于optimization下面的minimizer属性
41. TerserPlugin压缩JS
    1. webpack5以上的版本 已经内置好了 4以下的版本需要自己安装
    2. test 匹配符合规则的文件进行处理
    3. terserOptions 自定义规则
        1. 给上一个对象
        2. 可以设置不同的属性
    4. minimize 设置false 是不做压缩
42. scope hosting
    1. webpack.optimize.ModuleConcatenationPlugin
43. usedExports属性配置 摇树 摇掉没用的代码
    1. 告知每个模块使用的导出内容 改为true时候 会标记函数 那些函数没去用
    2. minimize true 开启 表示我们使用minimizer功能
    3. minimizer 开启terserPlugin extractComments false
44. sideEffects配置
    1. 处理副作用
    2. 在package.json 加入sideEffects属性 标为false 表示不使用模块副作用 true的话表示使用
        1. 可以给为数组 表示允许哪些文件副作用 \['src/own.js'] *.css 匹配所有css
    3. 在 rules规则css中加入sideEffects true
45. Css-TreeShaking
    1. css中没用的元素去摇掉
    2. purgecss-webpack-plugin
        1. 引入plugin
        2. 传参
            1. paths 路径 找到我们的css glob.sync(文件，{nodir：true}) 是文件不是目录
                1. 安装glob库
                2. 注释掉也算有这个元素 只有删除 才会被认为没有这个元素
            2. 加入safelist 是一个函数 safelist:function(){return{standard:\['div','html','body',] }} 标记后不会摇掉设置的内容
46. 资源压缩
    1. compression-webpack-plugin
    2. 引入plugin
        1. test 匹配需要压缩的内容
        2. minRatio 压缩后的大小除以压缩前的大小 比例值 0.2===20%
        3. threshold 字节 大于这个字节就会压缩
        4. algorithm 选择压缩算法 gzip
47. inlineChunkHtmlPlugin
    1. 向当前html里面注入一些内容
    2. 传两个参数
        1. html-webpack-plugin
        2. test 正则匹配
        3. new InlineChunkHtmlPlugin(HtmlWebpackPlugin,\[/runtime.*\.js/])
        4. 会把这些内容直接注入到html里面
48. webpack打包library
    1. 如何用webpack打包我们自己的库
    2. output添加libraryTarget:umd
    3. library:utils 自定义的文件名称
    4. globalObject:'this' 修改this
49. 打包时间和内容分析
    1. 分析打包时间 通过引入一个插件speed-measure-webpack-plugin
    2. const smp=new SpeedMeasureWebpackPlugin()
    3. 包裹我们最终的配置文件
    4. 绿色不用管 红色代表太慢了
    5. BundleAnalyzerPlugin 生成内容图

#### WebPack4学习

1. 模块化打包工具的由来
    1. ESModules的兼容问题
    2. 模块文件多网络请求频繁
    3. 所有的前端资源都需要模块化
    4. 将我们开发环境的问题 编译成 生产环境可以兼容的版本
    5. 支持不同种类的资源文件
2. 模块化打包工具的概要
    1. 模块加载器 对其进行编译转换
    2. 代码拆分 将应用中代码按照我们的需要进行打包
    3. 资源模块
    4. 所有打包工具都是以模块化为目标
3. Webpack4
    1. 安装webpack以及webpack-cli
    2. 书写配置文件
        1. entry设置入口文件
        2. output设置输出文件相关配置
            1. filename 文件名
            2. path 文件路径
    3. 工作模式
        1. 根据我们配置中的属性开始工作
    4. webpack打包结果运行原理
        1. 先定义对象 用于存放缓存模块
        2. 定义require函数 加载模块
        3. 挂载数据 工具函数
        4. 调用require函数
    5. webpack资源模块加载
        1. 前端工程的模块打包工具
        2. 处理 非js文件时候 需要加载器
        3. 设置module rules属性 通过test匹配需要打包的文件
    6. webpack导入资源模块
        1. 学习新事物要搞明白为什么这样设计 而不是照着文档学习怎么用
    7. webpack文件资源加载器
        1. file-loader
        2. 处理 图片 字体等文件的模块化 打包
    8. webpack URL加载器
        1. dataUrl 我们可以直接使用 而不用通过http请求 data base64
        2. url-loader会将图片转换为 dataUrl 资源
        3. 小文件使用data url 大文件单独存放 提高加载速度
        4. 添加 url-loader的配置 通过limit 去限制文件大小
    9. webpack 资源加载器分类
        1. 文件资源加载器
        2. url加载器
        3. 按类型分
            1. 编译转换类
            2. 文件操作类
            3. 代码检查类
    10. webpack 与 es2015
        1. 通过babel-loader去处理es6的转换
        2. 配置@babel/preset-env
    11. webpack加载资源的方式
        1. 遵循ESModule的import声明
        2. 遵循CommonJS的require
        3. 遵循AMD的define 和 require 函数
        4. css加载方式
            1. css-loader style-loader
        5. html加载方式
            1. html-loader
            2. 默认只会处理img src
            3. 需要添加options:{attr:['img:src','a:href']}
    12. webpack核心原理
        1. 将各个模块打包合并到bundle中 最后输出到页面里面
    13. 开发一个loader
        1. 处理结果必须是一个js代码
        2. md解析器 使用 自定义loader 读出数据返回 给html-loader
        3. 对同一资源可以使用多个loader
    14. webpack插件机制
        1. 为了增强项目自动化的能力
        2. 清除dist目录 拷贝 压缩
    15. webpack自动清除输出目录插件
        1. clean-webpack-plugin
    16. 自动生成html插件
        1. html-webpack-plugin
        2. 传入参数
            1. {title 设置标题，viewport 设置标签}
        3. 大量生成 可以 自定义模板 <% htmlwebpackplugin.options.title%>
        4. 通过template 指定我们的模板
        5. 同时输出多个页面文件
        6. 多次调用htmlWebpackPlugin生成实例对象
    17. webpack插件使用总结
        1. copy-webpack-plugin 实现拷贝我们的文件
        2. {public 目录拷贝 }
    18. 开发一个插件
        1. 钩子
        2. 一个函数或者一个包含apply方法的对象
        3. 使用compile.hooks.emit 访问webpack钩子
        4. 通过endWith判断是否以js结尾
    19. webpack开发体验问题
        1. 必须用http服务
        2. 使用ajaxAPI
    20. webpack自动编译
        1. 可以自动编译 刷新页面 观看结果
    21. webpack自动刷新浏览器
        1. BrowserSync 自动刷新浏览器
        2. 监听文件
        3. 缺点 太麻烦了 需要开启两个命令行
        4. BrowserSync会读写磁盘 两次磁盘操作
    22. webpack DevServer
        1. 可以自动编译和自动刷新浏览器
        2. webpack-dev-server
    23. webpack-dev-server静态资源访问
        1. 写入到配置当中
        2. contentBase 额外指定一个静态资源目录
    24. 代理API服务器
        1. 利用webpack-dev-server的proxy
    25. SourceMap
        1. 解决的就是开发环境无法定位到问题
    26. webpack配置sourceMap
        1. devtool 配置工具
        2. 设置为sourceMap
    27. webpack eval模式下的sourceMap
        1. 不会生成sourceMap文件
        2. 报错时候会定位到文件 不会定位到行列信息
    28. devtool模式对比
        1. 可以webpack返回一个数组 每个数组传入不同对象 进而生成不同的打包结果
        2. 生成不同的文件后可以通过访问不同文件进而分析不同文件下的打包结果
        3. nosources-source-map 保护源代码 提示行信息 但不显示源代码
    29. 选择source-map模式
        1. 开发环境
            1. cheap-module-eval-source-map
                1. 缩小版的source map 只定位行信息
        2. 生产环境
            1. nosources-source-map
                1. 保护源代码
    30. 自动刷新问题
        1. 额外代码刷新前保存 页面状态不丢失
        2. 模块更新
    31. HMR模块热替换
        1. 热替换不影响当前页面的状态
    32. 开启HMR
        1. 通过webpack-dev-server --hot
        2. 或者配置devServer属性hot 为 true
    33. HMR疑问
        1. 手动处理模块热更新逻辑
        2. 样式自动替换
        3. js文件无规律所以没法自动替换 所以只能手动加入热更新逻辑
    34. 使用HMR API
        1. 通过module.hot.accept 方法去接收一个模块
            1. 第一个参数是文件 第二个参数是处理后的回调函数
    35. 处理JS热更新模块
        1. 函数重新创建 并添加到我们的页面中
        2. 原来的状态也需要保存下来
        3. 通过拿到原来的页面 并重新替换到更新后的函数中 去实现热更新
        4. 不同模块不同逻辑 所以要手动设置热更新模块
    36. 图片热更新替换
        1. 通过module.hot.accept 在回调函数里面去 处理图片的src
    37. HMR注意事项
        1. 处理热替换代码有错误 将hotOnly 改为true
        2. HMR API 报错 没有开启HMR会报错 所以先去判断后去使用
    38. 生产环境优化
        1. 生产环境注重运行效率
        2. 为不同环境创建不同配置
    39. 生产环境优化以及注意事项
        1. 不同环境的配置
            1. 添加相应的配置条件 根据不同条件去导出不配置
            2. 每个环境都有一个相应的配置文件
        2. 也可以设置mode 去配置
        3. 使用webpack-merge 去合并选项
    40. definePlugin
        1. 为代码注入全局成员
        2. process.evn.NODE_ENV
        3. 通过webpack.DefinePlugin({}) 去设置常量
    41. Tree Shaking
        1. 摇树 摇掉代码中未引用的代码
        2. 生产环境下自动启用
        3. 加入optimization属性
            1. 开启usedExports 标记哪些函数未用
            2. 开启minimize  摇掉未使用函数
    42. 合并模块
        1. concatenateModules
            1. 尽可能将所有代码放在一个函数中
            2. scope hoisting 作用域提升
    43. TreeShaking和Babel
        1. 使用TreeShaking会使得Babel失效
        2. 由webpack使用时候 会用ESModule
        3. 而使用babel时候会转换ESModule 为 其他规范
        4. 所以会失效
        5. 最新版的babel不会使得 TreeShaking失效
    44. sideEffects
        1. 副作用
        2. 通过配置标记我们是否有副作用
        3. 先去配置中加入并开启
        4. 在package.json 加入sideEffects 用来标识是否有副作用 false是无副作用
        5. 确定 代码无副作用再去使用
    45. 代码分割
        1. 并不是每个模块在启动时候是必要的
        2. CodeSplitting 代码分包 分割
    46. 多入口打包
        1. 传统的多页面程序
        2. 在entry通过配置对象 去进行多个文件进行打包
        3. 在output中通过\[name]去设置多打包的结果
        4. 在htmlWebpackPlugin中配置不同的chunks  从而获取不同的文件
    47. 提取公共模块
        1. 在optimization 中配置splitChunks 并标记 'all'
    48. 动态导入
        1. 动态导入的模块会自动分包
        2. import(文件).then({default:文件内容})=>{通过append等操作注入页面}
        3. 会自动处理分包和按需加载
    49. 魔法注释
        1. /*webpackChunkName:'文件名'*/ 通过魔法注释的方法去给分包name去取相应的名字
    50. MiniCssExtractPlugin
        1. 提取css到单个文件
        2. 在plugins中导入
        3. style-loader与MiniCssExtractPlugin冲突
        4. 一个是以标签注入 一个是以文件注入
    51. OptimizeCssAssetsWebpackPlugin
        1. 通过插件将css压缩 可以添加到plugin中去处理
        2. 配置到optimization中 会根据minimize属性会启用optimization中配置的插件
    52. 输出文件缓存
        1. 开启缓存可能导致无法及时更新
        2. 需要添加hash值
        3. CompressionWebpackPlugin 插件 压缩
            1. thread 文件大小
        4. 为打包文件 添加\[name].\[chunkHash] 也可以通过hash:3 去指定hash长度
##### 其他打包工具
1. Rollup
    1. ESModule打包器
    2. 与webpack类似 小巧的多
    3. HMR Rollup没法完全支持
    4. 并不是为了竞争 只是为了充分利用ESModule特性高效打包
2. 快速上手
    1. 安装 cli工具
    2. --format 以什么格式 --file 导出文件
    3. 只会保留用到的部分 默认就有TreeShaking
3. 配置文件方式
    1. rollup.config.js
    2. 需要通过--config 去指定配置文件 默认不访问
4. 使用插件
    1. rollup-plugin-json
    2. json() 直接运行
5. 加载npm 模块
    1. rollup-plugin-node-resolve
    2. 可以直接导入node_modules中的模块了
    3. 也会被打包到bundle中
6. 加载CommonJS
    1. rollup-plugin-commonjs 插件
    2. 用来转换CommonJS模块
7. 代码拆分
    1. 动态导入
    2. import{文件}.then({log// 解构出log方法}=>{})
    3. 打包格式不能是IIFE 应该使用AMD标准 --format amd
    4. 在配置中使用dir:'dist' 指定文件夹
8. 多入口打包
    1. 将input属性使用数组形式
    2. 而且需要将打包后的格式改为amd
    3. 并加入dir
    4. 引入require js
    5. 通过data-main 选择需要引入的文件
9. 选用原则
    1. rollup
        1. 输出结果扁平
        2. 打包结果可以正常阅读
        3. 加载非ESModule比较麻烦
        4. HMR比较麻烦
    2. 开发应用程序 比较大 选用webpack
    3. 开发类库或者框架 选用rollup
    4. webpack大而全
    5. rollup小而美
10. Parcel
    1. 零配置的前端打包器
    2. 命令行使用
    3. 热更新 module.hot.accept
    4. 提供的函数 只接受一个参数
    5. 自动安装依赖 只管导入 会自动安装
    6. 下载parcel-bundle
    7. 支持动态导入方式 会自动拆分
    8. 生产模式
        1. parcel build src/index.html
        2. 多进程 同步工作
        3. 自动安装插件
        4. 构建速度快
        5. 对比webpack
            1. 更好的生态
            2. 找错误
### 规范化
#### 践行前端重要的部分

> 为什么要有规范化的标准
1. 软件开发需要多人协同
2. 不同开发者有不同的编码习惯
3. 不同的喜好增加项目维护成本
4. 每个项目或者团队需要明确统一的标准
> 哪里需要规范化标准
1. 代码 文档 提交日志都需要规范
2. 开发过程中人为编写的成果
3. 代码规范标准化最为重要
> 实施规范化的方法
1. 编码前人为的标准约定
2. 通过工具实现Lint

#### ESLint
1. 介绍
    1. 最为主流的lint工具 用来检测代码质量
    2. 很容易统一开发者的编码风格
    3. 可以帮助开发者提升编码能力
2. 安装
    1. 初始化项目
    2. 安装ESLint模块为开发依赖
    3. 通过CLI命令校验安装结果
3. 快速上手
    1. 编写 问题 代码 检测下eslint是否工作
    2. 使用eslint相关命令去检查
    3. 完成eslint使用配置
    4. 通过npm eslint --init 去初始化
4. 配置文件解析
    1. env用来解析运行在那个环境中
    2. 在globals中将window document navigator选项设置了全局成员 值设为readonly
    3. extends 共享一些配置
    4. parserOptions 使用es新特性 设置一些属性
    5. rules 开启一些规则 属性值of warn error
5. ESLint配置注释
    1. 直接通过注释方式 去注入到代码中 从而不去校验某块代码
6. ESLint结合自动化工具
    1. 集成后 一定会工作
    2. 整合在一起 会更方便 与项目统一
    3. 使用standard风格
7. 结合webpack
    1. 安装对应模块
    2. 安装eslint
    3. 安装eslint-loader
    4. 初始化.eslintrc.js配置文件
8. 现代化项目继承ESLint
    1. 现在的cli都已经继承了ESLint
9. ESLint检查TypeScript
    1. 初始化Eslint
    2. 通过自行配置去校验
10. StyleLint认识
    1. 提供默认的代码检测
    2. 提供了CLi工具
    3. 通过插件支持 less sass postCss
    4. 支持gulp和webpack 集成
    5. 通过命令行终端使用
    6. 在项目终端添加配置文件
11. Prettier的使用
    1. 通用的前端代码格式化工具
    2. 通过命令行 --write 文件名 去进行格式化
    3. 通过 . --write 去格式化所有的代码
12. Git Hooks 工作机制
    1. 防止提交前未格式化代码
    2. 在git的hooks子目录
    3. 发现很多sample文件
    4. pre-commit 就是在commit前的一些检测
13. Eslint 结合 Git Hooks
    1. Husky模块
    2. 在package json 中定义husky属性
    3. 将pre-commit设置 npm run lint 我们设置的eslint命令


####WebPack源码解析
1. 打包后文件分析
    1. 打包后的文件就是一个函数的自调用 当前函数传入一个对象
    2. 这个对象我们为了方便将之成为模块定义 他就是一个键值对
    3. 这个键名就是当前被加载模块的文件名和某个目录的拼接
    4. 这个键值就是一个函数和node.js里的模块加载类似 会将被加载模块中的内容包裹在一个函数中
    5. 这个函数在未来的某个时间点上会被调用 同时会接收到一定的参数 利用这些参数就可以实现模块的加载操作
    6. 调用webpack require 方法去导入模块
    7. 针对上述概念就是相当于将模块定义部分传给了modules
2. 单文件打包后源码调试
    1. installModules是什么
    2. webpack.require方法上挂载一些属性
    3. m 用于存放modules
    4. c 用于挂载缓存
    5. d 挂载getter
    6. r 挂载ESModule
    7. p publicPath
    8. s 赋值当前的文件路径 传入到我们的webpack.require 形参 moduleId
    9. 用我们当前的moduleId作为键 挂载到installModules对象中
        1. i:moduleId l:false exports:{}
    10. 通过module\[moduleId].call 通过ID拿到函数去调用
        1. 值就是我们的模块定义函数
        2. 如果需要递归就 可以通过exports往下传
        3. 走完后 我们导出的 module.exports 就挂载上我们文件里面的导出内容
3. 功能函数说明
    1. 还是传入了一个模块定义 不过这次打包两个文件 会生成两个键值对
    2. 一个传入了webpack_require 因为需要递归处理
    3. l标记为true 标识当前已经加载过了
    4. \_\_webpack_require__
        1. webpack自定义的一个加载方法 核心功能就是返回被加载模块中导出的内容
        2. .m 将模块定义保存一份 挂载到自定义方法身上
        3. .c 与m方法类似
        4. .o 方法判断被传入的对象obj身上是否有指定的属性 有就是true
        5. .d 方法 处理exports当前是否具备name属性 有的话 设置访问器 可枚举
        6. .r 方法 通过是否存在Symbol判断当前环境是否属于ESModule
            1. 加入Symbol.toStringTag方法
            2. 不成立 也人为的添加上一个__esmodule属性
        7. .t 方法 提供了四个开关
            1. 调用t方法后我们会拿到被加载模块中的value
            2. 对于value我们可能直接返回 也可能处理之后返回
        8. .n 方法
            1. 定义了getter 并返回了getter
        9. .p方法 publicPath 值 会在这
        10. .s 方法 接收我们模块定义中的键
4. CommonJS模块打包
    1. 尝试用自定义的require方法去加载内容 \_\_webpack_require__
5. ESModuleJS模块打包
    1. 先去找缓存找不到后就通过webpack的require方法导入并标记ESModule
    2. 传递给n方法
    3. n方法
        1. 判断传进来的module身上有没有__module没有的话添加上
        2. 然后调用d方法设置一个a属性 getter传出
6. 功能函数手写实现
    1. 定义对象用于将来缓存被加载的模块
    2. 定义一个webpack require方法 来替换import require 加载操作
        1. 判断当前缓存中是否存在要被加载的模块内容 如果存在则直接返回
        2. 如果不存在我们需要自己定义{}加载被导入的模块内容
        3. 调用当前moduleId对应的函数 然后完成内容加载
        4. 当上述方法调用完成后 我们就可以修改l的值为true表示当前模块已经加载完成了
        5. 加载工作完成后 要将拿回来的内容返回至调用的位置
    3. 定义m属性用于保存modules
    4. 定义c属性用来存储缓存
    5. 定义o方法用来判断对象的身上是否存在指定的属性
    6. 定义d方法用于在对象上添加指定的属性 同时给属性添加getter
    7. 定义r方法用于标记当前模块时es6类型
    8. 定义n方法用于设置具体的getter
    9. 定义p属性用来保存资源访问路径
    10. 调用webpack_require方法执行模块导入与加载操作
7. 懒加载内容实现流程梳理
    1. 调用e方法并在then回调中调用t方法
    2. 运用JSONP去加载js 核心原理
        1. 通过mode&1  7&1 成立 为true 0111 0001
    3. t方法可以针对内容进行不同处理 取决于传入的数值 二进制
8. t方法实现以及分析
    1. 接收两个参数 value mode 第一个用来表示被加载模块ID 第二个是二进制数值
    2. t方法内部做的第一件事 调用自定义的require方法加载value对应的模块内容导出 重新赋值给value
    3. 当获取到了value值后 余下的2 4 8 ns 都是对当前的内容进行加工处理 然后返回使用
    4. 当mode与上8 或者1成立时 直接返回value CommonJS
    5. 如果1和4成立了 也会返回value 但是返回的是加工过的 esModuleJS
    6. 如果上述条件不成立 还是要继续处理value 定义一个ns{}
        1. 如果拿到的value是一个可以直接使用的内容 挂载到ns的default属性上
9. 懒加载源码分析
    1. 将import关键字替换为webpack_require方法
    2. 通过 window\['webpackJSONP'] 调用webpack 的jsonp方法
    3. 定义了webpackJsonpCallback方法
    4. 定义了e方法
    5. 创建promise数组 存放
    6. 将文件内容通过append添加到head里面
    7. 跳进t方法里面进行加载执行
10. 懒加载手写实现
    1. 定义变量存放数组
        1. 查找window\[webpackJsonp] 查找是否有 有的话 就取用 没有就赋值\[]后取用
        2. 重写了数组原生的push方法
            1. webpackJsonpCallBack实现
            2. 获取需要被动态加载的模块ID 参数1
            3. 获取需要被动态加载的模块依赖对象 参数2
            4. 循环判断chunkId里对应的模块内容是否已经完成了加载
    2. 定义e方法
        1. 实现jsonp来加载内容
        2. 利用promise来实现异步加载操作
        3. 主要实现
            1. 定义一个数组用于存放promise
            2. 获取chunkId对应的chunk是否已经完成了加载
            3. 依据当前是否已经完成加载的状态来执行后续的逻辑
            4. 定义jsonpScriptSrc实现
                1. 创建script标签 创建scr标签  合并标签并写入到页面中
11. webpack与tapAble
    1. webpack编译流程
        1. 配置初始化
        2. 内容编译
        3. 输出编译后的内容
    2. 事件驱动型的事件工作流机制
    3. tapAble是一个独立的库
        1. 实例化hook的监听
        2. 通过hook触发事件监听
        3. 执行懒加载生成的可执行代码
    4. hook本质就是tapAble中的实例对象
        1. 执行特点
        2. Hook：普通钩子 监听器之间互不干扰
        3. BailHook：熔断钩子 某个监听返回非undefined时后续不执行
        4. WaterfallHook：瀑布钩子 上一个监听的返回值可以传递给下一个
        5. LoopHook：循环钩子 如果当前未返回false 则一直执行
    5. tapAble同步钩子
        1. 同步钩子
            1. SyncHook
            2. SyncBailHook
            3. SyncWaterfallHook
            4. SyncLoopHook
        2. 异步串行钩子
            1. AsyncSeriesHook
            2. AsyncSeriesBailHook
            3. AsyncSeriesWaterfallHook
        3. 异步并行钩子
            1. AsyncParallelHook
            2. AsyncParallelBailHook
12. 同步钩子使用以及测试
    1. SyncHook
        1. 导入我们想要的钩子类 通过new SyncHook生成钩子实例
        2. 实例化实例
        3. 添加事件监听 hook.tap定义钩子函数
        4. 触发事件监听 hook.call执行钩子函数
        5. 从上往下触发 高内聚低耦合
    2. SyncBailHook
        1. 与上面步骤保持一致
        2. 如果某一个钩子函数中有返回了 则后面不会执行 熔断
        3. 但是如果返回的是undefined则还会继续执行
    3. SyncWaterfallHook
        1. 与上面步骤保持一致
        2. 返回值将会作为下一个钩子的参数
    4. SyncLoopHook
        1. 与上面步骤保持一致
        2. do while循环 直到 钩子函数返回的是undefined的时候会跳过
        3. 否则的话就一直执行钩子 从头开始  直到所有函数返回的都是undefined
13. 异步钩子的使用
    1. 对于异步钩子的使用 再添加事件监听时会存在三种方式
        1. tap tapSync tapPromise
    2. 异步并行钩子
        1. AsyncParallelHook
            1. 钩子函数的使用一样
            2. 使用tapSync 并行处理
        2. AsyncParallelBailHook
    3. 异步串行钩子
        1. AsyncSeriesHook
        2. AsyncSeriesBailHook
        3. AsyncSeriesWaterfallHook
14. 实现SyncHook类
    1. 新建一个类 继承于我们的Hook父类（新建我们的Hook父类）
    2. Hook父类
        1. constructor中 接收一个默认值为[]的args参数
            1. this.args=args 赋值args
            2. 定义taps 将来用于存放组装好的{}
            3. 定义_x 默认值为undefined 将来在代码工厂函数中会给_x赋值
        2. 创建tap方法
            1. 通过options关键字接收信息
                1. 对options做合并处理
            2. 第二个参数接收fn
            3. 调用_insert方法将组装好的options添加到[]中
        3. 创建_insert方法
            1. 调用tap方法并传入参数
        4. 创建call方法
            1. 创建将来要具体执行的函数代码结构
            2. 调用_creatCall函数  通过 callFn.apply 将this传入进去
        5. 创建_creatCall方法
            1. 调用compile方法 应由我们的子类去实现
            2. 传入参数  taps args
    3. 子类实现compile方法
        1. 调用工厂函数中的factory.setup(this,options)
        2. 返回 factory.creat(options)
    4. 定义HookCodeFactory类
        1. 设置setup函数
            1. 先准备后续需要使用到的数据
        2. 设置create函数
            1. 核心就是创建一段可以执行的代码体然后返回
        3. 设置header函数 用于创建新函数时候 做拼接插入工作
15. 实现AsyncParallelHook方法
    1. 主要还是在父类Hook里面进行更改
    2. 新增tapAsync
16. 定位webpack打包入口
    1. cmd文件的核心就是定位到具体文件
    2. webpack下面的bin目录的cli文件
    3. 拿到用户命令行信息
    4. 拿到后去我们业务模块进行处理分发
17. 编译主流程调试
    1. compiler编译函数
    2. 拿到options.plugins数组
    3. 需要compiler具有读写之后 将plugin挂载
    4. 重新赋值了plugin
    5. 钩子函数
        1. beforeRun
        2. run
        3. thisCompilation
        4. compilation
        5. beforeCompile
        6. compile
        7. make
        8. afterMake
18. 手写webpack 实现
    1. 提供一个webpack变量是一个函数
        1. 接受两个参数 options callBack（暂时不处理）
    2. 实例化compiler
        1. 新建一个compiler类 并作初始化
        2. 将options挂载到compiler实例对象上
        3. 需要安装tapAble库 去实现钩子 版本1 有tapAble模块 2里面没有
        4. 通过compiler继承 tapAble
        5. 定义run方法
            1. 参数为callback
            2. 回调里面 第一个是null 第二个是个对象
            3. 对象里面有个toJson函数 返回一个对象
                1. entries当前次打包的入口信息
                2. 当前次打包的chunk信息
                3. 模块信息
                4. 当前次打包生成的最终资源
    3. 初始化nodeEnvironmentPlugin 具备文件读写能力
        1. 新建一个NodeEnvironmentPlugin类
        2. 接收options并保存到当前类身上
    4. 挂载所有的plugins至compiler对象身上
        1. 将options上的plugins挂载到compiler身上
        2. 通过插件身上的apply方法 调用plugin.apply(compiler)
    5. 挂载所有的webpack内置的插件（入口）
    6. 返回compiler实例对象
19. EntryOptionPlugin分析
    1. 有一个apply方法
    2. 调用compiler.hooks.entryOption.tap 注册了监听
    3. 在new完entryOptionPlugin时候就调用了
    4. itemToPlugin 是一个函数 接收三个参数 context item main
    5. SingleEntryPlugin
        1. 在调用itemToPlugin的时候返回一个对象
        2. 有一个构造函数 负责接收上下文中的context entry name
        3. compilation钩子监听
        4. make钩子监听
20. EntryOptionPlugin手写
    1. 新建webpackOptionApply 类
        1. 创建process方法
        2. 接收两个形参  options compiler
        3. 调用实例new EntryOptionPlugin.apply(compiler) 方法
    2. 新建entryOptionPlugin类
        1. 创建apply方法
        2. 调用 compiler.hooks.tap方法
        3. 调用itemToPlugin方法
    3. 新建SingleEntryPlugin类
        1. 初始化apply方法并添加钩子 compiler.hooks.make.tapAsync
    4. 挂载webpackPlugin 内置入口
21. run方法的流程以及实现
    1. 先定义finalCallBack函数
    2. 定义onCompiled函数
22. compile实现及分析
    1. newCompilationParams方法调用 返回了params normalModuleFactory
    2. 上述操作为了获取到params
    3. 接着调用beforeCompile钩子监听 在他的回调中触发了compile监听
    4. 调用了newCompilation方法传入了params  并返回了 compilation
    5. 调用了createNewCompilation
    6. 上述操作完成后就可以触发我们的make钩子监听
23. make流程回顾
    1. 实例化compiler对象（会贯穿工作的整个流程）
    2. 由compiler调用run方法
    3. compiler实例化操作
        1. compiler继承tapAble 因此具备了钩子的能力
        2. 在实例化compiler对象之后就往他的身上挂载了很多属性 其中NodeEnvironmentPlugin这个操作就让他具备了文件读写的能力（我们自己写时候用的是node的fs模块）
        3. 具备了fs操作能力之后 又将plugins中的插件挂载到compiler对象身上
        4. 将内部默认的插件于compiler建立关系 其中EntryOptionPlugin处理了入口模块的ID
        5. 在实例化Compiler的时候只是监听了make钩子 SingleEntryPlugin
            1. 在SingleEntryPlugin模块的apply方法中由两个钩子监听
            2. 其中compilation钩子就是让compilation具备了利用normalModuleFactory工厂创建一个普通模块的能力
            3. 因为他就是利用一个自己创建的模块来加载需要被打包的模块
            4. addEntry方法调用
        6. run方法执行
            1. run 方法里就是一堆钩子按着顺序触发
            2. compile方法执行
                1. 准备参数
                2. 触发钩子beforeCompile
                3. 将第一步的参数传给一个函数 开始创建一个compilation
                4. 调用newCompilation内部
                    1. 调用了creatCompilation
                    2. 触发了this.compilation钩子和compilation钩子的监听
            3. 当创建了compilation对象后就触发了make钩子
            4. 当我们触发了make钩子监听的时候 将compilation对象传递了过去
24. addEntry流程分析
    1. make钩子在被触发的时候接受了一个compilation对象实例 他身上挂载了很多属性
    2. 从compilation身上解构出三个值
        1. entry 当前需要被打包的模块的相对路径
        2. name main
        3. context 当前项目根路径
    3. dep是当前入口模块中的依赖关系进行处理
    4. 调用了addEntry
    5. 在compilation的实例身上有个addEntry 然后内部调用了_addModuleChain方法 去处理依赖
    6. 在compilation中我们通过NormalModuleFactory工厂来创建一个普通的模块对象
    7. 在webpack内部默认开启了一个100并发量的打包操作 当前我们看到的normalModule.create()
    8. 在beforeResolve里面会触发一个factory钩子监听（这个部分其实是处理loader）
    9. 上述流程完成之后获取一个函数被存在factory里面 然后对他进行调用
    10. 在这个函数调用里又触发了一个叫resolver的钩子（处理loader的 拿到了resolver意味着所有loader都处理完毕）
    11. 调用resolver方法后 就会进入afterResolve这个钩子里面 触发 new NormalModule
    12. 在完成上述操作后就将module进行了保存和一些其他属性的添加
    13. 调用build方法开始编译
25. addEntry初始化
    1. 使用compilation.addEntry 所有需要先创建compilation类
    2. addEntry
        1. context
        2. entry
        3. name
        4. callBack
26. _addModuleChain方法实现
    1. context
    2. entry
    3. name
    4. callBack
    5. 调用normalModuleFactory.create
27. buildModule实现
    1. 定义方法
    2. 需要接收两个参数
        1. Module 需要操作模块
        2. callback 回调
    3. 调用module.build方法
        1. 方法在NormalModule中进行声明定义
        2. 有两个参数compilation和callback
    4. doBuild方法 语法树处理
28. build以及parse实现
    1. 引入babylon
    2. 将parse类继承于tapAble类
    3. 并返回babylon.parse
    4. 新建stats函数
        1. 设置toJson方法
29. 依赖模块处理
    1. 从文件中读取相关module通过相关loader转为ast语法树
    2. 导入 babel traverse 遍历ast语法树做相关处理
    3. 导入 babel generator 作语法树的翻译
30. 最终生成重要步骤
    1. chunks
        1. 在make钩子触发时 去处理chunk并写入到指定的文件
        2. 然后输出到dist目录
        3. 通过compilation.seal方法
    2. assets
