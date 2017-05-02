### EmberJS vs React
<br>
<span style="color:gray">comparison of client side JS frameworks</span>
<br>

---

### Client Side Frameworks
<span style="color:gray">Common Flow</span>
<br>

![FLOW](https://www.smashingmagazine.com/wp-content/uploads/2016/01/04-client-side-rendering-opt.png)

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
#### Conditionals - JSX
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
#### Conditionals - JSX
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
#### Iterating - JSX
```js
function ListItem(props){
    return <li>{props.name}</li>
}
return (
    <ul>
    {persons.map((person)=>
        <ListItem key="{person.id}" name={person.name}/>
    )}
    </ul>
)
```
