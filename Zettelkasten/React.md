01-05-2025 17:28
Status: #project
Tags: [[React]], [[Frontend]]

# React

Estos apuntes sobre React comienzan sobre el curso de Midudev, y serán organizados a modo de esquema o mapa mental para profundizar en esta librería.

Tenemos que entender que React en si no hace magia, y por debajo, es javascript vanilla, que lo interpretan aplicaciones o empaquetadores de aplicaciones como **SWC** (Speedy Web Compiler), **Babel**, o **Webpack**.

Por ejemplo, vamos a ver como sería un botón usando React pero sin usar JSX:

```javascript
const button = React.createElement('button', {"data-id": 123}, 'Button 1');
```

Con JSX sería (algo más cercano a como sería en Angular también):

```javascript
const buttonJSX = <button data-id="123">Button 1</button>;
```

Algo más reactivo sería:

```javascript
const name = 'Sergio';
<h1>Hola, {name}>
```





---
# Backlinks

```dataview
LIST
FROM [[]]
SORT file.name ASC
```
