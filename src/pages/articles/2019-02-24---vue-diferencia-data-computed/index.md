---
title: "¿Qué diferencia hay entre data y computed en Vue?"
date: "2019-02-24T22:12:03.284Z"
layout: post
draft: false
path: "/posts/diferencia-data-computed-vue"
category: "vue"
tags:
  - vue
  - data
  - computed
description: "¿Qué diferencia hay entre data y computed en Vue?"
---

En Vue [podemos definir para cada componente (o instancia Vue) un conjunto de datos](https://vuejs.org/v2/guide/instance.html#Data-and-Methods) (bajo la propiedad [`data`](https://vuejs.org/v2/guide/instance.html#Data-and-Methods)) para que estén disponibles (de manera reactiva) tanto en sus métodos como en el template

Pero también podemos reaccionar aplicando cierta lógica para [dejar disponibles ciertos valores _computados_](https://vuejs.org/v2/guide/computed.html) definiéndolos bajo la propiedad [`computed`](https://vuejs.org/v2/guide/computed.html) que también reacciona cuando cambian los datos

Vamos a ver un poco más en detalle cuando aplicar uno u otro

## [`data`](https://vuejs.org/v2/guide/instance.html#Data-and-Methods)

Cuando se crea una instancia Vue (o se monta un componente), todas las propiedades que encuentra bajo la propiedad `data` son añadidas al sistema reactivo de Vue → cuando los valores de estas propiedades cambian, la vista _reacciona_ actualizando sus valores para que se correspondan con estos nuevos valores

En este ejemplo... 

```javascript
<template>
  <div id="app">
    <div>
      <button @click="changeMsg" data-msg="Hola!">Hola!</button>
      <button @click="changeMsg" data-msg="Tere, Maailm!">Tere, Maailm!</button>
      <button @click="changeMsg" data-msg="Hello World! How are you?">
        Hello World! How are you?
      </button>
      <button @click="changeMsg" data-msg="Hello Wêreld!">Hello Wêreld!</button>
    </div>
    <h1>{{ selectedMessage }}</h1>
    
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld";

export default {
  name: "App",
  data: function() {
    return {
      selectedMessage: "..."
    };
  },
  methods: {
    changeMsg(e) {
      const { msg } = e.target.dataset;
      this.selectedMessage = msg;
      this.onTheFlySelectedMessage = msg
    }
  }
};
</script>
```

Cada vez que pulso un botón, modifico el valor de `selectedMessage` definido en data y la vista _reacciona_ actualizando el dato la vista

En realidad, añadir datos a `data` para tenerlos disponibles en el `template` no es estrictamente necesario.

Fíjate que Si añado al template 

```
<h1>{{ onTheFlySelectedMessage }}</h1>
```

y a `changeMsg` 

```
changeMsg(e) {
  const { msg } = e.target.dataset;
  this.selectedMessage = msg;
  this.onTheFlySelectedMessage = msg
}
```

funciona igual.

Por lo que entiendo, añadir los datos que queremos dejar disponibles al `template` (para componente o instancia) es una buena práctica por:

- evito un `undefined` inicial si la propiedad no está definido en `data`
- la suma de `props` + `data` + `computed` nos deja el total de propiedades que puedo mostrar en el `template`
- en `data` quedan las variables con las que voy a trabajar
  - datos iniciales → carga de datos
  - datos que voy a alterar a través de métodos

Si quisiera mostrar datos generados a través de los que hay en `data` (o que vengan vía `props`) puedo hacer directamente en el template cosas como...

```
<h3>{{ selectedMessage.length }}</h3>
```

Pero lo suyo es utilizar la propiedad `computed` para definir estos datos _computados_

## [`computed`](https://vuejs.org/v2/guide/computed.html)

Para dejar el template simple y declarativo todas aquellos datos que necesitemos como resultado de aplicar ciertas operaciones sobre los ya existentes deberían ir en `computed`

Así en el ejemplo anterior lo suyo sería añadir...

```js
computed: {
  totalWordsSelectedMessage: function() {
    return this.selectedMessage.length;
  }
}
```

para luego poder hacer (elegantemente) en el template...

```
<h3>{{ totalWordsSelectedMessage }}</h3>
```

Además, si lo que queremos es _computar_ datos a partir de valores que nos vengan vía props, no nos queda otra que definirlos en `computed` si no no funcionará


<iframe src="https://codesandbox.io/embed/5mr33rkmkk?codemirror=1&fontsize=14" style="width: 100%; margin-left: auto ; margin-right: auto; height:500px; border:0; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>