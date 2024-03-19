---
num: "Lecture 18"
desc: "Quicksort, Binary Search Tree Deletion"
ready: true
lecture_date: 2024-03-07 14:00:00.00-7:00
---

[Slides folder]({{ site.slides_url }})

# Quicksort
* Quicksort is another divide-and-conquer algorithm
* Can improve running times to **O(n log n)** in a typical case, but we’ll see how this can also lead to O(n<sup>2</sup>) in a worst-case scenario
* **Idea:**
  * We can sort a list by subdividing the list based on a **pivot** value
  	* Place elements < pivot to the left-side of the list
  	* Place elements > pivot right-side of the list
  	* Recurse for each left / right portion of the list
  	* When sub list sizes == 1, then entire list is sorted

## Partition the list into left/right sublists
1. Choose pivot (typically first element in list)
2. leftmark index is on left-most side of list, rightmark index is on right-most side of list, and both leftmark and rightmark work inwards
3. Find an element in the left side (using leftmark index) that’s out-of-place (> pivot)
4. Find an element in the right side (using rightmark index) that’s out-of-place (< pivot)
5. Swap out-of-place elements with each other
	* We’re putting them in the correct side of the list!
6. Continue doing steps 1 - 5 until the rightmark index < leftmark index
7. Swap the pivot with rightmark index
	* We’re putting the pivot element in the correct place!

## Example

![quicksort_example](https://github.com/ucsb-cs9/w24/assets/122947907/a34c0f83-0adf-4743-a4b5-8b6d763d2f6b)

# Quicksort Analysis

* Best-case running time is **O(n log n)**, just like Mergesort
	* In the best case, there are **log n** levels. Each level is **O(n)** when performing the partition step
* However, the worst case is **O(n<sup>2</sup>)**
	* Consider the case where the list is already sorted (or in reverse order)
	* The sub lists aren't evenly divided for every recursive call
	* Quick Sort performance is dependent on the pivot value!
	* Can try to improve the pivot choice by selecting random values and choosing the medium
	* Textbook describes the median of three approach
		* Choose first, middle, and last element. Choose the median of these values
		* But even then, there is no guarantee that these values are good pivot values, but it does improve our chances that they are
* Note that Quicksort DOES NOT need extra space (unlike merge sort), so that tradeoff is not relevant here

# BST Deletion

* When deleting a node from a BST, there are three possible cases based on the number of children:
    * **Case 1:** The node we are deleting is a leaf (no children)
    * **Case 2:** The node we are deleting has one child
    * **Case 3:** The node we are deleting has two children

## Case 1: Deleting a leaf node
* Find the node that needs to be deleted (traverse the bst)
* Simply remove the parent reference (either left child or right child) to the deleted node

![bstdelete_leaf](https://github.com/ucsb-cs9/w24/assets/122947907/535ff822-1aa4-4bee-9c29-62cd6bfac31a)

## Case 2: Deleting a node with 1 child
* Set parent reference of the deleted node's child to the deleted node's parent

![bstdelete_1child](https://github.com/ucsb-cs9/w24/assets/122947907/dfc8f923-4378-403b-b8cf-86014e181d90)



## Case 3: Deleting a node with 2 children
* First find the successor (node with next greater value) in the right subtree
	* This can be done by traversing the left children of the node to be deleted’s right subtree (the leftmost/least node in the deleted node's right subtree)
	* Also note that the successor will always only have **at most** one child
		* If the successor had a left child, then it wouldn’t be the successor!
* Temporarily store the successor and delete the successor from the tree
	* Deleting the successor will fall in either Case 1 or Case 2
* Replace the deleted node’s value with the successor’s value

![bstdelete_2chldren](https://github.com/ucsb-cs9/w24/assets/122947907/3e1e9726-2a7f-4750-868c-5a836ff7759a)
