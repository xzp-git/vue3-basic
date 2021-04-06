 ## defineComponent

 - 函数   接受一个 对象的参数  返回一个对象 可以 让对象获得更多的类型提示
 - 可以兼容Vue2
 - vue3新增的属性  setup

## setup
- 两个参数， props    context
- props
- context  有三个属性  attrs  slots emit
## Teleport组件  瞬间移动
- Dialog 被包裹在其他组件之中，容易被干扰
- 样式也在其他组件中，容易变得非常混乱
- 使用  标签包裹  传送传送渲染至 对应选择器的 DOM节点中
```html
<teleport to="" ></teleport>
<!-- to 里面写  类似 queryselect 的选择器  -->
```
## Suspense 
- 异步请求的困境
- Suspense是Vue3推出的一个内置的特殊组件
- 如果使用Suspense, 要返回一个promise

## 全局API修改

Vue2全局API遇到的问题
- 在单元测试中，全局配置非常容易污染全局环境

- 在不同的apps中，共享一份有不同配置的Vue对象，也变得非常困难

config.productionTip 被删除

config.ignoredElements 改名为config.isCustomElement

config.keyCodes 被删除
- 全局注册类API
1. Vue.component -> app.component
2. Vue.directive -> app.directive
- 行为扩展类API
1. Vue.mixin -> app.mixin
2. Vue.use -> app.use 

## 完美的Vue实践项目是怎么样的？
- 数据的展示 - 最好是有多级复杂数据的展示
- 数据的创建 - 可以发散出多个功能
- 组件抽象 - 循序渐进的组件开发

- 整体状态数据结构的设计和实现
- 权限管理和控制
- 真实的后端API
