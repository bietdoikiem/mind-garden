# B-Tree fundamentals

## What's B-Tree
B-tree is a tree data structure that allows the performance of these following operations in amortized O(logN):
- Search
- Insertion
- Deletion

It is optimized for reading and writing large blocks of data. Its common use case are in database and file system.

## B-Tree rules
Important properties of B-Tree:
- B-Tree node has more than two children
- A B-Tree node may contain more than a single element (etc. Node.val = [2, 3])

Every B-Tree depends on a constant integer MINIMUM, which is used to determine how many elements are held in each node.
- **Rule 1:** The root can have as few as one element (it can be empty if root has not children). Every other node must have at least MINIMUM elements
- **Rule 2:** The maximum number of values/elements in a node is twice the value of MINIMUM
- **Rule 3:** The element/value of each B-Tree's node is store in a partially filled array, whose array's items are sorted in ascending order (smallest one first, or largest one last)
- **Rule 4:** The number of subtrees below the current node is always larger than the number of elements in the current node.
- **Rule 5:** For any leaf non-leaf node, the element at index i of the node is:
	1. is greater than all elements in subtree number i of the current node
	2. is less than all elements in the subtree number i+1 of current node
- **Rule 6:** Since B-Tree is a type of self-balance trees, every leaf node in B-Tree has the same depth, which avoids the unbalanced problem.

![B-Tree's Subtree structure](Attachments/Pasted%20image%2020220921105928.png)
*Figure 1.1 - Current node has more subtrees than its current number of elements*

![B-Tree's Node Values](Attachments/Pasted%20image%2020220921105946.png)
*Figure 1.2 - Each node is following Rule no. 5 as indicated above*

## References
1. https://www.cpp.edu/~ftang/courses/CS241/notes/b-tree.htm#:~:text=A%20B%2Dtree%20is%20a,in%20database%20and%20file%20systems.