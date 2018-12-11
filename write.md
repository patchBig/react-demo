# react 学习

## --save 和 --save-dev 的区别

npm i 时使用 --save 和 --save-dev，可分别将依赖（插件）记录到 package.json 中的 dependencies 和 devDependencies 下面。

dependencies 下记录的是项目在运行时必须依赖的插件，常见的例如 react jquery 等，即及时项目打包好了、上线了，这些也是需要用的，否则程序无法正常执行。

devDependencies 下记录的是项目在开发过程中使用的插件，例如这里我们开发过程中需要使用 webpack 打包，而在工作中 使用 fis3 打包，但是一旦项目打包发布、上线之后，webpack 和 fis 就都没有用了。

----

## actions, reducers, store

In the previous sections,we defined the actions that represent the facts about "what happened" and the reducers that update the state according to those actions.

The Store is the object that brings them together.The store has the following responsibilities:

- Holds application state;
- Allows access to state via getState()
- Allows state to be updated via dispatch(action);
- Registers listeners via subscribe(listener);
- Handles unregistered of listeners via the function returned by subscribe(listener).

### dispatch(action)

The Store's reducing function Will be called with the current getState() result and the given action synchronously. It's return value will be considered the next state. It will be returned from getState() from now on, and the change listeners will immediately be notified.

----

## prop-types

### Usage

PropTypes was originally exposed as part of the React core module, and is commonly used with React components. Here is an example of using PropTypes with a React component, which also documents the different validators provided:

```javascript
Todo.propTypes = {
    onClick: PropTypes.func.isRequired,
    completed: PropTypes.bool.isRequired,
    text: PropTypes.string.isRequired
}
```

> <http://cn.redux.js.org/docs/basics/UsageWithReact.html>