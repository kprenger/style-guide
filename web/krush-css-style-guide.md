# Krush CSS/SCSS Style Guide

## Table of Contents

  1. [Terminology](#terminology)
    - [Rule Declaration](#rule-declaration)
    - [Selectors](#selectors)
    - [Properties](#properties)
  2. [CSS](#css)
    - [Formatting](#formatting)
    - [Comments](#comments)
    - [OOCSS and BEM](#oocss-and-bem)
    - [ID Selectors](#id-selectors)
    - [Border](#border)
  3. [Sass](#sass)
    - [Syntax](#syntax)
    - [Ordering](#ordering-of-property-declarations)
    - [Variables](#variables)
    - [Mixins](#mixins)
    - [Extend directive](#extend-directive)
    - [Nested selectors](#nested-selectors)
  4. [Kudos](#kudos)

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

[aria-hidden] {
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

## CSS

### Formatting

* Use soft tabs (4 spaces) for indentation.
* Prefer dashes over camelCasing in class names.
  * Underscores and PascalCasing are okay if you are using BEM (see [OOCSS and BEM](#oocss-and-bem) below).
  * Use a single underscore for BEM elements. E.g. .block_element instead of .block__element.
* Do not use ID selectors for CSS styling.
* Avoid using HTML tags in CSS selectors.
  * Exceptions can be made for `html` tags that are more semantic like `article` or `input` but use your best judgment. 
* Don't nest more than 3 levels deep (Specificity issues).
* Avoid nesting unless it's for pseudo selectors (i.e. :hover, :focus, etc), pseudo elements (i.e. ::before, ::after), and state selectors (.button--active). 
* When using multiple selectors in a rule declaration, give each selector its own line.
* Put a space before the opening brace `{` in rule declarations.
* In properties, put a space after, but not before, the `:` character and end every property with a `;`.
* Put closing braces `}` of rule declarations on a new line.
* Put blank lines between rule declarations.

**Bad**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Good**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Comments

* Prefer line comments (`//` in Sass-land) to block comments.
* Prefer comments on their own line. Avoid end-of-line comments.
* Write detailed comments for code that isn't self-documenting:
  - Uses of z-index
  - Compatibility or browser-specific hacks

### OOCSS and BEM

We encourage some combination of OOCSS and BEM for these reasons:

  * It helps create clear, strict relationships between CSS and HTML
  * It helps us create reusable, composable components
  * It allows for less nesting and lower specificity
  * It helps in building scalable stylesheets

**OOCSS**, or “Object Oriented CSS”, is an approach for writing CSS that encourages you to think about your stylesheets as a collection of “objects”: reusable, repeatable snippets that can be used independently throughout a website.

  * Nicole Sullivan's [OOCSS wiki](https://github.com/stubbornella/oocss/wiki)
  * Smashing Magazine's [Introduction to OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)

**BEM**, or “Block-Element-Modifier”, is a _naming convention_ for classes in HTML and CSS. It was originally developed by Yandex with large codebases and scalability in mind, and can serve as a solid set of guidelines for implementing OOCSS.

  * CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
  * Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

We recommend a variant of BEM with PascalCased “blocks”, which works particularly well when combined with components (e.g. React). Underscores and dashes are still used for modifiers and children.

**Example**

```jsx
// ListingCard.jsx
function ListingCard() {
  return (
    <article class="ListingCard ListingCard--featured">

      <h1 class="ListingCard_title">Adorable 2BR in the sunny Mission</h1>

      <div class="ListingCard_content">
        <p>Vestibulum id ligula porta felis euismod semper.</p>
      </div>

    </article>
  );
}
```

```css
/* ListingCard.css */
.ListingCard { }
.ListingCard--featured { }
.ListingCard_title { }
.ListingCard_content { }
```

  * `.ListingCard` is the “block” and represents the higher-level component
  * `.ListingCard_title` is an “element” and represents a descendant of `.ListingCard` that helps compose the block as a whole.
  * `.ListingCard--featured` is a “modifier” and represents a different state or variation on the `.ListingCard` block.

### ID selectors

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.

### Viewport Units vs Percentage.

Don't use viewport units unless you have to. If you have to, using `vh` can be more helpful than `%` when defining a height but `%` is generally better than `vw` for widths. [Source](https://bitsofco.de/viewport-vs-percentage-units/)

### Border

Use `0` instead of `none` to specify that a style has no border. Using `0` will set `border-width: 0;` while `none` will set `border-style: none;`. `0` is [preferred](http://stackoverflow.com/questions/2922909/should-i-use-border-none-or-border-0) because it's shorter.

**Bad**

```css
.foo {
  border: none;
}
```

**Good**

```css
.foo {
  border: 0;
}
```

## Sass

### Syntax

* Use the `.scss` syntax, not the original `.sass` syntax
* Order your regular CSS and `@include` declarations logically (see below)

### Ordering of property declarations

1. @ rules that aren't @include (see #6)
2. Layout / box-model properties - e.g. margin, padding, box-sizing, position, overflow, width, etc.
3. Typographic properties - e.g. line-height, letter-spacing, text-size, etc.
4. Cosmetic properties - e.g. color, background-*, animation, border, etc.
5. UI properties - e.g. appearance, cursor, user-select, pointer-events, etc.
6. @include declarations
7. Pseudo-elements - e.g. ::after, ::before, ::selection, etc.
8. Pseudo-selectors - e.g. :hover, :focus, :active, etc.
9. Modifier classes (BEM) - e.g. .btn--big
10. Nested elements

An example:

```scss
.element {
  //strongly consider whether it is helpful to use @extend
  @extend %some--extend
  
  display: inline-block;
  width: 100px; 
  float: left;
  
  text-align: left; 
  text-transform: uppercase;
  letter-spacing: 1px;
  
  background-color: green;
  border-radius: 2px;
  color: rgba(255,255,255,.5);
  
  cursor: auto;
  
  @include transition(background .5s ease);
  
  &::before {
    content: '';
  }
  
  &:hover {
    cursor: pointer;
    background-color: red;
  } 
  
  &--active {
    border: 1px solid red;
  }
  
  .child {
    margin-left: 20px;
    text-align: center; 
  }
}
```

### Variables

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).

### Dimensions & Sizing

Consider using variables for common sizes. 

**Bad**
```scss
.bad-element {
  margin-bottom: 104px; 
  padding-left: 57px;
} 

.bad-icon {
  width: 12px; 
  height: 7em; 
}
```

**Good**
```scss
.good-element {
  margin-bottom: $unit-5; 
  padding-left: $unit-3;
} 

.good-icon {
  width: $icon-unit; 
  height: $icon-unit; 
}
```

### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. Mixins that accept no arguments can be useful for this, but note that if you are not compressing your payload (e.g. gzip), this may contribute to unnecessary code duplication in the resulting styles.

### Extend directive

`@extend` should be avoided because it has unintuitive and potentially dangerous behavior, especially when used with nested selectors. Even extending top-level placeholder selectors can cause problems if the order of selectors ends up changing later (e.g. if they are in other files and the order the files are loaded shifts). Gzipping should handle most of the savings you would have gained by using `@extend`, and you can DRY up your stylesheets nicely with mixins.

### Nested selectors

**Do not nest selectors more than three levels deep!**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

When selectors become this long, you're likely writing CSS that is:

* Strongly coupled to the HTML (fragile) *—OR—*
* Overly specific (powerful) *—OR—*
* Not reusable


Again: **don't nest ID selectors!**

If you must use an ID selector in the first place (and you should really try not to), they shouldn't be nested. If you find yourself doing this, you need to revisit your markup, or figure out why such strong specificity is needed. If you are writing well formed HTML and CSS, you shouldn't need to do this.

## Kudos

* Originally forked from [Airbnb's reasonable approach to CSS & Sass](https://github.com/airbnb/css)
* Other ideas sampled from [Dropbox's SCSS Style Guide](https://github.com/dropbox/css-style-guide) and the [Sky CSS Style Guide](https://github.com/sky-uk/css).