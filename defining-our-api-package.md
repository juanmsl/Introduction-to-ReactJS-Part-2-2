# Defining our API package

Now lets define our API package to make requests to an API, and get the information, that we need to show.

The API that we are going to use is [https://api.canillitapp.com/](https://api.canillitapp.com/)

{% hint style="info" %}
See the API documentation [here](https://github.com/Canillitapp/headlines-api).
{% endhint %}

## The API

Basically that API return data about news in different categories, or dates, so we are going to use the following endpoints.

{% api-method method="get" host="https://api.canillitapp.com" path="/latest/:date" %}
{% api-method-summary %}
/latest/:date
{% endapi-method-summary %}

{% api-method-description %}
Returns a list of all the news fetched that day ordered from most recent to oldest.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="date" type="string" required=true %}
Date in format **YYYY-MM-DD**
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
List of news of day requested
{% endapi-method-response-example-description %}

```javascript
[
  {
    "news_id": 132240,
    "url": "http://tn.com.ar/musica/hoy/encontraron-muerto-walter-romero-el-excantante-de-banda-xxi_784972",
    "title": "Encontraron muerto a Walter Romero, el excantante de Banda XXI",
    "date": 1491704606,
    "source_id": 1,
    "img_url": "http://cdn.tn.com.ar/sites/default/files/styles/470x269/public/2017/04/08/1266581830507_f.jpg",
    "source_name": "TN",
    "category": "Espectáculos",
    "reactions": []
  },
  {
    "news_id": 132238,
    "url": "http://www.telam.com.ar/notas/201704/185120-futsal-argentina-brasil-copa-amarica-de-san-juan.html",
    "title": "Argentina y Brasil empataron en la Copa América de San Juan",
    "date": 1491703140,
    "source_id": 10,
    "img_url": "http://www.telam.com.ar/advf/imagenes/2017/04/58e9963caf3f5_400x225.jpg",
    "source_name": "Telam",
    "category": "Deportes",
    "reactions": []
  }
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

**Example** [https://api.canillitapp.com/latest/2019-11-8](https://api.canillitapp.com/latest/2019-11-8)

{% api-method method="get" host="https://api.canillitapp.com" path="/search/:foo" %}
{% api-method-summary %}
/search/:foo
{% endapi-method-summary %}

{% api-method-description %}
Searches all the news that contains `foo` on the title.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="foo" type="string" required=true %}
Parameter to search
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
List of news with foo in the title
{% endapi-method-response-example-description %}

```javascript
[
  {
    "news_id": 1045307,
    "url": "https://tn.com.ar/musica/hoy/foo-fighters-arrancara-trabajar-en-su-nuevo-disco-la-proxima-semana_998808",
    "title": "Foo Fighters arrancará a trabajar en su nuevo disco la próxima semana",
    "date": 1569956164,
    "source_id": 1,
    "img_url": "https://cdn.tn.com.ar/sites/default/files/styles/470x269/public/2019/10/01/foo-fighters.jpg",
    "source_name": "TN",
    "category": "Espectáculos",
    "reactions": []
  },
  {
    "news_id": 1040307,
    "url": "https://tn.com.ar/musica/hoy/foo-fighters-sorprende-al-mundo-con-su-nuevo-ep_997790",
    "title": "Foo Fighters sorprende al mundo con su nuevo EP",
    "date": 1569605173,
    "source_id": 1,
    "img_url": "https://cdn.tn.com.ar/sites/default/files/styles/470x269/public/2019/09/27/063_1176440200.jpg",
    "source_name": "TN",
    "category": "Espectáculos",
    "reactions": []
  }
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

**Example** [https://api.canillitapp.com/search/github](https://api.canillitapp.com/search/github)

{% api-method method="get" host="https://api.canillitapp.com" path="/news/category/:id" %}
{% api-method-summary %}
/news/category/:id
{% endapi-method-summary %}

{% api-method-description %}
Return all news with an specific category
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="id" type="number" required=false %}
**\[1-5\]** the id for each category.  
**1** - Politics  
**2** - International  
**3** - Technology  
**4** - Shows  
**5** - Sports
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
The list news related to an specific category
{% endapi-method-response-example-description %}

```javascript
[
  {
    "news_id": 1100200,
    "url": "https://caras.perfil.com/noticias/espectaculos/luis-novaresio-relacion-hija-novio-braulio-bauab.phtml",
    "title": "Luis Novaresio habló de su relación con la hija de su novio, Braulio Bauab",
    "date": 1573498450,
    "source_id": 26,
    "img_url": "https://fotos.perfil.com/2019/11/11/trim/1140/641/luis-novaresio-hablo-de-su-relacion-con-la-hija-de-su-novio-braulio-bauab-803626.jpg",
    "source_name": "Perfil",
    "category": "Espectáculos",
    "reactions": []
  },
  {
    "news_id": 1100197,
    "url": "https://exitoina.perfil.com/noticias/escandalo/julieta-prandi-bronca-multa-situacion-judicial-claudio-contardi.phtml",
    "title": "Bronca y multa: así sigue la situación judicial de Julieta Prandi",
    "date": 1573498624,
    "source_id": 26,
    "img_url": "https://fotos.perfil.com/2019/02/21/trim/1140/641/0221-julieta-prandi-643420.jpg",
    "source_name": "Perfil",
    "category": "Espectáculos",
    "reactions": []
  }
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

**Example** [https://api.canillitapp.com/news/category/4](https://api.canillitapp.com/news/category/4)

{% hint style="warning" %}
If the link examples return **Forbidden**, just copy the link and paste it in a new tab.
{% endhint %}

## API in Axios

Now we are going to use **`axios`** that is an HTTP client for Javascript to do request through an API.

So remember it, you already installed it, but you can install **`axios`**, running

```bash
yarn add axios
```

{% hint style="info" %}
See axios documentation [here](https://github.com/axios/axios).
{% endhint %}

So in the file `src`**`/`**`api`**`/`**`index.js` lets define a **`class`**, where each **`static method`**, fetch a different **endpoint** of the API.

{% tabs %}
{% tab title="src/api/index.js" %}
```javascript
import axios from 'axios';

class API {
  static apiURL = process.env.REACT_APP_API_URL;
  static timeout = 5000;

  static defaultCB = () => {
  };

  static getLatest = (date, success, error = API.defaultCB, always = API.defaultCB) => {
    axios({
      url: `/latest/${date}`,
      method: "GET",
      baseURL: API.apiURL,
      timeout: API.timeout
    }).then((response) => success(response))
      .catch((response) => error(response))
      .then(always());
  };

  static getCategory = (categoryId, success, error = API.defaultCB, always = API.defaultCB) => {
    axios({
      url: `/news/category/${categoryId}`,
      method: "GET",
      baseURL: API.apiURL,
      timeout: API.timeout
    }).then((response) => success(response))
      .catch((response) => error(response))
      .then(always());
  };

  static getSearch = (searchValue, success, error = API.defaultCB, always = API.defaultCB) => {
    axios({
      url: `/search/${searchValue}`,
      method: "GET",
      baseURL: API.apiURL,
      timeout: API.timeout
    }).then((response) => success(response))
      .catch((response) => error(response))
      .then(always());
  };
}

export default API;
```
{% endtab %}
{% endtabs %}

Now as you can see, each **`static method`** receives, the _**parameters**_ that each endpoint required and the callbacks when the request _**success**_, return an _**error**_, and the callback that _**always**_ is executed even is it success or failed.

Also, in **`line 4`**, you can see that we use an environment variable **`(REACT_APP_API_URL)`**, to define the api url that in our case is [https://api.canillitapp.com/](https://api.canillitapp.com/). So to define it, we need to add that env variable in **`.env`** file as follows.

{% tabs %}
{% tab title=".env" %}
```bash
REACT_APP_API_URL="https://api.canillitapp.com"
```
{% endtab %}
{% endtabs %}

All env variables in React should be called REACT\_APP\_&lt;VAR NAME&gt;, for example if I want an variable for author page name, I should name it **`REACT_APP_AUTHOR_NAME="Juan Manuel Sánchez"`**

Then when I'm gonna use it on the code, I refer to it as **`process.env.REACT_APP_AUTHOR_NAME`**

{% hint style="info" %}
To learn more about how to work with environment variables and .env files, see the [create-react-app documentation](https://create-react-app.dev/docs/adding-custom-environment-variables).
{% endhint %}

## Constants

Now to handle the categories id easier, we can create constants in our code to refer that id's, so in the file

`src`**`/`**`common`**`/`**`constants`**`/`**`index.js`

{% tabs %}
{% tab title="src/common/constants/index.js" %}
```javascript
const categories = {
  politica: {
    label: "Política",
    id: 1
  },
  internacional: {
    label: "Internacional",
    id: 2
  },
  tecnologia: {
    label: "Tecnología",
    id: 3
  },
  espectaculos: {
    label: "Espectáculos",
    id: 4
  },
  deportes: {
    label: "Deportes",
    id: 5
  }
};

export {
  categories
};
```
{% endtab %}
{% endtabs %}



