---
layout: post
title:  "React Learning Notes: the Basic Knowledge"
date:   2017-05-28 10:00:00 +0800
categories: jekyll update
---

<link rel="stylesheet" href="http://yandex.st/highlightjs/6.2/styles/googlecode.min.css">

<script src="http://code.jquery.com/jquery-1.7.2.min.js"></script>
<script src="http://yandex.st/highlightjs/6.2/highlight.min.js"></script>

<script>hljs.initHighlightingOnLoad();</script>
<script type="text/javascript">
 $(document).ready(function(){
      $("h2,h3,h4,h5,h6").each(function(i,item){
        var tag = $(item).get(0).localName;
        var className = $(item).get(0).className;
        if (className != "footer-heading"){
            $(item).attr("id","wow"+i);
            $("#category").append('<a class="new'+tag+'" href="#wow'+i+'">'+$(this).text()+'</a></br>');
            $(".newh2").css("margin-left",0);
            $(".newh3").css("margin-left",20);
            $(".newh4").css("margin-left",40);
            $(".newh5").css("margin-left",60);
            $(".newh6").css("margin-left",80);
      }});
 });
</script>
<div id="category"></div>

## React for building UI
> React: A JAVASCRIPT LIBRARY FOR BUILDING USER INTERFACES (UI).

- **React is Declarative**.
React makes it **painless** to create **interactive** UIs. Design simple views for each state in your application, and React will efficiently update and render just the right components when your data changes. Declarative views make your code more predictable and easier to debug.
- **React is Component-Based**.
Build encapsulated components that manage their own state, then **compose** them to make complex UIs.
Since component logic is written in JavaScript instead of templates, you can easily pass rich data through your app and keep state out of the DOM.
- **Learn Once, Write Anywhere**.
We don't make assumptions about the rest of your technology stack, so you can develop new features in React without rewriting existing code. React can also render on the server using Node and power mobile apps using React Native.

## Show UI in React
**React** is a **JavaScript** library. To learn React, basic knowledge of JavaScript is needed.

### ES6
**ES6**, known as the **next generation** of JavaScript, has some great new features such as arrow functions, classes, template literals, let, and const statements. React uses ES6 syntax in a sparing manner.

### JSX
**JSX**, a syntax extension to JavaScript, is considered as the recommended way in React to describe **what the UI should look like**. JSX produces React "elements". JSX has the following characteristics.

- You can embed any JavaScript expression in JSX by *wrapping it in curly braces*.
- Recommend to split JSX over *multiple lines* for readability.
- Recommend *wrapping it in parentheses* to avoid the pitfalls of automatic semicolon insertion.
- After compilation, JSX expressions become *regular JavaScript objects*.
- You can **use JSX inside of `if` statements and `for` loops, assign it to variables, accept it as arguments, and return it from functions**.
- You may use quotes to specify *string literals* as attributes.
- You may also use curly braces to embed a JavaScript expression in an *attribute*.
- Don't put quotes around curly braces when embedding a JavaScript expression in an attribute. Otherwise JSX will treat the attribute as a string literal rather than an expression.
- You should either use quotes (for string values) or curly braces (for expressions), but not both in the same attribute.
- If a tag is empty, you may close it immediately with `/>`, like XML.
- JSX tags may contain children.
- It is safe to embed user input in JSX.
- By default, React DOM escapes any values embedded in JSX before rendering them. Thus it ensures that you can never inject anything that's not explicitly written in your application. Everything is converted to a string before being rendered. This helps prevent XSS (cross-site-scripting) attacks.
- Babel compiles JSX down to `React.createElement()` calls.
- `React.createElement()` performs a few checks to help you write bug-free code.
- These coverted JacaScrpit objects are called **"React elements"**. You can think of them **as descriptions of what you want to see on the screen. React reads these objects and uses them to construct the DOM and keep it up to date**.

>**Note:**
1. Since JSX is closer to JavaScript than HTML, React DOM uses `camelCase` property naming convention instead of HTML attribute names. For example, `class` becomes `className` in JSX, and `tabindex` becomes `tabIndex`.
2. We recommend searching for a "Babel" syntax scheme for your editor of choice so that both ES6 and JSX code is properly highlighted.

### React Components
Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.
Conceptually, **components are like JavaScript functions**. They accept arbitrary inputs (called "**props**") and **return React elements describing what should appear on the screen**.

- The simplest way to define a component is to write a JavaScript function. We call such components "**functional components**" because they are literally JavaScript functions.
- You can also use an **ES6 class** to define a component.

Previously, we only encountered React elements that represent DOM tags. **Elements can also represent user-defined components**. When React sees an element representing a user-defined component, it passes JSX attributes to this component as a single object. We call this object "props".

