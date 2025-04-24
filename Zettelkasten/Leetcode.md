04-03-2025 18:59
Status: #idea
Tags: [[Leetcode]]

- [[#Problemas de TypeScript:|Problemas de TypeScript:]]
	- [[#Problemas de TypeScript:#Valid Parenthesis|Valid Parenthesis]]
	- [[#Problemas de TypeScript:#Contains Duplicate|Contains Duplicate]]
	- [[#Problemas de TypeScript:#Valid Anagram|Valid Anagram]]
	- [[#Problemas de TypeScript:#Intersection of Two Arrays|Intersection of Two Arrays]]
	- [[#Problemas de TypeScript:#Reverse String|Reverse String]]
	- [[#Problemas de TypeScript:#First Unique Character in a String|First Unique Character in a String]]
	- [[#Problemas de TypeScript:#Length of Last Word|Length of Last Word]]
	- [[#Problemas de TypeScript:#Majority Element|Majority Element]]
	- [[#Problemas de TypeScript:#Most Frequent Even Element|Most Frequent Even Element]]
	- [[#Problemas de TypeScript:#Plus One|Plus One]]
	- [[#Problemas de TypeScript:#Search Insert Position|Search Insert Position]]
	- [[#Problemas de TypeScript:#Move Zeroes|Move Zeroes]]
	- [[#Problemas de TypeScript:#Binary Search|Binary Search]]

# Leetcode

## Problemas de TypeScript:
### Valid Parenthesis

Dada una cadena s que sólo contiene los caracteres '(', ')', '{', '}', '[' y ']', determine si la cadena de entrada es válida.

Una cadena de entrada es válida si:

- Los corchetes abiertos deben estar cerrados por el mismo tipo de corchetes.
- Los corchetes abiertos deben cerrarse en el orden correcto.
- Cada corchete cerrado tiene su correspondiente corchete abierto del mismo tipo.
 

```typescript
function isValid(s: string): boolean { 
	const stack: string[] = []; 
	const map = new Map<string, string>(
	[ 
		[")", "("],
		["]", "["],
		["}", "{"]
	]
	); 

	// Recorremos string char a char
	for (const char of s) {
		// Si el mapa tiene el char, quiere decir que es de cierre 
		if (map.has(char)) { 
			// Extraemos el último elemento del stack
			const top = stack.pop(); 
			// Si no se corresponde con el paréntesis de apertura...
			if (top !== map.get(char)) { 
				return false; 
			} 
		} else {
			// Si es un paréntesis de apertura lo metemos en el stack
			stack.push(char); 
		} 
	} 
	
	return stack.length === 0; }
```

Runtime: **Beats 80.19%**.

### Contains Duplicate

Dada una matriz de enteros nums, devuelve verdadero si algún valor aparece al menos dos veces en la matriz, y devuelve falso si cada elemento es distinto.

```typescript
function containsDuplicate(nums: number[]): boolean {
    const set = new Set<number>();
    
    for (let i = 0; i < nums.length; i++) {
        if (set.has(nums[i])) {
            return true;
        }
        set.add(nums[i]);
    }
    
    return false;
};
```

Runtime: **Beats 84.01%**.

### Valid Anagram

Dadas dos cadenas s y t, devuelve verdadero si t es un anagrama de s, y falso en caso contrario.

```typescript
function isAnagram(s: string, t: string): boolean {
	// Si tienen distintos tamaños no es un anagrama
    if (s.length !== t.length) return false;

    const map: Map<string, number> = new Map();

	// En el mapa añadimos la ocurrencia de s y sacamos la de t
    for (let i = 0; i < s.length; i++) {
        map.set(s[i], (map.get(s[i]) || 0) + 1);
        map.set(t[i], (map.get(t[i]) || 0) - 1);
    }

	// Si el mapa no está a 0, no es un anagrama
    for (const count of map.values()) {
        if (count !== 0) return false;
    }

    return true;
}
```

Runtime: **Beats 98.28%**.

### Intersection of Two Arrays

Dados dos arrays de enteros nums1 y nums2, devuelve un array de su intersección. Cada elemento del resultado debe ser único y puede devolver el resultado en cualquier orden.

```typescript
function intersection(nums1: number[], nums2: number[]): number[] {
    let uniqueItems = new Set(nums1);
    const commonOcurrences = new Set<number>();

    for (let num of nums2) {
        if (uniqueItems.has(num) && !commonOcurrences.has(num)) {
			commonOcurrences.add(num);  
        }
    }

    return Array.from(commonOcurrences);

};
```

Runtime: 0-1 ms. **Beats 100%**.

### Reverse String

Escribe una función que dé la vuelta a una string. La string que se proporciona es un array de caracteres s.

Deberás hacerlo modificando el array directamente con memoria extra 0(1).

```typescript
function reverseString(s: string[]): void {
	// Puntero izquierdo
    let left = 0;
    // Puntero derecho
    let right = s.length - 1;

    while (left < right) {
	    // Variable temporal para hacer el swap
        const temp = s[left];
        s[left] = s[right];
        s[right] = temp;
        left++;
        right--;
    }
};
```

Runtime: 0ms. **Beats 100%**.

### First Unique Character in a String

Dada una cadena s, encuentra el primer carácter no repetido en ella y devuelve su índice. Si no existe, devuelve -1.

```typescript
function firstUniqChar(s: string): number {
    const map: Map<string, number> = new Map<string, number>();

    for (const char of s) {
        map.set(char, (map.get(char) || 0) + 1);
    }

    for (let i = 0; i < s.length; i++) {
        if (map.get(s[i]) == 1) {
            return i;
        }
    }

    return -1;

};
```

Runtime: 29ms. **Beats 76.34%**.

### Length of Last Word

Dada una string s conteniendo palabras y espacios, devuelve el tamaño de la última palabra en la string.

```typescript
function lengthOfLastWord(s: string): number {
    const arrayString: string[] = s.trim().split(" ");

    return arrayString[arrayString.length - 1].length;
};
```


### Majority Element

Dado un array nums del tamaño n, devuelve el elemento que aparece más veces.

El elemento que más aparece es el elemento que aparece más de [n/2]. Puedes asumir que el elemento mayoritario existirá siempre en el array.

Para este problema podemos usar el algoritmo de Boyer-Moore Majority Voting.

```typescript
function majorityElement(nums: number[]): number {
    let candidate = nums[0];
    let count = 0;

    for (const num of nums) {
        if (count === 0) {
            candidate = num;
        }
        count += (num === candidate) ? 1 : -1;
    }
    
    return candidate;
}
```

Runtime: 3ms. **Beats 71.47%**.

### Most Frequent Even Element

Dada un array de enteros nums, devuelve el elemento par más frecuente.

Si hay empate, devuelve el más pequeño. Si no hay tal elemento, devuelve -1.

```typescript
function mostFrequentEven(nums: number[]): number {

    const map = new Map<number, number>();

    // Contamos solo los números pares
    for (const num of nums) {
        if (num % 2 === 0) {
            map.set(num, (map.get(num) || 0) + 1);
        }
    }

    let result = -1;
    let maxFrequency = 0;

    for (const [key, value] of map.entries()) {
        if (value > maxFrequency || (value === maxFrequency && key < result)) {
            maxFrequency = value;
            result = key;
        }
    }

    return result;

}
```

Runtime: 6ms. **Beats 95%**.

### Plus One

Se le da un número entero grande representado como un array de dígitos, donde cada dígitos[i] es el i-ésimo dígito del número entero. Los dígitos están ordenados de mayor a menor importancia de izquierda a derecha. El entero grande no contiene ningún 0 a la izquierda.

Incrementa el entero grande en uno y devuelve el array de dígitos resultante.

```typescript
function plusOne(digits: number[]): number[] {
    const lastElement = digits[digits.length - 1];

    for (let i = digits.length - 1; i >= 0; i--) {
        if (digits[i] < 9) {
            digits[i] = digits[i] + 1;
            return digits;
        } else {
            digits[i] = 0;
        }
    }
    
    return [1, ...digits];
};
```

Runtime: 0ms. **Beats 100%**.

### Search Insert Position

Dada un array de enteros distintos y un valor objetivo, devuelve el índice si se encuentra el objetivo. Si no, devolver el índice donde estaría si se insertara ordenado.

Debe escribir un algoritmo con complejidad de ejecución O(log n).

```typescript
function searchInsert(nums: number[], target: number): number {
    let left = 0;
    let right = nums.length - 1;

    while (left <= right) {
        let mid = Math.floor((left + right) / 2);
        
        if (nums[mid] === target) {
            return mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
  
    return left;
}
```

Runtime: 0ms. **Beats 100%**.

### Move Zeroes

Se nos da un array de números, y necesitamos mover in-place los 0 al final del array sin hacer copias del array.

Esto nos limita a no usar métodos nativos de JavaScript/TypeScript y usar punteros.

Razonamiento: Usamos el puntero writeIndex para indicar en que posición tenemos que poner nuestros elementos. Si `nums[i]` es distinto de 0, hacemos un swap de `nums[i]` con `nums[writeIndex]` e incrementamos el puntero writeIndex.

```typescript
function moveZeroes(nums: number[]): void {
    let writeIndex = 0;

    for (let i = 0; i < nums.length; i++) {
        if (nums[i] !== 0) {
            [nums[i], nums[writeIndex]] = [nums[writeIndex], nums[i]];
            writeIndex++;
        }
    }
};
```

Este approach es bueno siempre que quieras cambiar elementos de sitio en un array, ya que no se crean copias y es bastante eficiente.

Sería lo equivalente a hacer algo así:

```typescript
let temp = nums[i];
nums[i] = nums[writeIndex];
nums[writeIndex] = temp;
```


### Binary Search

Se nos pide que se implemente Binary Search, y que si se encuentra el elemento se devuelva el índice, si no que se devuelva -1.

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


---
# Backlinks

```dataview
LIST
FROM [[]]
SORT file.name ASC
```
