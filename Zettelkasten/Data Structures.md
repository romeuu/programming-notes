24-03-2025 18:24
Status: #idea
Tags: [[Programming Core]]

# Data Structures

- [[#Queues o colas|Queues o colas]]
- [[#Stacks o pilas|Stacks o pilas]]
- [[#Manejo de estructuras en memoria|Manejo de estructuras en memoria]]
- [[#Linked Lists|Linked Lists]]



Las estructuras de datos, como su nombre indica, nos permiten estructurar datos de una aplicación, y así manejar su uso.

Cuando pensamos en estructuras de datos, podemos caer en la tentación de pensar que son inmutables, es decir, algo que es muy complicado de romper. Sin embargo, esto no es así, ya que, en un nivel más abstracto, las estructuras de datos se definen como modelos conceptuales (como listas, pilas o colas), y su implementación concreta puede variar según el lenguaje o el caso de uso, permitiendo distintos grados de flexibilidad.

Vamos a empezar viendo estructuras como queues, pilas, etc.

## Queues o colas

Cuando pensamos en queues, pensamos en filas de algo ordenadas, es decir, una cola normal para pagar en un supermercado o para ir al cine, normalmente por orden de llegada.

Esto nos lleva al siguiente concepto, llamado **FIFO** (**First in, First out**), es decir, el primero que entra, es el primero que sale.

Esta estructura de datos tendría dos funcionalidades básicas, **enqueue**, y 
**dequeue**, que nos permitirían añadir y sacar elementos de la cola.

```typescript
const MAX_CAPACITY: number = 50;

class Queue<Person> {
	private persons: Person[];
	private readonly capacity: number;

	constructor(item: Person, capacity: number = MAX_CAPACITY): void {
		this.persons = [];
		this.capacity = capacity;
	}

	enqueue(item: Person): void {
		if (this.persons.length < this.capacity) {
			this.persons.push(item);
		}
	}

	dequeue(): Person | undefined {
		return this.persons.shift();
	}
}
```

Como vemos, la cola tiene una capacidad, que siempre va a ser la misma, ya que es una constante, y comprobamos que mientras las personas totales (size) sea menor que la capacidad, se pueda seguir metiendo personas en la cola.

Es importante distinguir entre size y capacidad, ya que el size es el número actual de personas en la cola, y la capacidad siempre va a ser el número máximo de personas en la cola, por eso mismo se define como const.

En lenguajes como C, se tiene que seguir el valor del size, ya que podríamos tener 50 elementos de "basura" en memoria. Pero en otros, como TypeScript, este manejo es automático.

## Stacks o pilas

Si una cola estaba ordenada por el concepto **FIFO**, los stacks son todo lo contrario, ya que el último elemento en entrar, será el primero en salir, lo que se llamaría **LIFO** (**Last In, First Out**).

Cuando pienses en esta estructura de datos, piensa en cosas apilables, como un montón de ropa. Si tienes toda tu ropa en una silla, siempre usas la prenda de ropa que está encima, ya que es la última que pusiste, mientras que las demás permanecen en el fondo.

O piensa en tu email, cualquier cliente de email, pone los emails más recientes arriba, siendo así una pila.

En los stacks, tendremos dos funcionalidades básicas, **push** y **pop**, básicamente, añadir algo al final de la lista y borrar el primer elemento de esta.

```typescript
const MAX_CAPACITY: number = 50;

class Stack<Item> {
	private items: Item[];
	private readonly capacity: number;

	constructor(item: Item, capacity = MAX_CAPACITY) {
		this.items = [];
		this.capacity = capacity;
	}

	push(item: Item) {
		if (items.length < this.capacity) {
			this.items.push(item);
		}
	}

	pop() {
		if (items.length > 0) {
			return this.items.pop();
		}
	}
}
```

## Manejo de estructuras en memoria

En lenguajes como TypeScript, no se necesita hacer un manejo exclusivo de la memoria. Es decir, si tienes un array, y quieres aumentar el tamaño de este, lo puedes hacer sin problema. Pero, ¿qué pasa en lenguajes como C?

Pues en lenguajes en los que se tiene que manejar la memoria, es más complicado, ya que tendríamos que copiar el array y moverlo a un slot de memoria en el que "quepa" este array, por así decirlo, ya que si no, corremos el riesgo de pisar otras variables o funciones que estén directamente después de nuestro array.

```c
#include <stdio.h>
#include <stdlib.h>

int main(void) {
	int *list = malloc(3 * sizeof(int));

	if (list == NULL) {
		free(list);
		return 1;
	}

	list[0] = 1;
	list[1] = 2;
	list[2] = 3;

	// Time passes, what if we want the array to grow?

	int *tmp = malloc(4 * sizeof(int));
	if (tmp == NULL) {
		return 1;
	}

	for (int i = 0; i < 3; i++) {
		tmp[i] = list[i];
	}

	tmp[3] = 4;

	free(list);

	list = tmp;

	// Time passes by and we liberate the memory of the original list
	free(list);
}
```

Como vemos, la lista con nombre list tiene reservada en memoria un chunk de 3 espacios exactamente. ¿Qué sucede si queremos que crezca el array? Pues tendríamos que crear un array nuevo, al que le asignaremos 4 slots, y posteriormente, pasaremos los valores de list a tmp. Y por último, vaciaremos la memoria que tenía el array original, y reasignaremos a la variable que teníamos antes.

Esto sería una manera manual de hacerlo, pero C nos proporciona funciones como **realloc**, que nos permitiría aumentar el bloque de memoria en caso de que exista un espacio contiguo disponible. ¿Y qué pasaría si no existe? Pues copiaría los datos a un nuevo bloque, y liberaría el anterior.

```c
#include <stdio.h>
#include <stdlib.h>

int main(void) {
	int *list = malloc(3 * sizeof(int));

	if (list == NULL) {
		free(list);
		return 1;
	}

	list[0] = 1;
	list[1] = 2;
	list[2] = 3;

	// Time passes, what if we want the array to grow?

	int *tmp = realloc(list, 4 * sizeof(int));
	if (tmp == NULL) {
		return 1;
	}

	tmp[3] = 4;

	free(list);

	list = tmp;

	// Time passes by and we liberate the memory of the original list
	free(list);
}
```

Y ahora que sabemos esto, podemos pasar a las **Linked Lists**, que serían estructuras de datos dinámicas. A diferencia de los arrays, no necesitan un bloque de memoria contiguo, ya que cada nodo contiene un puntero que indica la dirección del siguiente nodo en la lista. Esto permite que los elementos se almacenen en diferentes partes de la memoria sin necesidad de mover la estructura completa al redimensionar.

## Linked Lists

El fundamento de las linked lists sería vincular elementos entre si, que no tienen porque estar en sitios contiguos de memoria, pero que sepan cual es el siguiente elemento en la lista. Es decir, si tenemos estos elementos:

- Un elemento 1 con dirección de memoria 0x123.
- Un elemento 2 con dirección de memoria 0x456.
- Un elemento 3 con dirección de memoria 0x789.

En cada uno de estos elementos se almacenará un **pointer** o **puntero**, que apuntará al siguiente elemento. Por ejemplo, el elemento 1 tendrá su dirección de memoria, pero el puntero indicará la dirección 0x456, que sería la dirección correspondiente al número 2, y así sucesivamente hasta que el puntero sea nulo, lo que indicaría que llegamos al último elemento.

La estructura base para crear un nodo (elemento de la lista) en C sería la siguiente: 

```c
typedef struct node
{
	int number;
	struct node *next;
} node;
```

En TypeScript sería de la siguiente manera:

```typescript
class Node {
	number: number;
	next: Node | null;

	constructor(number: number, next: Node | null = null) {
		this.number = number;
		this.next = next;
	}
}
```




---
# Backlinks

```dataview
LIST
FROM [[]]
SORT file.name ASC
```
