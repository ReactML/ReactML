# Proposal for React Markup Language（RML）

[中文文档](./README.zh_CN.md)

RML is a simple, easy-to-use, full-featured template markup language that can help developers better separate logic and view.
RML is a continuation of JSX & [JSX+](https://github.com/jsx-plus/jsx-plus) in syntax style, but its routines are not variables. You cannot determine whether to generate an instance by dynamically executing logic, but you can use x-if and x-for instead of corresponding capabilities.

## Problem to solve

1. Difficulty in implementation with some pre-compiled scenarios. Sometimes it is necessary to compile a multi-tree describing the DOM structure in advance. If this problem can be solved, the applet, Eagel, SSR, and Solution solutions will be simpler and more efficient
2. Visualize the output, you need to be able to edit it twice
3. JSX has poor development experience on some common features (such as loops, conditions). The template syntax can make up for this.

## Template Function
- Scope
  - The accessible variables in the template come from the scope, which is an externally assigned object
- Child node `slot`
- Condition x-if
  - You can fill in any value in condition judgment, same as JSX +
- Loop with directive: `x-for`
  - Same as JSX +
- Value interpolation `{foo}`
  - Performs the same as JSX, but does not allow template insertion
  - Allow JS literals, variables, expressions
- Component import (import)
  - The import syntax is a reference statement that will be replaced with an import statement when compiled into JS. The path pointed to by src is the JS path by default

## Template Stynax

- Follow XML template syntax
- import tag, used to import template dependencies or other parts
- from specifies the path relationship with the current template
- class / className is ok
- Support JSX+
- `{}` syntax is the way to bind value, which can bind any data in scope



**Example**

```jsx
// ----- component.rml -----
<script>
 import {useState, useCallback} from 'rax';
  
 export default function (props) {
  const [foo, setFoo] = useState (true);
  const handleClick = useCallback ((evt) => {
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
  {"Interpolation"} // Literal literal
  {foo} // Identifier variable
  {foo + '...'} // Expression binary expression
  {foo? '1': '2'} // Expression ternary expression
  {fn (...)} // Expression
  {...} // other expressions
  <View x-slot:header className="foo" onTouchMove={handleClick}> property binding, event handler binding </View>
  <View x-for="item in list"> loop node {item} </View>
  <View x-if={isLoading}> Condition node if </View>
  <View x-elseif={condition}> condition node elseif </View>
  <View x-else> condition node else </View>
  <slot name="header"> Child nodes </slot>
</View>
```