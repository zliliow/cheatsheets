## Installing React App 
```sh
npx create-react-app <project-name>
```

## Project structure
```yaml
-- src 			: source code
|
-- public 		: static files (images) 
|
-- node_modules		: project dependencies
|
-- package.json 	: configuraton for project (like scripts) and dependencies 
|
-- package-lock.json 	: versions of dependencies 
|
-- README.md 		: readme
```
## Starting app
`npm start`

## Mandatory dependencies
```js
import React from 'react';
import ReactDOM from 'react-dom';
```

## React component
> Is a function/class that produces HTML (using JSX) and handles feedback from user (by Event Handlers).

## JSX
> JSX is an XML/HTML-like syntax used by React that extends ECMAScript so that XML/HTML-like text can co-exist with JavaScript/React code. JSX is then transpiled by Babel.

```js
const App = () => <div>Hello there</div>;
```
=> 
```js
var App = function App() {
  return React.createElement("div", null, "Hello there");
};
```

## JSX vs HTML
### Inline Style
```html
<div style = "background-color: red;"></div>
```
=>
```js
<div style = {{ backgroundColor: 'red' }}></div>
```
> `{` for referencing javascript variable inside JSX
> `{}` inside is just js literal for an object

### ClassName
```html 
<label class = "label"></label>
```
=> 
```js
<label className = "label"></label>
```

### JS variables ref
```js
<label className = { labelName }></label>
```

## `'` vs `"` in JSX
- `"` when JSX string property
- `'` when non-JSX property

## Tenets of React Compoment
- Nesting
- Reusability
- Configuration

## `props` system
> System for passing data from a parent component to a child component. 
> Goal is to customize or configure a child component. 

### `props` attributes
```js
<CommentDetail author="Sam" />
```
```js
const CommentDetail = props => {
	return <div>{ props.author }</div>
}
```

### `props` children
```js
const App = () => {
    <ApprovalCard>
        <CommentDetail author="goobi" />
    </ApprovalCard>
};

const ApprovalCard = props => {
    return <div>{ props.children }</div>
};
```

### `props` defaults
```js
App.defaultProps = {
	author="foo"
}
```

## `class` component
- extends `React.Component`
- must impl `render`

```js
class App extends React.Component {
	render() {
		return <div>foo</div>;
	}
```

## `state`
- js object that contains data relevant for component
- updating `state` causes component to instantly rerender
- must be initialized when component is created 
- state can *only* be updated using the `setState` function

### Init `state` with constructor
```js
class App extends React.Component {
	constructor(props) {
		super(props);	
		// WARN: this is *only* time we do direct assignment
		this.state = { foo: null };
		// WARN: this is *only* way how to update state
		this.setState( { foo: asyncFetchFoo() } );
	}		
}
```

> *WARNING:* `render` function is called multiple times - avoid putting performance heavy operations.

> Can be used with member variables like so:
```js
class App extends React.Component {
	state = { foo: null };
}
```

## Component Lifecycle
```yml
       constructor
            |
          render
            |
* content visible on the screen *
            |
    componentDidMount
            |
   * waits for updates *	
            |
    componentDidUpdate
            |
   * waits for destroy *
            |
   componentWillUnmount
```

