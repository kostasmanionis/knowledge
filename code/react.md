### React Native

- Use primitives that are cross compatible between the web and react native.

## React

[Compound components](https://www.youtube.com/watch?v=hEGg-3pIHlE) - You know it's a good abstraction when you can easily implement the old abstraction with it.

[Animating with react](https://www.youtube.com/watch?v=W5AdUcJDHo0)

[Coordinating async React with non-React views](https://gist.github.com/acdlite/f31becd03e2f5feb9b4b22267a58bc1f)

#### Beware of context rerenders

The new context API’s `<Context.Provider>` components is a bit smarter about re-rendering than your average component. In fact, there is only a single scenario in which it will re-render its children:

`<Context.Provider>` will only re-render if its children prop does not share reference equality with its previous children prop.

Notably, `<Context.Provider>` will not re-render if its value changes while its children stay the same. In this case, only the associated `<Context.Consumer>` components will re-render. This makes it possible to update a context’s consumers without requiring that the entire app be re-rendered.

Even this will rerender

```jsx
<NavigationContext.Provider value={this.state}>
  <Log name="NavigationProvider" />
  <AppLayout>
    <Log name="AppLayout" />
    <Route href="/">
      <Log name="home Route" />
      <h1>Welcome to Frontend Armory!</h1>
    </Route>
    <Route href="/browse/">
      <Log name="browse Route" />
      <h1>Browse courses and guides</h1>
    </Route>
  </AppLayout>
</NavigationContext.Provider>
```

because it will always recreacte the elements

```javascript
React.createElement(
  NavigationContext.Provider,
  { value: this.state },
  React.createElement(Log, { name: "NavigationProvider" }),
  React.createElement(
    AppLayout,
    null,
    React.createElement(Log, { name: "AppLayout" }),
    React.createElement(
      Route,
      { href: "/" },
      React.createElement(Log, { name: "home Route" }),
      React.createElement("h1", null, "Welcome to Frontend Armory!")
    ),
    React.createElement(
      Route,
      { href: "/browse/" },
      React.createElement(Log, { name: "browse Route" }),
      React.createElement("h1", null, "Browse courses and guides")
    )
  )
);
```

But if you move it in separate component like this, they won't

```javascript
export class NavigationProvider extends React.Component {
  constructor(props) {
    super(props);

    // Store the `navigation` object in component state
    this.state = {
      pathname: window.location.pathname,
      navigate: this.navigate
    };

    // Handle the user clicking the `back` and `forward` buttons
    window.onpopstate = () => {
      this.setState({ pathname: window.location.pathname });
    };
  }

  render() {
    return (
      <NavigationContext.Provider value={this.state}>
        {this.props.children}
      </NavigationContext.Provider>
    );
  }

  // The navigation's `navigate` method updates `navigation` object, and uses
  // the browser's `pushState` method to change the window's URL.
  navigate = pathname => {
    this.setState({ pathname });

    // Update the URL within the browser's history
    window.history.pushState(null, null, pathname);
  };
}
```

[State Reducer Pattern with React Hooks](https://kentcdodds.com/blog/the-state-reducer-pattern-with-react-hooks)

```jsx
function useToggle({ reducer = (s, a) => a.changes } = {}) {
  const [{ on }, dispatch] = React.useReducer(
    (state, action) => {
      const changes = toggleReducer(state, action);
      return reducer(state, { ...action, changes });
    },
    { on: false }
  );
  const toggle = () => dispatch({ type: useToggle.types.toggle });
  const setOn = () => dispatch({ type: useToggle.types.on });
  const setOff = () => dispatch({ type: useToggle.types.off });
  return { on, toggle, setOn, setOff };
}
```

[Dancing between state and effects - a real-world use case](https://github.com/facebook/react/issues/15240)
