04-04-2025 18:29
Status: #idea
Tags: [[Programming Core]]

# Algorithms

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




---
# Backlinks

```dataview
LIST
FROM [[]]
SORT file.name ASC
```
