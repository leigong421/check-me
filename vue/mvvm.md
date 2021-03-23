# MVVM
- M: model(数据模型)
- V: view(视图DOM)
- VM: viewModel (视图模型)

- viewModel和model通过数据绑定, viewModel和view之间通过dom监听响应数据
- 是通过数据劫持+发布订阅订阅模式实现双向绑定的

## vue的双向绑定

1. vue2.x实现方式
- vue2.x是对传入的数据循环遍历通过Object.defineProperty对传入 **对象属性** 进行劫持(监控各属性的 getter 、setter);
- 在使用对象属性的时候,通过getter收集到使用对象属性的watcher放入Dep依赖中;
- 在改变对象属性的时候,通过setter触发Dep依赖中对应的watcher;
- 触发watcher后,通过解析器Compile解析模板中的directive指令,收集指令所依赖的方法和数据,等数据变化然后进行视图的更新(Updater),

2. 缺点
- 无法对对象的新增和删除进行响应,只能针对已有的属性进行监听,
- 只能劫持对象的属性,因此需要对每个对象的属性进行遍历,如果属性值也是对象那么需要深度遍历;
- 无法监听到通过**数组索引**改变数组值的变化,以及通过改变**数组长度length**的变化
- 对数组的push(),pop(),shift(),unshift(),splice(),sort(),reverse()等七种方法在底层重写实现的数据响应;
-  需要通过vue.$set()来对数组改变以及对象的新增和删除属性实现响应,
- 可以兼容到ie8


2. vue3.x实现方式
- vue3.x是对传入的数据通过Proxy对传入的**整个对象**进行监听返回一个新对象,解决了对象的新增和删除无法响应的问题,以及数组通过改变索引和长度无法响应的问题;
- Proxy有多达13种拦截方法,不限于apply、ownKeys、deleteProperty、has等等是Object.defineProperty不具备的
- Proxy返回的是一个新对象,我们可以只操作新的对象达到目的,而Object.defineProperty只能遍历对象属性直接修改。
- Proxy作为新标准将受到浏览器厂商重点持续的性能优化，也就是传说中的新标准的性能红利。
- Proxy的劣势就是兼容性问题,而且无法用polyfill磨平

