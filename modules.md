# Modules

{% code title="src/modules/home/index.js" %}
```jsx
import React from 'react';
import {withRouter} from "react-router-dom";
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

export default RenderCardHOC(ContainerHOC(withRouter(Home)));
```
{% endcode %}

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
    console.log(searchValue);
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



