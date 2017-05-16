name: intro
class: center, middle

# Deep Dive on the React Lifecycle
#### http://bit.ly/universal-app-lessons

---
class: center, middle

# whoami

### Jonathan Creamer

<img src="images/family.jpg" style="width: 80%;" />

---

# whoami

* Currently Senior Front End Engineer at [Lonely Planet](http://lonelyplanet.com)
* Past JavaScript Engineer appendTo
* Nashville, TN

<img src="images/lonelyplanet_bw.png" style="width: 10em" />

* Love JavaScript, tweet at [@jcreamer898](http://twitter.com/jcreamer898), blog at [jonathancreamer.com](http://jonathancreamer.com)
* [Microsoft MVP](https://mvp.microsoft.com/en-us/MyProfile/Preview?previewAs=Public)

????

class: center, middle

---

### Agenda

1. What is a React component?
1. Lifecycle methods
1. Use cases
1. Testing

---

### Pure Functions

A function is pure if...

> Given the same input, will always return the same output.
> Produces no side effects.
> Relies on no external mutable state.

* One way to build a React component is as a pure function

---

### Can you even Component bro?

```
function HelloWorld({
  text
}) {
  return (
    <h1>{text}</h1>
  );
}

ReactDOM.render(<HelloWorld text="hello world" />, document.body);
```

* This is a pure function
* Sometimes they're just functions
* Always start with capital
* Might be all you need

???

Sometimes all you need is to take props and return elements  

---

### JSX to JS

```
function HelloWorld(_ref) {
  var text = _ref.text;

  return React.createElement(
    "h1",
    null,
    text
  );
}
```

* JSX is an abstraction over creating element trees
* Different renderers like ReactDOM

---

# React Components as Classes

```
class HelloWorld extends React.Component {
  render() {
    const { text } = this.props;
    return (
      <h1>{text}</h1>
    );
  }
}
```

* If all you have is `render`, stay functional
* Don't screw with props, they're Read Only
* Sometimes we need some state

---
class: center, middle

# constructor

---

### constructor

```js
constructor(super) {
  super(props);
  this.state = {}:
}
```

* Perform any initial setup
* Called once per mounted component
* Initialize state

---

### State

> What's going on around here right now?

* Has a user clicked or typed anything?
* Any data needing to be fetched?
* Any stored information?

---

### setState

```js
class WYSIWYG extends React.Component {
  update() { /*...*/ }
  render() {
    const { text } = this.state;
    return (
      <textarea
        onChange={this.update}
      />
    );
  }
}
```

* Pull from `state`
* A lot of things happen when you do this...

---


### setState

```js
constructor(props) {
  super(props);

  this.state = {
    text: "",
  };

  this.update = this.update.bind(this);
}
update(e) {
  const { text } = e.target.value;
  this.setState({ text });
}
```

* Must `bind` for `this` to work
* After `setState`, `render` will fire
* Do NOT update state w/ `this.state.text = "foo";`

???

First lifecycle hook is constructor

---
class: center, middle

# componentDidMount

---

### componentDidMount

```js
import ace from "aceeditor";

export default class Editor extends React.Component {
  componentDidMount() {
    this.editor = ace.edit(this.$text);
  }
  render() {
    return (
      <div
        ref={(node) => this.$text = node}
      />
    )
  }
}
```

* jQuery plugin time :trollface:
* DOM is ready here
* Stand up plugins
* `ref` is now a function

---
class: center, middle

# componentWillUnmount

---

### Be a good citizen

```js
componentWillUnmount() {
  this.editor.destroy();
}
```

* 
* No leaks

---

### componentDidMount IRL

```js
export default class PoiDetail {
  componentDidMount() {
    if (!this.state.poi) {
      this.fetchPoi(this.props.params.id);
    }

    if (!this.state.related) {
      this.fetchRelated(this.props.params.id);
    }
  }
}
```

* Use `componentDidMount` to fire actions

---

### componentDidMount IRL

```js
import { connect } from "react-redux";
import { bindActionCreators } from "redux";

const mapStateToProps = state => ({
  poi: state.poi,
  related: state.related
});
const mapDispatchToProps = (dispatch) => ({
  ...bindActionCreators(actions, dispatch)
});
const connected = connect(mapStateToProps, mapDispatchToProps)(Poi);
export { connected };
```

* Setup the state, and props

---
class: center, middle

# componentWillReceiveProps

---
class: center, middle

# shouldComponentUpdate

---

class: center, middle

# Testing



---

# Thanks!

<img style="width:50%" src="images/clap.gif" />

### [@jcreamer898](http://twitter.com/jcreamer898)
### [jonathancreamer.com](http://jonathancreamer.com)