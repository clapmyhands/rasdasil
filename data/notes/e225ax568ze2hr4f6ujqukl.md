
[Why use heap over red-black tree?](https://cs.stackexchange.com/questions/105899/why-use-heap-over-red-black-tree)

heap and rbt have similar insert/remove property
finding element in heap is O(n) while rbt is O(lg n)
rbt takes more memory footprint (more poi   nters) and bookkeeping on the structure itself but can be sparse
while heap usually use array which have dynamic resize cost but clustered in memory
can use treemap for heap operation (get min/max)