Components can refer to other components in their output. This lets us use the same component abstraction for any level of detail. A button, a form, a dialog, a screen: in React apps, **all those are commonly expressed as components**.

Don't be afraid to **plit components into smaller components**. Extracting components might seem like grunt work at first, but having a palette of reusable components pays off in larger apps. A good rule of thumb is that if a part of your UI is used several times (`Button`, `Panel`, `Avatar`), or is complex enough on its own (`App`, `FeedStory`, `Comment`), it is a good candidate to be a reusable component.

Whether you declare a component as a function or a class, it must never modify its own props. Such functions are called "pure" because they do not attempt to change their inputs, and always return the same result for the same inputs.
React is pretty flexible but it has a single strict rule:

**All React components must act like pure functions with respect to their props.**

>**Note:**
1. Always start component names with a capital letter. For example, `<div />` represents a DOM tag, but `<Welcome />` represents a component and requires `Welcome` to be in scope.
2. **Components must return a single root element**. This is why we added a `<div>` to contain all the `<Welcome />` elements.

### Convert a Function to a Class
You can convert a functional component like `Clock` to a class in five steps:

1. Create an ES6 class with the same name that extends `React.Component`.
2. Add a single empty method to it called `render()`.
3. Move the body of the function into the `render()` method.
4. Replace `props` with `this.props` in the `render()` body.
5. Delete the remaining empty function declaration.

## Update UI in React

### Local State
To update UI, one simple way is setting up a ticking clock.
However, we may only want to update the specified part, not the whole UI. To implement this, we need to add "state" to the `Clock` component. **State is similar to props, but it is private and fully controlled by the component**.

Components defined as classes have some additional features. **Local state** is exactly that: a feature available only to classes. Some characteristics of local state include:
(1) Adding Local State to a Class, (2) Add a class constructor that assigns initial values, (3) Pass `props` to the base constructor, (4) Class components should always call the base constructor with `props`, (5) Declare special methods on the component class to run some code when a component mounts and unmounts. These methods are called "lifecycle hooks". The `componentDidMount()` hook runs after the component output has been rendered to the DOM.

If you don't use something in `render()`, it shouldn't be in the state. We use `this.setState()` to schedule updates to the component local state. Note that:

1. **Do Not Modify State Directly**.
The only place where you can assign `this.state` is the constructor.
2. **State Updates May Be Asynchronous**.
React may batch multiple `setState()` calls into a single update for performance. Because `this.props` and `this.state` may be updated asynchronously, you should not rely on their values for calculating the next state.
3. **State Updates are Merged**.
When you call `setState()`, React merges the object you provide into the current state.

It is not accessible to any component other than the one that owns and sets it. A component may choose to pass its state down as props to its child components. This is commonly called a "top-down" or "unidirectional" data flow:

- Any state is always owned by some specific component.
- Any data or UI derived from that state can only affect components "below" them in the tree.

In React apps, **whether a component is stateful or stateless is considered an implementation detail of the component that may change over time**. You can use stateless components inside stateful components, and vice versa.

### Events
Handling events with React elements is very similar to handling events on DOM elements. There are some syntactic differences:

- React events are named using camelCase, rather than lowercase.
- With JSX you pass a function as the event handler, rather than a string.

Another difference is that you cannot return `false` to prevent default behavior in React. You must call `preventDefault` explicitly.

In React, `e` is a synthetic event. React defines these synthetic events according to the [W3C spec](https://www.w3.org/TR/DOM-Level-3-Events/), so you don't need to worry about cross-browser compatibility. See the [`SyntheticEvent`](/react/docs/events.html) reference guide to learn more.

When using React you should generally not need to call `addEventListener` to add listeners to a DOM element after it is created. Instead, just provide a listener when the element is initially rendered.

When you define a component using an [ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes), a common pattern is for an event handler to be a method on the class.

You have to be careful about the meaning of `this` in JSX callbacks. **In JavaScript, class methods are not [bound](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) by default**. If you forget to bind `this.handleClick` and pass it to `onClick`, `this` will be `undefined` when the function is actually called.

This is not React-specific behavior; it is a part of [how functions work in JavaScript](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/). Generally, if you refer to a method without `()` after it, such as `onClick={this.handleClick}`, you should bind that method.

If calling `bind` annoys you, there are two ways you can get around this. If you are using the experimental [property initializer syntax](https://babeljs.io/docs/plugins/transform-class-properties/), you can use property initializers to correctly bind callbacks.
If you aren't using property initializer syntax, you can use an [arrow function](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) in the callback.

The problem with this syntax is that a different callback is created each time the `LoggingButton` renders. In most cases, this is fine. However, if this callback is passed as a prop to lower components, those components might do an extra re-rendering. We generally recommend binding in the constructor or using the property initializer syntax, to avoid this sort of performance problem.
