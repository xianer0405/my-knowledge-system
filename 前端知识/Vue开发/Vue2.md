1. Provide和Inject问题
缺点：绑架了上层组件，当组件复用时，必须提供一个provide，否则相关逻辑就会不work

2. 复杂函数
function createFunc () {
    // 逻辑代码

    function f1(){}

    function f2(){}

    function f3(){}

    // f1,f2,f3是函数内部的辅助方法

    return function realFunc() {

    }
}