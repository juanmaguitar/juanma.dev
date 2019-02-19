---
title: "Qué es y para que sirve NPX"
date: "2018-11-03T22:12:03.284Z"
layout: post
draft: false
path: "/posts/npx"
category: "npm"
tags:
  - "npm"
  - "npx"
description: "Que es NPX"
---

`npx` está disponible de manera nativa con npn a partir de su versión [5.2](https://github.com/npm/npm/releases/tag/v5.2.0)

## Instalación

Para comprobar que lo tenemos instalado podemos hacer
```sh
which npx
```

Si tenemos una versión anterior o por lo que fuera no lo tenemos instalado, lo podemos instalar haciendo
```sh
npm install -g npx
```

## Llamadas a binarios locales

Tradicionalmente, para llamar a un binario de un paquete global teníamos que hacer algo así

```sh
./node_modules/.bin/jest
```

Con `npx` podemos hacer directamente

```sh
npx jest
```

## Llamadas a binarios locales sin instalación

Tradicionalmente, para llamar a un binario global teníamos que instalarlo primero para luego poder hacer llamarlo como cualquier otro comando

Un caso típico es `create-react-app` donde primero había que hacer

```sh
npm install -g create-react-app
```

para luego poder hacer

```
create-react-app best-todo-app-ever
```

Con `npx` podemos hacer directamente

```sh
npx create-react-app best-todo-app-ever
```

o para lanzar un servidor local en una carpeta...

```
npx http-server
```


### Recursos

- <https://www.npmjs.com/package/npx>
- <https://blog.scottlogic.com/2018/04/05/npx-the-npm-package-runner.html>
- <https://medium.com/@maybekatz/introducing-npx-an-npm-package-runner-55f7d4bd282b>

