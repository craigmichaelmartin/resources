# HTML Styleguide

_Attribution: The [guidelines](https://gist.github.com/fat/a47b882eb5f84293c4ed) [Jacob Thorton](https://github.com/fat) wrote for [Medium](https://medium.com/@fat/mediums-css-is-actually-pretty-fucking-good-b8e2a6c78b06); The [best practices](http://nicolasgallagher.com/about-html-semantics-front-end-architecture/) [Nicolas Gallagher](https://github.com/necolas) wrote in developing [SUIT CSS](https://github.com/suitcss/suit)._


**Introduction**

These naming conventions are adapted from the SUIT CSS framework, itself an evolution of methodologies from BEM, OOCSS, and SMACSS. which seek to challenge the cascade of styles with meaningful naming coventions designed to limit the raw number of global classes through block namespaces and class driven relationships, contrasted against complex, generic, and therefore often brittle cascading selector expressions.  This is to say, it relies on _structured class names_ and _meaningful hyphens_ (i.e., not using hyphens merely to separate words). This is to help work around the current limits of applying CSS to the DOM (i.e., the lack of style encapsulation) and to better communicate the relationships between classes.

**Table of contents**

* [JavaScript](#javascript)
* [Utilities](#utilities)
  * [u-utilityName](#u-utilityName)
* [Components](#components)
  * [componentName](#componentName)
  * [componentName--modifierName](#componentName--modifierName)
  * [componentName-descendantName](#componentName-descendantName)
  * [componentName.is-stateOfComponent](#is-stateOfComponent)
* [Classes vs IDs](#hooks)
* [Take Away](#takeaway)

<a name="javascript"></a>
## JavaScript

syntax: `js-<targetName>`

JavaScript-specific classes reduce the risk that changing the structure or theme of components will inadvertently affect any required JavaScript behaviour and complex functionality. It is not neccesarry to use them in every case, just think of them as a tool in your utility belt. If you are creating a class, which you dont intend to use for styling, but instead only as a selector in JavaScript, you should probably be adding the `js-` prefix. In practice this looks like this:

```html
<a href="/login" class="btn btn-primary js-login"></a>
```

**Again, JavaScript-specific classes should not, under any circumstances, be styled.**

<a name="utilities"></a>
## Utilities

Utility classes are low-level structural and positional traits. Utilities can be applied directly to any element; multiple utilities can be used together; and utilities can be used alongside component classes.

Utilities exist because certain CSS properties and patterns are used frequently. For example: floats, containing floats, vertical alignment, text truncation. Relying on utilities can help to reduce repetition and provide consistent implementations. They also act as a philosophical alternative to functional (i.e. non-polyfill) mixins.


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
    â€¦
  </p>
</div>
```

<a name="components"></a>
## Components

Syntax: `<componentName>[--modifierName|-descendantName]`

Component driven development offers several benefits when reading and writing HTML and CSS:

* It helps to distinguish between the classes for the root of the component, descendant elements, and modifications.
* It keeps the specificity of selectors low.
* It helps to decouple presentation semantics from document semantics.

You can think of components as custom elements that enclose specific semantics, styling, and behaviour.


<a name="componentName"></a>
### ComponentName

The component's name must be written in camel case.

```css
.myComponent { /* â€¦ */ }
```

```html
<article class="myComponent">
  â€¦
</article>
```

<a name="componentName--modifierName"></a>
### componentName--modifierName

A component modifier is a class that modifies the presentation of the base component in some form. Modifier names must be written in camel case and be separated from the component name by two hyphens. The class should be included in the HTML _in addition_ to the base component class.

```css
/* Core button */
.btn { /* â€¦ */ }
/* Default button style */
.btn--default { /* â€¦ */ }
```

```html
<button class="btn btn--primary">â€¦</button>
```
<a name="componentName-descendantName"></a>
### componentName-descendantName

A component descendant is a class that is attached to a descendant node of a component. It's responsible for applying presentation directly to the descendant on behalf of a particular component. Descendant names must be written in camel case.

```html
<article class="tweet">
  <header class="tweet-header">
    <img class="tweet-avatar" src="{$src}" alt="{$alt}">
    â€¦
  </header>
  <div class="tweet-body">
    â€¦
  </div>
</article>
```

<a name="is-stateOfComponent"></a>
### componentName.is-stateOfComponent

Use `is-stateName` for state-based modifications of components. The state name must be Camel case. **Never style these classes directly; they should always be used as an adjoining class.**

JS can add/remove these classes. This means that the same state names can be used in multiple contexts, but every component must define its own styles for the state (as they are scoped to the component).

```css
.tweet { /* â€¦ */ }
.tweet.is-expanded { /* â€¦ */ }
```

```html
<article class="tweet is-expanded">
  â€¦
</article>
```

<a name="hooks"></a>
## Avoid using IDs

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. If you need a javascript hook, use a `js-` prefixed class. This has the advantage of being descriptive as to its use (css or js), and avoids broken behavior due to ID collisions which are hard to track down and annoying. Also, ID selectors introduce an unnecessarily high level of specificity to your rule declarations, and they are not reusable.

<a name="takeaway"></a>
## The Take Away
* Classes are to be named meaningfully.
* Classes to be used as javascript hooks should be prefixed with `js-`, proceed with camelcase name, and have no css styles associated with them.
* Classes to be used as css hooks for style either are arranged according to components - which use camel case for multiple words, hyphons to denote descendants, double hyphons to denote modifiers, and `is-` to denote state (never style `is-` classes directly) - or are utilities `u-` (proceed camelcased).
* Never cross purpose css and js classes.
* Don't using IDs. If you need a javascript hook, use a `js-` prefixed class.

```html
<article class="tweet is-open js-tweet">
  <header class="tweet-header tweet-header--large">
    <img class="tweet-avatar js-tweetAvatar" src="{$src}" alt="{$alt}">
    …
  </header>
  <div class="tweet-body tweet-body--large">
    …
  </div>
</article>
```
```css
.tweet {}                       /* .componentName                       */
.tweet.is-open {}               /* .component.Nameis-StateOfComponent   */
.tweet--large {}                /* .componentName--modifier             */
.tweet-header {}                /* .componentName-descendant            */
.tweet-header--large {}         /* .componentName-descendant--modifier  */
.tweet-body {}                  /* .componentName-anotherDescendant     */
```
