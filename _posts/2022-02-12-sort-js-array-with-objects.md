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

That's all!



