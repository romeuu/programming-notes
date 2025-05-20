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
- [[#Search|Search]]
	- [[#Search#Linear Search|Linear Search]]
		- [[#Linear Search#Tiempo de ejecución|Tiempo de ejecución]]
	- [[#Search#Binary Search|Binary Search]]
		- [[#Binary Search#Implementación|Implementación]]
			- [[#Implementación#Pseudocódigo|Pseudocódigo]]
			- [[#Implementación#TypeScript|TypeScript]]
		- [[#Binary Search#Two Crystal Ball Problem|Two Crystal Ball Problem]]
			- [[#Two Crystal Ball Problem#Implementación|Implementación]]





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

## Search

### Linear Search

La búsqueda lineal o linear search es una de las búsquedas más comunes que podemos hacer en estructuras como arrays, ya que funciones como **indexOf** la utilizan.

El funcionamiento y razonamiento es sencillo, tenemos una función a la que le pasamos un array y un valor que queremos buscar en este. Esta función se encargará de recorrer el array, índice a índice y preguntarle a este si el valor que queremos buscar se corresponde con el que ha encontrado en ese índice.

#### Tiempo de ejecución

El peor caso posible que tenemos sería que el valor que buscamos no se encuentre en el array, lo que nos daría **O(n)**.

### Binary Search

Como habíamos visto anteriormente, en la búsqueda lineal, recorríamos todo el array para buscar el valor que necesitamos, lo que nos llevaba a un tiempo de ejecución **O(n)** en el peor de los casos.

En la búsqueda binaria o binary search, el peor de los casos nos dará un **O(log n)**, lo cual es una mejora en rendimiento.

¿Y por qué? Pues imagínate un array ordenado que tiene N tamaño. En la búsqueda lineal iríamos desde la posición inicial (x0), hasta la final. En la binaria no, ya que accederemos desde el valor medio del array, es decir, iremos a la posición N/2, lo que nos dirá si el elemento estará más a la izquierda o a la derecha, lo que nos permite dividir nuestro campo de búsqueda a la mitad.

Imagina que el array tiene 4096 elementos, cada paso de búsqueda cortaría el array a la mitad, siendo así 2048, 1024, 512, 256, 128, 64, 32, 16, 8, 4, 2, 1. Es decir, hay una serie de pasos hasta llegar a 1, en este caso 12, ya que el logaritmo en base 2 de 4096 es 12 (**log2 4096 = 12**).


> [!TIP] Truco de Big O
> Normalmente, cuando el campo de búsqueda se reduce en mitades, resultará en **O(log n) / O(NlogN)**

#### Implementación

Prime en este caso usa rangos semiabiertos, es decir, `[low, high)`, en vez de usar `[low, high]`, esto nos ayuda a no tener que estar preguntando todo el rato si `low <= high` y simplemente preguntar si `low < hi`.

Esto hace que se prevengan errores out of bound y que el algoritmo sea más limpio.

##### Pseudocódigo

Sabemos que tendremos una función que nos pasará un array, el valor mínimo por el que tenemos que buscar, el valor máximo, y un valor esperado que llamaremos n (needle).

Lo primero que tendremos que hacer será establecer el punto medio de búsqueda para hacer el primer barrido:

```text
do {
m = (low + (high - low) / 2)
v = arr[m]

if (v == n)
	return true
elseif v > n
	low = m + 1
else
	high = m

} while (lo < hi)

return false
```

Esto nos permitirá tener en v el valor del elemento del medio del array y actuar en función de si es mayor o menor al valor que buscamos.

> [!DANGER] ¿Por qué usar (low + (high - low) / 2) en vez de (low + high) / 2?
> En lenguajes como Java, C o C++, si `low + high` supera el límite del tipo de dato (por ejemplo, `int`), puede causar un **overflow** y devolver un valor incorrecto.
>  Con `low + (high - low) / 2`, primero restamos (número pequeño), luego dividimos, y por último sumamos, evitando ese problema.

##### TypeScript

```typescript
export default function bs_list(haystack: number[], needle: number): boolean {
    let low = 0;
    let high = haystack.length;
  
    do {
        const mid = Math.floor(low + (high - low) / 2);
        const value = haystack[mid];

        if (value === needle) {
            return true;
        } else if (value < needle) {
            // Ajustamos el low a la mitad + 1, ya que ya sabemos que la mitad no es igual al needle
            low = mid + 1;
        } else {
            // Si el valor es mayor que la needle, acotamos el array a la mitad
            high = mid;
        }
    } while(low < high);
    return false;
}
```

#### Two Crystal Ball Problem

Tenemos el siguiente problema:

> [!NOTE] Two Crystal Ball
> Dadas dos bolas de cristal que se romperán al tocar el suelo desde una altura determinada (piso de un edificio), encuentra el sitio exacto en el que se romperán de manera optimizada.

A priori, si tuviésemos una sola bola, podríamos realizar una búsqueda lineal, pero podríamos tener problemas para llegar a resolver el problema, ya que estaremos enfrentando directamente el peor caso posible, que sería que se rompan en los dos últimos pisos del edificio, lo que sería **O(N)**.

Por otra parte, podríamos empezar en la mitad y hacer una binary search, pero esto lo podríamos hacer si tenemos bolas infinitas, ya que si empezamos a descartarlas desde el medio, se nos romperán las dos y posiblemente no hayamos llegado a nuestro número objetivo. Y lo que pasaría sería que haríamos una búsqueda linear desde donde hemos encontrado el punto en el que se ha roto, y posiblemente sea la mitad del array, por lo tanto, tendríamos que recorrer **1/2** N, lo que si quitamos las constantes, sería **O(N)**.

El enfoque correcto sería hacer la raíz cuadrada del número de pisos que hay, y posteriormente, detectar donde se rompe la primera bola. Con esto nos dará un intervalo en el que podremos buscar de manera lineal, y encontraremos nuestro número objetivo de una manera optimizada. Esto nos dará un runtime de **O(sqrt N)**.

![[two-crystal-balls.png]]

##### Implementación

```typescript
export default function two_crystal_balls(breaks: boolean[]): number {
    // Determinamos el número de saltos que tenemos que dar
    const jumpAmount = Math.floor(Math.sqrt(breaks.length));

    let i = jumpAmount;

	// Arrancamos el bucle para encontrar el primer breakpoint
	// Y saltamos cada jumpAmount
    for (; i < breaks.length; i += jumpAmount) {
        if (breaks[i]) {
            break;
        }
    }

	// Retrocedemos al piso en el que no se rompió
	// Por ejemplo, sabemos que si se rompe en el 50, volvemos atrás al 40
    i -= jumpAmount;

	// Búsqueda linear hasta encontrar el piso donde se rompe
	// la segunda bola
    for(let j = 0; j <= jumpAmount && i < breaks.length; ++j, ++i) {
        if (breaks[i]) {
            return i;
        }
    }

    return -1;
}
```

## Sorting

### Bubble Sort

El principio del bubble sort es sencillo, se compara el número i con el elemento i+1, y si es mayor, se cambian de posición.

Con la primera iteración, siempre va a quedar el elemento más grande al final del array. Esto nos permite optimizar un poco más el algoritmo, ya que en la siguiente iteración, no tendríamos que mirar el último elemento, ya que sabemos que está ordenado. Esto se repetirá hasta que tengamos el array completado, ya que en la segunda iteración, el segundo elemento más grande, ya estará ordenado también.

![[Pasted image 20250520183600.png]]
*Imagen en la que se muestran las iteraciones del algoritmo*

![[Pasted image 20250520183649.png]]
*Explicación del runtime O(n^2 de este problema, ya que deriva de Gauss.*





---
# Backlinks

```dataview
LIST
FROM [[]]
SORT file.name ASC
```
