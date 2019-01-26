---
title: "Cómo configurar nuestros proyectos para trabajar con Webpack, Babel, ESLint y Prettier"
date: "2019-01-26T22:12:03.284Z"
layout: post
draft: false
path: "/posts/set-webpack-babel-eslint-prettier"
category: "webpack"
tags:
  - webpack
  - eslint
  - babel
  - prettier
description: "Cómo configurar nuestros proyectos para trabajar con Webpack, Babel, ESLint y Prettier"
---

# Webpack

## Webpack: Configuración Básica

Para configurar un proyecto preparado para trabajar con javascript "moderno", tenemos que crear nuestro `package.json`, instalar una serie de dependencias y configurar webpack para poder importar módulos JS ES2015 y lanzar nuestro servidor local que se recargará automáticamente cuando grabemos.

### Ideas Claras

- Para crear nuestro `package.json` vacio hacemos `npm init -y`
- Para instalar webpack y añadirlo como dependencia al `package.json` hacemos `npm i -S webpack`

### `package.json` básico

Utilizaremos este `package.json` básico para empezar a trabajar  

```
{
  "name": "starter-project",
  "version": "1.0.0",
  "description": "## Packages Webpack",
  "main": "index.js",
  "scripts": {
    "build": "rimraf dist && webpack --progress",
    "server": "webpack-dev-server --inline --progress"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "html-webpack-plugin": "^2.30.1",
    "rimraf": "^2.6.2",
    "webpack": "^3.8.1",
    "webpack-dev-server": "^2.9.4",
    "webpack-cli": "^3.2.1"
  }
}
```

Las dependencias instaladas son:

