![](https://cdn-images-1.medium.com/max/2000/1*yBxZo9LNEjRaL7eKUBqRSA.png)
Since the beginnings of the Internet we’ve always had the need to style our websites, CSS has been around forever and has evolved at its own pace over the years, this article will take you through it.

To begin with we need to be on the same page of what CSS is, I think we can all agree that css is used for describing the [presentation](https://en.wikipedia.org/wiki/Presentation_semantics "Presentation semantics") of a document written in a [markup language](https://en.wikipedia.org/wiki/Markup_language "Markup language").

It’s no news that CSS has evolved along the way and has become more powerful nowadays, but it’s widely known that additional tooling needs to be used in order to make css somehow work for teams.

### CSS in the wild-west

In the 90’s we use to focus on creating “fancy” interfaces, the wow factor was the most important thing, inline styles were the thing back then, we just didn’t care if things looked different, in the end Webpages were like cute toys we could threw some gifs, marquees and other horrible (at the time impressive) elements over and expect to catch your visitors’ attention.

![](https://pbs.twimg.com/media/BICSfzSCMAAyi5X.png)

After that, we started creating dynamic sites, but css remained a consistent mess, every developer had his/her way of doing css. Some of us struggled with **specificity**, causing visual regressions when introducing new code, we relied on using **!important** to set in stone our strong will to make a UI element look in a certain way. But we soon realized that:

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

If you analyze a bit the markup you can see immediately the Block Element Modifier convention in play here.

You can see that we have two very explicit blocks in the code: .scenery and .sky, each one of them have their own blocks. Sky is the only one that has modifiers as it could be foggy, daytime or dusk, those are different characteristics that could be applied to the same element.

Let’s take a look at the companion css with some pseudo code that will let us analyze it better:

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

If you want to have an in-depth understanding of how BEM works, I recommend you [take a look at this article](https://m.alphasights.com/bem-i-finally-understand-b0c74815d5b0#.9vdcmiugz) , written by my colleague and friend Andrei Popa.

BEM is good in the sense that you made sure that components were unique #reusabilityFtw. With this kind of thinking some apparent patterns became more evident as we started migrating our old stylesheets into this new convention.

But, another set of problems came along:

*   Classname selection became a tedious task

*   Markup became bloated with all those long class names

*   You needed to explicitly extend every ui component whenever you wanted to reuse

*   Markup became unnecessarily semantic

### CSS Modules and local scope

Some of the problems that neither SASS or BEM fixed was that in the language logic there is no concept of true encapsulation, thus relying on the developer to choose unique class names. A process that felt could be solved by tools rather by conventions.

And this is exactly what CSS modules did, it relied on creating a dynamic class names for each locally defined style, making sure no visual regressions are caused by injecting new css properties, all styles were properly encapsulated.

CSS-Modules quickly gained popularity in the React ecosystem and now it’s common to see many react projects using it, it has it’s pros and cons but over all it proves to be a good paradigm to use.

But… CSS Modules by itself doesn’t solve the core problems of CSS, it only shows you a way of localizing style definitions: _a clever way of automating BEM so you don’t need to think about chosing a class name ever again_ (or at least think about it less).

But it does not alleviate the need for a good and predictable style architecture that is easy to extend reuse and control with the least amount of effort.

This is how local css looks like:

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

You can see that it’s just css, the main difference is that all classNames prepended with :local will generate a unique class name that looks something like this:

.app-components-button-__root — 3vvFf {}

You can configure the generated ident with the `localIdentName` query parameter. Example: `css-loader?localIdentName=[path][name]---[local]---[hash:base64:5]` for easier debugging.

That’s the simple principle behind Local CSS Modules. If you can see, local modules became a way to automate the BEM notation by generating a unique className that was sure it wouldn’t clash with other’s even if they used the same name. Quite convenient.

### Styled Components to blend css in JS (fully)

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

It quickly became apparent that CSS Modules nor Styled Components by themselves was not the perfect solution, it needed some kind of pattern in order for it to work and scale. The pattern emerged by defining what a component is and separating it fully from logic, creating core components which sole purpose is to style and nothing more.

An example implementation of such component using CSS Modules:

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

If you see, there’s nothing fancy in here, just a component that receives props and those are mapped to the children component. In other words: the wrapping component transfers all the props to the children.

Then your component can be consumed in the following way

```javascript
import React from "react"
import Button from "components/core/button"

const = Component = () => <Button theme={ Button.theme.secondary }>Some Button</Button>

export default Component
```

Let me show you a similar example of a full implementation of a button using styled-components:

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

What’s interesting about this pattern is that the component is dumb and only serves as a wrapper of css definitions mapped to the parent component. There is one advantage of doing this:

_It lets us define a base UI api which you can swap at will and make sure that all UI remains consistent throughout the application._

This way we can fully isolate the design process from the implementation process, making it possible to trigger them in parallel if wanted; you can have 1 developer focusing on the implementation of the feature and another polishing the UI achieving full separation of concerns.

Sounds like a great solution so far, internally we had discussions around it and thought it was a good idea to follow this pattern. Together with this pattern we started identifying other useful patterns as well:

#### **Prop receivers**

These do the function of listening to props passed to any component, thus making it easy to use these functions in any component you want, making it the holy grail for reusability and extending the capabilities of any given component, you can think of it as a way of inheriting modifiers, an example of what I mean by this:

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

Example of how to use prop receivers

This way you are sure that you won’t need to hardcode all the borders again for each specific component 🏆, saving you tons of time.

#### Placeholder / Mixin like functionality

In styled components you can use the full power of JS to be able to create functions not just as prop receivers but also as a way of sharing code between different components, here is an example:

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
