

## PHP Manifest from Webpack

__NOTE__: This repository is no longer maintained. The mantle has been taken up by the excellent folks at Cast Iron Coding. Please find the [up-to-date version of this project there](https://github.com/castiron/webpack-php-manifest).


_Please note this module depends on webpack 2+._

This webpack plugin will create a PHP file in your `output.path` directory with a PHP class in it having two static
attributes: `$jsFiles` and `$cssFiles`. These can be accessed by your PHP application to learn about what to include
when rendering the frontend of the site.

Note that the php manifest is created with (node fs)[https://nodejs.org/api/fs.html] independently of the Webpack
output cycle. It will therefor be created in the same location when running a standard Webpack build *or* Webpack Dev Server

### Install

With Yarn:

```
yarn add webpack-php-manifest
```

With NPM:

```
npm i webpack-php-manifest
```

### Usage
```javascript
// ...in your webpack config file

const PhpManifestPlugin = require('webpack-php-manifest');
const phpManifest = new PhpManifestPlugin(options);

module.exports = {
  // ...
  plugins: [
    phpManifest
  ]
}
```

## Available options:

`output` (default: "assets-manifest")

The name of the manifest file to write. Will be written to webpack's
`output.path` directory and appended with `.php`.

&nbsp;&nbsp;

`path` (default: "")

The relative file path to the asset files. Example:
```javascript
const phpManifest = new PhpManifestPlugin({
  path: assets // Asset path will look like /assets/bundle.js
});
```

&nbsp;&nbsp;

`pathPrefix` (default: "")

An optional absolute URL, that is prepended the asset file paths in php.
Primarily used with the `devServer` option below.

&nbsp;&nbsp;

`devServer` (default: false)

A flag that tells the plugin if webpack-dev-server is running, and adds
`webpack-dev-server.js` to the list of JS assets in the file list.

If used, this should be passed as part of an environment or config variable.
pathPrefix should also be used to point to the devServer url. Example:
```javascript
const phpManifest = new PhpManifestPlugin({
  devServer: process.env.WEBPACK_DEV_SERVER, // This should be an env or config boolean
  // In this example, path prefix is included conditionally so that the prefix is only used when dev server is running
  pathPrefix:
  process.env.WEBPACK_DEV_SERVER ? `http://localhost:${port}` : null
  });
  ```

  &nbsp;&nbsp;

  `phpClassName` (default: "WebpackBuiltFiles")

  The PHP class name to use for the class. You can generally ignore this
  unless you have a conflicting PHP class named `\WebpackBuiltFiles` in your PHP environment.

  ## Consuming the manifest

  Currently there's only one implementation for consuming the generated manifest in a PHP application:

  #### October CMS

  Use the October CMS plugin called [webpackassets-plugin](https://packagist.org/packages/castiron/webpackassets-plugin).
