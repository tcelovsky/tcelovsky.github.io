---
layout: post
title: Algorithm Series - Insertion Sort
date: 2021-11-17 16:00
permalink: algorithm_series_-_insertion_sort
tags: [Algorithms]
# featured_image_thumbnail:
featured_image: assets/images/posts/algorithm-series/dee-copper-and-wild-ViEj6V9SUXA-unsplash.jpg
image_caption: true
photographer_name: Dee @ Copper and Wild
photographer_name_url: https://unsplash.com/@copperandwild
image_source: Unsplash
image_source_url: https://unsplash.com/
featured: true
hidden: true
---

_This is a quick tutorial on the Insertion Sort algorithm and its implementation in Javascript._

### What is Insertion Sort Algorithm

Insertion Sort is a [sorting algorithm](<[https://en.wikipedia.org/wiki/Sorting_algorithm](https://en.wikipedia.org/wiki/Sorting_algorithm)>) that places the element in its appropriate place based on the sorting order. A good example of Insertion Sort is sorting cards held in your hands during a card game.

Let's take a look at Insertion Sort when trying to sort the elements of an array in an ascending order:

1. Assume that the first element of the array is property sorted;
2. Store the second element of the array in a `key`;
3. Compare `key` to the first element - if `key` is smaller that the first element, then `key` is placed in front of the first element;
4. Store the next unsorted element of the array in a `key`;
5. Compare `key` to the sorted element and place it in the appropriate place with respect to the sorted elements based on whether `key` is smaller or greater than each of the sorted elements;
6. Continue with steps 4 and 5 until all elements of the array are sorted.

The array is sorted when all the unsorted elements are placed in their correct positions.

### Insertion Sort Code in Javascript

Let's take a look at the code for the Insertion Sort algorithm described above (ascending order):

<script src="https://gist.github.com/tcelovsky/8c8d41a80c6d9525b69b7daeaf1c4b12.js"></script>

Let's take a look at the code for the Insertion Sort algorithm described above (ascending order):

### Insertion Sort and Big-O

Insertion Sort compares the adjacent elements, hence, the number of comparisons is:

(n-1) + (n-2) + (n-3) +.....+ 1 = n(n-1)/2

This nearly equals to n2, therefore Big-O is O(n²) or quadratic time. We can also deduce this from observing the code: insertion sort requires two loops, therefore Big-O is expected to be O(n²).

### Conclusion

Insertion Sort inserts each element of the array in its appropriate place based on whether the array is being sorted in ascending or descending order. It is a simple way to sort a list when complexity does not matter and the list that needs sorting is short.

### Resources 

[Insertion Sort Algorithm gist](https://gist.github.com/tcelovsky/4c7b1b5a852adacf13ba7a3604000f79)

[Let's Talk About Big-O](http://tatyanacelovsky.com/let's_talk_about_big_o)
