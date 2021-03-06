[TOC]



# React

## JSX

It looks like HTML and can be placed in JS code. Determines the content of our React app just like normal HTML.

##  React vs ReactDOM

React is split into two separate libraries.

`React` knows what a component is and how to make components work together

`ReactDOM` knows how to take a component and make it show up in the DOM.

`create-react-app` is the scaffold in React



## What `webpack` does?

![Diagram-006 - Webpack](imgs/webpack.png)



## What is React component?

![react-component](imgs/react-component.png)

**All React components must act like pure fuctions with respect to their props.**

> This means that whatever happenning in the function will not modify the original value of ***props*** object.

Of course, application UIs are dynamic and change over time. That's the reason why React introduces a new concept of "state". ***State*** allows React components to change their output over time in response to user actions, network responses, and anything else, without violating this rule.



## How `React` works behind the scenes

![overview of React](imgs/overview-of-React.png)

>Very similar to Vue, React only allows to render one closing tag.
>
>From my point of view, this aims to promise the component tree structure so that the diff algorithm works perfectly.

## Why single `bundle.js` matters

![multiscript](imgs/multiscript.png)



## JSX return HTML?

Actually, the returned "html" will be parsed using API `React.createElement()`;



## Functional Components vs Class Components?

Functional components just use in the case where you just needs to create a "mixin" as Vue do.

--- update at 4/4/2019----

Functional components do not support lifecycle functions and self-state management. However, these useful functions have corresponding `hooks` as polyfills. 

## Props

`React` is known as `functional programming` ; as the way `Vue` does, you can pass data from parent component to children components through properties. 

> Note that the concept of `slots` in `Vue` behaves the same as other data type in `React` `props`; however, you should use fixed keyword `props.children` rather than the customized name.

Note: In React, one direction of data flow, which means you can only pass information from parent component to child component. To reference the data stored in parent, say some specific state, you can use `closure` or `currying` by passing a function binded with `this` or just `arrow functions` introduced from ES6.



## Lifecycle - Async behind the scenes

This issue comes when we need to use asynchronous apis. 

![timeline-async](imgs/timeline-async.png)



### Commonly used hooks with calling timings

![commonlyUsedLifeCycleHooks](imgs/commonlyUsedLifeCycleHooks.png)



-----update 04/04/2019----

Here I find a practical usecase for `shouldComponentUpdate`. Changing one of item in the list while it doesn't cause other items to re-render, which profoundly improve the performance of your app.

