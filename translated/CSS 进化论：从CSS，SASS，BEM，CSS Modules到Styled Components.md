[![](http://p0.qhimg.com/t01506d3df6af6e6078.png)](https://camo.githubusercontent.com/b8addf40dcbd76b8a90818536aca60acde0c0da7/68747470733a2f2f63646e2d696d616765732d312e6d656469756d2e636f6d2f6d61782f323030302f312a7942785a6f394c4e456a52614c37654b5542715253412e706e67)自从有互联网以来，我们就需要为我们的网站设计样式，而CSS也一路伴随并且快速发展，这篇文章将会让您了解它的发展历史。

首先,在“什么是CSS”这个问题上，我们需要有相同的认识，而共识就是，CSS是一种写在[标记语言](https://en.wikipedia.org/wiki/Markup_language "Markup language")中的用于描述文档的语言。

众所周知，虽然CSS早已沿途发展的越来越强大，但是要想CSS为我们的项目服务，我们仍需要使用额外的工具。

### [](https://github.com/36KrFE/36krfe.github.io/blob/master/originals/CSS%20Evolution-%20From%20CSS,%20SASS,%20BEM,%20CSS%20Modules%20to%20Styled%20Components.md#css的西部蛮荒时期)CSS的西部蛮荒时期

在90年代，我们专注于创造“花式”界面，网页令人拍案叫绝是最重要的事情， 内联风格的样式就在这时兴起, 我们并不关心网页是不是看起来很怪异，并会扔一些gif，marquee和其他“可怕的”（当时令人印象深刻的）元素进我们的网页，最终网页就会像一个只为了能吸引访客的注意力的<span style="font-size: 1rem;">可爱玩具</span>。

[![](http://p0.qhimg.com/t016e20ba50344ae697.png)](https://camo.githubusercontent.com/f8e3cba8610c1dbe8d25477d10bee2b256d1b21c/68747470733a2f2f7062732e7477696d672e636f6d2f6d656469612f42494353667a53434d4141796935582e706e67)

在那之后，我们开始创建动态网站， 但是CSS还是一如既往的混乱，每个开发人员都有自己的编写CSS的方式。 我们中的一些人在**声明**备受折磨：当我们引入新代码时会导致页面的混乱， 因此我们不得不依靠 **!important** 让我们的页面看起来正常一点。 但是我们很快意识到：

[![](http://p0.qhimg.com/t019e6fb41afe19a33f.jpg)](https://camo.githubusercontent.com/33cbd8c6c7cb159c9be6d91cd265765869cbaad3/687474703a2f2f70302e7168696d672e636f6d2f743031396536666234316166653139613333662e6a7067)

因为随着项目规模、复杂程度和团队成员日益增加，所有这些做法就变成更大更显著的难题。 所以没有一个**一致的模式**书写风格就成了有经验和没有经验的开发人员之间最大的_绊脚石_之一，因此大家都在努力寻找一种书写CSS的正确方式。 没有对错之分，我们只是关心如何让样式看起来正常。

![](http://p0.qhimg.com/t019b7b880342492f9b.gif)

### [](https://github.com/36KrFE/36krfe.github.io/blob/master/originals/CSS%20Evolution-%20From%20CSS,%20SASS,%20BEM,%20CSS%20Modules%20to%20Styled%20Components.md#sass来解救)**SASS来解救**

SASS<span style="font-size: 1rem;">以预处理引擎的形式</span>将CSS转换为一种得体的编程语言，使**嵌套，变量，混合，扩展**和**逻辑**在样式表中得以实现，因此大家可以更好地组织CSS文件，而且更棒的是有了把CSS代码块解构成更小的文件的方法。

它本质上采用SASS代码，对其进行预处理，并以全局CSS打包的形式输出文件的编译版本。看起来很棒对吗？ 但是我不会这么说，因为过一会儿你就会很明显地发现，除非有策略性和最佳实践的使用，否则比起减少的麻烦，SASS只会造成更多问题。

突然之间，我们不知道预处理器在暗地里做了什么，并且懒惰地依靠嵌套来解决特异性冲突，这只会导致编译的样式表变得很大。

直到BEM来了...

### [](https://github.com/36KrFE/36krfe.github.io/blob/master/originals/CSS%20Evolution-%20From%20CSS,%20SASS,%20BEM,%20CSS%20Modules%20to%20Styled%20Components.md#bem和组件化思考)BEM和组件化思考

BEM的来临为我们带来了一股新鲜空气，让我们更多地思考可重用性和组件化。它基本上将语义化提升到一个新的水平，通过使用简单的模块+元素+修饰符的命名规范，使我们确保className是唯一的，从而减少特异性冲突的风险。来看下面的例子：

```
<body class="scenery">
  <section class="scenery__sky">
    <div class="sky [sky--dusk / sky--daytime] [sky--foggy]">
      <div class="sky__clouds"></div>
      <div class="sky__sun"></div>
    </div>
  </section>
  <section class="scenery__ground"></section>
  <section class="scenery__people"></section>
</body>

```

分析一下上面的代码，就会立即看到BEM规范在这里的应用。

可以看到我们在代码中有两个非常明确的模块：.scenery和.sky，它们中的每一个都有自己的块。.sky是唯一具有修饰符的模块，因为它可能是foggy，daytime或dusk三种状态，这些状态是可以应用于同一元素的不同特征。

我们来看一下相关的CSS代码，这样可以让我们更好的分析它：

```
// Block
.scenery {
   //Elements
  &__sky {
    fill: screen;
  }
  &__ground {
    float: bottom; 
  }
  &__people {
    float: center;
  }
}

//Block
.sky {
  background: dusk;

  // Elements

  &__clouds {
    type: distant;
  }

  &__sun {
    strength: .025;
  }

  // Modifiers
  &--dusk {
    background: dusk;
    .sky__clouds {
      type: distant;
    }
    .sky__sun {
      strength: .025;
    }
  }

  &--daytime {
    background: daylight;
    .sky__clouds {
      type: fluffy;
      float: center;
    }
    .sky__sun {
      strength: .7;
      align: center;
      float: top;
    }
  }
}

```

如果你想深入了解BEM是如何工作的，我推荐你 [看一下这篇文章](https://m.alphasights.com/bem-i-finally-understand-b0c74815d5b0#.9vdcmiugz) ，是由我的同事和朋友 Andrei Popa撰写的.

BEM在您确保组件的唯一性和可重用性方面是很有意义的。通过这种思想，当我们开始将旧样式表移植到这个新的规范中时，以前一些明显的命名形式在移植后将会变得更加清晰明白。

但是，另一些问题来了：

*   为不同的类起名字成为了一个乏味的任务

*   CSS会因为带有这些长的类名而变得臃肿

*   每当你想要重用时，你需要明确地扩展每个ui组件

*   标记的语义化变得不再必要

### [](https://github.com/36KrFE/36krfe.github.io/blob/master/originals/CSS%20Evolution-%20From%20CSS,%20SASS,%20BEM,%20CSS%20Modules%20to%20Styled%20Components.md#css-modules-和-local-作用域)CSS Modules 和 local 作用域

SASS或BEM都没有解决的问题，是在语言逻辑中没有真正封装的概念，因此需要依靠开发人员选择唯一的class名。但这应该是通过工具而不是通过约定来解决的。

这正是 CSS modules 所做的，它为每个本地定义的样式创建动态class名，以确保注入新的CSS属性不会互相影响，所有样式都被正确地封装。

CSS-Modules 在 React 生态系统中迅速获得普及，现在许多 react 项目都使用它，虽然有利有弊，但它已经被证明是一个很好的实践。

但是，CSS Modules 本身并不能解决CSS的核心问题，只实现了一种local样式定义的方式：实现BEM自动化，因此不必为定义class名称焦虑了（或至少可以少考虑一些）。

但是，一个好的样式架构单单靠CSS Module还是不够的，好的架构需要让重用和可控的成本最低。

现在本地的CSS看起来是这样的:

```
@import '~tools/theme';

:local(.root) {
  border: 1px solid;
  font-family: inherit;
  font-size: 12px;
  color: inherit;
  background: none;
  cursor: pointer;
  display: inline-block;
  text-transform: uppercase;
  letter-spacing: 0;
  font-weight: 700;
  outline: none;
  position: relative;
  transition: all 0.3s;
  text-transform: uppercase;
  padding: 10px 20px;
  margin: 0;
  border-radius: 3px;
  text-align: center;
}

@mixin button($bg-color, $font-color) {
  background: $bg-color;
  color: $font-color;
  border-color: $font-color;

  &:focus {
    border-color: $font-color;
    background: $bg-color;
    color: $font-color;
  }

  &:hover {
    color: $font-color;
    background: lighten($bg-color, 20%);
  }

  &:active {
    background: lighten($bg-color, 30%);
    top: 2px;
  }
}

:local(.primary) {
  @include button($color-primary, $color-white)
}

:local(.secondary) {
  @include button($color-white, $color-primary)
}

```

可以看到，长得很像CSS，最主要的区别就是前缀为 :local 的 class 名，会自动生成如下面这样的 class 名：

.app-components-button-__root — 3vvFf {}

你可以使用参数 `localIdentName` 来设置前缀的名称。如: `css-loader?localIdentName=[path][name]---[local]---[hash:base64:5]` 以便简化调试.

这就是 Local CSS Modules 背后的简单规则。Local CSS Modules 通过生成一个唯一的className来使BEM自动化的方式，确保即使他们使用相同的名称也不会冲突。这很方便！

### [](https://github.com/36KrFE/36krfe.github.io/blob/master/originals/CSS%20Evolution-%20From%20CSS,%20SASS,%20BEM,%20CSS%20Modules%20to%20Styled%20Components.md#在js中混合css的styled-components完全)在JS中混合CSS的Styled Components（完全）

Styled-components用作包装组件；它们可以映射到实际的html标签，并且他们所做的是用Styled-component来包装子组件。

下面的代码可以解释的更好：

```
import React from "react"
import styled from "styled-components"
// Simple form component

const Input = styled.input`
 background: green
`

const FormWrapper = () => <Input placeholder="hola" />

// What this compiles to:
<input placeholder="hola" class="dxLjPX">Send</input>

```

如你看到的，Styled-component非常简单易懂，它使用模板字符串来定义CSS属性，这也是Styled-components核心团队此次特地引入的，因为它融合了ES6和CSS的全部功能。

Styled-component提供了一种非常简单的模式——可重用，并将功能和状态组件完全分离。创建一个可以在浏览器中访问，或者使用React Native访问原生标签的API。

如下是将自定义属性（或修饰符）传递给Styled Component的例子：

```
import styled from "styled-components"

const Sky = styled.section`
 ${props => props.dusk && 'background-color: dusk' }
 ${props => props.day && 'background-color: white' }
 ${props => props.night && 'background-color: black' }
`;

// You can use it like so:
<Sky dusk />
<Sky day />
<Sky night />

```

你可以看到props突然变成了每个组件接收到的修饰符，并且它们可以被处理以输出不同的CSS代码，看起来很整齐吧？

这使我们能够更快地迭代，并使用JS的全部功能处理我们的样式，同时确保它们保持一致和重用性。

### [](https://github.com/36KrFE/36krfe.github.io/blob/master/originals/CSS%20Evolution-%20From%20CSS,%20SASS,%20BEM,%20CSS%20Modules%20to%20Styled%20Components.md#core-ui-for-everyone-to-reuse)能复用的核心UI组件

很明显，CSS Modules和Styled Components本身不是完美的解决方案，它们需要某种模式才能工作和扩展，这个模式就是定义一个分离了逻辑后只包含UI样式的核心组件。 

如下是使用CSS Modules实现的一个此类组件:

```
import React from "react";

import classNames from "classnames";
import styles from "./styles";

const Button = (props) => {
  const { className, children, theme, tag, ...rest } = props;
  const CustomTag = `${tag}`;
  return (
    <CustomTag { ...rest } className={ classNames(styles.root, theme, className) }>
      { children }
    </CustomTag>
  );
};

Button.theme = {
  secondary: styles.secondary,
  primary: styles.primary
};

Button.defaultProps = {
  theme: Button.theme.primary,
  tag: "button"
};

Button.displayName = Button.name;

Button.propTypes = {
  theme: React.PropTypes.string,
  tag: React.PropTypes.string,
  className: React.PropTypes.string,
  children: React.PropTypes.oneOfType([
    React.PropTypes.string,
    React.PropTypes.element,
    React.PropTypes.arrayOf(React.PropTypes.element)
  ])
};

export default Button;

```

如你所见，这里没什么奇特的，只是一个包装组件用于接受属性，并将这些属性映射到子组件上，换句话说这个包装组件将所有属性传递给子组件。

这样你的组件就能像下面一样使用

```
import React from "react"
import Button from "components/core/button"

const = Component = () => <Button theme={ Button.theme.secondary }>Some Button</Button>

export default Component

```

我们再来看一个类似的全部借助styled-components实现的例子：

```
import styled from "styled-components";

import {
  theme
} from "ui";

const { color, font, radius, transition } = theme;

export const Button = styled.button`
 background-color: ${color.ghost};
 border: none;
 appearance: none;
 user-select: none;
 border-radius: ${radius};
 color: ${color.base}
 cursor: pointer;
 display: inline-block;
 font-family: inherit;
 font-size: ${font.base};
 font-weight: bold;
 outline: none;
 position: relative;
 text-align: center;
 text-transform: uppercase;
 transition:
 transorm ${transition},
 opacity ${transition};
 white-space: nowrap;
 width: ${props => props.width ? props.width : "auto"};
 &:hover,
 &:focus {
 outline: none;
 }
 &:hover {
 color: ${color.silver};
 opacity: 0.8;
 border-bottom: 3px solid rgba(0,0,0,0.2);
 }
 &:active {
 border-bottom: 1px solid rgba(0,0,0,0.2);
 transform: translateY(2px);
 opacity: 0.95;
 }
 ${props => props.disabled && `
 background-color: ${color.ghost};
 opacity: ${0.4};
 pointer-events: none;
 cursor: not-allowed;
 `}
 ${props => props.primary && `
 background-color: ${color.primary};
 color: ${color.white};
 border-color: ${color.primary};
 &:hover,
 &:active {
 background-color: ${color.primary}; 
 color: ${color.white};
 }
 `}
 ${props => props.secondary && `
 background-color: ${color.secondary};
 color: ${color.white};
 border-color: ${color.secondary};
 &:hover,
 &:active {
 background-color: ${color.secondary}; 
 color: ${color.white};
 }
 `}
`;

```

这个模式的有趣之处在于，组件看起来非常呆板（译者：不具备任何逻辑），它只是一个把CSS映射到父组件上的CSS包装器。这样做有一个优点:

_它允许我们定义一个基础的 UI api，通过这个api可以随意进行交换，并确保所有UI在整个应用程序中保持一致。_

这样我们就可以将UI样式从具体的逻辑实现中剥离出来，使得二者能够并行开发; 可以让1位开发人员专注于实施该功能，其他人专注于UI实现，从而实现逻辑/UI完全分离开发的想法。

目前听起来像是一个很好的解决方案，在内部我们已经讨论过，认为遵循这种模式是一个好主意。 连同这种模式，我们也论证其他有用的模式：

#### [](https://github.com/36KrFE/36krfe.github.io/blob/master/originals/CSS%20Evolution-%20From%20CSS,%20SASS,%20BEM,%20CSS%20Modules%20to%20Styled%20Components.md#属性接收器)**属性接收器**

这个函数就是监听任意组件接受到的属性，从而可以轻松地在任何所需的组件中使用这些函数，让它在得到复用的同时，也能够扩展任何给定组件的功能，可以将其视为继承修饰（译者：更像聚合），我用下面这个例子来解释一下：

```
// Prop passing Shorthands for Styled-components
export const borderProps = props => css`
 ${props.borderBottom && `border-bottom: ${props.borderWidth || "1px"} solid ${color.border}`};
 ${props.borderTop && `border-top: ${props.borderWidth || "1px"} solid ${color.border}`};
 ${props.borderLeft && `border-left: ${props.borderWidth || "1px"} solid ${color.border}`};
 ${props.borderRight && `border-right: ${props.borderWidth || "1px"} solid ${color.border}`};
`;

export const marginProps = props => css`
 ${props.marginBottom && `margin-bottom: ${typeof (props.marginBottom) === "string" ? props.marginBottom : "1em"}`};
 ${props.marginTop && `margin-top: ${typeof (props.marginTop) === "string" ? props.marginTop : "1em"}`};
 ${props.marginLeft && `margin-left: ${typeof (props.marginLeft) === "string" ? props.marginLeft : "1em"}`};
 ${props.marginRight && `margin-right: ${typeof (props.marginRight) === "string" ? props.marginRight : "1em"}`};
 ${props.margin && `margin: ${typeof (props.margin) === "string" ? props.margin : "1em"}`};
 ${props.marginVertical && `
 margin-top: ${typeof (props.marginVertical) === "string" ? props.marginVertical : "1em"}
 margin-bottom: ${typeof (props.marginVertical) === "string" ? props.marginVertical : "1em"}
 `};
 ${props.marginHorizontal && `
 margin-left: ${typeof (props.marginHorizontal) === "string" ? props.marginHorizontal : "1em"}
 margin-right: ${typeof (props.marginHorizontal) === "string" ? props.marginHorizontal : "1em"}
 `};
`;
// An example of how you can use it with your components

const SomeDiv = styled.div`
 ${borderProps}
 ${marginProps}
`

// This lets you pass all borderProps to the component like so:

<SomeDiv borderTop borderBottom borderLeft borderRight marginVertical>

```

如何使用 prop receivers的例子

这个方法可以让你不用为每一个组件都硬编码所有的border，从而节约大量时间

#### [](https://github.com/36KrFE/36krfe.github.io/blob/master/originals/CSS%20Evolution-%20From%20CSS,%20SASS,%20BEM,%20CSS%20Modules%20to%20Styled%20Components.md#占位--函数性质混入)**占位 / 函数化Mixin**

在Styled components中，您可以使用JS的全部功能来创建函数，不仅可以作为样式接收器，还能在不同组件之间共享代码，如下是一个示例：

```
// Mixin like functionality

const textInput = props => `
 color: ${props.error ? color.white : color.base};
 background-color: ${props.error ? color.alert : color.white};
`;

export const Input = styled.input`
 ${textInput}
`;

export const Textarea = styled.textarea`
 ${textInput};
 height: ${props => props.height ? props.height : '130px'}
 resize: none;
 overflow: auto;
`;

```

#### [](https://github.com/36KrFE/36krfe.github.io/blob/master/originals/CSS%20Evolution-%20From%20CSS,%20SASS,%20BEM,%20CSS%20Modules%20to%20Styled%20Components.md#布局组件)**布局组件**

我们发现，在开发应用程序时需要做的第一件事是布局UI元素，为此，我们选择了一些能够在这个过程中有帮助的组件。

事实证明，这些组件是非常有用的，因为有些开发人员（对CSS定位技术不够熟悉）很难设置结构，如下是布局组件的一个例子：

```
import styled from "styled-components";
import {
  theme,
  borderProps,
  sizeProps,
  backgroundColorProps,
  marginProps
} from "ui";

const { color, font, topbar, gutter } = theme;

export const Panel = styled.article`
 ${marginProps}
 padding: 1em;
 background: white;
 color: ${color.black};
 font-size: ${font.base};
 font-weight: 300;
 ${props => !props.noborder && `border: 1px solid ${color.border}`};
 width: ${props => props.width ? props.width : "100%"};
 ${props => borderProps(props)}
 transition: 
 transform 300ms ease-in-out,
 box-shadow 300ms ease-in-out,
 margin 300ms ease-in-out;
 box-shadow: 0 3px 3px rgba(0,0,0,0.1);
 ${props => props.dark && `
 color: ${color.white};
 background-color: ${color.black};
 `}
 &:hover {
 transform: translateY(-5px);
 box-shadow: 0 6px 3px rgba(0,0,0,0.1);
 }
`;

export const ScrollView = styled.section`
 overflow: hidden;
 font-family: ${font.family};
 -webkit-overflow-scrolling: touch;
 overflow-y: auto;
 ${props => props.horizontal && `
 white-space: nowrap;
 overflow-x: auto;
 overflow-y: hidden;
 `
 }
 ${props => sizeProps(props)}
`;

export const MainContent = styled(ScrollView)`
 position: absolute;
 top: ${props => props.topbar ? topbar.height : 0};
 right: 0;
 left: 0;
 bottom: 0;
 font-size: ${font.base};
 padding: ${gutter} 3em;
 ${props => props.bg && `
 background-color: ${props.bg};
 `}
`;

export const Slide = styled.section`
 ${backgroundColorProps}
 font-weight: 400;
 flex: 1;
 height: ${props => props.height ? props.height : "100%"};
 width: ${props => props.width ? props.width : "100%"};
 justify-content: center;
 flex-direction: column;
 align-items: center;
 text-align: center;
 display: flex;
 font-size: 3em;
 color: ${color.white};
`;

export const App = styled.div`
 *, & {
 box-sizing: border-box;
 }
`;

```

你是否注意到了使用width和height作为props的<ScrollView/>组件, 它同样也接收是否水平的prop使得滚动条出现在下方。

#### [](https://github.com/36KrFE/36krfe.github.io/blob/master/originals/CSS%20Evolution-%20From%20CSS,%20SASS,%20BEM,%20CSS%20Modules%20to%20Styled%20Components.md#helper-components)**帮助器组件**

帮助器组件使我们开发更轻松，并允许我们复用。在帮助器组件中我们组织所有的常见模式。

这些是我迄今发现的非常有用的帮助器：

```
import styled, { css } from "styled-components";

import {
  borderProps,
  marginProps,
  backgroundColorProps,
  paddingProps,
  alignmentProps,
  positioningProps,
  sizeProps,
  spacingProps,
  theme
} from "ui";

const { screenSizes } = theme;

export const overlay = `
 position: fixed;
 top: 0;
 left: 0;
 right: 0;
 bottom: 0;
 display: flex;
 align-items: center;
 justify-content: center;
 background: rgba(0,0,0,0.5); 
`;

// You can use this like ${media.phone`width: 100%`}

export const media = Object.keys(screenSizes).reduce((accumulator, label) => {
  const acc = accumulator;
  acc[label] = (...args) => css`
 @media (max-width: ${screenSizes[label]}em) {
 ${css(...args)}
 }
 `;
  return acc;
}, {});

// Spacing

export const Padder = styled.section`
 padding: ${props => props.amount ? props.amount : "2em"};
`;

export const Spacer = styled.div`
 ${spacingProps}
`;

// Alignment

export const Center = styled.div`
 ${borderProps}
 ${marginProps}
 ${backgroundColorProps}
 ${paddingProps}
 ${alignmentProps}
 ${positioningProps}
 ${sizeProps}
 text-align: center;
 margin: 0 auto;
`;

// Positioning

export const Relative = styled.div`
 ${props => borderProps(props)};
 position: relative;
`;

export const Absolute = styled.div`
 ${props => marginProps(props)};
 ${props => alignmentProps(props)};
 ${props => borderProps(props)};
 position: absolute;
 ${props => props.right && `right: ${props.padded ? "1em" : "0"}; `}
 ${props => props.left && `left: ${props.padded ? "1em" : "0"}`};
 ${props => props.top && `top: ${props.padded ? "1em" : "0"}`};
 ${props => props.bottom && `bottom: ${props.padded ? "1em" : "0"}`};
`;

// Patterns
export const Collapsable = styled.section`
 opacity: 1;
 display: flex;
 flex-direction: column;
 ${props => props.animate && `
 transition: 
 transform 300ms linear,
 opacity 300ms ease-in,
 width 200ms ease-in,
 max-height 200ms ease-in 200ms;
 max-height: 9999px;
 transform: scale(1);
 transform-origin: 100% 100%;
 ${props.collapsed && `
 transform: scale(0);
 transition: 
 transform 300ms ease-out,
 opacity 300ms ease-out,
 width 300ms ease-out 600ms;
 `}
 `}
 ${props => props.collapsed && `
 opacity: 0;
 max-height: 0;
 `}
`;

export const Ellipsis = styled.div`
 max-width: ${props => props.maxWidth ? props.maxWidth : "100%"};
 overflow: hidden;
 text-overflow: ellipsis;
 white-space: nowrap;
`;

export const Circle = styled.span`
 ${backgroundColorProps}
 display: inline-block;
 border-radius: 50%;
 padding: ${props => props.padding || '10px'};
`;

export const Hidden = styled.div`
 display: none;
`;

```

#### [](https://github.com/36KrFE/36krfe.github.io/blob/master/originals/CSS%20Evolution-%20From%20CSS,%20SASS,%20BEM,%20CSS%20Modules%20to%20Styled%20Components.md#主题)**主题**

有一个可在整个应用程序中重复使用的主题会让开发如虎添翼，它存储应用程序最常被重用的值，实践证明这个功能很有用，就像调色板一样。

```
export const theme = {
  color: {
    primary: "#47C51D",
    secondary: '#53C1DE',
    white: "#FFF",
    black: "#222",
    border: "rgba(0,0,0,0.1)",
    base: "rgba(0,0,0,0.4)",
    alert: '#FF4258',
    success: 'mediumseagreen',
    info: '#4C98E6',
    link: '#41bbe1'
  },
  icon: {
    color: "gray",
    size: "15px"
  },
  font: {
    family: `
 -apple-system,
 BlinkMacSystemFont,
 'Segoe UI',
 Roboto,
 Helvetica,
 Arial,
 sans-serif,
 'Apple Color Emoji',
 'Segoe UI Emoji',
 'Segoe UI Symbol'`,
    base: '13px',
    small: '11px',
    xsmall: '9px',
    large: '20px',
    xlarge: '30px',
    xxlarge: '50px',
  },
  headings: {
    family: 'Helvetica Neue',
  },
  gutter: '2em',
  transition: '300ms ease-in-out'
};

export default theme;

```

**优点**

*   我们可以操控JS来帮助我们实现需求，这意味着与组件的UI完全通信。

*   消除了通过用className映射组件和样式的需要 (底层帮你实现的)

*   目前来看这是超棒的开发体验, 它减少了为类起名字并将其映射到组件的时间。

**缺点**

*   未经过广泛测试

*   只为React构建样式组件样式组件

*   还是很新的特性

*   测试需要通过动态标签或使用classNames来完成

### [](https://github.com/36KrFE/36krfe.github.io/blob/master/originals/CSS%20Evolution-%20From%20CSS,%20SASS,%20BEM,%20CSS%20Modules%20to%20Styled%20Components.md#conclusion)结论

无论是SASS，BEM，CSS Modules还是Styled Components，无论使用什么技术，都不能单独成为一种能够直观地为其他开发人员提供代码参照，而不影响或引入新的模块到系统中来的完美的样式架构。

Styled Components对于适当的扩展至关重要，即使用纯CSS和BEM也可以实现，主要的区别是每个实现方式所需的工作量和代码行数，总体而言，Styled Components就像一个给几乎所有的React项目的伟大套装，虽尚未广泛测试，但确实相当有前途。