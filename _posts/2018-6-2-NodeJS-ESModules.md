---
layout: post
title: ES Modules in NodeJS
categories: Nodejs
tags: [nodejs,javascript]
---

ES Modules were introduced in ECMAScript 2015, and can now be used in most modern browsers.  NodeJS uses the CommonJS module system, but work is currently underway to finalise ES Modules support in NodesJS, and you can use it today by using experimental flags, or a pre-processor such as Babel.js.

<!--more-->

The NodeJS [module](https://nodejs.org/api/modules.html){{ site.new_tab }} system uses the [CommonJS](http://www.commonjs.org/){{ site.new_tab }} system (`require` and `module.exports`), but since version 8.9.0 NodeJS has included [ES Modules](https://nodejs.org/api/esm.html){{ site.new_tab }} as an experimental feature, which can be used with the flag `--experimental-modules`, as this is experimental it should not be used in a live environment.  When running with this experimental flag any files named `*.mjs` (see below) will be processed as ES Modules.  Note that `import` will work for both ES Modules and CommonJS modules.

## Filenames

Any ES Modules need to be named *.mjs, otherwise, they will be processed as CommonJS modules.  If you try using import from a .js file you will get an error:

```bash
> node --experimental-modules module.js

(node:2640) ExperimentalWarning: The ESM module loader is experimental.
D:\Users\Neil\Workspace\github\nodejs-esmodules\module.js:1
(function (exports, require, module, __filename, __dirname) { import message from './myUtil';
                                                              ^^^^^^

SyntaxError: Unexpected token import
    ...
    at file:///D:/Users/Neil/Workspace/github/nodejs-esmodules/module.js:8:36
```

Similarly, if a .js file uses export:

```bash
> node --experimental-modules module.mjs

(node:13532) ExperimentalWarning: The ESM module loader is experimental.
D:\Users\Neil\Workspace\github\nodejs-esmodules\myUtil\index.js:1
(function (exports, require, module, __filename, __dirname) { export default 'This has been imported (ES modules export)';
                                                              ^^^^^^

SyntaxError: Unexpected token export
    ...
    at file:///D:/Users/Neil/Workspace/github/nodejs-esmodules/myUtil/index.js:8:36
```

and if you import a CommonJS module named .mjs:

```bash
> node --experimental-modules module.mjs

(node:13644) ExperimentalWarning: The ESM module loader is experimental.
SyntaxError: The requested module does not provide an export named 'default'
    at checkComplete (internal/loader/ModuleJob.js:75:27)
    at moduleJob.linked.then (internal/loader/ModuleJob.js:58:11)
    at <anonymous>
```

As the import is expecting to find an ES Module default export.

> Note- browsers do not determine file types using file extensions, so in browsers, it makes no difference if a file using ES Modules is named *.js or *.mjs, however, to be consistent with NodeJS it is recommended to use *.mjs for ES Modules being used in the browser.

## Webpack

An alternative to using the experimental flag is to use Webpack to bundle your files - Webpack [supports ES Modules out of the box](https://webpack.js.org/api/module-methods/#es6-recommended-){{ site.new_tab }} as of version 2.0, so can bundle both CommonJS and ES Modules without any extra confirguration.  For Webpack 4 you can install the `webpack` and `webpack-cli` dependencies

```bash
npm i -D webpack webpack-cli
```

and then run :

```bash
webpack module.mjs -o webpacktest.js --mode=development
```

to create a new webpacktest.js bundled file that will include any ES Module imports.

## Babel

An alternative is to use Babel to transpile any ES Module code back to CommonJS code using Babel.  For this we first need to install `babel-cli` and `babel-preset-env` :

```bash
npm i -D babel-cli babel-preset-env
```
and create a `.babelrc` config file containing

```json
{
    "presets": ["env"]
}
```

Now we can run babel to produce files that can be run in standard node

```bash
babel module.mjs myUtil/index.mjs myUtil2/index.js -d dist/babeltest
```

## Example

See my [nodejs-esmdules](https://github.com/Neil188/nodejs-esmodules.git){{ site.new_tab }} repo for a basic example of using the experimental flag, a webpack build and a Babel build.
