An idea of which method to solve certain problem is the best

- It’s important to have a precise vocabulary to talk about how the code performs
- Useful of discussing trade-offs between different approaches
- When the code slows down or crashes, identifying parts of the code that are inefficient can help us find pain points in our applications

Suppose we want to write a function that calculates the sum of all numbers from 1 up to (and including) some number *n*.

```javascript
function addUpTo(n){ 
	let total = 0;
	for (let i = 0; i <= n; i++){
		total += i;
	} 
	return total;
}
addUpTo(4); // 10 (1 + 2 + 3 + 4)
```

Here’s a more concise way.

```javascript
function addUpTo(n){
	return n * (n + 1) / 2;
}
```

Which one is better????? Then, what does “better” mean?? Faster? Less memory-intensive? More readable?

![[Pasted image 20230929114241.png]]

## Time Complexity

[[Time Complexity]]

## Space Complexity (in JS)

[[Space Complexity]]

## Logarithmic

[[Logarithmic]]




