# cs1302-ce27.5 Paired Sorting Algorithm Analysis

![Approved for: Fall 2019](https://img.shields.io/badge/Approved%20for-Fall%202019-brightgreen)

In this class exercise, students will implement two different algorithms for
sorting an array, then derive the timing functions for those algorithms for
the purposes of completing a set of algorithm analysese at a later time. Code
and notes will be shared between group members via a private Git repository. 

## Course-Specific Learning Outcomes
* **LO3.d:** Apply pair-programming principles in a software-based project.
* **LO5.a:** Utilize a version control tool such as Git or Subversion to store 
and update source code in a multi-programmer software solution.
* **LO6.c:** (Partial) Implement, analyze, and assess combinations of searching/sorting 
algorithms such as linear search, binary search, quadratic sorts, and linearithmic sorts.

## References and Prerequisites

* [Setting up your own GitHub Account](https://github.com/cs1302uga/cs1302-tutorials/blob/master/github-setup.md)
* [CSCI 1302 Algorithm Analysis Tutorial](https://github.com/cs1302uga/cs1302-tutorials/blob/master/algo-analysis.md)

## Questions

In your notes, clearly answer the following questions. These instructions assume that you are 
logged into the Nike server. 

**NOTE:** If a step requires you to enter in a command, please provide in your notes the full 
command that you typed to make the related action happen. If context is necessary (e.g., the 
command depends on your present working directory), then please note that context as well.

### Getting Started

1. **You need to be in a group with at least two people for this exercise.**
   We recommend keeping the groups small. Some steps in this exercise need to be
   done by each group member individually.

1. **Be sure that all group members have completed [cs1302-ce27](https://github.com/cs1302uga/cs1302-ce27).**
   If you have a new group member, then be sure to: i) add them as collaborator to the
   private `cs1302-ce27-ce28` repository on GitHub that you created in **cs1302-ce27**; and
   ii) have them clone the repository onto their Nike account. 
   
1. **Pick an ordering for the group members** (e.g., Group Member 1, Group Member 2, etc.).
   If a step is being performed by one group member, then everyone is expected
   to watch, pay attention, and take notes.

<hr/>

## Exercise Steps

For this next checkpoint, we will have you implement a simple sorting algorithm called
[Bubble Sort](https://en.wikipedia.org/wiki/Bubble_sort). There are many different ways to
explain the execution of this algorithm. We will take the approach of breaking up the
algorithm into two methods, `bubble` and `bubbleSort` that work together to sort an array.

**Bubble Algo:** This is a helper algorithm that does not, itself, sort the array. 
This method takes an array, two valid index positions `lo` and `hi` (both inclusive) 
within the array such that `lo <= hi` and a `Comparator` that is used to perform comparisions. 
The method iterates over the array from `lo` to `hi - 1` (inclusive) and **swaps adjacent elements**
that are **not in order** according to the ordering induced by the comparator (i.e., calling 
`c.compare`. Here is the signature for the method:
   
   ```java
   public static <T> void bubble(T[] array, int lo, int hi, Comparator<T> c)
   ```
   
   1. Here is an example of before and after calling `bubble(array, 0, 4, Integer::compareTo)`
      on an array with elements `[ 2, 3, 1, 4, 5 ]`:
      
      ```java
      System.out.println(Arrays.toString(array)); // [ 2, 3, 1, 4, 5 ]
      bubble(array, 0, 4, Integer::compareTo);
      System.out.println(Arrays.toString(array)); // [ 2, 1, 3, 4, 5 ]
      ```
      
   1. Here is another example before and after calling `bubble(array, 0, 4, Integer::compareTo)`
      on an array with elements `[ 3, 2, 1, 5, 4 ]` :
      
      ```java
      System.out.println(Arrays.toString(array)); // [ 3, 2, 1, 5, 4 ]
      bubble(array, 0, 4, Integer::compareTo);
      System.out.println(Arrays.toString(array)); // [ 2, 1, 3, 4, 5 ]
      ```
   
This method gets its name from the idea that a call "bubbles" the bigger values to the right
of the specified range (i.e., from `lo` to `hi`). After a call to `bubble`, 
**the largest value in the range is guaranteed to be at index `hi`.**

1. As a group, pick a **DRIVER.**, then the have the **DRIVER** implement the `bubble` method
   in `BubbleSort.java`. You may want to implement a static `swap` method to help you perform
   the adjacent swaps. 
   
1. Write some code in the `main` method to test the implementation of `bubble`. Make sure you
   test a few different dataypes and vary the starting (`lo`) and ending (`hi`) indices.
   Once your group is confident that the code compiles and runs correctly,
   have the **DRIVER** stage and commit `BubbleSort.java` to their local repository, then
   push the changes up to the repository on GitHub. Everyone else should pull the changes
   after that.
   
1. View the condensed, graphical version of your Git log using `git adog`.

<hr/>

**CHECKPOINT**

**Bubble Sort Algo**: This method also takes an array, two valid index positions `lo` and `hi` (both inclusive) 
within the array such that `lo <= hi` and a `Comparator` that is used to perform comparisions.
The method simply calls `bubble(array, 0, hi)` for all valid `hi` values **in reverse order**
except for `0`. Here is the signature for the method:

   ```java
   public static <T> void bubbleSort(T[] array, int lo, int hi, Comparator<T> c)
   ```

To sort an entire array of integers referred to by `array`, for example, you might call:
   
   ```java
   bubbleSort(array, 0, array.length - 1, Integer::compareTo);
   ```
   
This method gets its name from the fact that uses repeated calls `bubble` in order to sort the array. 
Visually, the algorithm works by breaking up the array into two subsequences: unsorted and sorted.
Initially, the unsorted sequence is the entire array and the sorted sequence is empty. After each
call to `bubble`, we know that the largest value in the range is guaranteed to be at index `hi`.
   
   1. Here is a trace of the algorithm, one row for each call to `bubble`:
   
      | Before              | Call                      | After (Unsorted `/` Sorted) |
      |---------------------|---------------------------|-----------------------------|
      | `[ 5, 4, 2, 3, 1 ]` | `bubble(array, 0, 4, c);` | `[ 4, 2, 3, 1/ 5 ]`         |  
      | `[ 4, 2, 3, 1, 5 ]` | `bubble(array, 0, 3, c);` | `[ 2, 3, 1/ 4, 5 ]`         |
      | `[ 2, 3, 1, 4, 5 ]` | `bubble(array, 0, 2, c);` | `[ 2, 1/ 3, 4, 5 ]`         |
      | `[ 2, 1, 3, 4, 5 ]` | `bubble(array, 0, 1, c);` | `[ 1/ 2, 3, 4, 5 ]`         |
      
   1. Here is an example before and after calling `bubbleSort(array, 0, 4, Integer::compareTo)`
      on an array with elements `[ 5, 4, 2, 3, 1 ]`:
      
      ```java
      System.out.println(Arrays.toString(array)); // [ 5, 4, 2, 3, 1 ]
      bubbleSort(array, 0, 4, Integer::compareTo);
      System.out.println(Arrays.toString(array)); // [ 1, 2, 3, 4, 5 ]
      ```
1. As a group, pick a _new_ **DRIVER.**, then the have the **DRIVER** implement the `bubbleSort` 
   method in `BubbleSort.java`. 
   
1. Write some code in the `main` method to test the implementation of `bubbleSort`. You can
   most likely use your `bubble` tests - just change them to do a full `bubbleSort`. Make sure 
   you test a few different dataypes and vary the starting (`lo`) and ending (`hi`) indices.
   Once your group is confident that the code compiles and runs correctly,
   have the **DRIVER** stage and commit `BubbleSort.java` to their local repository, then
   push the changes up to the repository on GitHub. Everyone else should pull the changes
   after that.
   
1. View the condensed, graphical version of your Git log using `git adog`.

<hr/>

**CHECKPOINT**

1. In your notes, write down the source code for `bubble` and `bubbleSort`, then derive the
   timing functions for two different algorithm analyses of the **Bubble Sort Algo**. Here,
   let the problem size be defined as `n = hi - lo + 1`. 
   
   1. What is `T(n)` for a call to `bubbleSort` if the set of key processing steps includes
      only swap operations? Include the diagram for your derivation. As `bubbleSort` calls
      `bubble`, this will involve mathematical function composition.
      
   1. What is `T(n)` for a call to `bubbleSort` if the set of key processing steps includes
      only comparison operations (i.e., calls to `c.compare`)? Include the diagram
      for your derivation. As `bubbleSort` calls `bubble`, this will involve mathematical 
      function composition.

<hr/>

**CHECKPOINT**

For this next checkpoint, we will have you implement a simple sorting algorithm called
[Selection Sort](https://en.wikipedia.org/wiki/Selection_sort). There are many different ways to
explain the execution of this algorithm. We will take the approach of breaking up the
algorithm into two methods, `selectMin` and `selectionSort` that work together to sort an array.

1. **Select Min Algo:** This method takes an array, two valid index positions `lo` and `hi` (both inclusive) 
   within the array such that `lo <= hi` and a `Comparator` that is used to perform comparisions. 
   The method iterates over the array from `lo` to `hi` (inclusive) and finds the minimum element 
   according to the ordering induced by the comparator (i.e., calling `c.compare`), then swaps that
   element with the element as index `lo`. 
   Here is the signature for the method:
   
   ```java
   public static <T> void selectMin(T[] array, int lo, int hi, Comparator<T> c)
   ```
   
   1. Here is an example of before and after calling `selectMin(array, 0, 4, Integer::compareTo)`
      on an array with elements `[ 2, 3, 1, 4, 5 ]`:
      
      ```java
      System.out.println(Arrays.toString(array)); // [ 2, 3, 1, 4, 5 ]
      selectMin(array, 0, 4, Integer::compareTo);
      System.out.println(Arrays.toString(array)); // [ 1, 3, 2, 4, 5 ]
      ```
      
   1. Here is another example before and after calling `selectMin(array, 1, 4, Integer::compareTo)`
      on an array with elements `[ 1, 3, 2, 4, 5 ]` :
      
      ```java
      System.out.println(Arrays.toString(array)); // [ 1, 3, 2, 4, 5 ]
      selectMin(array, 1, 4, Integer::compareTo);
      System.out.println(Arrays.toString(array)); // [ 1, 2, 3, 4, 5 ]
      ```
   
   This method gets its name from the idea that it repeatedly selects a minimum in the 
   specified range (i.e., from `lo` to `hi`). After a call to `selectMin`, 
   **the smallest value in the range is guaranteed to be at index `lo`.**

1. As a group, pick a **DRIVER.** (no repeats), then the have the **DRIVER** implement the `selectMin` method
   in `SelectionSort.java`. You may want to implement a static `swap` method to help you perform
   the swaps. Be sure to include some code in the `main` method to test the 
   implementation. Once your group is confident that the code compiles and runs correctly,
   have the **DRIVER** stage and commit `SelectionSort.java` to their local repository, then
   push the changes up to the repository on GitHub. Everyone else should pull the changes
   after that.

1. **Selection Sort Algo**: This method also takes an array, two valid index positions `lo` and `hi` (both inclusive) 
   within the array such that `lo <= hi` and a `Comparator` that is used to perform comparisions.
   The method simply calls `selectMin(array, i, hi)` for all valid `i` values starting with `lo`, 
   **in order**, except for `array.length - 1`. Here is the signature for the method:

   ```java
   public static <T> void selectionSort(T[] array, int lo, int hi, Comparator<T> c)
   ```

   To sort an entire array of integers referred to by `array`, for example, you might call:
   
   ```java
   selectionSort(array, 0, array.length - 1, Integer::compareTo);
   ```
   
   This method gets its name from the fact that uses repeated calls `selectMin` in order to sort the array. 
   Visually, the algorithm works by breaking up the array into two subsequences: sorted and unsorted.
   Initially, the sorted sequence contains a single element (i.e., the element as index `lo`) and the 
   unsorted sequence is the remaining elements in the array. After each call to `selectMin`, we know that 
   the smallest value in the range is guaranteed to be at the `lo` index passed to `selectMin`.
   
   1. Here is a trace of the algorithm, one row for each call to `selectMin`:
   
      | Before              | Call                         | After (Sorted `/` Unorted) |
      |---------------------|------------------------------|-----------------------------|
      | `[ 2, 5, 1, 3, 4 ]` | `selectMin(array, 0, 4, c);` | `[ 1/ 5, 2, 3, 4 ]`         |  
      | `[ 1, 5, 2, 3, 4 ]` | `selectMin(array, 1, 4, c);` | `[ 1, 2/ 5, 3, 4 ]`         |
      | `[ 1, 2, 5, 3, 4 ]` | `selectMin(array, 2, 4, c);` | `[ 1, 2, 3/ 5, 4 ]`         |
      | `[ 1, 2, 3, 5, 4 ]` | `selectMin(array, 3, 4, c);` | `[ 1, 2, 3, 4/ 5 ]`         |
      
   1. Here is an example before and after calling `selectionSort(array, 0, 4, Integer::compareTo)`
      on an array with elements `[ 5, 4, 2, 3, 1 ]`:
      
      ```java
      System.out.println(Arrays.toString(array)); // [ 5, 4, 2, 3, 1 ]
      selectionSort(array, 0, 4, Integer::compareTo);
      System.out.println(Arrays.toString(array)); // [ 1, 2, 3, 4, 5 ]
      ```
1. As a group, pick a _new_ **DRIVER.** (no repeats), then the have the **DRIVER** implement the `selectionSort` 
   method in `SelectionSort.java`. Be sure to include some code in the `main` method to test the 
   implementation. Once your group is confident that the code compiles and runs correctly,
   have the **DRIVER** stage and commit `SelectionSort.java` to their local repository, then
   push the changes up to the repository on GitHub. Everyone else should pull the changes
   after that.
   
1. View the condensed, graphical version of your Git log using `git adog`.

**CHECKPOINT**
1. Next time, your group will work on the **Selection Sort Analysis** and another interesting
   algorithm.

**NOT A CHECKPOINT**

<hr/>

[![License: CC BY-NC-ND 4.0](https://img.shields.io/badge/License-CC%20BY--NC--ND%204.0-lightgrey.svg)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

<small>
Copyright &copy; Michael E. Cotterell, Brad Barnes, and the University of Georgia.
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/">Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International License</a> to students and the public.
The content and opinions expressed on this Web page do not necessarily reflect the views of nor are they endorsed by the University of Georgia or the University System of Georgia.
</small>
