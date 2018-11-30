---
title: "Contexto en React (manera antigua, previa a React 16.3)"
date: "2018-10-31T22:12:03.284Z"
layout: post
draft: false
path: "/posts/react-context-manera-antigua"
category: "React"
tags:
  - "React"
  - "Context"
description: "Contexto en React (manera antigua, previa a React 16.3)"
---

*Context* en React es una manera de crear un espacio de datos accesible por una _rama_ del arbol de componentes de React

En el manejo del contexto intervienen dos actores: El _Provider_ y el _Consumer_

La manera de manejarlo ha cambiado desde la versión React 16.3. La manera legacy de manejar el contexto está documentada en React [aqui](https://reactjs.org/docs/legacy-context.html)

## Provider

En versiones anteriores a la 16.3 para definir el contexto que queremos propagar a hijos de un componente definimos en el **Provider** `childContextTypes` y `getChildContext`:

- `childContextTypes`: propiedad _static_  donde declaramos la estructura del objeto contexto que será pasado a los hijos. Como `propTypes` pero obligario en este caso
- `getChildContext`: métodp del prototipo que devuelve el objeto contexto que será pasado a los hijos en la jerarquia. Cada vez que cambie el _state_, o que el componente reciba nuevas _props_ se llamará este metodo.

```javascript
class ContextProvider extends React.Component {
  static childContextTypes = {
    currentUser: React.PropTypes.object
  };
  getChildContext() {
    return {currentUser: this.props.currentUser};
  }

  render() {
    return(...);
  }
}
```

## Consumer

Para acceder a esta información del contexto desde cualquier componente hijo Consumer podemos hacer...

Desde un componente con clase ES6

```javascript
class ContextConsumer extends React.Component {
  /*
     contexTypes is a required static property to declare
     what you want from the context
  */
  static contextTypes = {
      currentUser: React.PropTypes.object
  };
  render() {
    const {currentUser} = this.context;
    return <div>{currentUser.name}</div>;
  }
}
```

Desde un componente con clase ES6 con constructor propio

```javascript
class ContextConsumer extends React.Component {
  static contextTypes = {
      currentUser: React.PropTypes.object
  };
  constructor(props, context) {
    super(props, context);
    ...
  }
  render() {
    const {currentUser} = this.context;
    return <div>{currentUser.name}</div>;
  }
}
```

Desde un componente funcional sin estado

```javascript
/* Functional stateless component */
const ContextConsumer = (props, context) => (
  <div>{context.currentUser.name}</div>
);
ContextConsumer.contextTypes = {
  currentUser: React.PropTypes.object
};
```
