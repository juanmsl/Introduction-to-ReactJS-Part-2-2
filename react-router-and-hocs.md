# React Router and HOC's

## React Router

React Router keeps your UI in sync with the URL. It has a simple API with powerful features like lazy code loading, dynamic route matching, and location transition handling built right in. Make the URL your first thought, not an after-thought.

Now our principal app return a **`Router`** component, that will wrap all other routes to manage our app. And each **`route`** will match a path and render an specified component, when the url path matched the path.

So in our main component `src`**`/`**`modules`**`/`**`index.js`

{% tabs %}
{% tab title="src/modules/index.js" %}
```javascript
import React from 'react';
import {BrowserRouter, Switch, Route, Redirect} from "react-router-dom";
import Home from './home';
import Category from './category';
import Search from './search';


class NewsFeedApp extends React.Component {
  render() {
    return (
      <BrowserRouter>
        <Switch>
          <Route exact path="/" component={Home}/>
          <Route exact path="/category/:categoryName" component={Category}/>
          <Route exact path="/search/:searchValue" component={Search}/>
          <Redirect to="/" />
        </Switch>
      </BrowserRouter>
    );
  }
}

export default NewsFeedApp;
```
{% endtab %}
{% endtabs %}

### Routes

So we are defining 3 routes for our app.

#### /

That will render our Home component and show us the news of the current date

#### /category/:categoryName

That will render the news of each of the 5 categories, and the category will be received as parameter, for example **`/category/politics`** in this case **`politics`** will be the value of **`categoryName`** parameter.

#### /search/:searchValue

That will render our search, and received the searchValue as parameter.

### Switch component

In addition to the **`routes`**, we are using the **`switch`** component, that allows us, render only one route between the routes. You can compare it with the **`switch/case`** of any programing language, and the **`redirect`** as default route if any route match with the current path, it will redirect to the path **`"/"`**.

If you want to test it, change **`lines 13-15`** to the code below, and that your app, changing the urls, and try it changing the parameter values.

{% embed url="http://localhost:3000/" %}

{% embed url="http://localhost:3000/category/juan" %}

{% embed url="http://localhost:3000/search/react" %}

```javascript
...
    <Route exact path="/" component={() => <p>Home</p>} />
    <Route exact path="/category/:categoryName" component={(props) => <p>Category {props.match.params.categoryName}</p>} />
    <Route exact path="/search/:searchValue" component={(props) => <p>Search {props.match.params.searchValue}</p>} />
...
```

{% hint style="warning" %}
Remember restore the lines you tested to te original values before continue.
{% endhint %}

### Link

Later to prevent reload all or when we change the url, we wont use **`<a></a>`** tag, we are going to use the **`Link`** component, that render the **`<a></a>`** tag, but handle the change or the app url, without reload the page in any click.

{% tabs %}
{% tab title="Example" %}
```javascript
...
  <Link to="/category/politics">Politics</Link>
...
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
To understand all stuff that React Router includes, [read his documentation](https://reacttraining.com/react-router/web/guides/quick-start).

And if you want to see more examples using React routes, go [here](https://reacttraining.com/react-router/web/example/url-params).
{% endhint %}

## HOC's \(Higher Order Components\)

When you want to share behaviour between components, as control your form inputs, or request an authorization token, or share a basic layout, you can use HOC's.

HOC's \(Higher Order Components\), are functions that receive any component, and create another class as a wrapper of the component, the new class contains the behaviour or layout you want to add to the component, and everything you need its to pass them as a props to the component.

```jsx
import React from 'react';

const withCounterHOC = (Component) => {
    class WrapComponent extends React.Component {
        constructor(props) {
            super(props);
            this.state = {
                counter: 0
            };
        }
        
        increment = () => {
            this.setState((prevState) => {
                return {
                    counter: prevState.counter + 1
                };
            });
        };
        
        decrement = () => {
            this.setState((prevState) => {
                return {
                    counter: prevState.counter - 1
                };
            });
        };
    
        render() {
            return <Component
                {...this.props}
                {...this.state}
                incrementCount={this.increment}
                decrementCount={this.decrement}
            />
        }
    }
    
    return WrapComponent;
};

export default withCounterHOC;
```

As you can see the example above, the **HOC** **`withCounterHOC`** is a function, that receive a **`Component`**, and return another component \(class\) **`WrapComponent`**.

This **HOC**, pass as prop to the **`Component`**, the **`counter`** state, the **`incrementCount`** and **`decrementCount`** functions.

And as you can see, we pass the original **`props`** to the **`Component`** in **`line 30`**, if the **`Component`** receive another **`props`**, we are resending to it.

Now lets define a component that use the HOC **`withCounterHOC`**.

```jsx
import React from 'react';
import withCounterHOC from './withCounterHOC';

class CategoryCounter extends React.Component {
    render() {
        const {
            categoryName,
            counter,
            incrementCount,
            decrementCount
        } = this.props;
    
        return (
            <section>
                <h1>{categoryName}</h1>
                <p><strong>Current count:</strong> {counter}</p>
                <button onClick={incrementCount}>Add another one</button>
                <button onClick={decrementCount}>Take one</button>
            </section>
        );
    }
}

export default withCounterHOC(CategoryCounter);
```

Now we create a component called CategoryCounter, and we want that component have the counter functionality, so instead of create the functionality in the component, we only use our HOC.

As a **HOC** is a function that receive one **`Component`** and returned **`another component`** that wrap our initial component, we just need to return the wrapped class that is returned by the HOC. As you can see it in the **`line 24`**, we send our component **`CategoryCounter`** to the **HOC** **`withCounterHOC`**, and then we **return as default the Class** that the **HOC** creates.

Now we can just use our component

```jsx
import React from 'react';
import CategoryCounter from './categoryCounter';

class App extends React.Component {
    render() {
        return (
            <section>
                <CategoryCounter categoryName="Books" />
                <CategoryCounter categoryName="Balls" />
                <CategoryCounter categoryName="Movies" />
            </section>
        );
    }
}

export default App;
```

As you can see we only need to pass the **`categoryName`** as props,  because the other props are passed by the **HOC** when the component **`CategoryCounter`** is exported.

If you want to create another component with the counter functionality, you only need to use the **HOC**, and receive the **`counter`** and the **`functions that manipulates the counter`** as props.

{% hint style="info" %}
Read more about HOC's [here](https://en.reactjs.org/docs/higher-order-components.html).
{% endhint %}

