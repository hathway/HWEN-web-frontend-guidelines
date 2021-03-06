---
layout: longform-document
title: Web Frontend Development Guidelines
category: style-guides
---

# Web Frontend Development Guidelines

## HTML

### Syntax

* Use soft tabs with two spaces—they're the only way to guarantee code renders the same in any environment.
* Nested elements should be indented once (two spaces).
* Always use either double quotes or no quotes, never single quotes, on attributes.
* Don't include a trailing slash in self-closing elements—the [HTML5 spec](http://dev.w3.org/html5/spec-author-view/syntax.html#syntax-start-tag) says they're optional.
* Don’t omit optional closing tags (e.g. `</li>` or `</body>`).

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Page title</title>
  </head>
  <body>
    <img src="images/company-logo.png" alt="Company">
    <h1 class="hello-world">Hello, world!</h1>
  </body>
</html>
```

### HTML5 doctype

Enforce standards mode and more consistent rendering in every browser possible with this simple doctype at the beginning of every HTML page.

```html
<!DOCTYPE html>
<html>
  <head>
  </head>
</html>
```

### Semantics

HTML5 provides us with lots of semantic elements aimed to describe precisely the content. Make sure you benefit from its rich vocabulary.

```html
<!-- bad -->
<div id="main">
  <div class="article">
    <div class="header">
      <h1>Blog post</h1>
      <p>Published: <span>21st Feb, 2015</span></p>
    </div>
    <p>…</p>
  </div>
</div>

<!-- good -->
<main>
  <article>
    <header>
      <h1>Blog post</h1>
      <p>Published: <time datetime="2015-02-21">21st Feb, 2015</time></p>
    </header>
    <p>…</p>
  </article>
</main>
```

Make sure you understand the semantic of the elements you're using. It's worse to use a semantic
element in a wrong way than staying neutral.

Strive to maintain HTML standards and semantics, but not at the expense of practicality. Use the least amount of markup with the fewest intricacies whenever possible.

```html
<!-- bad -->
<h1>
  <figure>
    <img alt=Company src=logo.png>
  </figure>
</h1>

<!-- good -->
<h1>
  <img alt=Company src=logo.png>
</h1>
```

### Brevity

Keep your code terse. Forget about your old XHTML habits.

```html
<!-- bad -->
<!doctype html>
<html lang=en>
  <head>
    <meta http-equiv=Content-Type content="text/html; charset=utf-8" />
    <title>Contact</title>
    <link rel=stylesheet href=style.css type=text/css />
  </head>
  <body>
    <h1>Contact me</h1>
    <label>
      Email address:
      <input type=email placeholder=you@email.com required=required />
    </label>
    <script src=main.js type=text/javascript></script>
  </body>
</html>

<!-- good -->
<!doctype html>
<html lang=en>
  <meta charset=utf-8>
  <title>Contact</title>
  <link rel=stylesheet href=style.css>

  <h1>Contact me</h1>
  <label>
    Email address:
    <input type=email placeholder=you@email.com required>
  </label>
  <script src=main.js></script>
</html>
```

### Accessibility

Accessibility shouldn't be an afterthought. You don't have to be a WCAG expert to improve your
website, you can start immediately by fixing the little things that make a huge difference, such as:

* learning to use the `alt` attribute properly
* making sure your links and buttons are marked as such (no `<div class=button>` atrocities)
* not relying exclusively on colors to communicate information
* explicitly labelling form controls

```html
<!-- bad -->
<h1><img alt="Logo" src="logo.png"></h1>

<!-- good -->
<h1><img alt="My Company, Inc." src="logo.png"></h1>
```

### Attribute Order

HTML attributes should come in this particular order for easier reading of code.

* `class`
* `id, name`
* `data-*`
* `src, for, type, href`
* `title, alt`
* `aria-*, role`

Classes make for great reusable components, so they come first. Ids are more specific and should be used sparingly (e.g., for in-page bookmarks), so they come second.

### Boolean attributes

A boolean attribute is one that needs no declared value. XHTML required you to declare a value, but HTML5 has no such requirement.

For further reading, consult the [WhatWG section on boolean attributes](http://www.whatwg.org/specs/web-apps/current-work/multipage/common-microsyntaxes.html#boolean-attributes):

>The presence of a boolean attribute on an element represents the true value, and the absence of the attribute represents the false value.

If you must include the attribute's value, and **you don't need to**, follow this WhatWG guideline:

>If the attribute is present, its value must either be the empty string or [...] the attribute's canonical name, with no leading or trailing whitespace.

**In short, don't add a value.**

### Language

While defining the language and character encoding is optional, it's recommended to always declare
both at document level, even if they're specified in your HTTP headers. Favor UTF-8 over any other
character encoding.

```html
<!-- bad -->
<!doctype html>
<title>Hello, world.</title>

