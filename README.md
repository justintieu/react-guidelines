# React/JSX Style Guide

*A somewhat reasonable but opinionated approach to React and JSX*

## Table of Contents

  1. [Basic Rules](#basic-rules)
  1. [Class vs `React.createClass` vs stateless](#class-vs-reactcreateclass-vs-stateless)
  1. [Naming](#naming)
  1. [Declaration](#declaration)
  1. [Alignment](#alignment)# React/JSX Style Guide

*A somewhat reasonable but opinionated approach to React and JSX*

## Table of Contents

  1. [Basic Rules](#basic-rules)
  1. [Class vs `React.createClass` vs stateless](#class-vs-reactcreateclass-vs-stateless)
  1. [Naming](#naming)
  1. [Declaration](#declaration)
  1. [Alignment](#alignment)
  1. [Quotes](#quotes)
  1. [Spacing](#spacing)
  1. [Imports](#imports)
  1. [Props](#props)
  1. [Refs](#refs)
  1. [Parentheses](#parentheses)
  1. [Tags](#tags)
  1. [Methods](#methods)
  1. [Ordering](#ordering)

## Basic Rules

  - Always use JSX syntax.
  - Do not use `React.createElement` unless you’re initializing the app from a file that is not JSX.

## Class vs `React.createClass` vs stateless

  - If you have internal state and/or refs, prefer `class extends React.Component` over `React.createClass`.

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

    And if you don’t have state or refs, prefer arrow functions over classes:

    ```jsx
    // bad
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>;
      }
    }

    // good
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );
    ```
## Naming

  - **Extensions**: Use `.jsx` extension for React components.
  - **Filename**: Use PascalCase for filenames. E.g., `GuidelineCard.js`.
  - **Reference Naming**: Use PascalCase for React components and camelCase for their instances. 

    ```jsx
    // bad
    import guidelineCard from '/GuidelineCard';

    // good
    import GuidelineCard from './GuidelineCard';

    // bad
    const GuidelineItem = <GuidelineCard />;

    // good
    const guidelineItem = <GuidelineCard />;
    ```

  - **Component Naming**: Use the filename as the component name. For example, `GuidelineCard.js` should have a reference name of `GuidelineCard`. However, for root components of a directory, use `index.jsx` as the filename and use the directory name as the component name:

    ```jsx
    // bad
    import Header from 'common/Header/Header';

    // bad
    import Header from 'common/Header/index';

    // good
    import Header from 'common/Header';
    ```
  - **Higher-order Component Naming**: Use a composite of the higher-order component’s name and the passed-in component’s name as the `displayName` on the generated component. For example, the higher-order component `withFoo()`, when passed a component `Bar` should produce a component with a `displayName` of `withFoo(Bar)`.

    > Why? A component’s `displayName` may be used by developer tools or in error messages, and having a value that clearly expresses this relationship helps people understand what is happening.

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

    // bad
    <MyComponent className="fancy" />

    // good
    <MyComponent variant="fancy" />
    ```

  - **Redux Naming**: Avoid using the same name for actions and reducers.

    > Why? It makes it confusing when a person comes to find actions/Foo.js and reducers/Foo.js. You should use camel case to differenate a component versus a redux file. You should also specify what type of redux file it is to make it easier to find.

    ```
    // bad
    action is called Foo.js

    // bad 
    reducer is called Foo.js

    // good
    actions called fooActions.js

    // good
    reducer is called fooReducers.js

## Declaration
  - **Variables**: A variable should be declared as const or let on it's own line.

    > Why? It makes it hard to debug when you can

  - **Methods**: Methods should be using ES6 arrow functions.

    > Why? It makes it confusing when a person comes to find actions/Foo.js and reducers/Foo.js. You should use camel case to differenate a component versus a redux file. You should also specify what type of redux file it is to make it easier to find.

    // bad

## Alignment

  - Follow these alignment styles for JSX syntax.

    ```jsx
    // bad
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // good
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />

    // if props fit in one line then keep it on the same line
    <Foo bar="bar" />

    // children get indented normally
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Quux />
    </Foo>

    // bad
    {showButton &&
      <Button />
    }

    // bad
    {
      showButton &&
        <Button />
    }

    // good
    {showButton && (
      <Button />
    )}

    // good
    {showButton && <Button />}
    ```
## Quotes

    - Always use double quotes (`"`). If you are using it for JSX attributes, don't use curly braces.

    ```jsx
    // bad
    <Foo bar='bar' />

    // bad
    <Foo bar={'bar'} />

    // bad
    <Foo bar={"bar"} />

    // good
    <Foo bar="bar" />
    ```

    ```jsx
    // bad
    const foo = 'bar';

    // good
    const foo = "bar";
    ```
## Spacing

  - Always include a single space in your self-closing tag. 

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
    ```

  - Do not pad JSX curly braces with spaces. 

    ```jsx
    // bad
    <Foo bar={ baz } />

    // good
    <Foo bar={baz} />
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

    ```jsx
    // bad
    <Foo
      hidden={true}
    />

    // good
    <Foo
      hidden
    />

    // good
    <Foo hidden />
    ```

  - Always include an `alt` prop on `<img>` tags. If the image is presentational, `alt` can be an empty string or the `<img>` must have `role="presentation"`. 

    ```jsx
    // bad
    <img src="hello.jpg" />

    // good
    <img src="hello.jpg" alt="Me waving hello" />

    // good
    <img src="hello.jpg" alt="" />

    // good
    <img src="hello.jpg" role="presentation" />
    ```

  - Do not use words like "image", "photo", or "picture" in `<img>` `alt` props. 

    > Why? Screenreaders already announce `img` elements as images, so there is no need to include this information in the alt text.

    ```jsx
    // bad
    <img src="hello.jpg" alt="Picture of me waving hello" />

    // good
    <img src="hello.jpg" alt="Me waving hello" />
    ```

  - Use only valid, non-abstract [ARIA roles](https://www.w3.org/TR/wai-aria/#usage_intro). 

    ```jsx
    // bad - not an ARIA role
    <div role="datepicker" />

    // bad - abstract ARIA role
    <div role="range" />

    // good
    <div role="button" />
    ```

  - Do not use `accessKey` on elements. 

  > Why? Inconsistencies between keyboard shortcuts and keyboard commands used by people using screenreaders and keyboards complicate accessibility.

  ```jsx
  // bad
  <div accessKey="h" />

  // good
  <div />
  ```

  - Avoid using an array index as `key` prop, prefer a stable ID when you can. If you cannot, provide context to what the item is.

> Why? Not using a stable ID [is an anti-pattern](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318) because it can negatively impact performance and cause issues with component state.

We don’t recommend using indexes for keys if the order of items may change.

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

  // good
  {columns.map((column, index) => (
    <Column
      {...column}
      key={`${column.name}-${index}`}
    />
  ))}
  ```

  - Always define explicit defaultProps for all non-required props.

  - Use spread props sparingly.
  > Why? Otherwise you’re more likely to pass unnecessary props down to components. And for React v15.6.1 and older, you could [pass invalid HTML attributes to the DOM](https://reactjs.org/blog/2017/09/08/dom-attributes-in-react-16.html).

  - Spreading objects with known, explicit props. This can be particularly useful when testing React components with Mocha’s beforeEach construct.

  ```jsx
  const Foo = props => {
    const props = {
      text: '',
      isPublished: false
    }

    return (<div {...props} />);
  }
  export default Foo;
  ```

  Notes for use:
  Filter out unnecessary props when possible. Also, use [prop-types-exact](https://www.npmjs.com/package/prop-types-exact) to help prevent bugs.

  ```jsx
  // bad
  render() {
    const { irrelevantProp, ...relevantProps } = this.props;
    return <WrappedComponent {...this.props} />
  }

  // good
  render() {
    const { irrelevantProp, ...relevantProps } = this.props;
    return <WrappedComponent {...relevantProps} />
  }
  ```

  - Avoid using arrow functions when passing a function as a property

  ```jsx
  // bad
  <Foo
    bar={ev => { this.doStuff(ev) }}
  />

  // good
  doStuff = (ev) => {
    // does stuff
  };
  <Foo
    bar={this.doStuff}
  />
  ```
## Refs

  - Always use ref callbacks

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

    ```jsx
    // bad
    render() {
      return <MyComponent variant="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // good
    render() {
      return (
        <MyComponent variant="long body" foo="bar">
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
## Tags

  - Always self-close tags that have no children.

    ```jsx
    // bad
    <Foo variant="stuff"></Foo>

    // good
    <Foo variant="stuff" />
    ```

  - If your component has multi-line properties, close its tag on a new line. 

    ```jsx
    // bad
    <Foo
      bar="bar"
      baz="baz" />

    // good
    <Foo
      bar="bar"
      baz="baz"
    />
    ```
## Methods

  - Use arrow functions to close over local variables. It is handy when you need to pass additional data to an event handler. Although, make sure they [do not massively hurt performance](https://www.bignerdranch.com/blog/choosing-the-best-approach-for-react-event-handlers/), in particular when passed to custom components that might be PureComponents, because they will trigger a possibly needless rerender every time.

    ```jsx
    const ItemList = (props) => {
      return (
        <ul>
          {props.items.map((item, index) => (
            <Item
              key={item.key}
              onClick={(event) => doSomethingWith(event, item.name, index)}
            />
          ))}
        </ul>
      );
    }
    ```

  - Bind event handlers for the render method in the constructor. 

    > Why? A bind call in the render path creates a brand new function on every single render. Do not use arrow functions in class fields, because it makes them [challenging to test and debug, and can negatively impact performance](https://medium.com/@charpeni/arrow-functions-in-class-properties-might-not-be-as-great-as-we-think-3b3551c440b1), and because conceptually, class fields are for data, not logic.

    ```jsx
    // bad
    class extends React.Component {
      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />;
      }
    }

    // very bad
    class extends React.Component {
      onClickDiv = () => {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv} />
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
        return <div onClick={this.onClickDiv} />;
      }
    }
    ```

    If you need to use `bind` to render a list and have a separate callback for item, you should pull 
    the list items into it's own component.
    ```jsx
    // bad
    const List extends React.Component {
      render() {
      const { items, onItemClick } = this.props;
        return (
          <ul>
            {items.map((item, index) =>
              <ListItem 
                item={item} 
                key={item.id} 
                onItemClick={onItemClick} 
              />
            )}
          </ul>
        );
      }
    }

    // good
    const List extends React.Component {
      render() {
        const { items, onItemClick } = this.props;
        return (
          <ul>
            {items.map(item =>
              <ListItem key={item.id} item={item} onItemClick={onItemClick} />
            )}
          </ul>
        );
      }
    }

    const ListItem extends React.Component {
      onClickHandler = () {
        const { item: { id }, onItemClick } = this.props;
        onItemClick(id);
      }

      render() {
        return (
          <li onClick={this.onClickHandler}>
            ...
          </li>
        );
      }
    }
    ```

  - Do not use underscore prefix for internal methods of a React component.
    > Why? Underscore prefixes are sometimes used as a convention in other languages to denote privacy. But, unlike those languages, there is no native support for privacy in JavaScript, everything is public. Regardless of your intentions, adding underscore prefixes to your properties does not actually make them private, and any property (underscore-prefixed or not) should be treated as being public. See issues [#1024](https://github.com/airbnb/javascript/issues/1024), and [#490](https://github.com/airbnb/javascript/issues/490) for a more in-depth discussion.

    ```jsx
    // bad
    React.createClass({
      _onClickSubmit() {
        // do stuff
      },

      // other stuff
    });

    // good
    class extends React.Component {
      onClickSubmit() {
        // do stuff
      }

      // other stuff
    }
    ```

  - Be sure to return a value in your `render` methods. 

    ```jsx
    // bad
    render() {
      (<div />);
    }

    // good
    render() {
      return (<div />);
    }
    ```
## Ordering
  - Ordering for imports
  1. import React
  1. import third party libraries (a-z)
  1. import common modules (a-z)
  1. import other modules internal (a-z)

  - Ordering for `class extends React.Component`:

  1. optional `static` methods
  1. `constructor`
  1. `componentDidMount`
  1. `getDerivedState`
  1. `shouldComponentUpdate`
  1. `getSnapshotBeforeUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers or eventHandlers* like `onClickSubmit()` or `onChangeDescription()`
  1. *getter methods for `render`* like `getSelectReason()` or `getHeaderContent()`
  1. *optional render methods* like `renderNavigation()` or `renderProfilePicture()`
  1. `render`

  - How to define `defaultProps`, etc...

    ```jsx
    import React from 'react';

    const defaultProps = {
      text: 'Hello World',
    };

    class Link extends React.Component {
      static methodsAreOk() {
        return true;
      }

      render() {
        const { id, text, url } = this.props;
        return <a href={url} data-id={id}>{text}</a>;
      }
    }

    Link.defaultProps = defaultProps;

    export default Link;
    ```

  - Ordering for props: props passed into a component should always be sorted in alphabetical order.
  The same should be done with the props when you deconstruct them.

  ```jsx
  // bad
  <Foo
    corge={"fuzz"}
    xyzzy={"fuzz"}
    fred={"fuzz"}
    wibble={"fuzz"}
    thud={"fuzz"}
    wubble={"fuzz"}
    plugh={"fuzz"}
    waldo={"fuzz"}
    grault={"fuzz"}
    baz={"fuzz"}
    garply={"fuzz"}
    wobble={"fuzz"}
    flob={"fuzz"}
    qux={"fuzz"}
  />

  // good
  <Foo
    baz={"fuzz"}
    corge={"fuzz"}
    flob={"fuzz"}
    fred={"fuzz"}
    garply={"fuzz"}
    grault={"fuzz"}
    plugh={"fuzz"}
    qux={"fuzz"}
    thud={"fuzz"}
    waldo={"fuzz"}
    wibble={"fuzz"}
    wobble={"fuzz"}
    wubble={"fuzz"}
    xyzzy={"fuzz"}
  />
  ```

  ```jsx
  // bad
  const {
    garply,
    wubble,
    baz,
    xyzzy,
    thud,
    fred,
    qux,
    wobble,
    grault,
    wibble,
    plugh,
    flob,
    waldo,
    corge
  } = this.props;

  // good
  {
    baz,
    corge,
    flob,
    fred,
    garply,
    grault,
    plugh,
    qux,
    thud,
    waldo,
    wibble,
    wobble,
    wubble,
    xyzzy
  } = this.props;
  ```

**[⬆ back to top](#table-of-contents)**

  1. [Quotes](#quotes)
  1. [Spacing](#spacing)
  1. [Imports](#imports)
  1. [Props](#props)
  1. [Refs](#refs)
  1. [Parentheses](#parentheses)
  1. [Tags](#tags)
  1. [Methods](#methods)
  1. [Ordering](#ordering)

## Basic Rules

  - Always use JSX syntax.
  - Do not use `React.createElement` unless you’re initializing the app from a file that is not JSX.

## Class vs `React.createClass` vs stateless

  - If you have internal state and/or refs, prefer `class extends React.Component` over `React.createClass`.

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

    And if you don’t have state or refs, prefer arrow functions over classes:

    ```jsx
    // bad
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>;
      }
    }

    // good
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );
    ```
## Naming

  - **Extensions**: Use `.jsx` extension for React components.
  - **Filename**: Use PascalCase for filenames. E.g., `GuidelineCard.js`.
  - **Reference Naming**: Use PascalCase for React components and camelCase for their instances. 

    ```jsx
    // bad
    import guidelineCard from '/GuidelineCard';

    // good
    import GuidelineCard from './GuidelineCard';

    // bad
    const guidelineItem = <GuidelineCard />;

    // good
    const guidelineItem = <GuidelineCard />;
    ```

  - **Component Naming**: Use the filename as the component name. For example, `GuidelineCard.js` should have a reference name of `GuidelineCard`. However, for root components of a directory, use `index.jsx` as the filename and use the directory name as the component name:

    ```jsx
    // bad
    import Header from 'common/Header/Header';

    // bad
    import Header from 'common/Header/index';

    // good
    import Header from 'common/Header';
    ```
  - **Higher-order Component Naming**: Use a composite of the higher-order component’s name and the passed-in component’s name as the `displayName` on the generated component. For example, the higher-order component `withFoo()`, when passed a component `Bar` should produce a component with a `displayName` of `withFoo(Bar)`.

    > Why? A component’s `displayName` may be used by developer tools or in error messages, and having a value that clearly expresses this relationship helps people understand what is happening.

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

    // bad
    <MyComponent className="fancy" />

    // good
    <MyComponent variant="fancy" />
    ```

  - **Redux Naming**: Avoid using the same name for actions and reducers.

    > Why? It makes it confusing when a person comes to find actions/Foo.js and reducers/Foo.js. You should use camel case to differenate a component versus a redux file. You should also specify what type of redux file it is to make it easier to find.

    ```
    // bad
    action is called Foo.js

    // bad 
    reducer is called Foo.js

    // good
    actions called fooActions.js

    // good
    reducer is called fooReducers.js

## Declaration
  - **Variables**: A variable should be declared as const or let on it's own line.

    > Why? It makes it hard to debug when you can

  - **Methods**: Methods should be using ES6 arrow functions.

    > Why? It makes it confusing when a person comes to find actions/Foo.js and reducers/Foo.js. You should use camel case to differenate a component versus a redux file. You should also specify what type of redux file it is to make it easier to find.

    // bad

## Alignment

  - Follow these alignment styles for JSX syntax.

    ```jsx
    // bad
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // good
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />

    // if props fit in one line then keep it on the same line
    <Foo bar="bar" />

    // children get indented normally
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Quux />
    </Foo>

    // bad
    {showButton &&
      <Button />
    }

    // bad
    {
      showButton &&
        <Button />
    }

    // good
    {showButton && (
      <Button />
    )}

    // good
    {showButton && <Button />}
    ```
## Quotes

    - Always use double quotes (`"`). If you are using it for JSX attributes, don't use curly braces.

    ```jsx
    // bad
    <Foo bar='bar' />

    // bad
    <Foo bar={'bar'} />

    // bad
    <Foo bar={"bar"} />

    // good
    <Foo bar="bar" />
    ```

    ```jsx
    // bad
    const foo = 'bar';

    // good
    const foo = "bar";
    ```
## Spacing

  - Always include a single space in your self-closing tag. 

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
    ```

  - Do not pad JSX curly braces with spaces. 

    ```jsx
    // bad
    <Foo bar={ baz } />

    // good
    <Foo bar={baz} />
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

    ```jsx
    // bad
    <Foo
      hidden={true}
    />

    // good
    <Foo
      hidden
    />

    // good
    <Foo hidden />
    ```

  - Always include an `alt` prop on `<img>` tags. If the image is presentational, `alt` can be an empty string or the `<img>` must have `role="presentation"`. 

    ```jsx
    // bad
    <img src="hello.jpg" />

    // good
    <img src="hello.jpg" alt="Me waving hello" />

    // good
    <img src="hello.jpg" alt="" />

    // good
    <img src="hello.jpg" role="presentation" />
    ```

  - Do not use words like "image", "photo", or "picture" in `<img>` `alt` props. 

    > Why? Screenreaders already announce `img` elements as images, so there is no need to include this information in the alt text.

    ```jsx
    // bad
    <img src="hello.jpg" alt="Picture of me waving hello" />

    // good
    <img src="hello.jpg" alt="Me waving hello" />
    ```

  - Use only valid, non-abstract [ARIA roles](https://www.w3.org/TR/wai-aria/#usage_intro). 

    ```jsx
    // bad - not an ARIA role
    <div role="datepicker" />

    // bad - abstract ARIA role
    <div role="range" />

    // good
    <div role="button" />
    ```

  - Do not use `accessKey` on elements. 

  > Why? Inconsistencies between keyboard shortcuts and keyboard commands used by people using screenreaders and keyboards complicate accessibility.

  ```jsx
  // bad
  <div accessKey="h" />

  // good
  <div />
  ```

  - Avoid using an array index as `key` prop, prefer a stable ID when you can. If you cannot, provide context to what the item is.

> Why? Not using a stable ID [is an anti-pattern](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318) because it can negatively impact performance and cause issues with component state.

We don’t recommend using indexes for keys if the order of items may change.

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

  // good
  {columns.map((column, index) => (
    <Column
      {...column}
      key={`${column.name}-${index}`}
    />
  ))}
  ```

  - Always define explicit defaultProps for all non-required props.

  - Use spread props sparingly.
  > Why? Otherwise you’re more likely to pass unnecessary props down to components. And for React v15.6.1 and older, you could [pass invalid HTML attributes to the DOM](https://reactjs.org/blog/2017/09/08/dom-attributes-in-react-16.html).

  - Spreading objects with known, explicit props. This can be particularly useful when testing React components with Mocha’s beforeEach construct.

  ```jsx
  const Foo = props => {
    const props = {
      text: '',
      isPublished: false
    }

    return (<div {...props} />);
  }
  export default Foo;
  ```

  Notes for use:
  Filter out unnecessary props when possible. Also, use [prop-types-exact](https://www.npmjs.com/package/prop-types-exact) to help prevent bugs.

  ```jsx
  // bad
  render() {
    const { irrelevantProp, ...relevantProps } = this.props;
    return <WrappedComponent {...this.props} />
  }

  // good
  render() {
    const { irrelevantProp, ...relevantProps } = this.props;
    return <WrappedComponent {...relevantProps} />
  }
  ```

  - Avoid using arrow functions when passing a function as a property

  ```jsx
  // bad
  <Foo
    bar={ev => { this.doStuff(ev) }}
  />

  // good
  doStuff = (ev) => {
    // does stuff
  };
  <Foo
    bar={this.doStuff}
  />
  ```
## Refs

  - Always use ref callbacks

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

    ```jsx
    // bad
    render() {
      return <MyComponent variant="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // good
    render() {
      return (
        <MyComponent variant="long body" foo="bar">
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
## Tags

  - Always self-close tags that have no children.

    ```jsx
    // bad
    <Foo variant="stuff"></Foo>

    // good
    <Foo variant="stuff" />
    ```

  - If your component has multi-line properties, close its tag on a new line. 

    ```jsx
    // bad
    <Foo
      bar="bar"
      baz="baz" />

    // good
    <Foo
      bar="bar"
      baz="baz"
    />
    ```
## Methods

  - Use arrow functions to close over local variables. It is handy when you need to pass additional data to an event handler. Although, make sure they [do not massively hurt performance](https://www.bignerdranch.com/blog/choosing-the-best-approach-for-react-event-handlers/), in particular when passed to custom components that might be PureComponents, because they will trigger a possibly needless rerender every time.

    ```jsx
    const ItemList = (props) => {
      return (
        <ul>
          {props.items.map((item, index) => (
            <Item
              key={item.key}
              onClick={(event) => doSomethingWith(event, item.name, index)}
            />
          ))}
        </ul>
      );
    }
    ```

  - Bind event handlers for the render method in the constructor. 

    > Why? A bind call in the render path creates a brand new function on every single render. Do not use arrow functions in class fields, because it makes them [challenging to test and debug, and can negatively impact performance](https://medium.com/@charpeni/arrow-functions-in-class-properties-might-not-be-as-great-as-we-think-3b3551c440b1), and because conceptually, class fields are for data, not logic.

    ```jsx
    // bad
    class extends React.Component {
      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />;
      }
    }

    // very bad
    class extends React.Component {
      onClickDiv = () => {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv} />
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
        return <div onClick={this.onClickDiv} />;
      }
    }
    ```

    If you need to use `bind` to render a list and have a separate callback for item, you should pull 
    the list items into it's own component.
    ```jsx
    // bad
    const List extends React.Component {
      render() {
      const { items, onItemClick } = this.props;
        return (
          <ul>
            {items.map((item, index) =>
              <ListItem 
                item={item} 
                key={item.id} 
                onItemClick={onItemClick} 
              />
            )}
          </ul>
        );
      }
    }

    // good
    const List extends React.Component {
      render() {
        const { items, onItemClick } = this.props;
        return (
          <ul>
            {items.map(item =>
              <ListItem key={item.id} item={item} onItemClick={onItemClick} />
            )}
          </ul>
        );
      }
    }

    const ListItem extends React.Component {
      onClickHandler = () {
        const { item: { id }, onItemClick } = this.props;
        onItemClick(id);
      }

      render() {
        return (
          <li onClick={this.onClickHandler}>
            ...
          </li>
        );
      }
    }
    ```

  - Do not use underscore prefix for internal methods of a React component.
    > Why? Underscore prefixes are sometimes used as a convention in other languages to denote privacy. But, unlike those languages, there is no native support for privacy in JavaScript, everything is public. Regardless of your intentions, adding underscore prefixes to your properties does not actually make them private, and any property (underscore-prefixed or not) should be treated as being public. See issues [#1024](https://github.com/airbnb/javascript/issues/1024), and [#490](https://github.com/airbnb/javascript/issues/490) for a more in-depth discussion.

    ```jsx
    // bad
    React.createClass({
      _onClickSubmit() {
        // do stuff
      },

      // other stuff
    });

    // good
    class extends React.Component {
      onClickSubmit() {
        // do stuff
      }

      // other stuff
    }
    ```

  - Be sure to return a value in your `render` methods. 

    ```jsx
    // bad
    render() {
      (<div />);
    }

    // good
    render() {
      return (<div />);
    }
    ```
## Ordering
  - Ordering for imports
  1. import React
  1. import third party libraries (a-z)
  1. import common modules (a-z)
  1. import other modules internal (a-z)

  - Ordering for `class extends React.Component`:

  1. optional `static` methods
  1. `constructor`
  1. `componentDidMount`
  1. `getDerivedState`
  1. `shouldComponentUpdate`
  1. `getSnapshotBeforeUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers or eventHandlers* like `onClickSubmit()` or `onChangeDescription()`
  1. *getter methods for `render`* like `getSelectReason()` or `getHeaderContent()`
  1. *optional render methods* like `renderNavigation()` or `renderProfilePicture()`
  1. `render`

  - How to define `defaultProps`, etc...

    ```jsx
    import React from 'react';

    const defaultProps = {
      text: 'Hello World',
    };

    class Link extends React.Component {
      static methodsAreOk() {
        return true;
      }

      render() {
        const { id, text, url } = this.props;
        return <a href={url} data-id={id}>{text}</a>;
      }
    }

    Link.defaultProps = defaultProps;

    export default Link;
    ```

  - Ordering for props: props passed into a component should always be sorted in alphabetical order.
  The same should be done with the props when you deconstruct them.

  ```jsx
  // bad
  <Foo
    corge={"fuzz"}
    xyzzy={"fuzz"}
    fred={"fuzz"}
    wibble={"fuzz"}
    thud={"fuzz"}
    wubble={"fuzz"}
    plugh={"fuzz"}
    waldo={"fuzz"}
    grault={"fuzz"}
    baz={"fuzz"}
    garply={"fuzz"}
    wobble={"fuzz"}
    flob={"fuzz"}
    qux={"fuzz"}
  />

  // good
  <Foo
    baz={"fuzz"}
    corge={"fuzz"}
    flob={"fuzz"}
    fred={"fuzz"}
    garply={"fuzz"}
    grault={"fuzz"}
    plugh={"fuzz"}
    qux={"fuzz"}
    thud={"fuzz"}
    waldo={"fuzz"}
    wibble={"fuzz"}
    wobble={"fuzz"}
    wubble={"fuzz"}
    xyzzy={"fuzz"}
  />
  ```

  ```jsx
  // bad
  const {
    garply,
    wubble,
    baz,
    xyzzy,
    thud,
    fred,
    qux,
    wobble,
    grault,
    wibble,
    plugh,
    flob,
    waldo,
    corge
  } = this.props;

  // good
  {
    baz,
    corge,
    flob,
    fred,
    garply,
    grault,
    plugh,
    qux,
    thud,
    waldo,
    wibble,
    wobble,
    wubble,
    xyzzy
  } = this.props;
  ```

**[⬆ back to top](#table-of-contents)**
