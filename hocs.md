# HOC's

### ContainerHOC

This **HOC** is to share between each view \(home, categories and search\) the layout of **header** and **navbar**.

{% code title="src/hocs/container/index.js" %}
```jsx
import React from 'react';
import {Header, Navbar} from "components";
import {Constants} from "common";

const ContainerHOC = (Children) => (
  class Container extends React.Component {
    constructor(props) {
      super(props);
      this.state = {
        navbarItems: [
          {href: "/", text: "Home"},
          ...Object.keys(Constants.categories).map((category) => ({
            href: `/category/${category}`, text: Constants.categories[category].label
          }))
        ]
      };
    }

    render() {
      const {navbarItems} = this.state;

      return (
        <main>
          <Header title="News feed"/>
          <Navbar items={navbarItems}/>
          <Children {...this.props}/>
        </main>
      );
    }
  }
);

export default ContainerHOC;
```
{% endcode %}

### RenderCardHOC

This **HOC** creates the logic about how each view needs to render a NewsCard, so the method **`renderCards`** is the prop we send to the component that use this HOC.

{% code title="src/hocs/renderCard/index.js" %}
```jsx
import React from 'react';
import {Card} from 'components';

const RenderCardHOC = (Container) => (
  class RenderCard extends React.Component {
    renderCards = (cards) => (
      cards.map((card, i) => (
        <Card key={i} {...card} />
      ))
    );

    render() {
      return <Container
        renderCards={this.renderCards}
        {...this.props}
      />
    }
  }
);

export default RenderCardHOC;
```
{% endcode %}

