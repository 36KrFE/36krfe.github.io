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

### **SASS来解救**

SASS将CSS转换为一种得体的编程语言，以预处理引擎的形式，使**嵌套，变量，混合，扩展**和**逻辑**在样式表中得以实现，因此您可以更好地组织您的css文件，而且至少有了一些把css块解构成更小的文件的方法，这当然是一件很棒的事情。

它本质上采用sass代码，对其进行预处理，并以全局css打包的形式输出文件的编译版本。 很棒对吗？ 但是我不会这么说，过一会儿你就会很明显地发现，除非有策略性和最佳实践的使用，否则比起减少的麻烦，SASS只会造成更多问题。

突然之间，我们不知道预处理器在暗地里做了什么，并且懒惰地依靠嵌套来解决特异性冲突，但是导致编译的样式表变得很大。

直到BEM来了...

### BEM和组件化思考

当BEM来临之际，这是一股新鲜空气，让我们更多地思考可重用性和组件化。 它基本上将语义化提升到一个新的水平，通过使用简单的模块+元素+修饰符的命名规范，使我们确保className是唯一的，从而减少特异性冲突的风险。 看下面的例子：

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

如果您分析一下这些标记，您会立即看到BEM规范在这里的应用。

您可以看到我们在代码中有两个非常明确的模块：.scenery和.sky，它们中的每一个都有自己的块。 .sky是唯一具有修饰语的模块，因为它可能是foggy，daytime或dusk三种状态，这些状态是可以应用于同一元素的不同特征。

我们来看一下相关的css代码，这样可以让我们更好的分析它：

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

*   类名选择成为了一个乏味的任务

*   标记会因为带有这些长的类名而变得臃肿

*   每当你想要重用时，你需要明确地扩展每个ui组件

*   标记的语义化变得不再必要

### CSS Modules and local scope

Some of the problems that neither SASS or BEM fixed was that in the language logic there is no concept of true encapsulation, thus relying on the developer to choose unique class names. A process that felt could be solved by tools rather by conventions.

And this is exactly what CSS modules did, it relied on creating a dynamic class names for each locally defined style, making sure no visual regressions are caused by injecting new css properties, all styles were properly encapsulated.

CSS-Modules quickly gained popularity in the React ecosystem and now it’s common to see many react projects using it, it has it’s pros and cons but over all it proves to be a good paradigm to use.

But… CSS Modules by itself doesn’t solve the core problems of CSS, it only shows you a way of localizing style definitions: _a clever way of automating BEM so you don’t need to think about chosing a class name ever again_ (or at least think about it less).

But it does not alleviate the need for a good and predictable style architecture that is easy to extend reuse and control with the least amount of effort.

This is how local css looks like:

![](http://p0.qhimg.com/t01622984a32b9bea15.jpg)

You can see that it’s just css, the main difference is that all classNames prepended with :local will generate a unique class name that looks something like this:

.app-components-button-__root — 3vvFf {}

You can configure the generated ident with the `localIdentName` query parameter. Example: `css-loader?localIdentName=[path][name]---[local]---[hash:base64:5]` for easier debugging.

That’s the simple principle behind Local CSS Modules. If you can see, local modules became a way to automate the BEM notation by generating a unique className that was sure it wouldn’t clash with other’s even if they used the same name. Quite convenient.

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

It quickly became apparent that CSS Modules nor Styled Components by themselves was not the perfect solution, it needed some kind of pattern in order for it to work and scale. The pattern emerged by defining what a component is and separating it fully from logic, creating core components which sole purpose is to style and nothing more.

An example implementation of such component using CSS Modules:

![](http://p0.qhimg.com/t01622984a32b9bea15.jpg)

If you see, there’s nothing fancy in here, just a component that receives props and those are mapped to the children component. In other words: the wrapping component transfers all the props to the children.

Then your component can be consumed in the following way

![](http://p0.qhimg.com/t01622984a32b9bea15.jpg)

Let me show you a similar example of a full implementation of a button using styled-components:

![](http://p0.qhimg.com/t01622984a32b9bea15.jpg)

What’s interesting about this pattern is that the component is dumb and only serves as a wrapper of css definitions mapped to the parent component. There is one advantage of doing this:

_It lets us define a base UI api which you can swap at will and make sure that all UI remains consistent throughout the application._

This way we can fully isolate the design process from the implementation process, making it possible to trigger them in parallel if wanted; you can have 1 developer focusing on the implementation of the feature and another polishing the UI achieving full separation of concerns.

Sounds like a great solution so far, internally we had discussions around it and thought it was a good idea to follow this pattern. Together with this pattern we started identifying other useful patterns as well:

#### **Prop receivers**

These do the function of listening to props passed to any component, thus making it easy to use these functions in any component you want, making it the holy grail for reusability and extending the capabilities of any given component, you can think of it as a way of inheriting modifiers, an example of what I mean by this:

![](http://p0.qhimg.com/t01622984a32b9bea15.jpg)

Example of how to use prop receivers

This way you are sure that you won’t need to hardcode all the borders again for each specific component 🏆, saving you tons of time.

#### Placeholder / Mixin like functionality

In styled components you can use the full power of JS to be able to create functions not just as prop receivers but also as a way of sharing code between different components, here is an example:

![](http://p0.qhimg.com/t01622984a32b9bea15.jpg)

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