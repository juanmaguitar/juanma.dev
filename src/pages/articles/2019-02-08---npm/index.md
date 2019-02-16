---
title: "NPM"
date: "2019-02-08T22:12:03.284Z"
layout: post
draft: false
path: "/posts/npm-cache"
category: "npm"
tags:
  - npm
  - cache
description: "La caché de NPM"
---

# NPM

Publicar un paquete en NPM es muy fácil. Se hace con [`npm publish`](https://docs.npmjs.com/cli/publish.html)

Una vez publicado podemos comprobar con `npm info` que efectivamente, el paquete se ha publicado con la versión que corresponde

```
npm info <%nombre-del-paquete%>
```

Si queremos instalar un paquete recien publicado podemos encontrar algunos problemas con NPM


### `package-lock.json`

Este archivo [`package-lock.json`](https://docs.npmjs.com/files/package-lock.json) [[1]](https://medium.com/coinmonks/everything-you-wanted-to-know-about-package-lock-json-b81911aa8ab8) se crea automáticamente cuando instalamos dependencias con NPM

Se utiliza para ayudar en la resolución de dependencias, y su existencia evita posibles conflictos en cuanto a las diferentes versiones que se pueden instalar en base al sistema [semver](https://semver.org/)

Pero también, puede ser un problema si lo que queremos es instalar una última versión recién publicada de un paquete que ya teníamos, ya que en este archivo queda escrita la versión y la ruta donde está.

### La caché

Por otro lado, NPM mantiene una cache interna en la que almacena las versiones ya instaladas de paquetes por lo que es posible que si intentamos instalar una última versión recién publicada de un paquete que ya teniamos, nos instale todo el rato la versión que ya teníamos (porque la sirve de su caché)

## Reinstalación limpia de paquetes NPM

Teniendo esto en cuenta, para hacer una instalación limpia en NPM podemos hacer...

```
rm -rf node_modules && rm -rf package-lock.json && npm cache clean --force && npm install
```
