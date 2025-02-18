18-02-2025 19:20
Status: #idea
Tags: [[Angular]]

- [[#Nivel básico|Nivel básico]]
	- [[#Nivel básico#Observables|Observables]]


# RxJS

RxJS es una librería para programación reactiva, que hace que el código asíncrono o con callbacks sea sencillo.

El componente central de RxJS es el observable, que es un mecanismo que nos permite manejar eventos, programación asíncrona y manejo de múltiples eventos lanzados al mismo tiempo.

## Nivel básico

### Observables

Un observable tiene un formato sencillo, ya que de la mano de un observer, podremos determinar que queremos hacer con el observable, y como se manejarán los datos.

En este ejemplo, podemos ver como el ``Observable`` va a emitir los valores 1, 2, 3 y finalmente se completará. 

Podremos ver por consola, a medida que se vayan mostrando los valores, lo que está emitiendo el observable, ya que nuestro observer indica que en el "next", se haga un console.log del valor, en el error, se haga un console.error del error, y en el complete, que nos indique un mensaje de finalización.

El `Observable` define cómo se generan los datos, y el `Observer` especifica qué hacer con esos datos.

Con nuestro ``Observer`` podemos hacer lo que queramos, por ejemplo, podríamos sumar los valores, multiplicarlos, o manipularlos como nosotros queramos.

```typescript
const observable = new Observable((subscriber) => {
	subscriber.next(1);
	subscriber.next(2);
	subscriber.next(3);
	subscriber.complete();
});

const observer = {
	next: (value: any) => {
		console.log(value);
	},
	error: (error: any) => {
		console.error(error);
	},
	complete: () => {
		console.log("observable completed");
	}
}

observable.subscribe(observer);
```

> [!DANGER] Observables y lazyness
> Cabe destacar que los observables son lazy, es decir, no hacen nada hasta que alguien se subscribe a estos.




---
# Backlinks

```dataview
LIST
FROM [[]]
SORT file.name ASC
```
