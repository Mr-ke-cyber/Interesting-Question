----
2020-03-03
# 1. express中app和router的区别？
express.Router 可以认为是一个微型的只用来处理中间件与控制器的app，它拥有和app类似的方法，例如get、post、all、use等等。
router它解决了直接把app暴露给其他模块使得app有被滥用的风险。
express主要是基于Node http模块实现的，也就是说express是一个HTTP Server端框架。








