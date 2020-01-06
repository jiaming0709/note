### promise原理
#####状态：
- pending
- resolve
- reject
> - 状态改变只能是pending->resolve, 或者pending->reject 状态改变不可逆；
> - promise的then方法接收两个可选参数，表示promise状态改变时的回调；
>- then 方法返回一个promise,then方法可以被同一个promise调用多次；

```js
function PromiseFn(callback) {
    try {
        callback(resolve, reject)
    } catch (e) {
        reject(e)
    }
    let self = this
    self.status = 'pending'
    self.resolveValue = undefined
    self.rejectValue = undefined
    
    self.keepResolveFn = []
    self.keepRejectFn = []

    function resolve(value) {
        if(self.status == 'pending') {
            self.status = 'resolve'
            self.resolveValue = value
            self.keepResolveFn.forEach(fn => fn())
        }
    }
    function reject(value) {
        if(self.status = 'pending) {
            self.status = 'reject'
            self.rejectValue = value
            self.keepRejectFn.forEach(fn => fn())
        }
    }
    function all() {}
    fimction race() {}
}
promiseFn.prototype.then = function(resolveFn, rejectFn) {
    let self = this
    if(self.status == 'resolve') {
        resolveFn(self.resolveValue)
    }
    if(self.status == 'reject') {
        rejectFn(self.rejectValue)
    }
    if(self.status == 'pending') {
        self.keepResolveFn.push(()=>{
            resolveFn(self.resolveValue)
        })
        self.keeyRejectFn.push(()=>{
            rejectFn(self.rejectValue)
        })
    }
}

promiseFn.prototype.catch = function(rejectFn) {
    let self = this
    if(self.status == 'reject) {
        rejectFn(self.rejectValue)
    }
    if(self.status == 'pending') {
        self.keeyRejectFn.push(()=>{
            rejectFn(self.rejectValue)
        })
    }
}

let p1 = new PromiseFn((resolve, reject) => {
    setTimeout(()=>{
        resolve(1)
    }, 2000)
})
```

##### 总结
###### 构造函数
- callback作为形参在创建Promise对象时传入构造函数
- resolve和reject作为实参传入callback函数
- value作为实参传和resolve和reject函数
- Promise中的**resolve**和**reject**需要**异步执行**

##### then方法

###参考
[Promise完整代码](https://github.com/xieranmaya/Promise3)
