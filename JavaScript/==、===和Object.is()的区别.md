# ==、===和Object.is()的区别

1、 使用==进行判断时，如果两值的类型相同时，则比较值是否相同；类型不同时，则自动转换为Number类型再进行比较

2、 ===不会自动转换类型，如果类型不同，直接返回false

3、 Object.is()是ES6新增的比较方法，与===相比，有两点不同：

    NaN === NaN // false
    Object.is(NaN, NaN) // true

    +0 === -0 // true
    Object.is(+0, -0) // false