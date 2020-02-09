![](badge.svg)

# html-webpack-prerender-plugin


A plugin to prerender and inject JavaScript apps into the static markup generated by [html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin) at build time.

[![npm version](https://badge.fury.io/js/html-webpack-prerender-plugin.svg)](https://badge.fury.io/js/html-webpack-prerender-plugin) [![Reuters open source software](https://badgen.net/badge/Reuters/open%20source/?color=ff8000)](https://github.com/reuters-graphics/)

### Why this?

At Reuters Graphics we are all about [JAM stack](https://jamstack.org/). Avoiding the overhead of server maintenance keeps us lean, minimizes our technical debt and protects our ability to scale with our audience. But being serverless sometimes makes it more complex to use the tools we like to their best effect.

For example, we like to use modern frameworks like React, but rendering our content only in the client makes our pages slower for our readers and less SEO friendly.

This plugin lets us reap the benefits of server-side rendering in those frameworks but in the context of a static page. With it, we can pre-render our content at build time and, in the client, still hydrate a dynamic app.

### Prior art

This app is heavily inspired by [static-site-generator-webpack-plugin](https://github.com/markdalgleish/static-site-generator-webpack-plugin), which was itself an important foundation for [Gatsby.js](https://www.gatsbyjs.org/).

## Quickstart

1. Install
  ```
  $ yarn add -D html-webpack-prerender-plugin html-webpack-plugin@next
  ```

2. Configure webpack.
  ```javascript
  // webpack.config.js
  const HtmlWebpackPlugin = require('html-webpack-plugin');
  const HtmlWebpackPrerenderPlugin = require('html-webpack-prerender-plugin');

  module.exports = {
    entry: './src/js/index.js',
    output: {
      path: './dist',
      filename: '[name].js',
      // This is important, because your app must
      // be executable in both a node AND browser
      // environment.
      libraryTarget: 'umd',
    },
    // ...
    plugins: [
      new HtmlWebpackPlugin({
        template: './src/templates/index.html',
      }),
      new HtmlWebpackPrerenderPlugin({ main: '#root' }),
    ],
  };
  ```

3. Create a template with a root container for your app.
  ```html
  <!-- src/templates/index.html -->
  <!DOCTYPE html>
  <html lang="en" dir="ltr">
    <head></head>
    <body>
      <div id='root'></div>
    </body>
  </html>
  ```

4. Make sure your app exports a default function that renders a string of markup.
  ```javascript
  // src/js/index.js
  export default = () => '<p>Hello world!</p>';
  ```

#### Rendered

```html
<!-- src/templates/index.html -->
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head></head>
  <body>
    <div id="root"><p>Hello world!</p></div>
    <script src="main.js"></script>
  </body>
</html>
```

## Next

Read the complete guide to the [plugin configuration options](docs/options.md).

Check out some example configurations:

- [Multiple apps](docs/multiple.md)
- [Static markup](docs/static.md)
- [React app](docs/react.md)
- [React/Redux app with preloaded state](docs/redux.md)
- [Usage with react-helmet-async](docs/helmet.md)
- [Async rendering](docs/async.md)


## Testing

```
$ yarn build && yarn test
```
