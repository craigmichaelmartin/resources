# HTML Styleguide

**Introduction**

These naming conventions are adapted from the SUIT CSS framework, itself an evolution of methodologies from BEM, OOCSS, and SMACSS, **which seek to challenge the cascade of styles with meaningful naming coventions designed to limit the raw number of global classes through block namespaces and class driven relationships**, contrasted against complex, generic, and therefore often brittle cascading selector expressions.  This is to say, it relies on _structured class names_ using _meaningful hyphens_ (i.e., not using hyphens merely to separate words) to scope styles. This is to help work around the current limits of applying CSS to the DOM (i.e., the lack of style encapsulation) and to better communicate the relationships between classes.

**Table of contents**

* [JavaScript](#javascript)
* [Utilities](#utilities)
  * [u-utilityName](#u-utilityName)
* [Components](#components)
  * [ComponentName](#ComponentName)
  * [ComponentName--modifierName](#ComponentName--modifierName)
  * [ComponentName-descendantName](#ComponentName-descendantName)
  * [ComponentName.is-stateOfComponent](#is-stateOfComponent)
* [Miscellaneous](#miscellaneous)
  * [Classes vs IDs](#hooks)
  * [!important](#important)
  * [Overqualified elements](#overqualified)
  * [Using the Filesystem](#filesystem)
  * [Is everything an island then?](#island)
* [Take Away](#takeaway)
* [Attributions and Further Reading](#attributions)

<a name="javascript"></a>
## JavaScript

syntax: `js-<targetName>`

JavaScript-specific classes reduce the risk that changing the structure or theme of components will inadvertently affect any required JavaScript behaviour and complex functionality. If you are creating a class, which you dont intend to use for styling, but instead only as a selector in JavaScript, you should probably be adding the `js-` prefix. The class begin with `js-` and proceeds camelcased. In practice this looks like this:

```html
<a href="/login" class="btn btn--primary js-login"></a>
```

**Again, JavaScript-specific classes should not, under any circumstances, be styled.**

<a name="utilities"></a>
## Utilities

Utility classes are low-level structural and positional traits. Utilities can be applied directly to any element; multiple utilities can be used together; and utilities can be used alongside component classes.

Utilities exist because certain CSS properties and patterns are used frequently. For example: floats, containing floats, vertical alignment, text truncation. Relying on utilities can help to reduce repetition and provide consistent implementations. They also act as a philosophical alternative to functional (i.e. non-polyfill) mixins. The class begins with `u-` and proceeds camelcased.


```html
<div class="u-clearfix">
  <p class="u-textTruncate">{$text}</p>
  <img class="u-pullLeft" src="{$src}" alt="">
  <img class="u-pullLeft" src="{$src}" alt="">
  <img class="u-pullLeft" src="{$src}" alt="">
</div>
```

<a name="u-utilityName"></a>
### u-utilityName

Syntax: `u-<utilityName>`

Utilities must use a camel case name, prefixed with a `u` namespace. What follows is an example of how various utilities can be used to create a simple structure within a component.

```html
<div class="u-clearfix">
  <a class="u-pullLeft" href="{$url}">
    <img class="u-block" src="{$src}" alt="">
  </a>
  <p class="u-sizeFill u-textBreak">
    ...
  </p>
</div>
```

<a name="components"></a>
## Components

Syntax: `<ComponentName>[--modifierName|-descendantName]`

Component driven development offers several benefits when reading and writing HTML and CSS:

* It helps to distinguish between the classes for the root of the component, descendant elements, and modifications.
* It keeps the specificity of selectors low.
* It helps to decouple presentation semantics from document semantics.

You can think of components as custom elements that enclose specific semantics, styling, and behaviour.


<a name="ComponentName"></a>
### ComponentName

The component's name must be written in pascal case.

```css
.MyComponent { /* ... */ }
```

```html
<article class="MyComponent">
  ...
</article>
```

<a name="ComponentName--modifierName"></a>
### ComponentName--modifierName

A component modifier is a class that modifies the presentation of the base component in some form. Modifier names must be written in camel case and be separated from the component name by two hyphens. The class should be included in the HTML _in addition_ to the base component class.

```css
/* Core button */
.Btn { /* ... */ }
/* Default button style */
.Btn--default { /* ... */ }
```

```html
<button class="Btn Btn--primary">...</button>
```
<a name="ComponentName-descendantName"></a>
### ComponentName-descendantName

A component descendant is a class that is attached to a descendant node of a component. It's responsible for applying presentation directly to the descendant on behalf of a particular component. Descendant names must be written in camel case.

```html
<article class="Tweet">
  <header class="Tweet-header">
    <img class="Tweet-avatar" src="{$src}" alt="{$alt}">
    ...
  </header>
  <div class="Tweet-body">
    ...
  </div>
</article>
```

<a name="is-stateOfComponent"></a>
### componentName.is-stateOfComponent

Use `is-stateName` for state-based modifications of components. The state name must be Camel case. **Never style these classes directly; they should always be used as an adjoining class.**

JS can add/remove these classes. This means that the same state names can be used in multiple contexts, but every component must define its own styles for the state (as they are scoped to the component).

```css
.Tweet { /* ... */ }
.Tweet.is-expanded { /* ... */ }
```

```html
<article class="Tweet is-expanded">
  ...
</article>
```

<a name="miscellaneous"></a>
## Miscellaneous
A few miscellaneous notes.

<a name="hooks"></a>
### Avoid using IDs
While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. If you need a javascript hook, use a `js-` prefixed class. This has the advantage of being descriptive as to its use (css or js), and avoids broken behavior due to ID collisions which are hard to track down and annoying. Also, ID selectors introduce an unnecessarily high level of specificity to your rule declarations, and they are not reusable.

<a name="important"></a>
### !important
The !important annotation is used to artificially increase the specificity of a given property value in a rule. This is usually an indication that the specificity of the entire CSS has gotten a bit out of control and needs to be refactored. The more frequently !important is found in CSS, the more likely it is that developers are having trouble styling parts of a page effectively.

<a name="overqualified"></a>
### Overqualified Elements
Writing selectors such as li.active are unnecessary unless the element name causes the class to behave differently. In most cases, it's safe to remove the element name from the selector, both reducing the size of the CSS as well as improving the selector performance (doesn't have to match the element anymore).

Removing the element name also loosens the coupling between your CSS and your HTML, allowing you to change the element on which the class is used without also needing to update the CSS.

<a name="filesystem"></a>
### Using the Filesystem
One question remains: Given this approach of limitting the number of global classes into component classes and utility classes, how do we ensure even these global classes do not collide? A simple solution is to use the filesytem: one component per file, with the filename the same as the component all in the same directory (no nested directories). The filesystem will ensure against collisions.

<a name="island"></a>
### Is Everything an Island, then?
Every component is an island - completelying encapsulating its styles without any "leaking" of styles outside of itself. However, that is not to say every component necessarily inherits nothing. The amount of cascade that comes to the components is up to your team's best judgement. Base typography and default link styling seems like reasonable ways to use the cascade.

<a name="takeaway"></a>
## The Take Away
* Classes are to be named meaningfully.
* Classes to be used as javascript hooks should be prefixed with `js-`, proceed with camelcase name, and have no css styles associated with them.
* Classes to be used as css hooks for style either are arranged according to components (which use camel case for multiple words, hyphons to denote descendants, double hyphons to denote modifiers, and `is-` to denote state (never style `is-` classes directly)) or are utilities `u-` (proceed camelcased).
* Never cross purpose css and js classes.
* Don't using IDs. If you need a javascript hook, use a `js-` prefixed class.

```css
/* Utility */
.u-utilityName {}

/* Component */
.ComponentName {}

/* Component modifier */
.ComponentName--modifierName {}

/* Component descendant */
.ComponentName-descendant {}

/* Component descendant modifier */
.ComponentName-descendant--modifierName {}

/* Component state (scoped to component) */
.ComponentName.is-stateOfComponent {}
```

EXAMPLE:

```html
<article class="Tweet is-open js-tweet">
  <header class="Tweet-header Tweet-header--large">
    <img class="Tweet-avatar" src="{$src}" alt="{$alt}">
    …
  </header>
  <div class="Tweet-body Tweet-body--large">
    …
  </div>
</article>
```
```css
.Tweet {...}                       /* .ComponentName                       */
.Tweet.is-open {...}               /* .ComponentName.is-stateOfComponent   */
.Tweet--large {...}                /* .ComponentName--modifier             */
.Tweet-header {...}                /* .ComponentName-descendant            */
.Tweet-header--large {...}         /* .ComponentName-descendant--modifier  */
.Tweet-body {...}                  /* .ComponentName-anotherDescendant     */
```

<a name="attributions"></a>
## Attributions and Further Reading
* ["About HTML semantics and front-end architecture"](http://nicolasgallagher.com/about-html-semantics-front-end-architecture/) by [Nicolas Gallagher](https://github.com/necolas).
* [SUIT CSS](https://github.com/suitcss/suit) methodology by [Nicolas Gallagher](https://github.com/necolas).
* [The guidelines](https://gist.github.com/fat/a47b882eb5f84293c4ed) [Jacob Thorton](https://github.com/fat) [wrote while refactoring](https://medium.com/@fat/mediums-css-is-actually-pretty-fucking-good-b8e2a6c78b06) Medium's CSS.
* ["Cascading Shit Show"](https://www.youtube.com/watch?v=iniwPUEbPUM) talk by [Jacob Thorton](https://github.com/fat).
* ["Stop the Cascade"](http://markdotto.com/2012/03/02/stop-the-cascade/) by [Mark Otto] for his [styleguide](http://codeguide.co/#css-classes)
* ["Side Effects in CSS"](http://philipwalton.com/articles/side-effects-in-css/) by [Philip Walton](https://github.com/philipwalton)
* ["CSS Architecture"](http://philipwalton.com/articles/css-architecture/) by [Philip Walton](https://github.com/philipwalton)
* ["Solidify CSS naming & structure conventions"](https://github.com/google/web-starter-kit/issues/300) issue by [Addy Osmani]("https://github.com/addyosmani")
* ["Airbnb CSS / Sass Styleguide"](https://github.com/airbnb/css)
* ["BEM"](https://en.bem.info/methodology/)
* ["OOCSS"](https://github.com/stubbornella/oocss) by [Nicole Sullivan](https://github.com/stubbornella).
* ["SMACSS"](https://smacss.com/)
