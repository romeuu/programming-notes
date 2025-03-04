04-03-2025 18:59
Status: #idea
Tags:

# Leetcode

## Problemas de TypeScript:
### **Valid Parenthesis**:

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







---
# Backlinks

```dataview
LIST
FROM [[]]
SORT file.name ASC
```