- [`html-webpack-plugin`](https://github.com/jantimon/html-webpack-plugin) → Nos permite añadir automaticamente el _bundle_ creado con _webpack_ a un `.html` (lo utilizamos en el `webpack.config.js`)
- [`rimraf`](https://github.com/isaacs/rimraf) → Para eliminar archivos desde node (lo utilizamos en los _scripts_ del `package.json`)
- [`webpack`](https://github.com/webpack/webpack) → El _bundler_ de módulos que utilizaremos. Nos permite manejar todas nuestras dependencias desde JS y se encarga de generar un `bundle.js` final con todo (lo utilizamos en los _scripts_ del `package.json`)
- [`webpack-dev-server`](https://github.com/webpack/webpack-dev-server) → Sirve una app webpack y actualiza el navegador cuando detecta cambios en los archivos (lo utilizamos en los _scripts_ del `package.json`)
- [`webpack-cli`](https://github.com/webpack/webpack-cli) → Proporciona el comando `webpack` para poder utilizarlo desde linea de comandos (o en los _scripts_ del `package.json`)



### `webpack.config.js` básico

Utilizaremos este `webpack.config.js` básico para empezar a trabajar  

```
const path = require("path");
const webpack = require("webpack");
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  entry: "./src/app/index.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "bundle.js"
  },
  new HtmlWebpackPlugin({ template: './src/index.html' }),
  devServer: {
    contentBase: "./src/public"
  }
};
```

Las propiedades configuradas son:
- [`entry`](https://webpack.js.org/concepts/#entry) → El punto de entrada de nuestra app desde el cual se empezaran a "rastrear" los módulos
- [`output`](https://webpack.js.org/concepts/#output) → La carpeta y el nombre del archivo donde se generará nuestra app
- [`plugins`](https://webpack.js.org/concepts/#plugins) → Los plugins de webpack que aplicamos al generar nuestro _bundle_
- [`devServer`](https://webpack.js.org/configuration/dev-server/#devserver) → La ruta desde la cual arrancará nuestro servidor local (y donde estatá nuestro `index.html`)

Añadimos también un `index.html` base en `src`

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Title Page</title>
  </head>
  <body>

  </body>
</html>

```


### `webpack.config.js` preparado para carga de módulos JS ES2015

Instalamos los modulos que necesitamos con 
```
npm i -S  \
  @babel/core \
  babel-loader
```

Añadimos la propiedad [`module`](https://webpack.js.org/configuration/module/) para definir las [`rules`](https://webpack.js.org/configuration/module/#module-rules) con las que configuraremos diferentes [_loaders_](https://webpack.js.org/loaders/) para diferentes tipos de assets

De entrada sólo preparamos webpack para cargar JS ES2015 con [`babel-loader`](https://github.com/babel/babel-loader)

```
module: {
  rules: [
    {
      test: /\.js$/,
      use: "babel-loader",
      exclude: /node_modules/
    }
  ]
}
```

## Webpack: CSS con Webpack y StandardJS con prettier en Visual Code

Ahora vamos a añador más loaders (css y html) y a configurar webpack para que genere un `styles.css` aparte para nuestros css cargados mediante webpack

Además configuraremos nuestro proyecto y el editor [Visual Code](https://code.visualstudio.com/) (que te recomiendo) para que nos chequee y nos aplique automáticamente un standard de formato de código javascript muy popular llamando [StandardJS](https://standardjs.com/)

### Añadiendo Loaders para cargar CSS/HTML desde JS

Instalamos los modulos que necesitamos con 

```bash
npm i -S \
  css-loader \
  raw-loader \
  style-loader \
  extract-text-webpack-plugin
```

Añadimos las nuevas `rules` con su respectiva configuración de `loaders` y nuevos plugins. 

**`webpack.config.js`**

```
const ExtractTextPlugin = require('extract-text-webpack-plugin')

...

module: {
  rules: [
    {
      test: /\.js$/,
      use: "babel-loader",
      exclude: /node_modules/
    },
    {
      test: /\.css$/,
      use: ExtractTextPlugin.extract({
        fallback: "style-loader",
        use: "css-loader"
      })
    },
    {
      test: /\.html$/,
      use: "raw-loader"
    }
  ]
},
plugins: [
  new HtmlWebpackPlugin({ template: "./src/index.html" }),
  new ExtractTextPlugin("styles.css")
],
```


Los loaders añadidos son:
- [`style-loader`](https://github.com/webpack-contrib/style-loader) y [`css-loader`](https://github.com/webpack-contrib/css-loader) → Para poder cargar _css_ desde _javascript_
- [`raw-loader`](https://github.com/webpack-contrib/raw-loader) → Para cargar archivos de texto (por ejemplo para cargar el contenido de `.html` externos)

El plugin añadido es:
- [`extract-text-webpack-plugin`](https://github.com/webpack-contrib/extract-text-webpack-plugin) → Nos permite extraer parte del _bundle_ en un archivo aparte (lo utilizaremos para separar los css en un `styles.css` aparte)


### Activando ultimas características de Javascript con Babel

[Babel](https://babeljs.io/) es la herramienta que utilizamos para _transpilar_ (traducir un código a otro código) código ES2015 a ES5. Cómo hay muchas features del lenguaje propuestas que aún no forman parte del standard, se utilizan los llamados [_presets_ y _plugins_](https://babeljs.io/docs/plugins/) para [indicar a nuestro babel](https://www.fullstackreact.com/articles/what-are-babel-plugins-and-presets/) qué tipo de características de JS queremos utilizar en nuestro proyecto.

En nuestro caso le vamos a indicar que queremos soporte completo a ES2015 (ES6) mediante el preset [`preset-env`](https://babeljs.io/docs/en/babel-preset-env) que determina que plugins y pollyfils son necesarios en base a los _browsers_ configurados. 

Sin ninguna configuración, este preset se comporta igual que [`babel-preset-latest`](https://babeljs.io/docs/en/babel-preset-latest) (o lo que es lo mismo, que [`babel-preset-es2015`](https://babeljs.io/docs/en/babel-preset-es2015), [`babel-preset-es2016`](https://babeljs.io/docs/en/babel-preset-es2016), and [`babel-preset-es2017`](https://babeljs.io/docs/en/babel-preset-es2017) juntos)

Asi que instalamos este módulo con `npm i -S @babel/preset-env`

Y creamos un archivos `.babelrc` con el que le indicamos a babel qué caracteristicas de Javascript queremos utilizar en nuestro proyecto

**`.babelrc`**

```
{
  "presets": ["@babel/preset-env"]
}
```

### Configurando `eslint` & `prettier` con Visual Code para aplicación automática de `javascript standard`

[ESLint](https://eslint.org/) es un "chequeador" de estilos que podemos aplicar a nuestro workflow de manera que podamos comprobar automaticamente si nuestro código sigue una determinada guia de estilos

Las normas (normalmente predefinidas ya en modulos NPM) que queremos que cumpla nuestro código las podemos definir en un `.eslintrc` que utilizará eslint para chequear nuestro código

En nuestro caso vamos a indicarle a eslint que tenga en cuenta las configuraciones de babel y que además chequee que nuestro código sigue el estilo [standardJS](https://standardjs.com/) 

Para aplicar este check automaticamente instalamos un formateador de código llamado [prettier](https://prettier.io/) a través del cual podremos aplicar automaticamente estos estilos desde nuestro editor

Asi que instalamos nuestras dependencias... 

```
  npm i -S babel-eslint eslint-config-standard eslint-plugin-import eslint-plugin-node eslint-plugin-promise eslint-plugin-standard prettier-eslint
```

Creamos y configuramos el `.eslintrc`

```
{
  "parser": "babel-eslint",
  "extends": [
    "standard"
  ]
}
````

Y añadimos la configuración correspondiente en Visual Code desde `User Settings Visual Code`

```
{
  ...
  "prettier.eslintIntegration": true
  ...
}
```

## Extras

### `script-ext-html-webpack-plugin`

Podemos instalar [`script-ext-html-webpack-plugin`](https://github.com/numical/script-ext-html-webpack-plugin) para mejora el plugin `html-webpack-plugin` y especificar atributos (como `defer`) al tag `<script>` de carga del _bundle_ (lo utilizamos en el `webpack.config.js`)

Lo instalamos con `npm i -D script-ext-html-webpack-plugin`
Y lo añadimos a `plugins` en nuestro `webpack.config.js`

```
 plugins: [
        ...,
        new ScriptExtHtmlWebpackPlugin({
            defaultAttribute: 'defer'
        })
    ],
```

### Añadir source maps 

También podemos configurar nuestro `webpack.config.js` para añadir _source maps_ con [`devtool`](https://webpack.js.org/configuration/devtool/)

Así cuando pongamos `debugger` veremos nuestro código original y no el transpilado por babel

```
module.exports = {
  entry: ...,
  output: ...,
  module: ...,
  plugins: ...,
  ...
  devtool: 'source-map'
}

```
