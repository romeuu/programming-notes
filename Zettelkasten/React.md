01-05-2025 17:28
Status: #project
Tags: [[React]], [[Frontend]]

- [[#Introducción|Introducción]]
	- [[#Introducción#Ejemplos|Ejemplos]]
		- [[#Ejemplos#Componente de "A quién seguir" Twitter|Componente de "A quién seguir" Twitter]]

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

Para los atributos se usa cammel case, es decir, por ejemplo, sería **dataId** y no data-id.

## Introducción

Este código es un poco arcaico, así que, como se crearía el primer proyecto de react?

Pues para esto usaremos **Vite**, que es un empaquetador de aplicaciones.

```terminal
npm init -y
mkdir projects && cd projects
npm create vite@latest
```

Al usar el comando de npm create, nos dará a escoger el flavor que queremos usar, en este caso React con JavaScript + SWC. Más adelante haré proyectos con TypeScript, ya que sería más similar a Angular.

Usaremos este comando para arrancar el proyecto: 

```terminal
npm run dev
```

Si entramos en nuestra aplicación veremos como tenemos index.html, main.jsx, App.jsx, es decir, una estructura similar a Angular.

- **Main.jsx**: Es el punto de entrada de la aplicación. Es donde se create el root y por ende donde se renderiza la aplicación.

```jsx
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
	<React.Fragment>
		<button>Hola Button</button>
		<button>Hola Button</button>
	</React.Fragment>
)
```

En este caso vemos como se renderizaría un fragmento en el que existirían dos botones.

¿Y que pasaría si queremos personalizar los botones? Pues podríamos meter un icono en svg dentro, por ejemplo, pero no es lo más correcto, ya que si queremos hacer eso en 10 botones, tendremos que copiar el mismo código una y otra vez.

Entonces, podremos crear los botones en formato de función para no repetirlo, por ejemplo:

```jsx
const createButton = ({text}) => {
	return {
		<button>{text}</button>
	} 
}

root.render(
	<React.Fragment>
		{createButton({text: 'Button 1'})}
		{createButton({text: 'Button 2'})}
		{createButton({text: 'Button 3'})}
	</React.Fragment>
)
```

Si queremos adentrarnos más en esto, existe el concepto del componente, al igual que en Angular. Y los nombres tienen que ser Pascal Case al igual que en Angular (MiComponente), o (ButtonHome).

```jsx
const Button = ({text}) => {
	return {
		<button>{text}</button>
	} 
}

root.render(
	<React.Fragment>
		<Button text="Button 1"/>
		<Button text="Button 2"/>
		<Button text="Button 3"/>
	</React.Fragment>
)
```

### Ejemplos

#### Componente de "A quién seguir" Twitter

Para crear este componente tendremos que crear un componente a parte, por ejemplo, en el fichero **TwitterFollowCard.jsx**. Además, tendremos que exportarlo para que podamos importarlo en otros lugares de la app:

```jsx
// TwitterFollowCard.jsx
export function TwitterFollowCard (userName, name, isFollowing) {
	const imageSrc = `https://unavatar.io/{userName}`;
	
	return (
		<article className='md-twitter-follow-card'>
			<header>
				<img alt="El avatar de midudev" src="{imageSrc}" />
				<div>
					<strong>{userName}</strong>
					<span>{username}</span>
				</div>
			</header>
		</article>
	)
}
```

```jsx
// App.jsx
export function App() {
	return (
		<section className="App">
			<TwitterFollowCard isFollowing username="midudev" name="Miguel Ángel"/>
			<TwitterFollowCard isFollowing={false} username="pheralb" name="Pablo Hernández"/>
		</section>	
	)
}
```

```jsx
// main.jsx
import App from 'App.jsx';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
	<App/>
)
```

Si queremos añadir clases a un elemento, hay que usar className en vez de class, ya que es una palabra reservada en JavaScript.

---
# Backlinks

```dataview
LIST
FROM [[]]
SORT file.name ASC
```
