24-03-2025 18:24
Status: #idea
Tags: [[Programming Core]]

# Data Structures

- [[#Arrays|Arrays]]
- [[#Queues o colas|Queues o colas]]
- [[#Stacks o pilas|Stacks o pilas]]
- [[#Manejo de estructuras en memoria|Manejo de estructuras en memoria]]
- [[#Linked Lists|Linked Lists]]
- [[#Trees|Trees]]
	- [[#Trees#Binary Search Tree|Binary Search Tree]]
- [[#Dictionaries|Dictionaries]]
- [[#Hashing and Hash Tables|Hashing and Hash Tables]]
	- [[#Hashing and Hash Tables#Hash Table|Hash Table]]
- [[#Tries (Radix/Prefix trees)|Tries (Radix/Prefix trees)]]
- [[#Ejercicios|Ejercicios]]
	- [[#Ejercicios#Stacks|Stacks]]
		- [[#Stacks#Min Stack|Min Stack]]


Las estructuras de datos, como su nombre indica, nos permiten estructurar datos de una aplicación, y así manejar su uso.

Cuando pensamos en estructuras de datos, podemos caer en la tentación de pensar que son inmutables, es decir, algo que es muy complicado de romper. Sin embargo, esto no es así, ya que, en un nivel más abstracto, las estructuras de datos se definen como modelos conceptuales (como listas, pilas o colas), y su implementación concreta puede variar según el lenguaje o el caso de uso, permitiendo distintos grados de flexibilidad.

Vamos a empezar viendo estructuras como arrays, queues, pilas, etc.

## Arrays

¿Sabías que **const a = []** no es un array ``técnicamente``? En esta sección vamos a ver un repaso sobre los arrays, curiosidades y como podemos indagar en ellos.

En si un array es un espacio contiguo de memoria, en el que almacenamos datos interpretados por el compilador. Cuando accedemos a un espacio concreto de un array, por ejemplo, **`a[0]`**, le estamos diciendo a nuestro ordenador, busca dentro del slot de memoria que tiene reservado el array, el primer elemento. 

Por ejemplo, en Node, si creamos un **ArrayBuffer**, nos dará algo así:

```node
// Creamos un ArrayBuffer de 6 espacios
> const a = new ArrayBuffer(6);

> a
ArrayBuffer { [Uint8Contents]: <00 00 00 00 00 00>, byteLength: 6}
```

Como vemos son 00, que podríamos modificar según queramos, y nos mostraría los valores en decimal (2d es 45 en decimal):

```node
> const a8 = new Uint8Array(a)

> a8[0] = 45;

> a

ArrayBuffer { [Uint8Contents]: <2d 00 00 00 00 00>, byteLength: 6} 

> a8[2] = 45


> a

ArrayBuffer { [Uint8Contents]: <2d 00 2d 00 00 00>, byteLength: 6} 

> const a16 = new Uint16Array(a)

> a16[2] = 0x4545

17733

> a

ArrayBuffer { [Uint8Contents]: <2d 00 2d 00 45 45>, byteLength: 6} 

```

Si usásemos un **Uint16Array**, sería en hexadecimal, por ejemplo, podríamos hacer **`a18[2]=0x4545`**.

El tiempo de ejecución de lectura, inserción y borrado es tiempo constante (**O(1)**).

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

La estructura base para crear un nodo (elemento de la lista) en C, y crear una lista sería la siguiente: 

```c
#include <cs50.h>
#include <stdio.h>
#include <stdlib.h>
//Definición de un node para la lista
typedef struct node
{
	int number;
	struct node *next;
} node;

int main(void) {
	node *list = NULL;

	for (int i = 0; i < 3; i++) {
		// Asignamos el bloque de memoria al nuevo node
		node *n = malloc(sizeof(node));
		if (n == NULL) {
			return 1;
		}

		n->number = get_int("Number: ");
		n->next = NULL;

		n->next = list;
		
		list = n;
	}

	// Pasa el tiempo

	// Usar variable ptr (pointer) para invertir la lista
 	node *ptr = list;

	while (ptr != NULL) {
		printf("%i\n", ptr->number);
		// Hacemos que ptr sea el siguiente nodo al que está apuntando
		ptr = ptr->next;
	}
}
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

class List {
	main() {
		list: Node | null = null;

		for (let i = 0; i < 3; i++) {
			let n = new Node(i);
			n.next = list;
			list = n;
		}

		let ptr: Node | null = list;
		while (ptr.next != null) {
			console.log(ptr.number);
			ptr = ptr.next;
		}
	}
}

const linkedList = new List();
linkedList.main();
```

A partir de aquí, tendremos que pensar en los tiempos de ejecución. Por ejemplo, el recorrido de una lista será **O(n)**, ya que tendremos que recorrer la lista de principio a fin. Pero sin embargo, con el código que hemos puesto, la inserción de un nuevo nodo es **O(1)**, ya que siempre introducimos el siguiente nodo al principio de la lista, lo que hace que no tengamos que recorrer toda la lista para añadir un nuevo nodo.

Si queremos que se introduzcan los elementos de nuestra lista al final, tendríamos que hacer estos cambios:

```c
#include <cs50.h>
#include <stdio.h>
#include <stdlib.h>
//Definición de un node para la lista
typedef struct node
{
	int number;
	struct node *next;
} node;

int main(void) {
	node *list = NULL;

	for (int i = 0; i < 3; i++) {
		// Asignamos el bloque de memoria al nuevo node
		node *n = malloc(sizeof(node));
		if (n == NULL) {
			//TODO: free any memory already malloc'd
			return 1;
		}

		n->number = get_int("Number: ");
		n->next = NULL;

		// Si la lista está vacía, ponemos el primer nodo
		if (list == NULL) {
			list = n;
		} else {
			// Si ya hay valores en la lista
			for (node *ptr = list; ptr != NULL; ptr = ptr->next) {
				// Si estamos al final de la lista
				if (ptr->next == NULL) {
					ptr->next = n;
					break;
				}
			}
		}
	}

	// Pasa el tiempo

	// Usar variable ptr (pointer) para invertir la lista
 	for (node *ptr = list; ptr != NULL; ptr = ptr->next) {
	 	printf("%i\n", ptr->number);
 	}
	return 0;
}
```

Y en TypeScript sería:

```typescript
class Node {
	number: number;
	next: Node | null;

	constructor(number: number, next: Node | null = null) {
		this.number = number;
		this.next = next;
	}
}

class List {
	main() {
		list: Node | null = null;

		for (let i = 0; i < 3; i++) {
			let n = new Node(i);
			
			if (list == null) {
				list = n;
			} else {
				for (let ptr = list; ptr != NULL; ptr = ptr.next) {
					if (ptr.next == NULL) {
						ptr.next = n;
						break;
					}
				}
			}
		}

		let ptr: Node | null = list;
		while (ptr.next != null) {
			console.log(ptr.number);
			ptr = ptr.next;
		}
	}
}

const linkedList = new List();
linkedList.main();
```

Ahora que hemos cambiado la lista para que los elementos nuevos vayan al final, nos encontramos con que ha aumentado el tiempo de ejecución de nuestro programa, ya que pasamos de **O(1)** a **O(n)** al tener que recorrer toda la lista para insertar un nuevo nodo.

Una vez estemos listos para liberar la memoria en C, tendremos que hacer lo siguiente:

```c
node *ptr = list;
while (ptr != NULL) {
	node *next = ptr->next;
	free(ptr);
	ptr = next;
}

return 0;
```

Con este trozo de código lo que haremos será liberar la memoria que estaba asociada a ptr, usando un puntero temporal llamado next, ya que si intentamos acceder directamente a ptr->next después de liberarlo en memoria, tendremos un error, ya que el sistema operativo puede decidir usar esa memoria justo después de que la liberemos.

## Trees

Los trees son estructuras de datos muy usadas en computación, que tienen una forma peculiar, como de un árbol familiar, es decir, desde un nodo inicial, a varios nodos subsecuentes hacia abajo. 

### Binary Search Tree

En estructuras como los arrays o las listas enlazadas, la búsqueda binaria no siempre es eficiente o posible. En una **linked list**, por ejemplo, no podemos acceder directamente a la mitad de la estructura, ya que tendríamos que recorrerla nodo por nodo.  

Un **árbol binario de búsqueda (BST)** permite realizar búsquedas eficientes dividiendo la estructura en mitades en cada paso, gracias a su organización jerárquica.

Imagina que tienes un array con los siguientes elementos: `[1,2,3,4,5,6]`, podríamos crear un árbol que vaya dividiendo las mitades de este array, por ejemplo, la raíz del árbol sería el valor intermedio, en este caso el 4, después tendremos el número 2 y 6, y posteriormente, los números 1 y 3, y 5 y 7, quedando un árbol de la siguiente manera:

![[Pasted image 20250326192052.png]]

Y que pasaría si queremos buscar el número 5 en concreto? En la linked list y en un array, tendríamos que recorrerlo todo hasta encontrarlo, pero con el árbol podríamos partir de la raíz, y aplicar el siguiente razonamiento:

- Si es mayor a la raíz, nos iremos al pointer del siguiente elemento, lo que nos llevará al 6.
- ¿5 es mayor que 6? No, entonces el 5 estará en el siguiente nivel, señalado por el pointer del número menor que 6. También se podría dar el caso de que 5 no exista y sea null.

Un ejemplo en C y TypeScript de como sería la estructura de un node de un tree sería la siguiente:

```c
typedef struct node {
	int number;
	struct node *left;
	struct node *right;
} node;
```

```typescript
class Node {
	number: number;
	left: Node | null;
	right: Node | null;

	constructor(number: number, left: Node | null, right: Node | null) {
		this.number = number;
		this.left = left;
		this.right = right;
	}
}
```

En cuanto a eficiencia y a tiempo de ejecución, los binary trees nos van a dar un tiempo de ejecución **O(log n)**, que es mucho mejor que **O(n)**, ¿pero cuál es la desventaja de estos árboles? Pues que consume más memoria, al pasar de arrays a linked lists ya consumimos más memoria, y al pasar de linked lists a árboles, consumiremos más aún.

Una implementación sencilla sería:

```c
/**
* Función que busca si un número existe en un binary tree
**/
bool search(node *tree, int number) {
	if (tree == NULL) {
		return false;
	// Si nuestro número es menor que la raíz
	} else if (number < tree->number) {
		return search(tree->left, number);
	// Si nuestro número es mayor que la raíz
	} else if (number > tree->number) {
		return search(tree->right, number);
	// Si nuestro número es igual que la raíz
	} else {
		return true;
	}
}
```

Puede que se den casos extremos en el que los árboles no estén balanceados y se parezcan mucho a linked lists, por lo tanto, siempre hay que intentar balancear los valores que se añaden.

## Dictionaries

Los diccionarios es algo con lo que estamos familiarizados los humanos, es una manera de vincular una palabra, con un valor, o su significado. Esto es así en todos los idiomas del mundo, y como no, en programación será lo mismo también.

Es una estructura de datos muy buena a parte de arrays y listas, que nos permitirá buscar el valor de una key que conozcamos.

Normalmente, el key, es un string, que podrá tener asociado un valor que sea un string, o otro tipo de datos.

En cuanto a términos de optimización, lo que nos estábamos encontrando a menudo es un tiempo de ejecución de **O(log n)**, que sería un tiempo bueno, comparado con otros tiempos como **O(n)**.

Pero hay una manera de llegar a un tipo de estructura de datos, que nos proporcione tiempo constante (**O(1)**), lo que sería el mejor tiempo que podemos conseguir.

## Hashing and Hash Tables

Esto que comentábamos lo podemos conseguir con funciones hash. Tenemos que pensar en las funciones hash, como funciones que toman lo que le pasemos como input, y estas lo transforman en un output.

Esto dará lugar a las hash tables, que es algo parecido a una combinación de un array con una linked list.

### Hash Table

Imagínate que quieres crear un diccionario con nombres y números de teléfono. Bien, para este tipo de problemas, podremos usar lo que se conoce como **bucketization**, que sería dividir el problema en **calderos** pequeños. Por ejemplo, los nombres siempre van a empezar por una letra, por lo tanto, podríamos crear 27 **buckets**, y dependiendo de la letra por la que empiece el nombre, meterlo en uno de esos calderos.

Usando el ejemplo anterior, ¿cuánto tiempo llevaría acceder a un nombre en concreto? Pues **tiempo constante**, ya que podemos acceder directamente al **bucket** donde se encuentre el nombre que queremos consultar.

Se puede presentar el problema de **collision**, ya que podremos encontrarnos con que dos nombres empiecen por la misma letra. Esto nos llevaría a volver al **tiempo lineal**, ya que tendríamos que buscar dentro de ese bucket donde está el nombre. En este caso, podríamos implementar una **linked list**.

![[Pasted image 20250331185212.png]]

¿Y que pasaría si pasa un caso raro en el que todos los nombres empiezan por la misma letra? Pues que volvemos a tiempo linear con **O(n)**.

Vamos a ver como podemos conseguir el tiempo constante usando una hash table y evitando estos problemas comunes.

```c
typedef struct node {
	char *name;
	char *number;
	struct node *next;
} node;

node *table[26];
```

En esta estructura que hemos hecho en C, podemos ver como cada nodo, tiene un nombre, un número, y un puntero al siguiente elemento, en este caso la siguiente persona con el nombre más cercano. También definimos la hash table como una tabla de 26 nodos.

Podemos rebajar la posibilidad de **collisions** creando más cubos, por ejemplo, si en vez de mirar por la primera letra, miramos por las primeras tres letras. El problema de este approach, sería que reservaríamos más memoria, además de que tendríamos combinaciones que no aparecerían nunca, como por ejemplo, aaa, o bbb o zzz.

Como vemos, no tenemos garantías de tiempo constante, ya que podemos caer en tiempo lineal con casos perversos, como que todos los nombres empiecen por las mismas letras.

## Tries (Radix/Prefix trees)

Podemos pensar en los tries como un árbol de arrays. 

Vamos a pensar en el anterior ejemplo, imagina que tienes un array horizontal de 27 espacios.

![[Pasted image 20250331194448.png]]

El funcionamiento de un try es sencillo: Para cada nivel del árbol, se usarán arrays subsecuentes. Por ejemplo, imagina que queremos insertar en la letra T, pues en la letra T habrá un pointer que nos lleve a otro array, en el que se encuentren los nombres que empiecen por la letra T. Si la segunda es O, nos llevará a los arrays en los que empiece por T y le siga la O, y así sucesivamente.

![[Pasted image 20250331194657.png]]

Esto puede seguir sucesivamente, imagínate que queremos llegar al nombre Toadette:

![[Pasted image 20250331194804.png]]

Esta estructura nos da un rendimiento bueno, ya que no importa el tamaño que tenga la estructura, ni cuantos arrays existan, tendremos siempre el mismo tiempo para insertar un nuevo nombre. El tiempo de búsqueda puede variar, ya que dependiendo de lo largo que sea el nombre, puede tardar más.

Es decir, esto nos da **O(1)**, tiempo constante, no lineal.

```c
typedef struct node {
	struct node *children[26];
	char *number;
} node;
```

El problema que traen estas estructuras, como hemos visto anteriormente, es la memoria que hay que asignarles, ya que habrá muchos arrays y elementos, y esto requiere más espacio.

## Ejercicios

### Stacks

#### Min Stack

Se nos plantea implementar una clase MinStack en tiempo constante:

- MinStack() inicializa el objeto.
- void push(int val) mete el elemento val en el stack.
- void pop() elimina el elemento que está más arriba en el stack.
- int top() devuelve el primer elemento del stack.
- int getMin() devuelve el valor mínimo del stack.

```typescript
class MinStack {
    array: number[] = [];
    minStack: number[] = [];
    
    constructor() {}

    push(val: number): void {
        this.array.push(val);

        if (this.minStack.length === 0 || val <= this.minStack[this.minStack.length - 1]) {
            this.minStack.push(val);
        }
    }


    pop(): void {
        const popped = this.array.pop();
        
        if (popped === this.minStack[this.minStack.length - 1]) {
            this.minStack.pop();
        }
    }

    top(): number {
        const length = this.array.length;
        return this.array[length - 1];
    }

  

    getMin(): number {
        return this.minStack[this.minStack.length - 1];
    }
}

/**

 * Your MinStack object will be instantiated and called as such:

 * var obj = new MinStack()

 * obj.push(val)

 * obj.pop()

 * var param_3 = obj.top()

 * var param_4 = obj.getMin()

 */
```

---
# Backlinks

```dataview
LIST
FROM [[]]
SORT file.name ASC
```
