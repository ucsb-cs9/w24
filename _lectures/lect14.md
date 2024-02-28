---
num: "Lecture 14"
desc: "Merge Sort"
ready: true
lecture_date: 2024-02-22 14:00:00.00-7:00
---

[Slides folder]({{ site.slides_url }})

# Merge Sort

* Merge Sort uses a **divide and conquer** strategy to perform sorting in a more efficient manner
* **Idea:**
    * Break a list into sublists where size == 1 (done recursively)
        * Note that a list with size == 1 is considered sorted
    * Merge the sublists together to form a larger list consisting of all elements from both lists in sorted order
    * Continue to merge until the list you end up with contains all elements of the original list in sorted order
 
  ## Example
    ![mergesortexample](https://github.com/ucsb-cs9/w24/assets/122947907/c9406425-132d-494f-9112-a9a6061e5ded)
    ![mergeexample](https://github.com/ucsb-cs9/w24/assets/122947907/44d78692-f079-4a80-aadc-4ea1d07e14df)

# Analysis

* Merge sort breaks the list into two "equal" parts per recursive call (note that if there's an odd number of elements it won't be completely equal)
* Just like binary search, by halving the list we are working with each time, we are creating a logarithmic algorithm
* On each merge step, at most n comparisons occur (as the merge steps puts the lists together)
* Therefore, if n comparisons occur on log n levels, the overall runtime is **O(n log n)**

# Python List Slicing

* List slicing operations using [:] return a new list object, and do not perform an in place modification
* During merge sort, one of the tradeoffs is the extra space taken up with every slice operation that is performed (every time a the list is "halved"
    * However, the actual list modifcation can be done in place since lists are a mutable type in Python (a new sorted list won't need to be created from scratch)
    
