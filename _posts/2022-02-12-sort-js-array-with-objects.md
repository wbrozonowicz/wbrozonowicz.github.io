---
title: "JavaScript arrays with objects - how to sort"
excerpt: "Sorting Js arrays by key"
last_modified_at: 2022-01-21T10:27:01-05:00
categories:
  - JavaScript
tags: 
  - basics
---

<!-- short introduction -->
## JavaScript arrays with objects - how to sort

To sort Js array of objects by key (for example field "id") use below method:

{% include code-header.html %}
```js
	array.sort(function(a, b) {
			var keyA = a.id,
			  keyB = b.id;
			if (keyA < keyB) return -1;
			if (keyA > keyB) return 1;
			return 0;
		  });
```

Shorter way (array with numbers as example):
{% include code-header.html %}
```js
	const nums = [2,4,3];
	nums.sort((a,b)=>a-b)  // ascending
	nums.sort((a,b)=>b-a)  // descending
```

.sort() method will return new array (after sorting) but also will change array, on which We use it.


Notes:
- sort() method by default compare two values from array converted to string. This is by default, when We don't implement any function inside. In case of objects it will not sort them - all objects will be converted to "[object Object]" f.e. String({a:2}) and comparison alway will return 0.
- inside sort() We should define function that return number > 0 , < 0 or ===0. Based on that sort() will change index of two elements (or if ===0 it will not change it)
- When sorting strings, big letters ("A") are smaller in comparison than small letters ("a"). After sort of ["a", "Z"] We will get ["Z", "a"]. To avoid this use .toLowerCase() function inside internal function in sort(), for example:


{% include code-header.html %}
```js
	array.sort((a, b)  => {
			a.text = a.text.toLowerCase();
			b.text = b.text.toLowerCase();
			if (a < b) return -1;
			if (a > b) return 1;
			return 0;
		  });
```



That's all!



