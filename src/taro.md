# Taro 多端统一开发框架

[多端统一开发框架--Taro](https://nervjs.github.io/taro/docs/README.html)

## 为什么使用 Taro

### 1. 小程序代码组织和语法

在小程序中，一个页面 page 可能拥有 page.js、page.wxss、page.wxml 、page.json 四个文件。

![目录结构](https://upload-images.jianshu.io/upload_images/7101063-5460043562f054e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样在开发的时候就需要来回进行文件切换，尤其是在同时开发模板和逻辑的时候，切来切去会显得尤其麻烦，影响开发效率，但小程序原生只支持这么写，就显得比较尴尬了。

而在语法上，小程序的语法可以说既像 React，又像 Vue，不能说显得有点不伦不类吧，但在使用上总是感觉有些别扭，对于开发者来说，等于又要学习一套新的语法，提升了学习成本。而且，小程序的模板由于没有编辑器插件的支持，书写的时候也没有智能提示与 lint 检查，书写起来显得有些麻烦。

### 2. 命名规范

在小程序中到处可见规范不统一的情况
例如组件的属性，以最简单的 <button \/> 组件为例，在小程序官方文档中

![button](https://upload-images.jianshu.io/upload_images/7101063-ba6686d91c1f6fe3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<button \/> 组件属性名既有以中划线分割多个单词的情况 session-form，也有多个单词连写的情况 bindgetphonenumber。当然这也不是最严重的，事件绑定的规范就是 bind + 事件名 ，而其他属性的规范就是中划线分割单词

![progress](https://upload-images.jianshu.io/upload_images/7101063-7a4d5188c6a996be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

同样的情况也出现在 [页面](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page.html#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E5%9B%9E%E8%B0%83%E5%87%BD%E6%95%B0) 与 [组件](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/lifetimes.html) 的生命周期方法中，页面 的生命周期方法有 onLoad、onReady、onUnload 等，但到了 组件 中则是 created、attached 、ready 等，这样规范又不统一了，为啥 页面 的生命周期方法是 on+Xxx 的格式，但到了 组件 里缺不一样了呢，有点费解。

### 3. 开发方式

小程序官方提供了 微信开发工具 作为开发编译工具，而对于代码本身没有提供一个类似 webpack 的工程化开发工具，来解决开发中的一些问题，所以小程序原生的开发方式显得不那么现代化，这也是很多小程序开发框架致力于解决的问题。例如，在小程序开发中：

- 不能使用 npm 管理依赖，在小程序中需要手动把第三方代码文件下载到本地，然后再 reuqire 进行使用，显得不那么优雅
- 不能使用 Sass 等 CSS 预处理器，由于没有预编译的概念，小程序开发中无法使用市面上流行的 CSS 预处理器，这样会使得样式代码难以管理
- 不完整的 ES Next 语法支持，小程序默认只能支持极少一部分 ES6 规范的语法，而 ES 是不断往前发展的，一些非常优秀的新语法特性就不能使用了
- 手动的文件处理，像图片压缩、代码压缩等等的一些文件操作，必须手工来处理，显得有些繁琐

## 小程序和 react 相似的地方

### 生命周期

app 及页面的生命周期

| 小程序 | React |
| :---: | :---: |
| onLaunch | componentWillMount |
| onLoad | componentWillMount |
| onReady | componentDidMount |
| onShow | 不支持，需要特殊处理 |
| onHide | 不支持，需要特殊处理 |
| onUnload | componentWillUnmount |

### 数据更新方式

在 React 中，组件的内部数据是用 state 来进行管理的，而在小程序中组件的内部数据都是用 data 来进行管理，两者具有一定相似性。而同时在 React 中，我们更新数据使用的是 setState 方法，传入新的数据或者生成新数据的函数，从而更新相应视图。在小程序中，则对应的有 setData 方法，传入新的数据，从而更新视图。

两者都是以数据驱动视图的方式进行更新，而且 api 神似。

### 事件绑定

小程序中绑定事件使用的是 bind + 事件名 的方式，例如点击事件，小程序中是 bindtap。

```html
<view bindtap="handlClick">1</view>
```

而在 React 里，则是 on + 事件名 的方式，例如点击事件， React web 中是 onClick

```html
<View onClick={this.handlClick}>1</View>
```

虽然看上去不一样，但其实是可以类比的，只需要在编译时将 on + 事件名 的形式编译成 bind + 事件名 的形式就可以了。

## 小程序和 react 巨大的差异

React 与小程序之间最大的差异就是他们的模板了，在 React 中，是使用 JSX 来作为组件的模板的，而小程序则与 Vue 一样，是使用字符串模板的。这样两者之间就有着巨大的差异了。

jsx

```jsx
render () {
  return (
    <View className='index'>
      {this.state.list.map((item, idx) => (
        <View key={idx}>{item}</View>
      ))}
      <Button onClick={this.goto}>走你</Button>
    </View>
  )
}
```

小程序模板

```html
<view class="index">
  <view wx:key={idx} wx:for="{{list}}" wx:for-item="item" wx:for-index="idx">{{item}}</view>
  <view bindtap="goto">走你</view>
</view>
```

JSX 其实本质上就是 JS，可以在里面写任意的逻辑代码，这样一来就比字符串模板的表现力与操作性要强多了，况且，小程序的字符串模板功能比较羸弱，只有一些比较基本的功能。那这样的话，要如何来实现用 JSX 来写小程序模板呢。

## 编译原理的力量

期望使用 JSX 来书写小程序模板，但小程序显然是不支持执行 JSX 代码的（要是支持的话，Taro 应该也就不存在了吧），我们也不能期望微信能给我们开个后门来跑 JSX。那么这个时候我们就想，我们要是能够将 JSX 编译成小程序模板就好了。

事实上在我们平时的开发中，这种编译的操作到处可见，babel 就是我们最常用的 JS 代码编译器，一般浏览器是不能支持一些非常新的语法特性的，但我们又想使用它们，这个时候就可以借助 babel 来将我们的高版本的 ES 代码，编译成浏览器可以运行的 ES 代码。而我们像要将 JSX编译成小程序模板，也是同样的道理。我们首先来了解一下 Babel 的运行机制。

Babel 作为一个 代码编译器 ，能够将 ES6/7/8 的代码编译成 ES5 的代码，其核心利用的就是计算中非常基础的编译原理知识，将输入语言代码，通过编译器执行，输出目标语言的代码。编译原理的一般过程就是，输入源程序，经过词法分析、语法分析，构造出语法树，再经过语义分析，理解程序正确与否，再对语法树做出需要的操作与优化，最终生成目标代码。

![代码编译](https://upload-images.jianshu.io/upload_images/7101063-5179a0ad5c97bc14.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Babel 的编译过程亦是如此，主要包含三个阶段

- 解析过程，在这个过程中进行词法、语法分析，以及语义分析，生成符合 [ESTree](https://github.com/estree/estree) 标准 虚拟语法树(AST)
- 转换过程，针对 AST 做出已定义好的操作，babel 的配置文件 .babelrc 中定义的 preset 、 plugin 就是在这一步中执行并改变 AST 的
- 生成过程，将前一步转换好的 AST 生成目标代码的字符串

为了更好地理解这些过程，大家可以利用 [Ast Explorer](https://astexplorer.net/) 这个网站接一下自己的代码，感受一下每一部分代码所对应的 AST 结构。

![词法分析](https://upload-images.jianshu.io/upload_images/7101063-abf83f8951845b50.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，一份源码经过编译器解析后，会变成类似如下的结构

```json
{
  type: "Program",
  start: 0,
  end: 78,
  loc: { start, end }
  sourceType: "module",
  body: [
    { type: "VariableDeclaration", ... },
    { type: "VariableDeclaration", ... },
    { type: "FunctionDeclaration", ... },
    { type: "ExpressionStatement", ... }
  ]
  ...
}
```

其中，body 里包含的就是示例代码的语法树结构，第一个 VariableDeclaration 对应的是 const a = 1，第三个 FunctionDeclaration 对应的则是 function sum (a, b) { }，分别就是 JS 中的变量定义与函数定义，每一个树节点里都会包含许多子节点，这样就形成了一个树形结构，更多的节点类型，请参考 [babel types](https://github.com/babel/babel/tree/master/packages/babel-types)。

当然在这儿只是简单介绍下编译原理与 babel，编译原理是一门非常深奥的课程， babel 也是一个非常优秀的工具。

再次回到需求，将 JSX 编译成小程序模板，非常幸运的是 babel 的核心编译器 babylon 是支持对 JSX 语法的解析的，可以直接利用它来构造 AST，而我们需要专注的核心就是如何对 AST 进行转换操作，得出我们需要的新 AST，再将新 AST 进行递归遍历，生成小程序的模板。

JSX 代码

```jsx
<View className='index'>
  <Button className='add_btn' onClick={this.props.add}>+</Button>
  <Button className='dec_btn' onClick={this.props.dec}>-</Button>
  <Button className='dec_btn' onClick={this.props.asyncAdd}>async</Button>
  <View>{this.props.counter.num}</View>
  <A />
  <Button onClick={this.goto}>走你</Button>
  <Image src={sd} />
</View>
```

编译生成小程序模板

```html
<import src="../../components/A/A.wxml" />
<block>
  <view class="index">
    <button class="add_btn" bindtap="add">+</button>
    <button class="dec_btn" bindtap="dec">-</button>
    <button class="dec_btn" bindtap="asyncAdd">async</button>
    <view>{{counter.num}}</view>
    <template is="A" data="{{...$$A}}"></template>
    <button bindtap="goto">走你</button>
    <image src="{{sd}}" />
  </view>
</block>
```

这时候，应该就能发现问题的难点所在了，要知道小程序的模板只是字符串，而 JSX 则是真正的 JS 代码扩展，其语法之丰富，显然不是字符串模板所能比，在这一步中，要做的操作，包括但不仅限于如下

- 理解三目运算符与逻辑表达式，例如三目运算符 `abc ? : <View>1</View> : <View>2</View>` 需要编译成 `<view wx:if="">1</view><view wx:else>2</view>`
- 理解数组 map 语法，例如 map 的使用 `abc.map(item => <View>item</View>)` 需要编译成 `<view wx:for="" wx:for-item="item">item</view>`
- 等等

以上仅仅是转换规则的冰山一角，JSX 的写法极其灵活多变，只能通过穷举的方式，将常用的、React 官方推荐的写法作为转换规则加以支持，而一些比较生僻的，或者是不那么推荐的写的写法则不做支持，转而以 eslint 插件的方式，提示用户进行修改。目前 Taro 支持的 JSX 转换规则，大致能覆盖到 JSX 80% 的写法操作。

## 还能不能干点别的

在平常的工作中，业务通常有一些多端的需求，就是要求小程序要有，H5 要有，甚至 RN 也能有就最好了，产品经理还看不上快应用，不然肯定要求快应用也上一套吧，这个时候，就会发现，差不多的界面和逻辑，你可能需要重复写上好几轮，这时候要是有个多端代码生成工具就好了，只写一份代码，可以多端运行。Write once, run anywhere，相信是所有工程师的梦想。

## 依然编译原理的力量

前文的内容，将一份代码编译成多端代码，这不正是编译原理干的事么，可以输入一份源代码，针对不同的端设定好对应的转换规则，再一键转换出对应端的代码。而且由于已经遵循 React 语法了，那我们再转成 H5 端（使用 Nerv）与 RN 端（使用 React）也就有了天然的优势。

![nerv](https://upload-images.jianshu.io/upload_images/7101063-aea18688a5dddc7c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

发现仅仅将代码按照对应语法规则转换过去后，还远远不够，因为不同端会有自己的原生组件，端能力 API 等等，代码直接转换过去后，可能不能直接执行。例如，小程序中普通的容器组件用的是 `<view />`，而在 H5 中则是 `<div />`；小程序中提供了丰富的端能力 API，例如网络请求、文件下载、数据缓存等，而在 H5 中对应功能的 API 则不一致。

所以，为了弥补不同端的差异，需要订制好一个统一的组件库标准，以及统一的 API 标准，在不同的端依靠它们的语法与能力去实现这个组件库与 API，同时还要为不同的端编写相应的运行时框架，负责初始化等等操作。通过以上这些操作，我们就能实现一份一键生成多端的需求了。在 Taro 最初的设计中，组件库与 API 的标准就是源自小程序的，因为既然已经有定义好的组件库与 API 标准，那为啥不直接拿来使用呢，这样不仅省去了定制标准的冥思苦想，同时也省去了为小程序开发组件库与 API 的麻烦，只需要让其他端来向小程序靠齐就好。

可能有些人会有疑问，既然是为不同的端实现了对应的组件库与端能力 API （小程序除外，因为组件库和 API 的标准都是源自小程序），那么是怎么能够只写一份代码就够了呢？因为我们有编译的操作，在书写代码的时候，只需要引入标准组件库 @tarojs/components 与运行时框架 @tarojs/taro ，代码经过编译之后，会变成对应端所需要的库。

![taro 运行适配](https://upload-images.jianshu.io/upload_images/7101063-c1645a3da7e0c5fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

既然组件库以及端能力都是依靠不同的端做不同实现来抹平差异，那么同样的，如果想为 Taro 引入更多的功能支持的话，有时候也需要按照这个套路来。例如，为了提升开发便利性，为 Taro 加入了 Redux 支持，做法就是，在小程序端，实现了 @tarojs/redux 这个库来作为小程序的 Redux 辅助库，并且以他作为基准库，它具有和 react-redux 一致的 API，在书写代码的时候，引用的都是 @tarojs/redux ，经过编译后，在 H5 端会替换成 nerv-redux（Nerv的 Redux 辅助库），在 RN 端会替换成 react-redux。这样就实现了 Redux 在 Taro 中的多端支持。

![多端支持](https://upload-images.jianshu.io/upload_images/7101063-d83abb64461968c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)