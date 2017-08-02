![](https://cdn-images-1.medium.com/max/2000/1*yBxZo9LNEjRaL7eKUBqRSA.png)
自从有互联网以来，我们就需要给我们的网站设计样式，而CSS也一直存在并以自己的速度发展起来，这篇文章将会让您了解它。

首先,在什么是CSS上我们需要有相同的认识，我认为我们都同意CSS是一种用于描述用[标记语言](https://en.wikipedia.org/wiki/Markup_language "Markup language")编写文档的[演示文稿](https://en.wikipedia.org/wiki/Presentation_semantics "Presentation semantics")。

众所周知，虽然CSS早已沿途发展的越来越强大，但是要想CSS为我们的项目服务仍需要我们使用额外的工具。

### CSS的西部蛮荒时期

在90年代，我们专注于创造“花式”界面，网页令人拍案叫绝是最重要的事情， 内联风格的样式就在这时兴起, 我们并不care网页是不是看起来很怪异，并会扔一些gifs，marquess和其他“可怕的”（当时令人印象深刻的）元素进我们的网页，最终网页就会像一个可爱的玩具，只希望能吸引访客的注意力。

![](https://pbs.twimg.com/media/BICSfzSCMAAyi5X.png)

在那之后，我们开始创建动态网站， 但是CSS还是一如既往的混乱，每个开发人员都有自己的编写CSS的方式。 我们中的一些人在**特征**挣扎, 当我们引入新代码时会导致页面的混乱， 我们不得不依靠我们强大的如顽石般的意志力用 **!important** 让我们的页面看起来正常一点。 但是我们很快意识到：

![](http://p0.qhimg.com/t019e6fb41afe19a33f.jpg)

一旦项目规模、复杂程度和团队成员日益增加，所有这些做法就变成更大更显著的难题。 所以没有一个**一致的模式**书写样式成为有经验和没有经验的开发人员最大的_绊脚石_之一，他们努力寻找一种书写CSS的正确方法。 最后没有对错之分，我们只是关心让样式看起来OK。

![](http://p0.qhimg.com/t010362752b0c3095e2.gif)

### **SASS来解救**

SASS将CSS转换为一种得体的编程语言，以预处理引擎的形式，使**嵌套，变量，混合，扩展**和**逻辑**在样式表中得以实现，因此您可以更好地组织您的css文件，而且至少有了一些把css块解构成更小的文件的方法，这当然是一件很棒的事情。

它本质上采用sass代码，对其进行预处理，并以全局css打包的形式输出文件的编译版本。 很棒对吗？ 但是我不会这么说，过一会儿你就会很明显地发现，除非有策略性和最佳实践的使用，否则比起减少的麻烦，SASS只会造成更多问题。

突然之间，我们不知道预处理器在暗地里做了什么，并且懒惰地依靠嵌套来解决特异性冲突，但是导致编译的样式表变得很大。

直到BEM来了...

### BEM和组件化思考

当BEM来临之际，这是一股新鲜空气，让我们更多地思考可重用性和组件化。 它基本上将语义化提升到一个新的水平，通过使用简单的模块+元素+修饰符的命名规范，使我们确保className是唯一的，从而减少特异性冲突的风险。 看下面的例子：

```html
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

如果您分析一下这些标记，您会立即看到BEM规范在这里的应用。

您可以看到我们在代码中有两个非常明确的模块：.scenery和.sky，它们中的每一个都有自己的块。 .sky是唯一具有修饰语的模块，因为它可能是foggy，daytime或dusk三种状态，这些状态是可以应用于同一元素的不同特征。

我们来看一下相关的css代码，这样可以让我们更好的分析它：

```css
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

*   类名选择成为了一个乏味的任务

*   标记会因为带有这些长的类名而变得臃肿

*   每当你想要重用时，你需要明确地扩展每个ui组件

*   标记的语义化变得不再必要

### CSS Modules 和 local 作用域

SASS或BEM都没有解决的问题是在语言逻辑中没有真正封装的概念，因此依靠开发人员选择唯一的class名。感觉可以通过工具而不是通过约定来解决这个问题。

这正是 CSS modules 所做的，它为每个本地定义的样式创建动态class名，以确保注入新的CSS属性不会互相影响，所有样式都被正确地封装。

CSS-Modules 在 React 生态系统中迅速获得普及，现在许多 react 项目都使用它，虽然有利有弊，但它已经被证明是一个很好的实践。

但是，CSS Modules 本身并不能解决CSS的核心问题，只实现了一种本地化样式定义的方式：一种使BEM自动化的聪明方法，因此您不必为定义class名称焦虑了（或至少可以少考虑一些）。

但是它并没有减轻对一个好的和可预测的风格架构的需求，好架构才可以让重用和可控的成本最低。

现在本地的CSS看起来是这样的:

```css
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

可以看到，长得很像css，最主要的区别就是前缀为 :local 的 class 名，会自动生成如下面这样的 class 名： 

.app-components-button-__root — 3vvFf {}

你可以使用参数 `localIdentName` 来设置前缀的名称。如: `css-loader?localIdentName=[path][name]---[local]---[hash:base64:5]` 以便简化调试.

这就是 Local CSS Modules 背后的简单规则。Local CSS Modules 通过生成一个唯一的className来自动化BEM符号的方式，确保它不会与他人冲突，即使他们使用相同的名称。这很方便！

### 在JS中混合CSS的Styled Components（完全）

Styled-components are pure visual primitives that act as a wrapping component; they can be mapped to actual html tags and what they do is wrap the children components with the styled-component.

This following code will explain it better:

```javascript
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

If you see the styled component is very simple to understand, it uses the template literal notation to define css properties, it seems that the core styled-components team nailed it this time as it blends the full power of ES6 and CSS.

Styled-components provides a very simple pattern to reuse and fully separate UI from Functional and Stateful components. Creating an api that has access to native tags either in the browser as HTML or Natively using React Native.

This is how you pass custom props (or modifiers) to a Styled Component:

```javascript
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

You can see that the props suddenly become the modifiers that each of the components receive and they can be processed to output different lines of css, neat right?

This allows us to move faster and use the full power of JS to process our styles while making sure they remain consistent and reusable.

### Core UI for everyone to reuse

很明显，CSS Modules和Styled Components本身不是完美的解决方案，它需要某种模式才能工作和扩展。这个模式就是定义一个分离了逻辑后只包含UI样式的核心组件
使用CSS Modules实现的一个此类组件:
```javascript
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


如你所见，这里没什么奇特的，只是一个接受属性并将属性映射到子组件上的组件， 换句话说这个组件将所有属性传递给子组件


这样你的组件就能像下面一样使用

```javascript
import React from "react"
import Button from "components/core/button"

const = Component = () => <Button theme={ Button.theme.secondary }>Some Button</Button>

export default Component
```

我们再来看一个全部使用styled-components实现的相似的例子：


```javascript
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

这个模式有趣的是，组件看起来非常呆板（译者：不具备任何逻辑），它是一个把css映射到父组件上的css包装器。这样做有一个优点:

*它允许我们定义一个基础的 UI api，您可以随意进行交换，并确保所有UI在整个应用程序中保持一致。（译者：达到一改全改的目的）*

这样我们就可以将设计过程与实现过程完全隔离开，使得二者并行开发成为可能; 您可以让1位开发人员专注于实施该功能，其他人专注于UI实现，从而实现逻辑/UI完全分离开发的想法。

目前听起来像是一个很好的解决方案，在内部我们已经讨论过，认为遵循这种模式是一个好主意。 连同这种模式，我们也论证其他有用的模式：


#### **属性接收器**


这个函数就是监听任意组件接受的到的属性，从而使您可以轻松地在任何所需的组件中使用这些函数，使其成为可重用性的圣杯，并扩展任何给定组件的功能，您可以将其视为继承修饰（译者：更像聚合），我用这个例子解释：

```javascript
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


这个方法肯定可以让你不用为每一个组件都硬编码所有的border，从而节约大量时间

#### 占位 / 函数性质混入

在样式化的组件中，您可以使用JS的全部功能来创建函数，不仅可以作为样式接收器，还能在不同组件之间共享代码，这里是一个示例：

```javascript
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

#### Layout Components

We’ve detected that one of the first things we need to do when working in an application is layout our UI elements, for this purpose, we’ve identified some components that aid us in the process.

These components have proven to be very useful as often some developers (not familiar enough with css positioning techniques) have a hard time setting the structure, here is an example of such components:

```javascript
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

If you can see we have the  component which takes a width and a height as props and also receives the horizontal prop so the scrollbar appears below.

#### Helper components

Helper components make our life easier and allow us to reuse heavily. This is the place where we store all our common patterns.

These are some of the helpers I’ve found quite useful so far:

```javascript
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

#### Theme

Having a theme lets you have 1 source of truth of values that can be reused throughout the application, it’s been proven useful for storing values that are commonly reused in the application like color palette and general look and feel.

```javascript
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

**Pros**

*   The full power of JS at our hands, meaning full communication with the component’s UI.

*   Eliminates the need of mapping components and styles through the use of a className (this is done under the hood)

*   Great development experience so far, it reduces the amount of time spent thinking about classNames and mapping them to the component.

**Cons**

*   Yet to be tested in the wild

*   Built for React

*   Super young

*   Testing needs to be done via aria-labels or using classNames

### Conclusion

Whatever technology you use whether it is SASS, BEM, CSS Modules or Styled Components there is no substitute for a well defined styling architecture that makes it intuitive for other developers to contribute to your code base without thinking too much, breaking or introducing new moving parts to the system.

This approach is crucial to scale properly and can be achieved even if using plain CSS and BEM, the main difference is the amount of work and LOC needed for each implementation, overall styled-components feels like a great suit for pretty much all React projects, yet to test it in the wild but quite promising indeed.
