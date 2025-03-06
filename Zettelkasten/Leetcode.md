04-03-2025 18:59
Status: #idea
Tags: [[Leetcode]]


- [[#Problemas de TypeScript:|Problemas de TypeScript:]]
	- [[#Problemas de TypeScript:#Valid Parenthesis:|Valid Parenthesis:]]

# Leetcode

## Problemas de TypeScript:
### Valid Parenthesis:

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

## Contains Duplicate

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

## Valid Anagram

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

## Intersection of Two Arrays

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


---
# Backlinks

```dataview
LIST
FROM [[]]
SORT file.name ASC
```
