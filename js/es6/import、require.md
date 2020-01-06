### import、require、模块化规范

#### 一. import、require
遵循规范
- require是AMD规范引入方式
- import是ES6的一个语法标准，如果要兼容浏览器必须转化成ES5的语法

调用时间
- require是运行时调用，所以require理论上可以运用在代码的任何地方
- import是编译时调用，所以必须放在文件开头

本质
- require是赋值过程，
其实require的结果就是对象、数字、字符串、函数等
再把require的结果赋值给某个变量
- import是解构过程
但是目前所有的引擎都还没有实现import，
node中使用babel支持ES6，将ES6转码为ES5再执行，import语法会被转码为require

#### 二、模块化规范
#####  CommonJS
服务器端模块的规范，Node.js采用了这个规范
- **一个单独的文件就是一个模块**，加载模块- 使用require, 返回文件内部的exports对象
- require是**同步**的， **只有加载完成才能执行后边的操作**
    ```js
    var math = require('math')
    math.add(2, 3)
    // 第二行math.add(2, 3)，在第一行require('math')之后运行，因此必须等math.js加载完成
    // 如果加载时间很长，应用会暂停
    ```
    node.js用于服务器编程，加载的模块文件存在服务大本地，比较快
    如果是浏览器环境，需要去服务器加载模块，就必须采用异步模式，AMD/CMD都是异步方案

##### AMD
AMD是**RequireJS**在推广过程中对模块定义的规范化产出
- **原理**：
    ```
    define(['Module1', 'Module2'], function(Module1, Module2){
        function foo() {
            Module1.test()
        }
        return {foo: foo}
    })
    ```

- 格式：
    **require([module], callback)**
    举例：
    ```
    require(['math'], function (math) {
    　　math.add(2, 3);
    });
    ```
    目前，主要有两个JS库实现了AMD规范：require.js和curl.js
- **AMD规范允许输出模块兼容CommonJS规范**
    ```
    define(function (require, exports, module) {
        
        var reqModule = require("./someModule");
        requModule.test();
        
        exports.asplode = function () {
            //someing
        }
    });
    ```
    优点：适合在浏览器中异步加载模块，可并行加载多个
    缺点：
    - 提高了开发成本，并且不能按需加载
    - 并且必须提前加载所有资源

##### CMD
AMD是**SeaJS**在推广过程中对模块定义的规范化产出
1. AMD/CMD区别：
    - 对于依赖的模块
        - AMD提前执行，RequireJS从2.0开始，也可以改为延迟执行
        - CMD延迟执行
    - .AMD推崇依赖前置**在定义模块时声明其依赖的模块**
    CMD推崇依赖就近 **只有在用到某个模块时再去require——按需加载**

2. **原理**
    ```
    define(function (requie, exports, module) {
        //依赖可以就近书写
        var a = require('./a');
        a.test();
        ...

        //软依赖
        if (status) {
            var b = requie('./b');
            b.test();
        }
    });
    ```