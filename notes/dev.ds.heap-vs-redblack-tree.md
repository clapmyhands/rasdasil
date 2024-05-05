---
id: e225ax568ze2hr4f6ujqukl
title: Heap Vs Redblack Tree
desc: ''
updated: 1714896124280
created: 1714895152679
tags: datastructure
---

[Why use heap over red-black tree?](https://cs.stackexchange.com/questions/105899/why-use-heap-over-red-black-tree)

heap and rbt have similar insert/remove property
finding element in heap is O(n) while rbt is O(lg n)
rbt takes more memory footprint (more poi   nters) and bookkeeping on the structure itself but can be sparse
while heap usually use array which have dynamic resize cost but clustered in memory
can use treemap for heap operation (get min/max)