<!-- good -->
<!doctype html>
<html lang=en>
  <meta charset=utf-8>
  <title>Hello, world.</title>
</html>
```

### Performance

Unless there's a valid reason for loading your scripts before your content, don't block the
rendering of your page. If your style sheet is heavy, isolate the styles that are absolutely
required initially and defer the loading of the secondary declarations in a separate style sheet.
Two HTTP requests is significantly slower than one, but the perception of speed is the most
important factor.

```html
<!-- bad -->
<!doctype html>
<meta charset=utf-8>
<script src=analytics.js></script>
<title>Hello, world.</title>
<p>...</p>

<!-- good-->
<!doctype html>
<meta charset=utf-8>
<title>Hello, world.</title>
<p>...</p>
<script src=analytics.js></script>
```

## CSS

### Coding Style

#### CSS Syntax

* Use soft tabs with two spaces—they're the only way to guarantee code renders the same in any environment.
* When grouping selectors, keep individual selectors to a single line.
* Include one space before the opening brace of declaration blocks for legibility.
* Place closing braces of declaration blocks on a new line.
* Include one space after `:` for each declaration.
* Each declaration should appear on its own line for more accurate error reporting.
* End all declarations with a semi-colon. The last declaration's is optional, but your code is more error prone without it.
* Comma-separated property values should include a space after each comma (e.g., `box-shadow`).
Don't include spaces after commas *within* `rgb(), rgba(), hsl(), hsla(),` or `rect()` values. This helps differentiate multiple color values (comma, no space) from * multiple property values (comma with space). Also, don't prefix values with a leading zero (e.g., `.5` instead of `0.5`).
* Lowercase all hex values, e.g., `#fff`. Lowercase letters are much easier to discern when scanning a document as they tend to have more unique shapes.
* Use shorthand hex values where available, e.g., `#fff` instead of `#ffffff`.
* Quote attribute values in selectors, e.g., `input[type="text"]`. [They’re only optional in some cases(http://mathiasbynens.be/notes/unquoted-attribute-values#css), and it’s a good practice for consistency.
* Specify units for zero values (for px values), e.g., `margin: 0px;` instead of `margin: 0;`.
* Prefer `px` values over `rems` or `ems`, but if you must use relative values, use `rems` over `ems`.

```css
/* bad */
.selector, .selector-secondary, .selector[type=text] {
  padding:15px;
  margin:0 0 15px;
  background-color:rgba(0, 0, 0, 0.5);
  box-shadow:0 1px 2px #CCC,inset 0 1px 0 #FFFFFF
}

/* good */
.selector,
.selector-secondary,
.selector[type="text"] {
  padding: 15px;
  margin: 0px 0px 15px;
  background-color: rgba(0,0,0,.5);
  box-shadow: 0px 1px 2px #ccc, inset 0px 1px 0px #fff;
}
```

#### Single declarations

In instances where a rule set includes **only one declaration**, consider removing line breaks for readability and faster editing. Any rule set with multiple declarations should be split to separate lines.

The key factor here is error detection—e.g., a CSS validator stating you have a syntax error on Line 183. With a single declaration, there's no missing it. With multiple declarations, separate lines is a must for your sanity.

```css
/* Single declarations on one line */
.span1 { width: 60px; }
.span2 { width: 140px; }
.span3 { width: 220px; }

/* Multiple declarations, one per line */
.sprite {
  display: inline-block;
  width: 16px;
  height: 15px;
  background-image: url(../images/sprite.png);
}
.icon { background-position: 0px 0px; }
.icon-home { background-position: 0px -20px; }
.icon-account { background-position: 0px -40px; }
```

#### Shorthand notation

Strive to limit use of shorthand declarations to instances where you must explicitly set all the available values. Common overused shorthand properties include:

* `padding`
* `margin`
* `font`
* `background`
* `border`
* `border-radius`

Often times we don't need to set all the values a shorthand property represents. For example, HTML headings only set top and bottom margin, so when necessary, only override those two values. Excessive use of shorthand properties often leads to sloppier code with unnecessary overrides and unintended side effects.
The Mozilla Developer Network has a great article on [shorthand properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Shorthand_properties) for those unfamiliar with notation and behavior.

```css
/* bad */
.element {
  margin: 0 0 10px;
  background: red;
  background: url("image.jpg");
  border-radius: 3px 3px 0 0;
}

/* good */
.element {
  margin-bottom: 10px;
  background-color: red;
  background-image: url("image.jpg");
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
}
```

#### Units

Favor `px` over `rem` or `em`. Favor `rem` if you use relative units. Prefer seconds over
milliseconds. Use unitless values for things like line-height.

```css
/* bad */
div {
  margin: 0;
  font-size: .9em;
  line-height: 22px;
  transition: 500ms;
}

/* good */
div {
  margin: 0px;
  font-size: 13px;
  line-height: 1.5;
  transition: .5s;
}
```

#### Colors

If you need transparency, use `rgba`. Otherwise, always use the hexadecimal format.

```css
/* bad */
div {
  color: hsl(103, 54%, 43%);
}

/* good */
div {
  color: #5a3;
}
```

#### Language

Prefer English over math.

```css
/* bad */
:nth-child(2n + 1) {
  transform: rotate(360deg);
}

/* good */
:nth-child(odd) { transform: rotate(1turn); }
```

#### Editor preferences

* Set your editor to the following settings to avoid common code inconsistencies and dirty diffs:
* Use soft-tabs set to two spaces.
* Trim trailing white space on save.
* Set encoding to UTF-8.
* Add new line at end of files.

Consider documenting and applying these preferences to your project's `.editorconfig` file. For an example, see [the one in Bootstrap](https://github.com/twbs/bootstrap/blob/master/.editorconfig). Learn more about [EditorConfig](http://editorconfig.org/).

### Selectors

#### CSS Selectors

* Use classes over generic element tag for optimum rendering performance.
* Avoid using several attribute selectors (e.g., `[class^="..."]`) on commonly occuring components. Browser performance is known to be impacted by these.
* Keep selectors short and strive to limit the number of elements in each selector to three.
* Scope classes to the closest parent only when necessary (e.g., when not using prefixed classes).

Additional reading:

* [Scope CSS classes with prefixes](http://markdotto.com/2012/02/16/scope-css-classes-with-prefixes/)
* [Stop the cascade](http://markdotto.com/2012/03/02/stop-the-cascade/)

Minimize selectors tightly coupled to the DOM. Consider adding a class to the elements
you want to match when your selector exceeds 3 structural pseudo-classes, descendant or
sibling combinators.

```css
/* bad */
div:first-of-type :last-chid > p ~ *

/* good */
div:first-of-type .info
```

Avoid overloading your selectors when you don't need to.

```css
/* bad */
img[src$=svg], ul > li:first-child {
  opacity: 0;
}

/* good */
[src$=svg], ul > :first-child {
  opacity: 0;
}
```

```
/* bad */
span { ... }
.page-container #stream .stream-item .tweet .tweet-header .username { ... }
.avatar { ... }

/* good */
.avatar { ... }
.tweet-header .username { ... }
.tweet .avatar { ... }
```

#### Classes

* Avoid excessive and arbitrary shorthand notation. `.btn` is useful for `button,` but `.s` doesn't mean anything.
* Keep classes as short and succinct as possible.
* Use meaningful names; use structural or purposeful names over presentational.
* Prefix classes based on the closest parent or base class.

```css
/* bad */
.t { ... }
.red { ... }
.header { ... }

/* good */
.tweet { ... }
.important { ... }
.tweet-header { ... }
```

#### IDs

IDs are fine in markup as they can be targeted by JavaScript and can identify unique elements, but should be used sparingly in CSS. IDs have higher specificity than classes and element selectors, making them difficult to override.

### Naming Conventions

#### OOCSS

OOCSS/SMACSS refers to a separation of concerns between structure and presenational/visual styles. Look for patterns that can be reused to promote modularity in CSS and HTML structure. Modules should be used whenever possible to decrease the amount of code written, minimize overriding styles, and make CSS more navigable and manageable.

Resources for learning OOCSS/SMACSS:
* http://code.tutsplus.com/tutorials/object-oriented-css-what-how-and-why--net-6986
* http://drewbarontini.com/articles/single-responsibility/
* https://smacss.com/
* http://www.smashingmagazine.com/2011/12/an-introduction-to-object-oriented-css-oocss/
* http://www.stubbornella.org/content/2010/06/25/the-media-object-saves-hundreds-of-lines-of-code/

#### BEM

BEM stands for Block, Element, Modifier. This, along with OOCSS and the Single Responsibility Pricinciple are an effort to keep our CSS highly modular.

A component (or block) is given a descriptive name. A block is usually a parent element containing one or more child elements, e.g. ```search```.

An element is a child of a block. It's class name is written with the name of the block followed by double underscores followed by the name of the element, e.g. ```search__field```.

A modifier is a class that changes the style of a block or element, e.g. ```search--fullwidth```.

```html
<div class="search search--fullwidth">
  <input class="search__field">
  <button class="button search__button button--large">Submit</button>
</div>
```

Not everything needs to use BEM. A logo for instance might be a single element that isn't dependent on a parent element. However, it could use aspects of BEM, like modifer: ```<img src="..." class="logo logo__small" alt="">```. Use logic to determine what would be best served using BEM. The main goals are modularity and making it immediately apparent to other developers how each element relates to another, and how to easily style them.

Resources for learning BEM:
https://css-tricks.com/bem-101/
https://medium.com/objects-in-space/objects-in-space-f6f404727
https://en.bem.info/

### Specificity

#### Minimizing Specificity

Don't make values and selectors hard to override. Minimize the use of `id`'s
and avoid `!important`.

```css
/* bad */
.bar {
  color: green !important;
}
.foo {
  color: red;
}

/* good */
.foo.bar { color: green; }
.foo { color: red; }
```

#### Overriding

Overriding styles makes selectors and debugging harder. Avoid it when possible.

```css
/* bad */
li {
  visibility: hidden;
}
li:first-child {
  visibility: visible;
}

/* good */
li + li { visibility: hidden; }
```

#### Inheritance

Don't duplicate style declarations that can be inherited.

```css
/* bad */
div h1, div p {
  text-shadow: 0 1px 0 #fff;
}

/* good */
div { text-shadow: 0 1px 0 #fff; }
```

### Preprocessors

#### SASS/SCSS

Hathway uses [Sass](http://sass-lang.com/) for preprocessing CSS. Use the SCSS syntax over the Sass syntax, as it is syntactically closer to CSS and easy to port over if needed.

Learn to use and take advantage of variables and mixins to avoid repetition and make stylesheet-wide updates easier, but not at the expense of readability.

Break your CSS up into partials/components for better organization and readability (and import into your main SCSS file).

Avoid unnecessary nesting. Just because you can nest, doesn't mean you always should. Consider nesting only if you must scope styles to a parent and if there are multiple elements to be nested.

Never process Sass/CSS in the browser. Always compile locally before deploying.

### Organization

#### Organizing CSS

* Organize sections of code by component.
* Develop a consistent commenting hierarchy.
* Use consistent white space to your advantage when separating sections of code for scanning larger documents.
* When using multiple CSS files, break them down by component instead of page. Pages can be rearranged and components moved.

#### Declaration order

Related property declarations should be grouped together following the order:

* Positioning
* Box model
* Typographic
* Visual

Positioning comes first because it can remove an element from the normal flow of the document and override box model related styles. The box model comes next as it dictates a component's dimensions and placement.

Everything else takes place inside the component or without impacting the previous two sections, and thus they come last.

```css
.declaration-order {
  /* Positioning */
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 100;

  /* Box-model */
  display: block;
  float: right;
  width: 100px;
  height: 100px;

  /* Typography */
  font: normal 13px "Helvetica Neue", sans-serif;
  line-height: 1.5;
  color: #333;
  text-align: center;

  /* Visual */
  background-color: #f5f5f5;
  border: 1px solid #e5e5e5;
  border-radius: 3px;

  /* Misc */
  opacity: 1;
}
```

### Responsive

#### Media queries

Write CSS to be mobile-first. Let base styles apply to mobile devices, and then write media queries to override the styles for larger screens. Minify use of media queries as much as possible. Minify number of breakpoints as much as possible. Try to stick to around 3 - 4 breakpoints.

Place media queries within CSS declarations. Don't bundle them all in a separate stylesheet or at the end of the document. Doing so only makes it easier for folks to miss them in the future.

Try to avoid closed breakpoints (eg. `@media (min-width: 768px) and  (max-width: 992px ))`. Use only `min-width` whenever possible.

```CSS
.some-component {
  /* base styles */
  background-color: #fff;
  font-size: 20px;
  border: 1px solid #ccc;
  @media (min-width: 768px) { border-width: 2px; }
  @media (min-width: 992px) { border-width: 3px; }
}
```

### Positioning

#### Box model

The box model should ideally be the same for the entire document. A global
`* { box-sizing: border-box; }` is fine, but don't change the default box model
on specific elements if you can avoid it.

```css
/* bad */
div {
  width: 100%;
  padding: 10px;
  box-sizing: border-box;
}

/* good */
div {
  padding: 10px;
}
```

#### Flow

Don't change the default behavior of an element if you can avoid it. Keep elements in the
natural document flow as much as you can. For example, removing the white-space below an
image shouldn't make you change its default display:

```css
/* bad */
img {  display: block; }

/* good */
img { vertical-align: middle; }
```

Similarly, don't take an element off the flow if you can avoid it.

```css
/* bad */
div {
  width: 100px;
  position: absolute;
  right: 0;
}

/* good */
div {
  width: 100px;
  margin-left: auto;
}
```

#### Positioning Elements

There are many ways to position elements in CSS but try to restrict yourself to the
properties/values below. By order of preference:

```
display: block;
display: flex;
position: relative;
position: sticky;
position: absolute;
position: fixed;
```

### Animation

#### CSS Animation

Favor transitions over animations. Avoid animating other properties than
`opacity` and `transform`.

```css
/* bad */
div:hover {
  animation: move 1s forwards;
}
@keyframes move {
  100% {
    margin-left: 100px;
  }
}

/* good */
div:hover {
  transition: 1s;
  transform: translateX(100px);
}
```

#### js-prefix

Add a class with a prefix of ```js-``` to indicate elements that will be the targets of JavaScript manipulation. This makes it apparent Javascript code may be dependent on that a particular element's markup or structure, and therefore care should be taken when changing or restructuring the element.

```html
<a class="js-add-to-cart button">Add to Cart</a>
```

In addition, add classes with a prefix of ```is-``` to indicate a visual element's state if it's being manipulated by JavaScript.

```html
<div class="modal is-visible"></div>
```

### Browser Support

#### Vendor prefixes

Kill obsolete vendor prefixes aggressively. If you need to use them, insert them before the
standard property. Use of [Autoprefixer](https://github.com/postcss/autoprefixer) will speed up your workflow in this regard greatly. Make it part of your build process.

```css
/* bad */
div {
  transform: scale(2);
  -webkit-transform: scale(2);
  -moz-transform: scale(2);
  -ms-transform: scale(2);
  transition: 1s;
  -webkit-transition: 1s;
  -moz-transition: 1s;
  -ms-transition: 1s;
}

/* good */
div {
  -webkit-transform: scale(2);
  transform: scale(2);
  transition: 1s;
}
```

### General

#### Drawing

Avoid HTTP requests when the resources are easily replicable with CSS.

```css
/* bad */
div::before {
  content: url(white-circle.svg);
}

/* good */
div::before {
  content: "";
  display: block;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #fff;
}
```

#### Hacks

Don't use them.

```css
/* bad */
div {
  // position: relative;
  transform: translateZ(0);
}

/* good */
div {
  /* position: relative; */
  will-change: transform;
}
```

## JavaScript

### Performance

Favor readability, correctness and expressiveness over performance. JavaScript will basically never
be your performance bottleneck. Optimize things like image compression, network access and DOM
reflows instead. If you remember just one guideline from this document, choose this one.

```javascript
// bad (albeit way faster)
const arr = [1, 2, 3, 4];
const len = arr.length;
var i = -1;
var result = [];
while (++i < len) {
  var n = arr[i];
  if (n % 2 > 0) continue;
  result.push(n * n);
}

// good
const arr = [1, 2, 3, 4];
const isEven = n => n % 2 == 0;
const square = n => n * n;

const result = arr.filter(isEven).map(square);
```

### Statelessness

Try to keep your functions pure. All functions should ideally produce no side-effects, use no outside data and return new objects instead of mutating existing ones.

```javascript
// bad
const merge = (target, ...sources) => Object.assign(target, ...sources);
merge({ foo: "foo" }, { bar: "bar" }); // => { foo: "foo", bar: "bar" }

// good
const merge = (...sources) => Object.assign({}, ...sources);
merge({ foo: "foo" }, { bar: "bar" }); // => { foo: "foo", bar: "bar" }
```

### Natives

Rely on native methods as much as possible.

```javascript
// bad
const toArray = obj => [].slice.call(obj);

// good
const toArray = (() =>
  Array.from ? Array.from : obj => [].slice.call(obj)
)();
```

### Coercion

Embrace implicit coercion when it makes sense. Avoid it otherwise. Don't cargo-cult.

```javascript
// bad
if (x === undefined || x === null) { ... }

// good
if (x == undefined) { ... }
```

### Loops

Don't use loops as they force you to use mutable objects. Rely on `array.prototype` methods.

```javascript
// bad
const sum = arr => {
  var sum = 0;
  var i = -1;
  for (;arr[++i];) {
    sum += arr[i];
  }
  return sum;
};

sum([1, 2, 3]); // => 6

// good
const sum = arr =>
  arr.reduce((x, y) => x + y);

sum([1, 2, 3]); // => 6
```
If you can't, or if using `array.prototype` methods is arguably abusive, use recursion.

```javascript
// bad
const createDivs = howMany => {
  while (howMany--) {
    document.body.insertAdjacentHTML("beforeend", "<div></div>");
  }
};
createDivs(5);

// bad
const createDivs = howMany =>
  [...Array(howMany)].forEach(() =>
    document.body.insertAdjacentHTML("beforeend", "<div></div>")
  );
createDivs(5);

// good
const createDivs = howMany => {
  if (!howMany) return;
  document.body.insertAdjacentHTML("beforeend", "<div></div>");
  return createDiv(howMany - 1);
};
createDivs(5);
```

### Arguments

Forget about the `arguments` object. The rest parameter is always a better option because:

1. it's named, so it gives you a better idea of the arguments the function is expecting
2. it's a real array, which makes it easier to use.

```javascript
// bad
const sortNumbers = () =>
  Array.prototype.slice.call(arguments).sort();

// good
const sortNumbers = (...numbers) => numbers.sort();
```

### Apply

Forget about `apply()`. Use the spread operator instead.

```javascript
const greet = (first, last) => `Hi ${first} ${last}`;
const person = ["John", "Doe"];

// bad
greet.apply(null, person);

// good
greet(...person);
```

### Bind

Don't `bind()` when there's a more idiomatic approach.

```javascript
// bad
["foo", "bar"].forEach(func.bind(this));

// good
["foo", "bar"].forEach(func, this);
```
```javascript
// bad
const person = {
  first: "John",
  last: "Doe",
  greet() {
    const full = function() {
      return `${this.first} ${this.last}`;
    }.bind(this);
    return `Hello ${full()}`;
  }
}

// good
const person = {
  first: "John",
  last: "Doe",
  greet() {
    const full = () => `${this.first} ${this.last}`;
    return `Hello ${full()}`;
  }
}
```

### Higher-order functions

Avoid nesting functions when you don't have to.

```javascript
// bad
[1, 2, 3].map(num => String(num));

// good
[1, 2, 3].map(String);
```

### Composition

Avoid multiple nested function calls. Use composition instead.

```javascript
// bad
const plus1 = a => a + 1;
const mult2 = a => a * 2;

mult2(plus1(5)); // => 12


// good
const pipeline = (...funcs) =>
  val => funcs.reduce((a, b) => b(a), val);

const plus1 = a => a + 1;
const mult2 = a => a * 2;
const addThenMult = pipeline(plus1, mult2);

addThenMult(5); // => 12
```

### Caching

Cache feature tests, large data structures and any expensive operation.

```javascript
// bad
const contains = (arr, value) =>
  Array.prototype.includes
    ? arr.includes(value)
    : arr.some(el => el === value);
contains(["foo", "bar"], "baz"); // => false

// good
const contains = (() =>
  Array.prototype.includes
    ? (arr, value) => arr.includes(value)
    : (arr, value) => arr.some(el => el === value)
)();
contains(["foo", "bar"], "baz"); // => false
```

### Variables

Favor `const` over `let` and `let` over `var`.

```javascript
// bad
var obj = {};
obj["foo" + "bar"] = "baz";

// good
const obj = {
  ["foo" + "bar"]: "baz"
};
```

### Conditions

Favor IIFE's and return statements over if, else if, else and switch statements.

```javascript
// bad
var grade;
if (result < 50)
  grade = "bad";
else if (result < 90)
  grade = "good";
else
  grade = "excellent";

// good
const grade = (() => {
  if (result < 50)
    return "bad";
  if (result < 90)
    return "good";
  return "excellent";
})();
```

### Object iteration

Avoid `for...in` when you can.

```javascript
const shared = { foo: "foo" };
const obj = Object.create(shared, {
  bar: {
    value: "bar",
    enumerable: true
  }
});

// bad
for (var prop in obj) {
  if (obj.hasOwnProperty(prop))
    console.log(prop);
}

// good
Object.keys(obj).forEach(prop => console.log(prop));
```

### Objects as Maps

While objects have legitimate use cases, maps are usually a better, more powerful choice. When in
doubt, use a `Map`.

```javascript
// bad
const me = {
  name: "Ben",
  age: 30
};
var meSize = Object.keys(me).length;
meSize; // => 2
me.country = "Belgium";
meSize++;
meSize; // => 3

// good
const me = Map();
me.set("name", "Ben");
me.set("age", 30);
me.size; // => 2
me.set("country", "Belgium");
me.size; // => 3
```

### Curry

Currying might have its place in other languages, but avoid it in JavaScript. It makes your code harder to read by introducing a foreign paradigm while the appropriate use cases are extremely unusual.

```javascript
// bad
const sum = a => b => a + b;
sum(5)(3); // => 8

// good
const sum = (a, b) => a + b;
sum(5, 3); // => 8
```

### Readability

Don't obfuscate the intent of your code by using seemingly smart tricks.

```javascript
// bad
foo || doSomething();

// good
if (!foo) doSomething();
```
```javascript
// bad
void function() { /* IIFE */ }();

// good
(function() { /* IIFE */ }());
```
```javascript
// bad
const n = ~~3.14;

// good
const n = Math.floor(3.14);
```

### Code reuse

Don't be afraid of creating lots of small, highly composable and reusable functions.

```javascript
// bad
arr[arr.length - 1];

// good
const first = arr => arr[0];
const last = arr => first(arr.slice(-1));
last(arr);
```
```javascript
// bad
const product = (a, b) => a * b;
const triple = n => n * 3;

// good
const product = (a, b) => a * b;
const triple = product.bind(null, 3);
```

### Dependencies

Minimize dependencies. Third-party is code you don't know. Don't load an entire library for just a couple of methods easily replicable:

```javascript
// bad
var _ = require("underscore");
_.compact(["foo", 0]));
_.unique(["foo", "foo"]);
_.union(["foo"], ["bar"], ["foo"]);

// good
const compact = arr => arr.filter(el => el);
const unique = arr => [...Set(arr)];
const union = (...arr) => unique([].concat(...arr));

compact(["foo", 0]);
unique(["foo", "foo"]);
union(["foo"], ["bar"], ["foo"]);
```