## Event Handlers
- `onClick`
- `onChange`
- `onSubmit`
- [more here](https://reactjs.org/docs/events.html#supported-event)

```js
onInputChange(event) {
	console.log(event.target.value);
}
```

```js
<input type="text" onChange={this.onInputChange} />
```
> *WARNING*: function reference, not call.

> `onInputChange` is a function name by convention: on\<El\>\<Event\>. 

## Controlled vs Uncontrolled elements
> Example above is uncontrolled element (state is managed by DOM itself). 

> Controlled element is element where its state is managed by react `state`.
```js
state = { term: '' }; 

<input type="text" value={ this.state.term } onChange={ (e) => this.setState({ term: e.target.value }) } />
```

> Controlled element is preferable because we don't have to touch DOM in order to get state.

## Invoking callback in children component
```js
class App extends React.Component {
	render() {
		<SearchBar onSubmit={ this.foo } />
	}
}
```

```js
class SearchBar extends React.Component {
	render() {
		<form onSubmit={ () => this.props.onSubmit(this.state.term) }>
	}	
}
```

## Unique id of el in list
> React try to render only new elements with missing id (~hashCode/equals contract).
```js
const images = images.map(img => {
	return <img key=${img.id} src=${img.url} />;
});
```
> *WARNING*: `key` must be on the returning root element.

## React `ref`
> Give access to a single DOM element.

```js
class ImageCard extends React.Component {
	constructor(props) {
		super(props);
		this.imageRef = React.createRef();
	}

	render() { return <img ref={this.imageRef} /> }
}
```

## Portals
> React Component attached to the specific DOM element. 

```js
const Modal = props => {
	return ReactDOM.createPortal(<div>Foo</div>, document.querySelector('#modal'));
}
```

## Fragments
> Group a list of children without adding extra nodes to the DOM.
```js
class Columns extends React.Component {
  render() {
    return (
      <>
        <td>Hello</td>
        <td>World</td>
      </>
    );
  }
}
```

## Context
> Props System: Passing data to a **direct** child component.

> Context System: Passing data to **any** child component. 

```yml
Source: Default / Provider

      |        |
      | Context|
     \| Object |/
      \        / 
       \      /
        \    /
         \  /
          \/

Target: `this.context` / Consumer

```
### Default
1. Create Context
```js
import React from 'react';
export default React.createContext('english');
```
2. Define Context type
```js
class Button extends React.Component {
	static contextType = LanguageContext;
}
```
> **WARN**: `contextType` is mandatory name. 

3. Get context using `this.context`
```js
render() { console.log(this.context); }
```

### Provider
```js
<LanguageContext.Provider value={this.state.language}>
	<UserCreate />
</LanguageContext.Provider>
```

### Consumer
```js
render() {
	return (
		<LanguageContext.Consumer>
			{(value) => value}
		</LanguageContext.Consumer>
	);
}
```

### `this.context` vs `Consumer`
> `this.context`: Component can take data only from **single** context object.

> `Consumer`: Component can take data from **multiple** context objects. 

## Hooks
> Let you use state and other React features without writing a class.

> Hook methods:

- `useState`
- `useEffect`
- `useContext`
- `useReducer`
- `useCallback`
- `useMemo`
- `useRef`
- `useImparativeHandle`
- `useLayoutEffect`
- `useDebugValue` 

### `useState`
```js
const [ currentValue, setCurrentValue ] = useState( inititalValue )
//          ||               ||                          ||       
//      this.state     this.setState           state = { foo: null }
```

### `useEffect`
> Combination of `componentDidMount` and `componentDidUpdate`.

```js
useEffect( () => {} , [ 'foo' ]
//          effect    previous value
```

> `effect` is called **only** when previous value is different from current value.


## Appendix
### CSS importing
```js
import './Foo.css'
```

### CSS and root div
> Use `className` of root `div` (container) same as react component (e.g. `SeasonDisplay` react component has root `<div className={'season-display'}>...</div>`. 

### CSS for center text
```css
.foo {
	display: flex;
	justify-content: center;
	align-items: center;
	height: 100vh;
}
```

### `render` and `return`
> Best practice is to have only one return statement in our `render` function.

### Project hiararchy
> Best practice is to to have all components in `/src/components` folder. 

### `import` position
> By conventions we place imports of third party libs above the ours.

### `unsplash.js`
```js
import axios from 

export default axios.create({baseUrl: 'foo.com'});
```

```js
import unsplash from '../api/unsplash'
```

### `index.js`
> Can be then imported only as e.g. `import {foo} from '../Actions'`

### Routing - `extractedPath.contains(path)`
> Router takes the path after port and runs `contains` on router path.

> e.g. extracted `path="/page"` matches `path="/"` **AND** `path="/page"`.

> `exact` means don't apply rule above and show **only** when path exactly matches.

### Hooks return
> Best practice is to return array from separeted hook method.

```js
const [lat, errorMessage] = useLocation();
```

## Sources
[1]: [React Documentation](https://reactjs.org/docs/getting-started.html)
[2]: [MDN Import documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)
[3]: [BabelJS](https://babeljs.io)
[4]: [Axios](https://github.com/axios/axios)
[5]: [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
[6]: [React Fragments](https://reactjs.org/docs/fragments.html)
[7]: [React Hooks](https://reactjs.org/docs/hooks-reference.html)
