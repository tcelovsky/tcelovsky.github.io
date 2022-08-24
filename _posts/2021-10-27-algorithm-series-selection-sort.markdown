---
layout: post
title: Algorithm Series - Selection Sort
date: 2021-10-27 8:50
permalink: algorithm_series_-_selection_sort
tags: [Algorithms]
# featured_image_thumbnail:
featured_image: assets/images/posts/algorithm-series/jess-bailey-l3N9Q27zULw-unsplash.jpg
image_caption: true
photographer_name: Jess Bailey
photographer_name_url: https://unsplash.com/@jessbaileydesigns
image_source: Unsplash
image_source_url: https://unsplash.com/
featured: false
hidden: false
---

_This is a quick tutorial on the selection sort algorithm and its implementation in Javascript._

### What is Selection Sort Algorithm

Selection sort is a [sorting algorithm](<[https://en.wikipedia.org/wiki/Sorting_algorithm](https://en.wikipedia.org/wiki/Sorting_algorithm)>) that divides the input list into two parts: a sorted sublist that is built up from left to right and a sublist of the remaining unsorted values. The sorted sublist is placed at the front (to the left of) the unsorted sublist. Initially, the sorted sublist is empty and the unsorted sublist consists of the entire input list. The algorithm selects the smallest (or largest, depending on the ask) element in the unsorted sublist, places that element at the beginning of the unsorted sublist and moves the sublist boundary one element to the right (because there is now one element present in the sorted sublist, while the unsorted sublist became smaller by one element).

Let's take a look at selection sort when trying to sort the elements of an array in an ascending order:

1. Set the first element of the array as `minimum`;
2. Compare `minimum` with the second element, if the second element is smaller than `minimum`, assign the second element as `minimum` , otherwise, do nothing;
3. Compare `minimum` with the following element, if that element is smaller than `minimum`, then assign `minimum` to that element, otherwise do nothing;
4. Continue step 3 above until the last element has been reached;
5. Move `minimum` to the front of the array and move the sublist boundary one element to the right (because there is now one element present in the sorted sublist, while the unsorted sublist became smaller by one element);
6. Continue with steps 3 - 5, until all elements are in their sorted positions.

The array is sorted when all the unsorted elements are placed in their correct positions.

### Selection Sort Code in Javascript

Let's take a look at the code for the selection sort algorithm described above (ascending order):

<script src="https://gist.github.com/tcelovsky/4c7b1b5a852adacf13ba7a3604000f79.js"></script>

The above code sorts the array in ascending order. To sort an array in descending order, replace the “greater than” sign in the `if` statement with a “less than” sign.

### Selection Sort and Big-O

Selection Sort compares the adjacent elements, hence, the number of comparisons is:

(n-1) + (n-2) + (n-3) +.....+ 1 = n(n-1)/2

This nearly equals to n2, therefore Big-O is O(n²) or quadratic time. We can also deduce this from observing the code: selection sort requires two loops, therefore Big-O is expected to be O(n²).

### Conclusion

Selection sort finds the lowest value of the array and moves that value to the beginning of the array, it then proceeds to look for the next lowest value and moves that in front of the unsorted elements. This continues until all values of the array have been sorted; it is a simple way to sort a list when complexity does not matter and the list that needs sorting is short.

### Resources 

[Selection Sort Algorithm gist](https://gist.github.com/tcelovsky/4c7b1b5a852adacf13ba7a3604000f79)

[Let's Talk About Big-O](http://tatyanacelovsky.com/let's_talk_about_big_o)
