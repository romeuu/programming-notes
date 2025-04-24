04-04-2025 18:29
Status: #idea
Tags: [[Programming Core]]

# Algorithms

- [[#Principios básicos|Principios básicos]]
	- [[#Principios básicos#Big O Time Complexity|Big O Time Complexity]]
		- [[#Big O Time Complexity#Ejemplos por tiempo de ejecución|Ejemplos por tiempo de ejecución]]
			- [[#Ejemplos por tiempo de ejecución#O(n^2)|O(n^2)]]
			- [[#Ejemplos por tiempo de ejecución#O(n^3)|O(n^3)]]
			- [[#Ejemplos por tiempo de ejecución#O(n log n)|O(n log n)]]
			- [[#Ejemplos por tiempo de ejecución#O(log n)|O(log n)]]
			- [[#Ejemplos por tiempo de ejecución#O(sqrt(n))|O(sqrt(n))]]
- [[#Search (Linear Search)|Search (Linear Search)]]
	- [[#Search (Linear Search)#Tiempo de ejecución|Tiempo de ejecución]]
- [[#Binary Search|Binary Search]]


## Principios básicos

### Big O Time Complexity

El time complexity es una manera que tenemos de categorizar el tiempo de ejecución de un algoritmo, basada en un input. No está pensado para ser una unidad exacta, es decir, no nos va a decir cuantos ciclos de CPU se van a usar, si no que se orienta a generalizar el crecimiento del algoritmo.

Así que si, alguien dice que nuestro algoritmo es **O(n)**, querrá decir que nuestro algoritmo crecerá de manera lineal basada al input.

Esta métrica nos ayuda a decidir si usamos un tipo de data structure o no. Saber como se comportará el rendimiento, nos permitirá optimizar nuestro código y conseguir una mayor velocidad.

Vamos a poner un ejemplo:

```typescript
function sum_char_codes(n: string): number {
	let sum = 0;
	for (let i=0; i < n.length; i++) {
		sum += n.charCodeAt(i);
	}

	return sum;
}
```

En este caso, este algoritmo sería **O(n)**, ya que recorre todos los caracteres de la cadena, en cada iteración se ejecuta la operación **n.charCodeAt(i)** y no hay bucles anidados ni llamadas recursivas.

No tenemos que ver **O(n)** como algo malo, si no que podremos verlo como aceptable en casos como cuando se necesite procesar todos los elementos de un input, o recorrer una string al completo, o si el input no es gigantesco. Pero, si que podría ser problemático en el caso de que se ejecuten varios bucles anidados haciendo **O(n^2)**, o si se ejecuta muchas veces por segundo en entradas muy grandes.

#### Ejemplos por tiempo de ejecución

##### O(n^2)

```typescript
function sum_char_codes(n: string): number {
	let sum = 0;
	for (let i=0; i < n.length; i++) {
		for (let j = 0; j < n.length; j++) {
			sum += n.charCodeAt(j);
		}
	}

	return sum;
}
```

##### O(n^3)

```typescript
function sum_char_codes(n: string): number {
	let sum = 0;
	for (let i=0; i < n.length; i++) {
		for (let j = 0; j < n.length; j++) {
			for (let k = 0; k < n.length; k++) {
				sum += n.charCode;
			}
		}
	}

	return sum;
}
```

##### O(n log n)

Aquí entrarían algoritmos como Quicksort.

##### O(log n)

Aquí entrarían estructuras como Binary Search Trees.

##### O(sqrt(n))

Es un tipo de tiempo bastante raro de ver, Prime dice que se lo ha encontrado una vez en un único problema.

## Search (Linear Search)

La búsqueda lineal o linear search es una de las búsquedas más comunes que podemos hacer en estructuras como arrays, ya que funciones como **indexOf** la utilizan.

El funcionamiento y razonamiento es sencillo, tenemos una función a la que le pasamos un array y un valor que queremos buscar en este. Esta función se encargará de recorrer el array, índice a índice y preguntarle a este si el valor que queremos buscar se corresponde con el que ha encontrado en ese índice.

### Tiempo de ejecución

El peor caso posible que tenemos sería que el valor que buscamos no se encuentre en el array, lo que nos daría **O(n)**.

## Binary Search

Como habíamos visto anteriormente, en la búsqueda lineal, recorríamos todo el array para buscar el valor que necesitamos, lo que nos llevaba a un tiempo de ejecución **O(n)** en el peor de los casos.

En la búsqueda binaria o binary search, el peor de los casos nos dará un **O(log n)**, lo cual es una mejora en rendimiento.

¿Y por qué? Pues imagínate un array ordenado que tiene N tamaño. En la búsqueda lineal iríamos desde la posición inicial (x0), hasta la final. En la binaria no, ya que accederemos desde el valor medio del array, es decir, iremos a la posición N/2, lo que nos dirá si el elemento estará más a la izquierda o a la derecha, lo que nos permite dividir nuestro campo de búsqueda a la mitad.

Imagina que el array tiene 4096 elementos, cada paso de búsqueda cortaría el array a la mitad, siendo así 2048, 1024, 512, 256, 128, 64, 32, 16, 8, 4, 2, 1. Es decir, hay una serie de pasos hasta llegar a 1, en este caso 12, ya que el logaritmo en base 2 de 4096 es 12 (**log2 4096 = 12**).





---
# Backlinks

```dataview
LIST
FROM [[]]
SORT file.name ASC
```
