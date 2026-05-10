
## Algo 1

```
x = n ;               // 1
while ( x > 0 ) {     // O(log2 n) car / 2
	y = n ;           // 1
	while ( y > 0 ) { // O(n)
		y = y – 1 ;   // 1
	}                 
	x = x / 2 ;       // 1
}
```
$$
T(n) = \sum_{i=1}^{\log_{2}{n}} \mathcal{O}(n) = \mathcal{O}(n) * \mathcal{O}(\log{n}) = \mathcal{O}(n\log{n})
$$


## Algo 2

```
x = n ;               // 1
while ( x > 0 ) {     // O(log n)
	y = x ;           // 1
	while ( y > 0 ) { // O(log2(n) + 1)
		y = y – 1 ;   // 1
	}
	x = x / 2 ;       // 1
}
```

$$
T(n) = \mathcal{O}(n)
$$