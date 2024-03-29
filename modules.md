# Modules

Now that we have created the components and HOC's, lets create our app, the 3 views \(Home, Search and Category\) are very similar, they fetch the data from the API, and display the news in the page. The difference is which endpoint will request, and the parameters that view receives.

### Home page

The **home page component** fetch the data the first time \(each time\) component its **mounted**, I use **`moment`** library to get the **current date**, and use the method that we create in **API** package **`API.getLatest`**to fetch the data.

Then we only handle the state of the component that is the **news list**, and render it using the **HOC** **`RenderCardHOC`**, and using the **HOC** **`ContainerHOC`** to integrate the layout of **`Header`** and **`Navbar`** with it.

And I use the package **`nprogress`** to show a load bar in the top of the page.

{% code title="src/modules/home/index.js" %}
```jsx
import React from 'react';
import API from "api";
import NProgress from "nprogress";
import moment from "moment";
import {ContainerHOC, RenderCardHOC} from "hocs";


class Home extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      data: []
    };
  }

  componentDidMount() {
    const today = moment().format("YYYY-MM-DD");
    NProgress.start();
    NProgress.inc();
    API.getLatest(today, (response) => {
      NProgress.done();
      this.setState({data: response.data});
    });
  }

  render() {
    const {data} = this.state;
    const {renderCards} = this.props;

    return (
      <main className="card__container">
        {renderCards(data)}
      </main>
    );
  }
}

export default RenderCardHOC(ContainerHOC(Home));
```
{% endcode %}

### Search page

Now for the **Search component** I use he **HOC** **`withRouter`** to extract the param in the **`pathname`** with the prop **`match`**.

In this component I use the method that we create **`API.getSearch`**, and it should fetch the data when the component its **mounted** and when the component **updated** in the param in the pathname.

{% code title="src/modules/search/index.js" %}
```jsx
import React from 'react';
import {withRouter} from "react-router-dom";
import {ContainerHOC, RenderCardHOC} from "hocs";
import NProgress from "nprogress";
import API from "api";


class Search extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      data: []
    };
  }

  getData = (searchValue) => {
    NProgress.start();
    NProgress.inc();
    API.getSearch(searchValue, (response) => {
      NProgress.done();
      this.setState({data: response.data});
      console.log(response);
    });
  };

  componentDidMount() {
    const {searchValue} = this.props.match.params;
    this.getData(searchValue);
  }


  componentDidUpdate(prevProps, prevState, snapshot) {
    const {searchValue} = this.props.match.params;
    const {searchValue: prevSearchValue} = prevProps.match.params;
    if (searchValue !== prevSearchValue) {
      this.setState({data: []});
      this.getData(searchValue);
    }
  }

  render() {
    const {data} = this.state;
    const {renderCards} = this.props;

    return (
      <main className="card__container">
        {renderCards(data)}
      </main>
    );
  }
}

export default RenderCardHOC(ContainerHOC(withRouter(Search)));
```
{% endcode %}

### Category Page

Al last the **Category component** receive as param the **category name**, but we need the **category id** to fetch the data, so we use the **constants** that we create, to know the id.

{% code title="src/modules/category/index.js" %}
```jsx
import React from 'react';
import {withRouter} from "react-router-dom";
import NProgress from "nprogress";
import API from "api";
import {Constants} from "common";
import {ContainerHOC, RenderCardHOC} from "hocs";


class Category extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      data: []
    };
  }

  getData = (categoryId) => {
    NProgress.start();
    NProgress.inc();
    API.getCategory(categoryId, (response) => {
      NProgress.done();
      this.setState({data: response.data});
    });
  };

  componentDidMount() {
    const {categoryName} = this.props.match.params;
    const categoryId = Constants.categories[categoryName].id;
    this.getData(categoryId);
  }

  componentDidUpdate(prevProps, prevState, snapshot) {
    const {categoryName} = this.props.match.params;
    const {categoryName: prevCategoryName} = prevProps.match.params;
    if (categoryName !== prevCategoryName) {
      const categoryId = Constants.categories[categoryName].id;
      this.setState({data: []});
      this.getData(categoryId);
    }
  }

  render() {
    const {renderCards} = this.props;
    const {data} = this.state;

    return (
      <main className="card__container">
        {renderCards(data)}
      </main>
    );
  }
}

export default RenderCardHOC(ContainerHOC(withRouter(Category)));
```
{% endcode %}



