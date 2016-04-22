# ReactJS Notes
Some notes I took for learning ReactJS. I went through documentation and ran through a few tutorials to better understand ReactJS logic. Most of the resources I followed use ES6 and therefore required transpiling and setting up an environment through nodeJS. They are listed below:
- [surviveJS: webpack + React](http://survivejs.com/)
- [egghead: React Fundamentals](https://egghead.io/series/react-fundamentals)
- [ES6 features](https://github.com/lukehoban/es6features)


### Developing with Webpack
- --save-dev: used to save the package for development purposes (unit tests, minification).
- --save: used to save the package required for the app to run

### Fundamental Topics
- JSX - allows us to write HTML ish syntax which gets transformed to lightweight JS objects
- Virtual DOM - JS representation of the actual DOM
- render method - what we want our HTML template to look like
- ReactDOM.render - renders a React component to a DOM node (mounts)
- state - internal data store (object) of a component
- props - data which is passed to the child component from the parent component (<Example txt=“hello” />) —> “txt" is the child component’s prop
- propTypes - allows you to control the presence/type of certain props
    - Slider.propTypes = { min: React.PropTypes.number, max: React.PropTypes.number.isRequired, … }
- defaultProps - default props on a component
- Component Lifecycle -
    - componentWillMount - fired before component mounts. Invoked once before initial render (setting up a Firebase reference)
    - componentDidMount - fired after component mounts. Invoked once after initial render
    - componentWillReceiveProps - fired whenever a change to props occurs. Not called on initial render.
    - componentWillUnmount - fired before the component unmounts. Invoked before unmounted - do all clean up here (cleaning up a Firebase reference)

### Concepts
- Binding
    - Often we want to bind a component’s function to reference “this” as in “this” component. this.update.bind(this) —> the update function of this component now uses the component as “this"
- Keys
    - Each child in an array/iterator should have a unique ‘key’ prop. For example, creating React components in an iteration, each component needs to have a unique key.
- Props vs. State (Detailed)
    - **props** are a component's **configuration**, its options. They are received from above and **immutable** as far as the Component receiving them is concerned. A Component cannot change its *props*, but it is responsible for putting together the *props* of its *child* Components.
    - **state** starts with a default value when a Component mounts and then **suffers from mutations in time (mostly generated from user events).** It's a serializable representation of one point in time -- a snapshot. A Component manages its own *state* internally, but -- besides setting an initial state -- has no business fiddling with the state of its children. State can be thought of as **private** b/c Components **manage their own state**.  

### ES6
