# CSS / Less style guide

## Table of Contents

1. [Terminology](#terminology)
    - [Rule Declaration](#rule-declaration)
    - [Selectors](#selectors)
    - [Properties](#properties)
1. [CSS](#css)
    - [Formatting](#formatting)
    - [OOCSS but no BEM](#oocss-but-no-bem)
    - [Immediate child selector](#immediate-child-selector)
    - [Balanced margin over padding](#balanced-margin-over-padding)

## Terminology

### Rule declaration

A “rule declaration” is the name given to a selector (or a group of selectors) with an accompanying group of properties. Here's an example:

```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

### Selectors

In a rule declaration, “selectors” are the bits that determine which elements in the DOM tree will be styled by the defined properties. Selectors can match HTML elements, as well as an element's class, ID, or any of its attributes. Here are some examples of selectors:

```css
.my-element-class {
  /* ... */
}
```

### Properties

Finally, properties are what give the selected elements of a rule declaration their style. Properties are key-value pairs, and a rule declaration can contain one or more property declarations. Property declarations look like this:

```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

**[⬆ back to top](#table-of-contents)**

## CSS

### Formatting

```css
component-name.theme--bright.active > .inner 
{
    background: red;
}
```

* Use soft tabs (4 spaces) for indentation
* Prefer single dashes over camelCasing in class names.
* Double dashes is for separation of names or used in modifiers.
* Do not use ID selectors
* When using multiple selectors in a rule declaration, give each selector its own line.

### OOCSS but no BEM

We encourage the idea of OOCSS to separate skin and structure, specially when building component that has different themes applied to it. 
However we don't encourage using BEM as BEM encourages flatten rule declarations with minimal nested styles, and that make it unpleasant to code with. 

1. BEM advocates that it lowers css specificity, it is a [half-truth](https://en.wikipedia.org/wiki/Half-truth), the true specificity can't be lowered as it is direct reflection of the complexity of the product. Instead BEM hides or shiftes the specificity to somewhere else -- when it lowers the specificity of selector, it also highers the specificity of each single selector names.
1. As a result of above point, BEM developers are usually end up coding long, repeatitive, redundant and tedious css class names, which neither pleasant to look at nor fun to maintain with.
1. The parent and child relationship expressed in BEM naming convention is a duplication of the natural html structure, when html gets restructured, developers will have to go back to change the BEM class name repeatitively for each declaration. Also people often forgot to go back to change the css name accordingly, with time, the code became unreadable.
1. BEM make it harder to work with CSS preprocessors. When using css preprocessors, the nesting became much more pleasant to work with, you don't have to repeatitively coding the parent class as they support nesting coding format naturally. Also because of the flattened structure, preprocessor variables usually will have to be placed in global, which also often cause naming conflict especially when project grows bigger.

So instead of BEM we use [Immediate child selector](#immediate-child-selector) to solve problems that BEM was trying to solve, with elegance and redundancies-free and it feels much more nature to do so when work with CSS preprocessors.

### Immediate child selector

We heavily relies to immediate child selector `>` when building our components, here are the reasons:

#### Using `>` makes the html structure and specificity clearer 

If you are nesting without `>`, you are very likely to forget to go back and correct the css specific when additional layer is added into html structure. Instead by using `>`, when extra layer is added or removed from html structure, the declaration immediate fail to target the original element, force developer to correct the specific of the element immediately.

#### Using `>` makes clear separation when components are nested

If `>` is not used and when components are nested together, if both components use very commonly named css class such as `.inner`, it is very likely the inner component gets some styles from outer one. Using `>` specifically targets the element you want to apply style with so it won't penetrate to the descendant elements.

#### Using `>` makes class name shorter and clearer

`.stage.theme--birght.active` No more worrying over `.active` is used or styled somewhere else, you can use it straight way.

#### Immediate child selector with shadow DOM

Whenever it is ripe to use shadow DOM we shall embrace it ASAP, but that doesn't mean we will have to abandon `>` because of it, `>` still has a lot of beneficials within the same component.

### Balanced margin over padding

Often in the pass you might saw lots of css toolkit, such as bootstrap, keeps setting only the `margin-top` or `margin-left` but leaves the `margin-bottom` or `margin-right` unset. 

These unbalances not only cause problem when reusing the style on RTL website, it also makes it harder to build a highly modularised component system. 

Imagine if a `heading` component is followed by `terms and conditions` component immediately, because the margin is only set on `terms and conditions` style, the gap between them will be way too small for the `heading`. 

So to solve the simliar issue on the Y axis we utilise margin collapsing, it naturely choose the largest margin between 2 and the gift from CSS 1.0 and should be use whenever it is possible. For X axis we will at least make sure there are balanced margin so it is neater and friendlier to RTL website.
