# ReactJS Notes
Some notes I took for learning ReactJS. I went through documentation and ran through a few tutorials to better understand ReactJS logic. Most of the resources I followed use ES6 and therefore required transpiling and setting up an environment through nodeJS. They are listed below:
- [surviveJS: webpack + React](http://survivejs.com/)
- [egghead: React Fundamentals](https://egghead.io/series/react-fundamentals)
- [ES6 features](https://github.com/lukehoban/es6features)


### Developing with Webpack
- refer to surviveJS' webpack section
- --save-dev: used to save the package for development purposes in nodeJS (unit tests, minification)
- --save: used to save the package required for the app to run

### Fundamental Topics
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

### Concepts
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

### Flux Architecture
![Flux](/flux.png)
Flux allows us to separate data and app state from our views. Flux implements unidrectional flow in contrast to frameworks like Angular & Ember (two-directional). Two-directional has benefits, but it can be hard to deduce what's happening.
- **Unidrectional vs Two-way binding**
    - **Two-way binding** - view is updated when the state changes, and vice versa. View <> State
    - **Unidirectional** - View is a function of app state; when state changes, the view automatically re-renders itself, so the same state produces the same view. Mutation of data is done via actions, so view components subscribe to stores and automatically re-render themselves using the new data. Action -> Store -> View

##### Actions and Stores

##### Dispatcher
When an action is triggered, the dispatcher will get notified, and it will be able to deal w/ possible dependencies between stores. It may be that one action needs to happen before another.

Once the dispatcher has dealt with an action, the stores listening to it get triggered, then it can update its internal state. After, the store will notify possible listeners of its new state.

##### Flux Dataflow
Usually the unidirectional process has a cyclical flow and doesn't necessarily end. It's the same idea, but with addition of a returning cycle.
![Flux](/flux2.png)

### Alt JS
In Alt, you deal with **actions** and **stores**. The dispatcher is hidden, but you have access if needed.


---

### Conventions
- Ordering
    1. `constructor()` function
    2. lifecycle hooks
    3. `render()`
    4. methods used by `render()`
    5. use `_` prefix for event handlers

### ES6
