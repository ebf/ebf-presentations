### EmberJS vs React
<br>
<span style="color:gray">comparison of client side JS frameworks</span>
<br>

---

### Basics
Feature | EmberJS | React
---------- | ------------ | -------------
Inception Year | 2011 | 2013
Current Version | 2.12 | 15.5.0
Type of framework | Fully fledged MVC | View layer only
"Rendering Engine" | Glimmer | Virtual DOM Diffing
"Templating Engine" | Handlebars | JSX

---

### Client Side Frameworks
<span style="color:gray">Common Flow</span>
<br>

![FLOW](https://www.smashingmagazine.com/wp-content/uploads/2016/01/04-client-side-rendering-opt.png)

---

### View Layer Basics - EmberJS
Typical Flow:
- `router` loads `route` for given url
- `route` loads a `model` (either from store or from backend)
- `route` passes the model to a `controller`
- if no `controller` for the given route is defined, ember generates a `pass-through controller` on the fly
- a `controller` may add additional `properties` (think `isToggled`) and `actions` (think `doToggle()`), which are invoked by the user
- `route` renders a `template` into the `{{outlet}}` of its parent route's template
- a `template` may use mulitple `components` to make up the UI, passing data down as `properties`
- user interactions are real events and bubble up, until a `controller` or `route` handles the `action`

---

### View Layer Basics - React
Components can be represented as functions or ES6 classes
```js
function Person(props) {
  return (
    <div>{props.person.name}</div>
  );
}
```
```js
class Person extends React.Component {
  render() {
    return (
      <div>{this.props.person.name}</div>
    );
  }
}
ReactDOM.render(<Person person={jsonResponse.person}/>, document.getElementBiId('root'));
```

### View Layer Concepts
#### Binding
EmberJS | React
---------- | ------------ 
two-way bindings possible, used for e.g. `<input>` elements | one-way bindings only
through the concept of mixins and dependency injection (service) this does not necessarily lead to complex top level components | leads to complex top level components

---

### View Layer Concepts
#### Props and State
EmberJS | React
---------- | ------------ 
Properties are passed from the parent  | Props are passed from the parent component to the child component, immutable
Must use this.set(‘obj’, obj) and this.get(‘obj’) | Must use this.setState({obj}) and this.state.obj
Actions are real events and bubble up | “Actions” must be passed in as props and are plain callbacks

---

### View Layer Examples
#### Conditionals - HBS
```hbs
{{#if person}}
  Welcome back, {{person.name}}
{{else}}
  Login
{{/if}}
```

---

### View Layer Examples
#### Conditionals - JSX - recommended way
```js
function Greeting(props){
    if (props.person){
        return (
            <div>
                Welcome back, {props.person.name}
            </div>
        )
    }
    return (
        <div>
            Login
        </div>
    )
}
ReactDOM.render(
    <Greeting person={jsonResponse.person}/>
    document.getElementById('root')
)
```

---

### View Layer Examples
#### Conditionals - JSX - inline way
```js
ReactDOM.render(
    <div>
        {this.props.person ? (
            <div>Welcome back, {this.props.person.name}</div>
        ) : (
            <div>Login</div>
        )}
    </div>,
    document.getElementById('root')
)
```

---

### View Layer Examples
#### Iterating - HBS
```hbs
<ul>
    {{#each persons as |person|}}
        <li>{{person.name}}</li>
    {{/each}}
</ul>
```

---

### View Layer Examples
#### Iterating - JSX - recommended way
```js
function ListItems(props){
    const listItems = props.persons.map(person => 
        <li>{person.name}</li>
    );
    return (
        <ul>{listItems}</ul>
    )
}
```

---

### View Layer Examples
#### Iterating - JSX - inline way
```js
function ListItem(props){
    return <li>{props.name}</li>
}
return (
    <ul>
    {persons.map(person =>
        <ListItem key="{person.id}" name={person.name}/>
    )}
    </ul>
)
```

---

### React Templates
#### Conditionals
```js
<div>
    <p rt-if="this.props.person">Welcome back, {person.name}</p>
    <ul>
        <li rt-repeat="person in this.props.persons">{person.name}</li>
    </ul>
</div>
```
- third party library
- no `else`, `switch`, `while` etc.
- must be precompiled

---

### React Data Flow
#### Lifting state up
- uni-directional flow isn’t very scalable
- to share state between separate components, you have to 1) find the nearest common ancestor component 2) store the shared state on it, and 3) feed callbacks through all intermediate components to the two components
- parent components will continue to bloat (fat interface problem)
- Intermediate components now have added responsibility of forwarding through callbacks they don’t really care about

---

### React Data Flow
#### Flux/Redux
Feature | Presentational Components | Container Components
--- | --- | ---
Purpose | How things look | How things work (fetching data, state updates)
Aware of Redux | No | Yes
read data | from props | by subscribing to redux state
write data | by invoking callbacks from props | by dispatching redux actions
generated | by hand | usually by react redux

---

### React Data Flow
#### Stores, Reducers, Actions

- In Redux, all the application state is stored as a single object (the __Store__), normalized as much as possible (think of it as a DB)
- __Actions__ are payloads of information that send data from your application to your store. They are the only source of information for the store. You send them to the store using store.dispatch().
- __Reducers__ are a pure functions that take the previous state and an action, and return the next state. Given the same arguments, it should calculate the next state and return it. No surprises. No side effects. No API calls. No mutations. Just a calculation.

--- 

### Routing - EmberJS

- builtin
- easy and flexible
```js
Router.map(function() {
  this.route('index', { path: '/' });
  this.route('posts', function() {
    this.route('new');
  });
  this.route('post', { path: '/post/:post_id' });
});
```

--- 

### Routing - React
- not builtin
- most common library is React-Router
```js
const Post = ({match}) => (
    <div>Post ID: {match.params.id}<div>
)
ReactDOM.render(
    <Router>
        <Route exact path="/" component={Home}/>
        <Route exact path="/posts" component={Posts}/>
        <Route exact path="/posts/new" component={NewPost}/>
        <Route path="/post/:id" component={Post}/>
    </Router>
)
```

---

### React Drawbacks
- Flow control (if blocks, loops), or anything involving lambdas, are awkward
- Often they need to be kept visually separate from the rest of your render code
- Component.render must return a single element (with any number of children)
- Tempting to want to return something like `<div></div><div></div>`, but that’s like returning div(),div() (and React doesn’t support returning arrays)
- Items in lists must have a key (in order for the rendering to be performant)
- requires lots of extensions to be useful, react-router, react-redux...

---

### Ideas for other presentations
- ES2015, ES2016, ES2017 enhancements
    - promises, generators, await/async
    - property shorthand, computed property names
    - rest parameter, spread operator, array destructuring
    - classes, modules
    - lodash vs new builtin methods (Object.assign, find, map, filter, each, _pluck...)
- comparison of build tools / bundlers
    - gulp, grunt, broccoli, webpack, bower, browserify
- comparison of package managers
    - npm vs yarn
- comparson of node ORM libraries
    - persistence.js, sequelize, node-orm2, bookshelf.js
