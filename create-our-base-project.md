# Create our base project

Now lets create our project and configure our folder structure.

```bash
yarn create react-app news-feed
```

## Packages

Now lets install the following libraries:

#### axios

#### moment

#### node-sass

#### nprogress

#### react-lazyload

#### react-router-dom

```bash
yarn add axios moment node-sass nprogress react-lazyload react-router-dom
```

## File Structure

Then remember in the **root of the folder app** configure the `jsconfig.json` file; also add a file called `.env` when we gonna create our environment variables.

{% tabs %}
{% tab title="jsconfig.json" %}
```javascript
{
  "compilerOptions": {
    "baseUrl": "src",
    "paths": {
      "*": ["src/*"]
    }
  },
  "include": ["src/*"]
}
```
{% endtab %}

{% tab title=".env" %}
```
# Empty by default
```
{% endtab %}
{% endtabs %}

Then in addition, to the basic file structure we use in the last workshop, lets gonna create 3 folders more

#### api

To set up out api configuration, and only from that package we are going to use axios to call our API or API's

#### hocs

Where we gonna create our HOC's components

![src folder file structure](.gitbook/assets/captura-de-pantalla-2019-11-11-a-la-s-10.21.46-a.-m..png)

As you can see, I create the folders for each **component** that we gonna create now, and in **`common`** folder, I create another one called **`constants`**, that folder will be useful later.

{% hint style="info" %}
To avoid create those files manually, download the src folder here and copy it to your project.
{% endhint %}

{% file src=".gitbook/assets/src.zip" caption="src folder" %}

{% hint style="info" %}
I also add **`scss`** folder with all styles already written, so in the process we are going to use the classnames already.
{% endhint %}

Now if you run your app with

```bash
yarn start
```

You should only see white page with only a text that says **News Feed App** 