[Here is the link of article in Medium](https://medium.freecodecamp.org/react-shouldcomponentupdate-demystified-c5d323099ef6)

[Here is the instance code on github](https://github.com/jpdelima/react-should-component-update-demystified/blob/master/src/App.jsx)

I am going to integrate this into my visual project.

## What is `state` in React?

- `State` is a JS object that contains data relevant to a component.

- Updating `state` on a component causes the component to (almost) instantly rerender
- `State` must be initialized when a component is created *(`constructor` function)*
- `State` can **only** be updated using the function `setState`



## Lifecycle Hooks

![React-lifecycle-hooks](imgs/React-lifecycle-hooks.png)



## Tips of combination of `destructuring` and `ternary`

```javascript
const seasonConfig = {
  summer: {
    text: 'Lets hit the beach!',
    iconName: 'sun'
  },
  winter:{
    text: 'Burr it is chilly',
    iconName: 'snowflake'
  }
};

const {text, iconName} = seasonConfig[season];
```

You can use built-in object literals to lookup corresponding configuration!



## Tips of Inline If with Logical && Operator

```react
{ unreadMessage > 0 &&	
    <h2>
      You have {unreadMessage.length} unread messages.
    </h2>
}
```

Use this pattern rather than use `if` !!! Concise and precise. 

The key point here is to recognize that {} can be used in JSX !!

```js
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
  	<div>
    	{isLoggedIn? (
       		<LogoutButton onClick={this.handleLogoutClick} />
    		) : ( 
    			<LoginButton onClick = {this.handleLoginClick} />
  		)}
    </div>
  )
}
```



## default props

```javascript
Spinner.defaultProps = {
  message: 'Loading...'
};
```



## Higher-Order Components

 **a higher-order component is a function that takes a component and returns a new component.**



## Render Props

The term [“render prop”](https://cdb.reacttraining.com/use-a-render-prop-50de598f11ce) refers to a technique for sharing code between React components using a prop whose value is a function.




## A common mistake - *this*

```javascript
  onFormSubmit (e) {
    e.preventDefault();
    console.log(this.state.term);
  }
```

when above callback is called, the global `window` will throw an error

- To fix this, you can use `bind(this)` after `this.callbackFunc = callbackFunc.bind(this)` in `constructor`.

  Using the `bind(this)` in the constructor is the recommended function in the documentation.

- Or **Arrow function** 

```js
onFormSubmit = (e) => {
  e.preventDefault();
  console.log(this.state.term);
}
```

- Or `closure`

```html
<form onSubmit={(event) => onFormSubmit(event)}></form>
```





## [ Concise ] Axios - refactor your file structure

```js
import axios from 'axios';

//set default configuration for specific URL by axios
export default axios.create({
  baseURL: "https://api.unsplash.com/",
  headers: {
    Authorization: 'pswd'
  }
});

----------------------- another file using this api

import unsplash from '../api/unsplash';

onSearchSubmit = async term => {
  const response =  await unsplash.get("/search/photos", {
    params: { query: term},
  });

  this.setState( { images: response.data.results });
}
```



## `Ref` to reference a DOM

As `Vue` does, to get the latest DOM object, you can reference a element in JSX in the `constructor`.

> Note that `jsx` is not html or DOM. If you need to collaborate with other package to refence a dom, please add `ref` to the element you need to use!

```jsx
  constructor(props) {
    super(props);
    this.videoRef = React.createRef();
  }

 <video ref={this.videoRef}/>
```



## Think in React

Now that you have your component hierarchy, it’s time to implement your app. The easiest way is to build a version that takes your data model and renders the UI but has no interactivity. It’s best to decouple these processes because building a static version requires a lot of typing and no thinking, and adding interactivity requires a lot of thinking and not a lot of typing. We’ll see why.

To build a static version of your app that renders your data model, you’ll want to build components that reuse other components and pass data using *props*. *props* are a way of passing data from parent to child. If you’re familiar with the concept of *state*, **don’t use state at all** to build this static version. State is reserved only for interactivity, that is, data that changes over time. Since this is a static version of the app, you don’t need it.

## Uncategorized Points



- Lists and keys: keys used within arrays should be unique among their siblings. However, they don't need to be globally unique.
- In React, data flow is `one-way`, that is, data passed via `props` from parent component to child components. Thus, if you want to synchronize two child components under the same parent component, you should 'store' the state in the parent component. However, how can we call the `this.setState()` in the parent component if the parameters in `setState` from children? In this case, we adopt the strategy propagating the handler functions as `props` which will be consumed in the specific child component.



# Redux

## What is `Redux`?

State management package - as `Vuex`

![What is redux](imgs/What-is-redux.png)

## Redux Cycle

Learn from an analogy: Insurance Company vs Cycle Terms:

![redux cycle illstration](imgs/redux-cycle-illstration.png)



## Analogy - Insurance Company VS Redux

There are several key concepts and terms in Redux; to make myself fully understand these, let's see an analogy:

![analogy-ins.co](imgs/analogy-ins.co.png)

Every form consists of '*type*' and '*payload*'

![policy-data structure](imgs/policy-data-structure.png)



There is one of instances how the system work behind the scenes:

![analogy-underlying-mechanism](imgs/analogy-underlying-mechanism.png)

>  Note that: in terms of Reducers:
>
> *Overall goal of reducer is to take some existing data some action and then modify and return that existing data based upon the contents of an action*
>
> we prefer: `[...oldList, action.payLoad]` 
>
> than `oldList.push(action.payLoad)`
>
> that means: Never manipulate on the original object (passed by reference)!!!



>  `index.js` is the default file `webpack` would try to import if you just appoint a directory



## Combine React with Redux

>  The demo code for this topic is in the `songs` directory.

![combineReactRedux](imgs/combineReactRedux.png)



![reduxWorkingFlow](imgs/reduxWorkingFlow.png)

It's our responsibility to create `connect` and `Provider`; in terms of other inner stuff, Redux will do us a favor for us.

`connect` functions is passed a `mapStateToProps` function so that the component receives the data or variables stored in `store`

`connect(mapStateToProps, {...actions})(Component)`

The keys you appointed in the `Redux.combineReducers(key: arrowFunctionHandlingLogic)` will be one of keys in `mapStateToProps(state)`; `state.key1 , state.key2`



Here is a convention of directory structure:

![recommandedFileStructure](imgs/recommandedFileStructure.png)



# Redux-thunk

![1553072160757](imgs/1553072160757.png)

`action` must be a plain JavaSript object with `type` property while `async` request will return a `request` object

So, we need `middleware` to help us handle `async`request by relaxing condition that you can return a function in action creators.

![flowChat-of-redux-thunk](imgs/flowChat-of-redux-thunk.png)



## Middleware

![middlewareInRedux](imgs/middlewareInRedux.png)



![newReduxCyclewithMiddleware](imgs/newReduxCyclewithMiddleware.png)

### How `thunk` works behind the scenes

![thunkBehindScences](imgs/thunkBehindScences-1553131455858.png)

## Review: Reducers

### Rules of Reduces

![rulesOfReducers](imgs/rulesOfReducers.png)

> **Misleading**: 
>
> ```javascript
> // piece of code from redux.js     
> hasChanged = hasChanged || nextStateForKey !== previousStateForKey
> ```
>
> <u>Through this line, we can tell that `redux` cannot detect mutations made in reducers. (because the same *reference*), which also explains why we also return a new array rather than orginal array.</u>



## Recommanded operation in Reducers

![DesignComparisons](imgs/DesignComparisons.png)

### Reducers are pure?

This means reducers' responsibility is only one thing: take last `state` and action to determine new `state`.

 ![reducersJob](imgs/reducersJob.png)



> Keep in mind: we are not going to mutate the `state` ever!

### effetive alternatives to mutate state



> It's also overkilled to pass a list of users to a component, then find only pieces of data in the list.
>
> To solve this problem, you can extract pieces of data in `mapStateToProps`.



## How to avoid overfetching ajax request?

### _.memoize (not best)

lodash provides us this high order function to cache the result of the passing in function. 

> In computing, *memoization* or memoisation is an optimization technique used primarily to speed up computer programs by storing the results of expensive function calls and returning the cached result when the same inputs occur again. 
>
> -- *from Wikipedia*

### chain the requests orderly(recommended method)

```js
export const fetchPostsAndUsers = () => async (dispatch, getState) => {
  await dispatch(fetchPosts()); 
  // the following codes will run after you fecth posts successfully
  const userIds = _.uniq(_.map(getState().posts, 'userId'));
 	// the above line gets a unique array of userIds to promise dispatch the fetchUser only once  
  userIds.forEach( id => dispatch(fetchUser(id)));
};
```

# React-Router

## How does it work?

![flowOfRouter](imgs/flowOfRouter.png)

![mechanismOflink](imgs/mechanismOflink.png)

## Tip: Render sidebar twice with different styles

```react
import React from "react";
import { BrowserRouter as Router, Route, Link } from "react-router-dom";

// Each logical "route" has two components, one for
// the sidebar and one for the main area. We want to
// render both of them in different places when the
// path matches the current URL. For instance, in one page, there are two siderbars; one for main navigation; another one for small navigation.

const routes = [
  {
    path: "/",
    exact: true,
    sidebar: () => <div>home!</div>,
    main: () => <h2>Home</h2>
  },
  {
    path: "/bubblegum",
    sidebar: () => <div>bubblegum!</div>,
    main: () => <h2>Bubblegum</h2>
  },
  {
    path: "/shoelaces",
    sidebar: () => <div>shoelaces!</div>,
    main: () => <h2>Shoelaces</h2>
  }
];

function SidebarExample() {
  return (
    <Router>
      <div style={{ display: "flex" }}>
        <div
          style={{
            padding: "10px",
            width: "40%",
            background: "#f0f0f0"
          }}
        >
          <ul style={{ listStyleType: "none", padding: 0 }}>
            <li>
              <Link to="/">Home</Link>
            </li>
            <li>
              <Link to="/bubblegum">Bubblegum</Link>
            </li>
            <li>
              <Link to="/shoelaces">Shoelaces</Link>
            </li>
          </ul>

          {routes.map((route, index) => (
            // You can render a <Route> in as many places
            // as you want in your app. It will render along
            // with any other <Route>s that also match the URL.
            // So, a sidebar or breadcrumbs or anything else
            // that requires you to render multiple things
            // in multiple places at the same URL is nothing
            // more than multiple <Route>s.
            <Route
              key={index}
              path={route.path}
              exact={route.exact}
              component={route.sidebar}
            />
          ))}
        </div>

        <div style={{ flex: 1, padding: "10px" }}>
          {routes.map((route, index) => (
            // Render more <Route>s with the same paths as
            // above, but different components this time.
            <Route
              key={index}
              path={route.path}
              exact={route.exact}
              component={route.main}
            />
          ))}
        </div>
      </div>
    </Router>
  );
}

export default SidebarExample;

```



# Authentication

## What is OAuth?

![OAuth](imgs/OAuth.png)

## What are Scopes?

Grant our right to process or visit your emails or files stored in google drive.

## OAuth for Servers vs OAuth for JS Browser Apps

![TypesOfAuth](imgs/TypesOfAuth.png)

## The underlying process of login with google.

![underlyingFlowOAuth](imgs/underlyingFlowOAuth.png)

## How to use google's api in the React?

`window.gapi` offers you `api` which can manipulate in the browser.

[Document of gapi](**developers.google.com/api-client-library/javascript/reference/referencedocs**)

# Form

There are countless and various senarios where frontend developers need to manipulate inputs from users. In most cases, it’s convenient to <u>have a JavaScript function that handles the submission of the form and has access to the data that the user entered into the form</u>. The standard way to achieve this is with a technique called `controlled component`. The definition of  `controlled compnent` from official React documentation:

> The React component that renders a form also controls what happens in that form on subsequent user input. An input form element whose value is controlled by React in this way is called a “controlled component”

The essence of it is ***the single source of truth***.

> **Single source of truth** (**SSOT**) is the practice of structuring information models and associated [data schema](https://en.wikipedia.org/wiki/Database_schema) such that every data element is mastered (or edited) in only one place. Any possible linkages to this data element are by [reference](https://en.wikipedia.org/wiki/Reference_(computer_science)) only.
>
> via Wikipedia

Personal interpretation:  `onSubmit` form and `value` of the input should be placed and controlled by React component within its `state`.

```react
class Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```



## Handling Multiple Inputs

When handling multiple controlled `input` elements, you can add a `name` attribute to each element and let the handler function choose what to do based on the value of `event.target.name`.

## Formik

Since handling tedious forms is an annoying task, the React team recommends to use Formik. Formik is a small library that helps you with the 3 most annoying parts:

1. Getting values in and out of form state
2. Validation and error messages
3. Handling form submission

### The main logic behind Formik

Formik keeps track of your form's state and then exposes it plus a few reusable methods and event handlers (`handleChange`, `handleBlur`, and `handleSubmit`), which are the most frequently or only used handlers, to your form via `props`. `handleChange` and `handleBlur` work exactly as expected--they use a `name` or `id` attribute to figure out which field to update.

```react
import React from 'react';
import { Formik } from 'formik';

const Basic = () => (
  <div>
    <h1>Anywhere in your app!</h1>
    <Formik
      initialValues={{ email: '', password: '' }}
      validate={values => {
        let errors = {};
        if (!values.email) {
          errors.email = 'Required';
        } else if (
          !/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$/i.test(values.email)
        ) {
          errors.email = 'Invalid email address';
        }
        return errors;
      }}
      onSubmit={(values, { setSubmitting }) => {
        setTimeout(() => {
          alert(JSON.stringify(values, null, 2));
          setSubmitting(false);
        }, 400);
      }}
    >
      {({
        values,
        errors,
        touched,
        handleChange,
        handleBlur,
        handleSubmit,
        isSubmitting,
        /* and other goodies */
      }) => (
        <form onSubmit={handleSubmit}>
          <input
            type="email"
            name="email"
            onChange={handleChange}
            onBlur={handleBlur}
            value={values.email}
          />
          {errors.email && touched.email && errors.email}
          <input
            type="password"
            name="password"
            onChange={handleChange}
            onBlur={handleBlur}
            value={values.password}
          />
          {errors.password && touched.password && errors.password}
          <button type="submit" disabled={isSubmitting}>
            Submit
          </button>
        </form>
      )}
    </Formik>
  </div>
);

export default Basic;
```

To make your life easier, Formik provides some components. Following code is the same effect as the previous one.

```react
// Render Prop
import React from 'react';
import { Formik, Form, Field, ErrorMessage } from 'formik';

const Basic = () => (
  <div>
    <h1>Any place in your app!</h1>
    <Formik
      initialValues={{ email: '', password: '' }}
      validate={values => {
        let errors = {};
        if (!values.email) {
          errors.email = 'Required';
        } else if (
          !/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$/i.test(values.email)
        ) {
          errors.email = 'Invalid email address';
        }
        return errors;
      }}
      onSubmit={(values, { setSubmitting }) => {
        setTimeout(() => {
          alert(JSON.stringify(values, null, 2));
          setSubmitting(false);
        }, 400);
      }}
    >
      {({ isSubmitting }) => (
        <Form>
          <Field type="email" name="email" />
          <ErrorMessage name="email" component="div" />
          <Field type="password" name="password" />
          <ErrorMessage name="password" component="div" />
          <button type="submit" disabled={isSubmitting}>
            Submit
          </button>
        </Form>
      )}
    </Formik>
  </div>
);

export default Basic;
```



## Redux-Form

### What it does behind the scences?

![ReduxFormDoes](imgs/ReduxFormDoes.png)



```jsx
  renderInput(formProps) {
    //this code looks magic but just es6 spread an object of formProps.input
    return <input {...formProps.input} />
  }

=============================================================================
  
 // by doing this, the form is taken over by redux-form which will prevent the default submission for us and provide us corresponding data.

      <form onSubmit={this.props.handleSubmit(this.onSubmit)} className="ui form">
        <Field name="title" component={this.renderInput} label="Enter Title"/>
        <Field name="description" component={this.renderInput} label="Enter Description"/>
        <button className="ui button primary">Submit</button>
      </form>
```



I am a little bit understanding the `single data flow` in React, that is, data only passing from parent to child through `props`. No magic in redux-form, which just acts like a syntax sugar helping you manage data with redux store. The usage is pretty similar to `redux-react`. You need to combine the form as `connect` does by `reduxForm` imported from `redux-form`. Even the syntax is identical. 



### Form Validation 

#### Redux-form Validation Flow

![validateFlow](imgs/validateFlow.png)

There is a pattern for form-control in all frameworks:

You need to "*inject* "the package or "*mount*" the package up to the form.

Once the validation package takes over the form, it will change the default behaviors of the form, such as extra valiadation, sanctuation checking etc.. Finally, it will return a object which either contains all validate key value pairs of data or key-value pairs error messages. 

Also, form management needs to provide us several *convention* values, such as `initialValues`, `formValues` (current you are editting) etc. To know more details, please refer to the document. 

# RESTful API

## Conventions

![RESTfulConventions](imgs/RESTfulConventions.png)

## Key Interpolation Syntax - Object-based approach

`{...state, [action.payload.id]: action.payload}`;

set `action.payload.id` as key and the value is `action.payload`

### `mapKeys` - Userful method for object-based search powerd by lodash.

![lodashMapKeys](imgs/lodashMapKeys.png)



## Redirect gracefully

It is inevitable for us to sometimes redirect user to another link after a sucessful operation.

### Normal `history` working flow

![normalHistory](imgs/normalHistory.png)

In `React`, it can be really painful if you just pass the `history` object across components. And you cannot control over the behavior of broswer `history` api. To handle the case gracefully, here provides a solution: create your own `history.js`



![customizedHistory](imgs/customizedHistory.png)

> `history` was installed automatically by `redux-router`.
>
> You can import it by `import {createBrowserHistory} from 'history';`
>
> if you wanna customize your history object, you need to update your import {`BrowserRouter`} from `redux-router` to plain `Router` (Not `Route` which is responsible for *routing*!)

> Whenever you need to get some data from `this.props`; 
>
> ```js
> const mapStateToProps = (state, ownProps) => {
>   //ownProps refers to the props of the instance provided by redux-react
>   return {stream: state.streams[ownProps.match.params.id]};
> };
> 
> ```



## Component Isolation with React Router

You can never presume that the data in the store will be dependent on another component. That is, you need to check whether the data exists in the store, then handle it.

## Why we use `portal`?



As I mentioned before, `React` is totally **<u>one-way flow</u>**. It is the normal case that a component is deeply attached to the root component, which is definitely a disastar for css styling. You are prone to be trapped by weird css style, no matter how skillful you are on CSS. To solve this issue, we can mount the component directly on `<body>`. In this way, we can overlap the root component much easier.   

![nestingComponent](imgs/nestingComponent.png)



## Why we need React.Fragment?

In JSX, we are supposed to return all contents wrapped by a single wrapper. Sometimes we just want to return multiple tags. To solve this, `React.Fragment` comes, which will not be rendered as any tag in the parsed htlm from jsx.



## `Switch` from redux-route-dom

```jsx
<Router history={history}>
  <div>
    <Header />
    <Switch>
      <Route path="/" exact component={StreamList} />
      <Route path="/streams/new" exact component={StreamCreate} />
      <Route path="/streams/edit/:id" exact component={StreamEdit} />
      <Route path="/streams/delete/:id" exact component={StreamDelete} />
      <Route path="/streams/:id" exact component={StreamShow} />
    </Switch>
  </div>
</Router>
```

In `React`, the path matching process is done by comparing all routes and returns all matching ones. That is the reason why we use `exact` attribute in the <Route>. However, please focus on the route `/streams/new` and `/streams/:id`, they are sharing the same pattern to React. That is, `:id`  just indicates a variable which can be named as a string - "new".  To resolve the route correctly, we introduced `Switch` from `react-route-dom` that will only return one matching result.

> Note that if you notice there are some extra HTML in your component, it might be the result of path matching.



# Context System in React

## What is it ?

[A great thread illustrating Context and Redux](https://blog.bitsrc.io/react-context-api-a-replacement-for-redux-6e20790492b3)

Nesting Components, it is really inconvenient to pass a data from ancestor component to some child component.

![nestingComponent](imgs/nestingComponent-1553851030599.png)

![ContextDiagram](imgs/ContextDiagram.png)

## Props vs Context

![VsProps](imgs/VsProps.png)

## Inject info in and fetch info out

![twoWayInandOut](imgs/twoWayInandOut.png)

## Redux vs Context

![ReduxVsContext](imgs/ReduxVsContext.png)

# Hooks

## Motivation

- It’s hard to reuse stateful logic between components

  Hooks allow you to reuse stateful logic without changing your component hierarchy.

- Complex components become hard to understand

  Hooks let you split one component into smaller functions based on what pieces are related (such as setting up a subscription or fetching data), rather than forcing a split based on lifecycle methods. For example, see how does `setEffect` group related logics from lifecycle functions:

  ```react
  class FriendStatusWithCounter extends React.Component {
    constructor(props) {
      super(props);
      this.state = { count: 0, isOnline: null };
      this.handleStatusChange = this.handleStatusChange.bind(this);
    }
  
    componentDidMount() {
      document.title = `You clicked ${this.state.count} times`;
      ChatAPI.subscribeToFriendStatus(
        this.props.friend.id,
        this.handleStatusChange
      );
    }
  
    componentDidUpdate() {
      document.title = `You clicked ${this.state.count} times`;
    }
  
    componentWillUnmount() {
      ChatAPI.unsubscribeFromFriendStatus(
        this.props.friend.id,
        this.handleStatusChange
      );
    }
  
    handleStatusChange(status) {
      this.setState({
        isOnline: status.isOnline
      });
    }
  ```

  Note how the logic that sets `document.title` is split between `componentDidMount` and `componentDidUpdate`. The subscription logic is also spread between `componentDidMount`and `componentWillUnmount`. And `componentDidMount` contains code for both tasks.

  ```react
  function FriendStatusWithCounter(props) {
    const [count, setCount] = useState(0);
    useEffect(() => {
      document.title = `You clicked ${count} times`;
    });
  
    const [isOnline, setIsOnline] = useState(null);
    useEffect(() => {
      function handleStatusChange(status) {
        setIsOnline(status.isOnline);
      }
  
      ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
      return () => {
        ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
      };
    });
    // ...
  }
  ```

  **Hooks let us split the code based on what it is doing** rather than a lifecycle method name. React will apply *every* effect used by the component, in the order they were specified.

  

- Classes confuse both people and machines

  You have to understand how `this` works in JavaScript, which is very different from how it works in most languages. You have to remember to bind the event handlers. 



## ✌️ Rules of Hooks

Hooks are JavaScript functions, but they impose two additional rules:

- Only call Hooks **at the top level**. Don’t call Hooks inside loops, conditions, or nested functions.
- Only call Hooks **from React function components**. Don’t call Hooks from regular JavaScript functions. (There is just one other valid place to call Hooks — your own custom Hooks).

## Class-Based VS Function-Based 

![shareLogicBetweenCompsHooks](imgs/shareLogicBetweenCompsHooks.png)

> This is a terrible design which will cause endless request!

```jsx

  async componentDidUpdate() {
    const response = await axios.get(
      `https://jsonplaceholder.typicode.com/${this.props.resource}`
    );
    this.setState({ resources: response.data});
  }

=========================modification========================================
  
  async componentDidUpdate(preProps){
    if(preProps.resource !== this.props.resource) {
      const response = await axios.get(
        `https://jsonplaceholder.typicode.com/${this.props.resource}`
        );
      this.setState({ resources: response.data});
    }
  }
```



## Hooks and their respective function

![HooksAndFunctions](imgs/HooksAndFunctions.png)

### useState

![useStateApi](imgs/useStateApi.png)

### useEffect

You’ve likely performed data fetching, subscriptions, or manually changing the DOM from React components before. These operations are called “side effects” (or “effects” for short) because they can affect other components and can’t be done during rendering.

>  Functional Programming: these operations can change the 'values'

In short: `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` in React classes, are unified into a single API.

![useEffectWillbeCalledWhenDataChanged](imgs/useEffectWillbeCalledWhenDataChanged.png)

```javascript
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // Only re-run the effect if count changes

```

don’t forget that React defers running `useEffect` until after the browser has painted, so doing extra work is less of a problem

#### Effects Without Cleanup

Sometimes, we want to **run some additional code after React has updated the DOM.** Network requests, manual DOM mutations, and logging are common examples of effects that don’t require a cleanup. By default, `useEffect` runs both after the first render *and* after every update. 

The `render` method itself shouldn’t cause side effects, while typically we want to perform our effects *after* React has updated the DOM. This is why in React classes, we put side effects into `componentDidMount` and `componentDidUpdate`.

> You might notice that the function passed to `useEffect` is going to be different on every render.
>
> If you really wanna achieve the `componentDidMount`, you can pass an empty array as the second argument in `useEffect`.
>
> **TL;DR**
>
> `useEffect(yourCallback, [])` - will trigger the callback only after the first render.
>
> **Detailed explanation**
>
> `useEffect` runs by default after **every** render of the component (thus causing an effect).
>
> When placing `useEffect` in your component you tell React you want to run the callback as an effect. React will run the effect after rendering and after performing the DOM updates.
>
> If you pass only a callback - the callback will run after each render.
>
> For example when placing `useEffect(() => console.log('hello'), [someVar, someOtherVar])` - the callback will run after the first render and after any render that one of `someVar` or `someOtherVar` are changed.
>
> By passing the second argument an empty array, React will compare after each render the array and will see nothing was changed, thus calling the callback only after the first render.

#### Effects with Cleanup

**we might want to set up a subscription** to some external data source. In that case, it is important to clean up so that we don’t introduce a memory leak.

If your effect returns a function, React will run it when it is time to clean up.

```javascript
  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });
```

### useReducer

If you find it annoying to pass callbacks through every level of a component tree, please combine `useReducer` with `useContext` together. The concept of `reducer` is identical in `Redux`, but more lightweight. The solution is to pass the `dispatch`  as the `context` value and then call it in the child components.

```react
const TodosDispatch = React.createContext(null);

function TodosApp() {
  // Note: `dispatch` won't change between re-renders
  const [todos, dispatch] = useReducer(todosReducer);

  return (
    <TodosDispatch.Provider value={dispatch}>
      <DeepTree todos={todos} />
    </TodosDispatch.Provider>
  );
}
```

Any child in the tree inside `TodosApp` can use the `dispatch` function to pass actions up to `TodosApp`:

```react
function DeepChild(props) {
  // If we want to perform an action, we can get dispatch from context.
  const dispatch = useContext(TodosDispatch);

  function handleClick() {
    dispatch({ type: 'add', text: 'hello' });
  }

  return (
    <button onClick={handleClick}>Add todo</button>
  );
}
```

