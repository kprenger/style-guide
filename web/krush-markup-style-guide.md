# Krush React, JSX, and Html Style Guide

Adapted from [Airbnb's React/JSX Style Guide](https://github.com/airbnb/javascript/tree/master/react) and http://codeguide.co/#html

## Table of Contents

  1. [Basic Rules](#basic-rules)
  1. [HTML](#html)
    1. [Be Semanitc](#be-semantic)
    1. [Tags](#tags)
      1. [Alignment](#alignment)
    1. [Naming](#naming)
    1. [Attribute Order](#attribute-order)
  1. [React](#react)
    1. [Class vs `React.createClass` vs stateless](#class-vs-reactcreateclass-vs-stateless)
    1. [Code Naming](#code-naming)
    1. [Declaration](#declaration)
    1. [Quotes](#quotes)
    1. [Props](#props)
    1. [Refs](#refs)
    1. [Parentheses](#parentheses)
    1. [Methods](#methods)
    1. [Ordering](#ordering)
    1. [Render Efficiently](#render-efficiently)
    1. [`isMounted`](#ismounted)

# Basic Rules

  - Use 4-space soft indentations
  - Only include one React component per file.
    - However, multiple [stateless functional components](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) are allowed per file.
    <!-- eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless). -->
  - Always use JSX syntax.

# HTML

## Be Semantic

Html 5 provides special tags that provide additional descriptive meaning, such as `header`, `aside`, and `main`.
Use these semantic tags whenever they fit to better define your markup.
[More Details](https://www.w3.org/wiki/HTML_structural_elements)

```jsx
//bad
<body>
    <div id="header">
        <!-- header content goes in here -->
    </div>
    <div id="sidebar">
        <!-- sidebar content goes in here -->
    </div>
    <div id="main">
        <!-- main page content goes in here -->
    </div>
</body>

//good
<body>
    <header>
        <!-- header content goes in here -->
    </header>
    <section class="sidebar">
        <!-- sidebar content goes in here -->
    </section>
    <main>
        <!-- main page content goes in here -->
    </main>
</body>
```

## Tags

  - Always self-close tags that have no children.
  <!-- eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md) -->

    ```jsx
    // bad
    <Foo className="stuff"></Foo>

    // good
    <Foo className="stuff" />
    ```

  - Always include a single space in single-line self-closing tags. Don't include the space for regular tags.
  <!-- eslint: [`no-multi-spaces`](http://eslint.org/docs/rules/no-multi-spaces), [`react/jsx-space-before-closing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-space-before-closing.md) -->

    ```jsx
    // bad
    <Foo/>

    // very bad
    <Foo                 />

    // bad
    <Foo
     />

    // good
    <Foo />

    // bad
    <Foo > ... </Foo>

    // good
    <Foo> ... </Foo>
    ```

### Alignment

  - Whenever practical, keep an entire tag on one line. If the properties are too long or many, follow these alignment styles:
    - start properties on the next line after the tag, indented once
    - place the end of a self-closing tag on its own line
    - End an opening tag (e.g., `>` of `<div>`) on the same line of the last property
    - Leave an empty line between properties and children

```jsx
// ideal (all props in line with tag)
<div id="prop" className="prop">
    <span id="prop" className="prop" />
    <span id="ha" />
</div>

// bad (> belongs on previous line)
<div
    id="prop"
    className="prop"
>
    <span
        id="prop"
        className="prop"
    />
    <span id="ha" />
</div>

// bad (missing space between props and children)
<div
    id="prop"
    className="prop">
    <span
        id="prop"
        className="prop" />

    <span id="ha" />
</div>

// bad (extra indentation for props)
<div
        id="prop"
        className="prop" >
    <span
        id="prop"
        className="prop" />
    <span id="ha" />
</div>

// bad (first prop belongs on new line)
<div id="prop"
     className="prop" >
    <span
        id="prop"
        className="prop" />
    <span id="ha" />
</div>

// good
<div
    id="prop"
    className="prop">

    <span
        id="prop"
        className="prop"
    />
    <span id="ha" />
</div>
```

## Naming

Use "dash-case" rather than camelCase or similar in `class`, `id`, `name`, and any other applicable attributes.
This includes any custom `data-*` attributes and their values, when appropriate. See [The CSS guide](./krush-css-style-guide.md) for exceptions.

```jsx
// bad
<Foo bar="Bar" data-bazBazBaz="baz" />

// good
<Foo bar="bar" data-baz-baz-baz="baz" />
```

## Attribute Order

HTML attributes should come in this particular order for easier reading of code.

  - `class`
  - `id`, `name`
  - `data-*`
  - `src`, `for`, `type`, `href`, `value`
  - `title`, `alt`
  - `role`, `aria-*`

# React

## Class vs `React.createClass` vs stateless

  - If you have internal state and/or refs, prefer `class extends React.Component` over `React.createClass` unless you have a very good reason to use mixins. Extend `React.PureComponent` when appropriate, or use a Stateless Functional Component for small, reusable components.
  <!-- eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md) [`react/prefer-stateless-function`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md) -->

    ```jsx
    // bad
    const Listing = React.createClass({
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    });

    // good
    class Listing extends React.Component {
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    }
    ```

    And if you don't have state or refs, prefer normal functions (not arrow functions) over classes:

    ```jsx
    // bad
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>;
      }
    }

    // bad (relying on function name inference is discouraged)
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );

    // good
    function Listing({ hello }) {
      return <div>{hello}</div>;
    }
    ```

## Code Naming

  - **Extensions**: Use `.tsx` extension for React components written in TypeScript.
  - **Filename**: Use PascalCase for filenames. E.g., `ReservationCard.tsx`.
  - **Reference Naming**: Use PascalCase for React components and camelCase for their instances.
  <!-- eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md) -->

    ```jsx
    // bad
    import reservationCard from './ReservationCard';

    // good
    import ReservationCard from './ReservationCard';

    // bad
    const ReservationItem = <ReservationCard />;

    // good
    const reservationItem = <ReservationCard />;
    ```

  - **Component Naming**: Use the filename as the component name. For example, `ReservationCard.jsx` should have a reference name of `ReservationCard`. However, for root components of a directory, use `index.jsx` as the filename and use the directory name as the component name:

    ```jsx
    // bad
    import Footer from './Footer/Footer';

    // bad
    import Footer from './Footer/index';

    // good
    import Footer from './Footer';
    ```

  - **Higher-order Component Naming**: Use a composite of the higher-order component's name and the passed-in component's name as the `displayName` on the generated component. For example, the higher-order component `withFoo()`, when passed a component `Bar` should produce a component with a `displayName` of `withFoo(Bar)`.

  > Why? A component's `displayName` may be used by developer tools or in error messages, and having a value that clearly expresses this relationship helps people understand what is happening.

    ```jsx
    // bad
    export default function withFoo(WrappedComponent) {
      return function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }
    }

    // good
    export default function withFoo(WrappedComponent) {
      function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }

      const wrappedComponentName = WrappedComponent.displayName
        || WrappedComponent.name
        || 'Component';

      WithFoo.displayName = `withFoo(${wrappedComponentName})`;
      return WithFoo;
    }
    ```

  - **Props Naming**: Avoid using DOM component prop names for different purposes.

  > Why? People expect props like `style` and `className` to mean one specific thing. Varying this API for a subset of your app makes the code less readable and less maintainable, and may cause bugs.

    ```jsx
    // bad
    <MyComponent style="fancy" />

    // good
    <MyComponent variant="fancy" />
    ```

## Declaration

  - Do not use `displayName` for naming components. Instead, name the component by reference.

    ```jsx
    // bad
    export default React.createClass({
      displayName: 'ReservationCard',
      // stuff goes here
    });

    // good
    export default class ReservationCard extends React.Component {
    }
    ```
    
## Quotes

  - Always use double quotes (`"`) for JSX attributes, but single quotes (`'`) for all other JS.
  <!-- eslint: [`jsx-quotes`](http://eslint.org/docs/rules/jsx-quotes) -->

  > Why? Regular HTML attributes also typically use double quotes instead of single, so JSX attributes mirror this convention.

    ```jsx
    // bad
    <Foo bar='bar' />

    // good
    <Foo bar="bar" />

    // bad
    <Foo style={{ left: "20px" }} />

    // good
    <Foo style={{ left: '20px' }} />
    ```

  - Do not pad JSX curly braces with spaces (but do still pad JS object braces).
  <!-- eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md) -->

    ```jsx
    // bad
    <Foo bar={ baz } />

    // good
    <Foo bar={baz} />

    // good
    <Foo bar={{ baz: biz }} />
    ```

## Props

  - Always use camelCase for prop names.

    ```jsx
    // bad
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // good
    <Foo
      userName="hello"
      phoneNumber={12345678}
    />
    ```

  - Omit the value of the prop when it is explicitly `true`.
  <!-- eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md) -->

    ```jsx
    // bad
    <Foo
      hidden={true}
    />

    // good
    <Foo
      hidden
    />
    ```

  - Always include an `alt` prop on `<img>` tags. If the image is presentational, the `<img>` must have `role="presentation"`.
  <!-- eslint: [`jsx-a11y/img-has-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-has-alt.md) -->

    ```jsx
    // bad
    <img src="hello.jpg" />

    // good
    <img src="hello.jpg" alt="Me waving hello" />

    // bad
    <img src="hello.jpg" alt="" />

    // good
    <img src="hello.jpg" role="presentation" />
    ```

  - Do not use words like "image", "photo", or "picture" in `<img>` `alt` props.
  <!-- eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md) -->

  > Why? Screenreaders already announce `img` elements as images, so there is no need to include this information in the alt text.

    ```jsx
    // bad
    <img src="hello.jpg" alt="Picture of me waving hello" />

    // good
    <img src="hello.jpg" alt="Me waving hello" />
    ```

  - Use only valid, non-abstract [ARIA roles](https://www.w3.org/TR/wai-aria/roles#role_definitions).
  <!-- eslint: [`jsx-a11y/aria-role`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md) -->

    ```jsx
    // bad - not an ARIA role
    <div role="datepicker" />

    // bad - abstract ARIA role
    <div role="range" />

    // good
    <div role="button" />
    ```

  - Do not use `accessKey` on elements.
  <!-- eslint: [`jsx-a11y/no-access-key`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md) -->

  > Why? Inconsistencies between keyboard shortcuts and keyboard commands used by people using screenreaders and keyboards complicate accessibility.

  ```jsx
  // bad
  <div accessKey="h" />

  // good
  <div />
  ```

  - Avoid using an array index as `key` prop, prefer a unique ID. ([why?](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318))

  ```jsx
  // bad
  {todos.map((todo, index) =>
    <Todo
      {...todo}
      key={index}
    />
  )}

  // good
  {todos.map(todo => (
    <Todo
      {...todo}
      key={todo.id}
    />
  ))}
  ```

  - Avoid React propTypes and defaultProps in favor of TypeScript Interfaces and default values

## Refs

  - Always use ref callbacks instead of strings. Use ref sparingly.
  <!-- eslint: [`react/no-string-refs`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-string-refs.md) -->

    ```jsx
    // bad
    <Foo
      ref="myRef"
    />

    // good
    <Foo
      ref={(ref) => { this.myRef = ref; }}
    />
    ```

## Parentheses

  - Wrap JSX tags in parentheses when they span more than one line.
  <!-- eslint: [`react/wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/wrap-multilines.md) -->

    ```jsx
    // bad
    render() {
      return <MyComponent className="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // good
    render() {
      return (
        <MyComponent className="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // good, when single line
    render() {
      const body = <div>hello</div>;
      return <MyComponent>{body}</MyComponent>;
    }
    ```

## Methods

  - Use arrow functions to close over local variables.

    ```jsx
    function ItemList(props) {
      return (
        <ul>
          {props.items.map((item, index) => (
            <Item
              key={item.key}
              onClick={() => doSomethingWith(item.name, index)}
            />
          ))}
        </ul>
      );
    }
    ```

  - Bind event handlers for the render method in the constructor.
  <!-- eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md) -->

  > Why? A bind call in the render path creates a brand new function on every single render.

    ```jsx
    // bad
    class extends React.Component {
      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />
      }
    }

    // good
    class extends React.Component {
      constructor(props) {
        super(props);

        this.onClickDiv = this.onClickDiv.bind(this);
      }

      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv} />
      }
    }
    ```

## Ordering

Most components will have only a few members. For clarity, order those members according to this list:

  1. `constructor`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers or eventHandlers* like `handleClickSubmit()` or `handleChangeDescription()`
  1. *getter methods for `render`* like `getSelectReason()` or `getFooterContent()`
  1. *optional render methods* like `renderNavigation()` or `renderProfilePicture()`
  1. `render`
  1. `static` methods

## Render Efficiently

Nearly all components should be "pure" and extend `React.PureComponent`. Be careful about mutable state in such cases, as this will shallow-compare state and props. With impure components, use shouldComponentUpdate to optimize React's render process if practical. Use "[stateless functional components][stateless]" for small, simple components that have no state.

[stateless]:https://facebook.github.io/react/docs/components-and-props.html#functional-and-class-components

## `isMounted`

  - Do not use `isMounted`.
  <!-- eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md) -->

  > Why? [`isMounted` is an anti-pattern][anti-pattern], is not available when using ES6 classes, and is on its way to being officially deprecated.

  [anti-pattern]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

**[⬆ back to top](#table-of-contents)**
