Here I have a simple application that renders the div Hello World to the dom using React and React Dom.
```
import * as React from 'react';
import * as ReactDOM from 'react-dom';

ReactDOM.render(
  <div>Hello world</div>,
  document.getElementById('root')
);
```

We can easily move this div into a stateful component called <App/> by extending from React.Component and returning the div from the render function

```
import * as React from 'react';
import * as ReactDOM from 'react-dom';

class App extends React.Component<{},{}> {
  render() {
    return (
      <div>Hello world</div>
    )
  }
}

ReactDOM.render(
  <App/>,
  document.getElementById('root')
);
```

Of course one big advantage of components is that you get to use props to change the component behaviour. React.Component takes two generic arguments. The First one is the prop. 

- We can go ahead and add a prop `message` of type string. 
- We can use this prop in our render method. 

And you can see TypeScript already complaining on the misusage of the component, it is saying that the component expects a prop message to be passed in, so lets go head and pass it in 

```
import * as React from 'react';
import * as ReactDOM from 'react-dom';

class App extends React.Component<{
  message: string,
}, {}> {
  render() {
    return (
      <div>{this.props.message}</div>
    )
  }
}

ReactDOM.render(
  <App message="Hello world prop" />,
  document.getElementById('root')
);
```
And you can see that now the div contents are driven by prop. 

Components that extend from `React.Component` are called stateful cause they can have their own internal state. 
- The second generic that React.Component takes is actually the type of this State. - Lets go ahead and setup our state have a count of type number. 
- We can initialize the state in our constructor. 
- When adding a constructor to a react component, you get passed the initial props which you simply pass to the super React.Component class. 
- Now we can setup the initial state, which thanks to our generic setup is rich with autocomplete.
- Finally we can use this state in other places like the render method.

```
import * as React from 'react';
import * as ReactDOM from 'react-dom';

class App extends React.Component<{
  message: string,
}, {
    count: number,
  }> {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    }
  }
  render() {
    return (
      <div>{this.props.message} {this.state.count}</div>
    )
  }
}

ReactDOM.render(
  <App message="Hello world prop" />,
  document.getElementById('root')
);
```

The key reason for having local state in a component is ofcourse that you get to manage it inside the component, for example you can add an increment function that uses react.component's setState to increment the count member of the state 

```
import * as React from 'react';
import * as ReactDOM from 'react-dom';

class App extends React.Component<{
  message: string,
}, {
    count: number,
  }> {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    }
  }
  render() {
    return (
      <div>{this.props.message} {this.state.count}</div>
    )
  }
  increment = () => {
    this.setState({
      count: this.state.count + 1
    })
  }
}

ReactDOM.render(
  <App message="Hello world prop" />,
  document.getElementById('root')
);
```

We can then call this function whenever the root div is clicked. 

```
import * as React from 'react';
import * as ReactDOM from 'react-dom';

class App extends React.Component<{
  message: string,
}, {
    count: number,
  }> {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    }
  }
  render() {
    return (
      <div onClick={this.increment}>
        {this.props.message} {this.state.count}</div>
    )
  }
  increment = () => {
    this.setState({
      count: this.state.count + 1
    })
  }
}

ReactDOM.render(
  <App message="Hello world prop" />,
  document.getElementById('root')
);
```

Now if we go ahead and click the div you can see that the state changes correctly causing the component to re-render with the new state.

One final thing of note is that it is conventional to mark all state members as optional. This way setState can be called with just the members you want changed.