---
num: "Lecture 16"
desc: "Priority Queues, MinHeaps"
ready: true
lecture_date: 2024-02-29 14:00:00.00-7:00
---

[Slides folder]({{ site.slides_url }})

# Trees Recap

* Node: An element in the tree. May have an incoming edge and may have outgoing edges
* Edge: A connection between nodes (can be directional or bidirectional)
* Root: The top most node (will have no incoming edges)
* Path: The sequence of nodes from one node to a destination node along the tree
* Children: Nodes that have incoming edges from another node
* Parent: Contains outgoing edges to other child nodes
* Siblings: Nodes that have the same parent
* Subtree: A tree structure where the root of the tree is a child of the parent
* Leaf: A node without any outgoing edges
* Level: Number of edges from the root node to a destination node
* Height: Maximum level of the entire tree

  ## Tree Properties
  
    * Binary Tree: Any node has at most 2 children
    * Balanced Binary Tree: The left and right subtrees of every node differ in height by no more than 1
    * Complete Tree: A binary tree where every level of the tree, except the deepest, must contain as many nodes as possible. The deepest level must have all nodes to the left as possible.

# Priority Queues

* In a normal queue structure, we can insert elements from the back and remove elements from the front
    * The order of elements in the queue were simply dictated by the order in which they were inserted
* **Prioirty queues** are similar, but items now have some value representing their *priority*, and items are ordered in the queue with respect to this value
* One way to implement this is with a heap structure

# MinHeaps

* **MinHeap:** A complete tree where the value of a node is never greater than the value of its children)

![minHeap](https://github.com/ucsb-cs9/w24/assets/122947907/34f329f7-9bdd-422a-83c4-fea3048d9ead)

* Since MinHeaps are complete trees, we can implement them using a Python list (filled left to right on the bottom layer)
    * The root of the binary heap is at index 1 (index 0 is meaningless, can be set to None and will be unused)
* You can find a node's parent/children based on its index as follows:
    * **Parent:** node_index // 2
    * **Left Child:** 2 * node_index
    * **Right Child:** 2 * node_index + 1

![heapList](https://github.com/ucsb-cs9/w24/assets/122947907/31ef2212-c7e3-4393-a37e-f0b5cbcdee81)

  ## Insertion

  * When inserting into a MinHeap, you must keep 2 properties true: the fact that a heap is a complete tree, and the fact that the value of a node is never greater than the value of its children.
  * So, inserting is done with the following steps:
  1. Insert the element in the first available location (keep tree complete)
  2. While the element's parent is less than the element, swap with the parent
   
  * This will run in O(log n) time because the height of the tree is log n
    
  ![minHeapInsert](https://github.com/ucsb-cs9/w24/assets/122947907/c54dc1e4-91d5-4eb0-a2df-c826b81d3ee2)

  ## Deletion

  * In the context of priority queues, the only element we will want to remove from a MinHeap is the root node (minimum value)
  * Deleting the min node is done as follows:
  1. Remove the root
  2. Move the rightmost element in the last layer into the root position
  3. While the new root is less than one of its children, swap the minimum child with the parent
  4. Return the original root element
   
  * Again, this will run in O(log n) time because the height of the tree is log n

  ![minHeapDelete](https://github.com/ucsb-cs9/w24/assets/122947907/c1195979-952a-4d70-9acf-94dd1aef0348)
