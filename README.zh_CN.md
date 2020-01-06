# React Markup Language（RML）

RML 是一种简洁、易使用的全功能模板标记语言，它帮助开发者更好的分离逻辑与视图。

语法风格上 RML 是 JSX & [JSX+](https://github.com/jsx-plus/jsx-plus) 的延续，但是它的节点不是变量，不能通过动态执行逻辑来决定是否生成某个节点，不过你可以用 `x-if` 和 `x-for` 替代对应能力

## 解决的问题

1. 某些预编译的场景下实现困难，有时候需要在构建时提前编译出描述 DOM 结构的多叉树，如果能解决这个问题，小程序、Eagel、SSR、Solution 方案会更简单高效
2. 可视化搭建产出物，需要做到可二次编辑
3. JSX 在部分常用特性(如循环, 条件)的开发体验上比较差，模板语法可以弥补这一方面的缺憾

## 可预见的使用场景

1. SSR PreRender / Eagel / TNode 模板预编译型，直出式渲染方案
2. 小程序转换
3. 可视化搭建 Pro Code 与 Low/No Code 互转
4. 普通 Rax/React 业务场景

## 模板功能

- 作用域
  - 模板中可访问的变量来自于作用域，这个作用域是外部赋予的对象
- 子节点 `slot` 
- 条件 `x-if` 
  - 条件判断中可以填写任意值，同 JSX+
- 循环 `x-for` 
  - 同 JSX+
- 插值 `{ foo }` 
  - 表现同 JSX，但是不允许插入模板
  - 允许 JS 字面量、变量、表达式
- 组件引入 (import)
  - import 语法是一种引用声明，将在编译成 JS 时替换成 import 语句，src 指向的路径默认是 JS 路径



## 模板语法

- 遵循 XML 方式的模板语法
- import 标签，用来引入模板依赖的模板或者其它部分

- - from 用来指定与当前模板的路径关系

- class/className 都可以
- 支持 JSX+
- `{}` 语法为值绑定语法，可以绑定作用域下任意数据



**Example**

```jsx
// ----- component.rml -----
<script>
 import { useState, useCallback } from 'rax';
  
 export default function (props) {
  const [foo, setFoo] = useState(true);
  const handleClick = useCallback((evt) => {
    // todo with evt.
  }, [foo]);

  return {
    color: 'yellow',
    handleClick,
    foo,
  };
}
</script>

<style>
  @import './index.css';
  .foo {
    color: red;
  }
</style>

<import default="View" from="rax-view" /> // import View from 'rax-view';

<View>
  { "插值" } // Literal 字面量
  { foo } // Identifier 变量
  { foo + '...' } // Expression 二元表达式
  { foo ? '1' : '2' } // Expression 三元表达式
  { fn(...) } // Expression 调用表达式
  { ... } // 其它表达式
  <View x-slot:header className="foo" onTouchMove={handleClick}>属性绑定、事件处理器绑定</View>
  <View x-for="item in list">循环节点 {item}</View>
  <View x-if={isLoading}>条件节点 if</View>
  <View x-elseif={condition}>条件节点 elseif</View>
  <View x-else>条件节点 else</View>
  <slot name="header">子节点</slot>
</View>
```

### 