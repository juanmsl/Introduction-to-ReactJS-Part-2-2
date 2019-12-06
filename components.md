# Components

## Form

As we see in the design of the app, we need an Input to handle the search parameter, but as an intermediate workshop of React, lets create a modular component to handle our forms.

### Input component

The input component its quite simple, we only receive as **props** the **`type`** and **`defaultValue`** of the input, then in the state we need to handle his value when **`onChange`** method is triggered. At last just pass the **`props`** and **`state`** to the input field.

{% code title="src/components/form/input/index.js" %}
```jsx
import React from 'react';

class Input extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      name: props.name,
      value: props.value
    };
  }

  static defaultProps = {
    type: "text",
    value: ""
  };

  handleChange = (e) => {
    e.preventDefault();
    const {value} = e.target;
    const nextState = {...this.state, value};
    this.setState(nextState);
    this.props.onChange(nextState);
  };

  render() {
    return (
      <input
        {...this.props}
        {...this.state}
        onChange={this.handleChange}
      />
    );
  }
}

export default Input;
```
{% endcode %}

### Form component

Now the **Form component** will handle the state of each **Form field** it will receive as child, for that reason, I use the methods of **React API** **`React.Children`** and  **`React.cloneElement`** to add custom props for each child.

{% hint style="info" %}
If you want to know more about the React API, and their methods, [follow his documentation](https://en.reactjs.org/docs/react-api.html).
{% endhint %}

{% code title="src/components/form/index.js" %}
```jsx
import React from 'react';

export default class Form extends React.Component {
  constructor(props) {
    super(props);
    this.state = {};
  }

  static defaultProps = {
    onSubmit: () => {
      console.log("Nothing it's happening with the form state, define onSubmit prop");
    }
  };

  handleChange = (state) => {
    this.setState({
      [state.name]: state.value
    })
  };

  handleSubmit = (e) => {
    e.preventDefault();
    this.props.onSubmit(this.state);
  };

  render() {
    const {children, ...props} = this.props;
    return (
      <form
        {...props}
        onSubmit={this.handleSubmit}
      >
        {React.Children.map(children, (children) => (
          React.cloneElement(children, {
            onChange: this.handleChange
          })
        ))}
      </form>
    );
  }
}


export {default as Input} from './input';
```
{% endcode %}

## Main components

### Header

Now the **Header component** needs to handle the **`state`** of the search input, so we use the **`Form component`** with an **`Input component`** as a child. Then when the form is **submitted** we push the **search value** to the url path using **React Router HOC** **`withRouter`**.

We can use 3 props that **React Router** have to handle the routing of our app, and we can have it in our component using the **HOC** **`withRouter`** as you can see it on **`line 22`** of the code below.

{% hint style="info" %}
It pass 3 props, location, match, history. To know more about it see [React Router Documentation about withRouter HOC](https://reacttraining.com/react-router/core/api/withRouter).
{% endhint %}

{% code title="src/components/header/index.js" %}
```jsx
import React from 'react';
import {Form, Input} from "components";
import {withRouter} from "react-router-dom";

const Header = ({title, history}) => {

  const search = (state) => {
    const searchValue = state["searchValue"];
    history.push(`/search/${searchValue}`);
  };

  return (
    <header className="header">
      <h1 className="header__title">{title}</h1>
      <Form onSubmit={search} className="header__form">
        <Input type="search" autoComplete="off" name="searchValue" className="input"/>
      </Form>
    </header>
  );
};

export default withRouter(Header);
```
{% endcode %}

### Navbar

The **navbar component** just will receive the items as a **prop**, where each item is an object with **`href`** and **`text`** of the link.

And at last use the **HOC** **`withRouter`**, to receive the **`location`** prop, and know the current **`pathname`** of our app.

{% code title="src/components/navbar/index.js" %}
```jsx
import React from 'react';
import {Link, withRouter} from "react-router-dom";

const Navbar = ({items, location}) => (
  <nav className="navbar">
    {items.map((item, i) => (
      <Link
        key={i}
        to={item.href}
        className={`navbar__item ${item.href === location.pathname ? "selected" : ""}`}
      >
        {item.text}
      </Link>
    ))}
  </nav>
);

export default withRouter(Navbar);
```
{% endcode %}

## News cards

### LoadImage

This component is to show an image while the original image its loading. Only use the event **`onLoad`** over the **`img`**.

{% code title="src/components/loadImage/index.js" %}
```jsx
import React from 'react';

class LoadImage extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      loaded: false,
      loader: "https://owips.com/sites/default/files/clipart/unlimited-clipart/148150/unlimited-clipart-infinity-ring-148150-9298874.gif"
    };
  }

  static defaultProps = {
    alt: ""
  };

  handleLoaded = (e) => {
    this.setState({loaded: true})
  };

  render() {
    const {src, alt, ...props} = this.props;
    const {loaded, loader} = this.state;
    return (
      <img
        {...props}
        alt={alt}
        src={loaded ? src : loader}
        onLoad={this.handleLoaded}
      />
    );
  }
}

export default LoadImage;
```
{% endcode %}

### Card

At last the Card component is to show each of the news.

{% code title="src/components/card/index.js" %}
```jsx
import React from 'react';
import moment from "moment";
import LazyLoad from 'react-lazyload';
import {LoadImage} from "components";

class Card extends React.Component {
  render() {
    const {
      news_id,
      url,
      title,
      date,
      img_url,
      category,
      source_name
    } = this.props;

    return (
      <LazyLoad once key={news_id} placeholder={<section>Loading...</section>}>
        <a href={url} target="_blank" rel="noopener noreferrer" className="card">
          <LoadImage src={img_url} className="card__image" title={title} />
          <section>
            <h3 className="card__title" title={title}>{title}</h3>
            <h5>{category}</h5>
          </section>
          <h6>{moment(date * 1000).format("LLLL")} | {source_name}</h6>
        </a>
      </LazyLoad>
    );
  }
}

export default Card;
```
{% endcode %}

