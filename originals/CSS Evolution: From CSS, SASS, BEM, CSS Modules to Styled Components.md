自从有互联网以来，我们就需要给我们的网站设计样式，而CSS也一直存在并以自己的速度发展起来，这篇文章将会让您了解它。

首先,在什么是CSS上我们需要有相同的认识，我认为我们都同意CSS是一种用于描述用[标记语言](https://en.wikipedia.org/wiki/Markup_language "Markup language")编写的文档的[演示文稿](https://en.wikipedia.org/wiki/Presentation_semantics "Presentation semantics")。

众所周知，虽然CSS变得越来越强大已，但是要想CSS为我们的项目服务仍需要我们使用额外的工具。

### CSS的西部蛮荒时期

在90年代，我们专注于创造“花式”界面，网页令人拍案叫绝是最重要的事情， 内联风格的样式就是在这个时候, 我们并不care网页是不是看起来很怪异，并会扔一些gifs，marquess和其他“可怕的”（当时令人印象深刻的）元素进我们的网页，最终网页就会像一个可爱的玩具，只希望能吸引访客的注意力。

![](http://p0.qhimg.com/t0136ab3bd2107bcd0d.png)

在那之后，我们开始创建动态网站， 但是CSS还是一如既往的混乱，每个开发人员都有自己的编写CSS的方式。 我们中的一些人在**特征**挣扎, 当我们引入新代码时会导致页面的混乱， 我们不得不依靠我们强大的如顽石般的意志力用 **!important** 让我们的页面看起来正常一点。 但是我们很快意识到：

![](http://p0.qhimg.com/t019e6fb41afe19a33f.jpg)

All those practices became more evident and bigger problems as soon as projects grew in size, complexity, and team members. So not having a **consistent pattern** to do styling became one of the biggest _blockers_ for experienced and inexperienced developers who struggled to find a right way to do things in CSS. In the end there was no right or wrong thing to do, we just cared to make the thing look ok.

![](http://p0.qhimg.com/t010362752b0c3095e2.gif)

### **SASS to the rescue**

SASS transformed CSS into a decent programming language in the form of a preprocessing engine that implemented **nesting, variables, mixins, extends** and **logic** into stylesheets, so you could better organize your css files and have at least some ways of deconstructing your css chunks in smaller files, which was a great thing back then.

It essentially takes scss code, preprocesses it and outputs the compiled versions of the file in a global css bundle. Great right? but not so much I’d say, After a while it became apparent that unless there were strategies and best practices in place, SASS only caused more troubles than it alleviated.

Suddenly we became unaware of what the preprocessor was doing under the hood and relied on lazily nesting to conquer the specificity battle but causing compiled stylesheets to go nuts in sizes.

Until BEM came along….

### BEM and component based thinking

When BEM came along it was a breath of fresh air that made us think more about reusability and components. It basically brought semanticity to a new level, it let us make sure that className is unique thus reducing the risk of specificity clash by using a simple Block Element Modifier convention. Look at the following example:

![](http://p0.qhimg.com/t01622984a32b9bea15.jpg)

If you analyze a bit the markup you can see immediately the Block Element Modifier convention in play here.

You can see that we have two very explicit blocks in the code: .scenery and .sky, each one of them have their own blocks. Sky is the only one that has modifiers as it could be foggy, daytime or dusk, those are different characteristics that could be applied to the same element.

Let’s take a look at the companion css with some pseudo code that will let us analyze it better:

![](http://p0.qhimg.com/t01622984a32b9bea15.jpg)

If you want to have an in-depth understanding of how BEM works, I recommend you [take a look at this article](https://m.alphasights.com/bem-i-finally-understand-b0c74815d5b0#.9vdcmiugz) , written by my colleague and friend Andrei Popa.

BEM is good in the sense that you made sure that components were unique #reusabilityFtw. With this kind of thinking some apparent patterns became more evident as we started migrating our old stylesheets into this new convention.

But, another set of problems came along:

*   Classname selection became a tedious task

*   Markup became bloated with all those long class names

*   You needed to explicitly extend every ui component whenever you wanted to reuse

*   Markup became unnecessarily semantic

### CSS Modules 和本地作用域

SASS或BEM都没有解决的问题是在语言逻辑中没有真正封装的概念，因此依靠开发人员选择唯一的class名。感觉可以通过工具而不是通过约定来解决这个问题。

这正是 CSS modules 所做的，它为每个本地定义的样式创建动态class名，以确保注入新的CSS属性不会互相影响，所有样式都被正确地封装。

CSS-Modules 在 React 生态系统中迅速获得普及，现在许多 react 项目都使用它，虽然有利有弊，但它已经被证明是一个很好的实践。

但是，CSS Modules 本身并不能解决CSS的核心问题，只实现了一种本地化样式定义的方式：一种使BEM自动化的聪明方法，因此您不必为定义class名称焦虑了（或至少可以少考虑一些）。

但是它并没有减轻对一个好的和可预测的风格架构的需求，好架构才可以让重用和可控的成本最低。

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

可以看到，长得很像css，最主要的区别就是前缀为 :local 的 class 名，会自动生成如下面这样的 class 名： 

.app-components-button-__root — 3vvFf {}

你可以使用参数 `localIdentName` 来设置前缀的名称。如: `css-loader?localIdentName=[path][name]---[local]---[hash:base64:5]` 以便简化调试.

这就是 Local CSS Modules 背后的简单规则。Local CSS Modules 通过生成一个唯一的className来自动化BEM符号的方式，确保它不会与他人冲突，即使他们使用相同的名称。这很方便！

### Styled Components to blend css in JS (fully)

Styled-components are pure visual primitives that act as a wrapping component; they can be mapped to actual html tags and what they do is wrap the children components with the styled-component.

This following code will explain it better:

![](http://p0.qhimg.com/t01622984a32b9bea15.jpg)

If you see the styled component is very simple to understand, it uses the template literal notation to define css properties, it seems that the core styled-components team nailed it this time as it blends the full power of ES6 and CSS.

Styled-components provides a very simple pattern to reuse and fully separate UI from Functional and Stateful components. Creating an api that has access to native tags either in the browser as HTML or Natively using React Native.

This is how you pass custom props (or modifiers) to a Styled Component:

![](http://p0.qhimg.com/t01622984a32b9bea15.jpg)

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

Button.displayName = Button.name 

;

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

![](http://p0.qhimg.com/t01622984a32b9bea15.jpg)

If you can see we have the  component which takes a width and a height as props and also receives the horizontal prop so the scrollbar appears below.

#### Helper components

Helper components make our life easier and allow us to reuse heavily. This is the place where we store all our common patterns.

These are some of the helpers I’ve found quite useful so far:

![](http://p0.qhimg.com/t01622984a32b9bea15.jpg)

#### Theme

Having a theme lets you have 1 source of truth of values that can be reused throughout the application, it’s been proven useful for storing values that are commonly reused in the application like color palette and general look and feel.

![](http://p0.qhimg.com/t01622984a32b9bea15.jpg)

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
                
