# ReactJS + ES6 Notes
Some notes I took for learning ReactJS. I went through documentation and ran through a few tutorials to better understand ReactJS logic. Most of the resources I followed use ES6 and therefore required transpiling and setting up an environment through nodeJS. They are listed below:
- [surviveJS: webpack + React](http://survivejs.com/)
- [egghead: React Fundamentals](https://egghead.io/series/react-fundamentals)
- [ES6 features](https://github.com/lukehoban/es6features)


## Developing with Webpack & NodeJS
- refer to surviveJS' webpack section
- `--save-dev`: used to save the package for development purposes in nodeJS (unit tests, minification)
- `--save`: used to save the package required for the app to run
- `import` statements import both Node modules and other JS component (files)
- `export` statements allows a file (module) to be imported
- `import`/`export` are ES6. By using Babel to render ES6 `import`/`export`, it converts them to CommonJS `require`/`module.exports` anyway
- `require` are NodeJS

## Fundamental Topics
- **JSX** - allows us to write HTML ish syntax which gets transformed to lightweight JS objects
- **Virtual DOM** - JS representation of the actual DOM
- **render method** - what we want our HTML template to look like
- **ReactDOM.render** - renders a React component to a DOM node (mounts)
- **state** - internal data store (object) of a component
- **props** - data which is passed to the child component from the parent component (<Example txt=“hello” />) —> “txt" is the child component’s prop
- **propTypes** - allows you to control the presence/type of certain props
    - Slider.propTypes = { min: React.PropTypes.number, max: React.PropTypes.number.isRequired, … }
- **defaultProps** - default props on a component
- **refs** - allow parent Components to refer to specific Child components in a way that would be inconvenient via typical React data-flow. For pros/cons for refs, refer to [documentation](https://facebook.github.io/react/docs/more-about-refs.html#summary)
- **Component Lifecycle** -
    - **componentWillMount** - fired before component mounts. Invoked once before initial render (setting up a Firebase reference)
    - **componentDidMount** - fired after component mounts. Invoked once after initial render. You have access to the DOM here, so we could use this hook to wrap a jQuery plugin within a component.
    - **componentWillReceiveProps** (object nextProps) - fired whenever a change to props occurs. Not called on initial render. You could modify your component state based on the upcoming props.
    - **shouldComponentUpdate(object nextProps, object nextState)** - allows you to optimize rendering. If you check props & state and see there's no need to update, return false.
    - **componentDidUpdate** - triggered after rendering. You can modify the DOM here. This can be useful for adapting other code to work with React.
    - **componentWillUnmount** - fired before the component unmounts. Invoked before unmounted - do all clean up here (cleaning up a Firebase reference)
    - **displayName** - can improve debug information by setting Component display name
    - **getInitialState** - same as **constructor()** method in class based approach. Calling super, binding functions, setting up state can all be done here.
    - **mixins** - contains an array of mixins to apply to components
    - **statics** - contain static props and methods for a component.

## Concepts
- **Binding**
    - Often we want to bind a component’s function to reference “this” as in “this” component. this.update.bind(this) —> the update function of this component now uses the component as “this"
- **Keys**
    - Each child in an array/iterator should have a unique ‘key’ prop. For example, creating React components in an iteration, each component needs to have a unique key.
- **Props and State (Detailed)**
    - **props** are a component's **configuration**, its options. They are received from above and **immutable** as far as the Component receiving them is concerned. A Component cannot change its *props*, but it is responsible for putting together the *props* of its *child* Components.
    - **state** starts with a default value when a Component mounts and then **suffers from mutations in time (mostly generated from user events).** It's a serializable representation of one point in time -- a snapshot. A Component manages its own *state* internally, but -- besides setting an initial state -- has no business fiddling with the state of its children. State can be thought of as **private** b/c Components **manage their own state**.  
    - **Stateless Component (Dumb as F*ck Components):** Only props, no state. Not much going on besides render() function, and all logic revolves around props they receive. These are easy to follow & test.
    - **Stateful Component**: both props and state. Also called *state managers*. They are in charge of client-server communication (XHR, web sockets, etc.), processing data, and responding to user events. These sort of logistics should be encapsulated in a moderate number of Stateful Components, while all visualization and formatting logic should move downstream into as many Stateless Components as possible.
    - state is optional. B/c state increases complexity and reduces predictability, a Component without state is preferable. Even though you clearly can't do without state in an interactive app, avoid having too many Stateful Components
    - If a Component needs to alter one of its attribute at some point in time, that attribute should be part of its state, otherwise it should just be a prop for that Component.

---

## Flux Architecture
![Flux](/flux.png)
Flux allows us to separate data and app state from our views. Flux implements unidrectional flow in contrast to frameworks like Angular & Ember (two-directional). Two-directional has benefits, but it can be hard to deduce what's happening.
- **Unidrectional vs Two-way binding**
    - **Two-way binding** - view is updated when the state changes, and vice versa. View <> State
    - **Unidirectional** - View is a function of app state; when state changes, the view automatically re-renders itself, so the same state produces the same view. Mutation of data is done via actions, so view components subscribe to stores and automatically re-render themselves using the new data. Action -> Store -> View

#### Actions and Stores
- A **store** is a single source of truth for a part of your app state.

#### Dispatcher
When an action is triggered, the dispatcher will get notified, and it will be able to deal w/ possible dependencies between stores. It may be that one action needs to happen before another.

Once the dispatcher has dealt with an action, the stores listening to it get triggered, then it can update its internal state. After, the store will notify possible listeners of its new state.

#### Flux Dataflow
Usually the unidirectional process has a cyclical flow and doesn't necessarily end. It's the same idea, but with addition of a returning cycle.
![Flux](/flux2.png)

## Alt JS
In Alt, you deal with **actions** and **stores**. The dispatcher is hidden, but you have access if needed.


---

## Conventions
- Ordering
    1. `constructor()` function
    2. lifecycle hooks
    3. `render()`
    4. methods used by `render()`
    5. use `_` prefix for event handlers

## ES6

#### Arrows
Arrows are a function shorthand using the `=>` syntax. Unlike functions, arrows share the same lexical `this` as their surrounding code.
```JavaScript
// Expression bodies
var odds = evens.map(v => v + 1);
var nums = evens.map((v, i) => v + i);
var pairs = evens.map(v => ({even: v, odd: v + 1}));

// Statement bodies
nums.forEach(v => {
  if (v % 5 === 0)
    fives.push(v);
});

// Implied returns
var func = (x, y) => { x+y; };

// Lexical this
var bob = {
  _name: "Bob",
  _friends: [],
  printFriends() {
    this._friends.forEach(f =>
      console.log(this._name + " knows " + f));
  }
}
```

#### Classes
ES6 classes are a simple sugar over the prototype-based OO pattern. Having a single convenient declarative form makes class patterns easier to use, and encourages interoperability. Classes support prototype-based inheritance, super calls, instance and static methods and constructors.

Some special methods provided by ES6 classes:

    - `constructor(...)`
    - `get prop`
    - `set prop`
    - `static func(...)` - static methods are called *without* instantiating their class (called on the *class* itself, not on an instance of it), and are **not** callable when it is instantiated. Static methods are used to create **utility functions** for an app.

```JavaScript
class SkinnedMesh extends THREE.Mesh {
  constructor(geometry, materials) {
    super(geometry, materials);

    this.idMatrix = SkinnedMesh.defaultMatrix();
    this.bones = [];
    this.boneMatrices = [];
    //...
  }
  update(camera) {
    //...
    super.update();
  }
  get boneCount() {
    return this.bones.length;
  }
  set matrixType(matrixType) {
    this.idMatrix = SkinnedMesh[matrixType]();
  }
  static defaultMatrix() {
    return new THREE.Matrix4();
  }
}
```

#### Template Strings
Syntactic sugar for constructing strings. Similar to string interpolation. Tags allow string construction to be customized.
```JavaScript
// Basic literal string creation
`In JavaScript '\n' is a line-feed.`

// Multiline strings
`In JavaScript this is
 not legal.`

// String interpolation
var name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`

// Construct an HTTP request prefix is used to interpret the replacements and construction
POST`http://foo.org/bar?a=${a}&b=${b}
     Content-Type: application/json
     X-Credentials: ${credentials}
     { "foo": ${foo},
       "bar": ${bar}}`(myOnReadyStateChangeHandler);
```

In the above example, a function called POST looks like
```JavaScript
function POST (strings, ...values) {
    // strings contains an array of string literals within the POST expression (http://foo..., Content-Type:, ...)
    // values contains the processed substitution expressions, like the value of a, b, credentials, ...
    console.log(strings[0]); // "http://foo.org/bar?a="
    console.log(values[0]); // value of credentials
}
```

#### Destructuring
Destructuring allows binding using pattern matching.
``` JavaScript
var foo = ["one", "two", "three"];

var [one, two, three] = foo;
console.log(one); // "one"
console.log(two); // "two"
console.log(three); // "three"

// Below, a and b have default values in the case that their assignment is undefined.
// In this case, b=7 is maintained because its assignment is undefined.
var a, b;

[a=5, b=7] = [1];
console.log(a); // 1
console.log(b); // 7
```

#### Default + Rest + Spread
```JavaScript
function f(x, y=12) {
    // y is 12 if no y argument, or if it's undefined
    return x + y;
}
f(3) === 15
```

```JavaScript
function f(x, ...y) {
    // push all args after x into an array
    return x * y.length;
}
f(3, 'hello', true) === 6
```

```JavaScript
function f(x, y, z) {
    return x + y + z;
}
// pass each elem of array as an argument
f(...[1,2,3]) === 6  
```

#### Let + Const
**Block-scoped** binding constructs. `let` is the new `var`. `const` is single-assignment.
```JavaScript
function f() {
    let x;
    {
        // this is OK - block scoped x is not the same as the x above!!
        const x = "sneaky";
        // error, const can't be reassigned
        x = "foo";
    }
    // error, already declared in block
    let x = "inner";
}
```

#### Iterators + For..Of
The for..of operator iterates over items in collections, rather than properties of objects (like for..in).
```JavaScript
let iterable = [1, 2, 3];

for (let value of iterable) {
  console.log(value);
}
// 1
// 2
// 3

let iterable = "boo";

for (let value of iterable) {
  console.log(value);
}
// "b"
// "o"
// "o"
```

#### Generators
Look more into this...

#### Modules
```JavaScript
// lib/math.js
export function sum(x, y) {
  return x + y;
}
export var pi = 3.141593;
```

```JavaScript
// app.js
import * as math from "lib/math";
alert("2π = " + math.sum(math.pi, math.pi));
```

```JavaScript
// otherApp.js
import {sum, pi} from "lib/math";
alert("2π = " + sum(pi, pi));
```

```JavaScript
// lib/mathplusplus.js
export * from "lib/math";
export var e = 2.71828182846;
export default function(x) {
    return Math.log(x);
}
```

```JavaScript
// app.js
// ln is the default function from within lib/mathplusplus
// {pi, e} comes from both mathplusplus mathplusplus' export * from lib/math
// so mathplusplus exports its own modules as well as those from lib/math
import ln, {pi, e} from "lib/mathplusplus";
alert("2π = " + ln(e)*pi*2);
```

#### Math + Number + String + Array + Object APIs
```JavaScript
Number.EPSILON
Number.isInteger(Infinity) // false
Number.isNaN("NaN") // false

Math.acosh(3) // 1.762747174039086
Math.hypot(3, 4) // 5
Math.imul(Math.pow(2, 32) - 1, Math.pow(2, 32) - 2) // 2

"abcde".includes("cd") // true
"abc".repeat(3) // "abcabcabc"

Array.from(document.querySelectorAll('*')) // Returns a real Array
Array.of(1, 2, 3) // Similar to new Array(...), but without special one-arg behavior
[0, 0, 0].fill(7, 1) // [0,7,7]
[1, 2, 3].find(x => x == 3) // 3
[1, 2, 3].findIndex(x => x == 2) // 1
[1, 2, 3, 4, 5].copyWithin(3, 0) // [1, 2, 3, 1, 2]
["a", "b", "c"].entries() // iterator [0, "a"], [1,"b"], [2,"c"]
["a", "b", "c"].keys() // iterator 0, 1, 2
["a", "b", "c"].values() // iterator "a", "b", "c"

Object.assign(Point, { origin: new Point(0,0) })
```

#### Promises
Promises are a library for asynchronous programming. Promises are a first class representation of a value that may be available in the future. 
