 ## defineComponent

 - 函数   接受一个 对象的参数  返回一个对象 可以 让对象获得更多的类型提示
 - 可以兼容Vue2
 - vue3新增的属性  setup

## setup
- 两个参数， props    context
- props
- context  有三个属性  attrs  slots emit
## Teleport  瞬间移动
- Dialog 被包裹在其他组件之中，容易被干扰
- 样式也在其他组件中，容易变得非常混乱
- 使用  标签包裹  传送传送渲染至 对应选择器的 DOM节点中
```html
<teleport to="" ></teleport>
<!-- to 里面写  类似 queryselect 的选择器  -->
```