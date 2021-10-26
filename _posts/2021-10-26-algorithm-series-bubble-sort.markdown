---
layout: post
title: Algorithm Series - Bubble Sort
date: 2021-10-26 16:50
permalink: algorithm_series_-_bubble_sort
tags: [Algorithms]
# featured_image_thumbnail:
featured_image: assets/images/posts/algorithm-series/pieter-Qa8Y_W4PORw-unsplash.jpg
image_caption: true
photographer_name: Pieter
photographer_name_url: https://unsplash.com/@pieterpanflute
image_source: Unsplash
image_source_url: https://unsplash.com/
featured: true
hidden: true
---

_This is a quick tutorial on the bubble sort algorithm and its implementation in Javascript._

### What is Bubble Sort Algorithm

Bubble sort is a [sorting algorithm](<[https://en.wikipedia.org/wiki/Sorting_algorithm](https://en.wikipedia.org/wiki/Sorting_algorithm)>) that compares two adjacent elements and swaps them if they are not in the intended order (ascending vs descending).

Let's take a look at bubble sort when trying to sort the elements of an array in an ascending order:

1. First iteration (compare and swap):
   - Starting from the first index, compare the first and second elements;
   - If the first element is greater than the second element, they are swapped;
   - Compare the second and third elements;
   - If the second element is greater than the third element, they are swapped;
   - Continue the above process until the last element of the array is reached.
2. Remaining iterations:

   Continue the same process for the remaining iterations, such that after each iteration, the largest element among the unsorted elements is placed at the end. In each iteration, the comparison takes place up to the last unsorted element.

The array is sorted when all the unsorted elements are placed in their correct positions.

### Bubble Sort Code in Javascript

Let's take a look at the code for the bubble sort algorithm described above (ascending order):

<script src="https://gist.github.com/tcelovsky/a06e2222cc1e5e5d2f0156f54293c9b7.js"></script>

The above code sorts the array in ascending order. To sort an array in descending order, replace the “greater than” sign in the `if` statement with a “less than” sign.

### Optimizing Bubble Sort Algorithm

In the code example above, all the comparisons between the array elements are made even if the array is already sorted - this increases the execution time. To solve this, we can introduce an extra variable called `swapped`. This variable can keep track of whether a swap occurs. If no swaps have occurred, then we know that the elements are already sorted and there is no need to perform further iterations. This will reduce the execution time and help optimize the bubble sort algorithm.

<script src="https://gist.github.com/tcelovsky/8c2e8179ccd8d847e7c5af219960aff5.js"></script>

The above code will keep track of whether a swap was made in an iteration. If no swap was made, then we know that the array is sorted and we can stop the bubble sort.

### Bubble Sort and Big-O

Bubble Sort compares the adjacent elements, hence, the number of comparisons is:

(n-1) + (n-2) + (n-3) +.....+ 1 = n(n-1)/2

This nearly equals to n2, therefore Big-O is O(n²) or quadratic time. We can also deduce this from observing the code: bubble sort requires two loops, therefore Big-O is expected to be O(n²).

### Conclusion

Bubble sort compares adjacent items in a list and swaps them if they are not in the right order. It is a simple way to sort a list when complexity does not matter and short and simple code is preferred.

### Resources 

[Bubble Sort Algorithm gist](https://gist.github.com/tcelovsky/a06e2222cc1e5e5d2f0156f54293c9b7)

[Optimized Bubble Sort Algorithm gist](https://gist.github.com/tcelovsky/8c2e8179ccd8d847e7c5af219960aff5)

[Let's Talk About Big-O](http://tatyanacelovsky.com/let's_talk_about_big_o)
