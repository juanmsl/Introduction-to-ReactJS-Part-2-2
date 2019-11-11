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



