---
layout: post
title: Let's Talk About Big-O - Time Complexity
date: 2021-07-14 10:00
permalink: let's_talk_about_big_o
tags: [Big-O, Algorithms]
# featured_image_thumbnail:
featured_image: assets/images/posts/let's-talk-about-big-o/insung-yoon-w2JtIQQXoRU-unsplash.jpg
image_caption: true
photographer_name: insung yoon
photographer_name_url: https://unsplash.com/@insungyoon
image_source: Unsplash
image_source_url: https://unsplash.com/
featured: false
hidden: false
---

_Big-O notation isn't something that we worry too much about while learning to code. After all, our primary goal is to learn how to code. It's no secret that in programming there can be multiple ways to correctly do something, for example, in Javascript there are multiple ways to write functions (think arrow functions vs regular functions), and once we've mastered the basics, it's time to focus on optimization. This is where Big-O notation will enter your vocabulary and you'll want to understand what it actually is. This article is a brief introduction to the Big-O notation._

## What is Big-O?

Big-O (pronounced "big-oh") is a theoretical measure of the execution of an algorithm; it tells us how much time or memory is needed to execute a function when given the input of size n. In this case, n represents the number of items that will be analyzed by the given function and algorithm is the function itself.

Big-O notation is the language used to talk about how much time the algorithm takes to run (time complexity) or how much memory is used to run it (space complexity). In this article we are going to focus primarily on the time complexity.

Big-O notation can express the best, worst or average-case running time of an algorithm. However, as software engineers, we are primarily concerned with the worst-case scenario. We want to know the largest possible number of steps or operations that could happen for an input of size n. The time required to complete all steps or operations of a given function is proportional to the number of "basic operations" in that function. Here are some examples of basic operations:

- one arithmetic operation (e.g., +, -, \*).
- one assignment (e.g. x := 0)
- one test (e.g., x = 0)
- one read (of a primitive type: integer, float, character, boolean)
- one write (of a primitive type: integer, float, character, boolean)

Note that for the purposes of Big-O notation, the time it takes to run an algorithm does not equate to seconds or milliseconds. Instead, we think of time as the number of steps or operations that are needed to complete a problem of size n. This is because Big-O does not take into account variables such as processors or language used to write the function. Rather, Big-O notation helps us track how quickly the runtime of the function grows relative to the size of the input.

### O(1) or Constant Time

Some algorithms perform the same number of operations every time they are called. For example, the basic operations described above require the same or constant amount of time to complete. Consider how easy it is to find your spot in a book if you've left a bookmark. Regardless of the size of the book it will always take you exactly one step.

O(1) simply means that it takes a constant time to run an algorithm, regardless of the size of the input.

In programming, a lot of operations are constant; here are some examples:

- basic operations
- accessing an array via the index
- accessing a hash via the key
- returning a value from a function

### O(n) or Linear Time

Consider an example where we have to insert an element into an array. We would have to move the current element and all subsequent elements one place to the right in order to make space for the new element. The worst case scenario would be if we had to insert an element at the beginning of an array, because in that case all of the elements of the array must be moved. Therefore, in the worst case, the time for insertion is directly proportional to the number of elements in the array. We say that the worst-case time for the insertion operation is linear in the number of elements in the array. For a linear-time algorithm, if the problem size doubles, the number of operations also doubles. Therefore, O(n) means that the run time increases at the same pace as the input.

In Javascript, methods like `forEach`, `map`, and `reduce` run through the entire collection of data, from start to finish. They are good examples of linear-time operations.

### O(n²) or Quadratic Time

Quadratic time means that the calculation runs in the time equivalent to the squared size of the input data. Here are some examples of the more basic sorting algorithms that have a worst-case run time of O(n²):

- [Bubble Sort](https://www.programiz.com/dsa/bubble-sort)
- [Insertion Sort](https://www.programiz.com/dsa/insertion-sort)
- [Selection Sort](https://www.programiz.com/dsa/selection-sort)

Note that in general, seeing two nested loops is a good indicator that the function has a run time of O(n²). Three nested loops would indicate a run time of O(n³).

### O(log n) or Logarithmic Time

An example of a phone book is often used to describe logarithmic time. Let's say you are looking for your friend's phone number in a phone book. You know that your friend's name starts with a letter "M". You will likely open the phone book around the middle and see whether the names there begin with any letter that precedes "M" or any letters that follows "M". Based on that, you will open the phone book closer towards the front or further towards the back and will, again, see if you've gotten close to the letter "M. You will continue performing these steps, and continue to decrease the size of the population by two, until you've located your friend's name. Note that you will perform the same steps regardless of how large the phone book is (first open in the middle, then move towards the front or the back, thus decreasing the population, depending on how close you are to the desired value).

O(log n) means that the running time grows in proportion to the logarithm of the input size, meaning that the run time barely increases as you exponentially increase the input. Logarithm is the inverse function to exponentiation, it's the power to which a number must be raised in order to get some other number. For example, the base ten logarithm of 100 is 2, because ten raised to the power of two is 100: log 100 = 2.

## Calculating Big-O

Remember that Big-O notation helps us track how quickly the runtime of the function grows relative to the size of the input and does not take into account variables such as processors or language used to write the function. Similarly, Big-O is not concerned with any constants or operations that run in constant time that may be a part of an algorithm. Such operations make very little difference in the overall run time approximation.

### O(n) or Linear Time

Take a look at the below code example:

<script src="https://gist.github.com/tcelovsky/a62b16ed73af772b7180d03e98ce5f4c.js"></script>

Here we have two separate loops that are iterating through the length of an array (linear time). Each loop logs an item in the collection (constant time). As mentioned above, we are not concerned with operation that run in constant time. Therefore, we only have to take the two loops into account and add them to calculate the Big-O. This gives us O(2n), but number 2 is a constant, so we drop it and are left with the Big-O of O(n).

### O(n²) or Quadratic Time

Now take a look at the code example below, it's very similar to what we just saw, but here we nested our loops:

<script src="https://gist.github.com/tcelovsky/f33683c40d6fdb30fb9f738879115f5b.js"></script>

In this example of nested loops we are logging `numbers[i]` five times. To calculate Big-O here we have to multiply O(n) \* O(n), because the execution of our log is dependent on iterating through the entirety of the second loop before we can increment `i` and move to the next index in our first loop. Thus, the Big-O in this case is O(n²).

<script src="https://gist.github.com/tcelovsky/aba19ebcfa797a4dc7eed889ceb932eb.js"></script>

The top portion of this function is the same as our previous example, we know its Big-O is O(n²). Then we may want to add O(n) related to the `reduce` function, which is linear. We get O(n² + n). However, Big-O is not concerned with non-dominant terms and because quadratic time is worse than linear time, we drop the second n. The final Big-O of the above function is O(n²).

### O(log n) or Logarithmic Time

Take a look at this code example:

<script src="https://gist.github.com/tcelovsky/dd455f417bd0eb09abd3db54e55b9f4a.js"></script>

Here we are trying to locate number 128 in a given (already sorted) array. In the first iteration of our `while` loop, we split our data (the array) in half and call that point a pivot. We then check to see if the value in the array at pivot equals 128. If it does, we return a statement to say where the number was located. If it's less than 128, then we change the value of our `startIndex` to the value of pivot plus one. That's because if the value of pivot is less than 128, then we know that all of the numbers preceding pivot will also be lower than 128 (remember, the array has been sorted). If the value of pivot is greater than 128, then we change the value of our `startIndex` to the value of pivot minus one. This way we are able to halve our data.

We continue executing the above steps until we've found the number we are looking for or returned `false` to indicate that the number does not exist in the given array. With each pass we divide our data by two. Whenever you see this pattern of dividing the population or data by two, know that you are looking at Big-O of O(log n).

## Conclusion

Having a good understanding of Big-O notation provides a more well-rounded context when designing algorithms. This article is only an introduction and if you'd like to read more on this topic, I would recommend [Big-O Cheat Sheet](https://www.bigocheatsheet.com/) and [What is Big O Notation Explained: Space and Time Complexity](https://www.freecodecamp.org/news/big-o-notation-why-it-matters-and-why-it-doesnt-1674cfa8a23c/).

## Resources 

[O(n) Example gist](https://gist.github.com/tcelovsky/a62b16ed73af772b7180d03e98ce5f4c)

[O(n²) Example gist](https://gist.github.com/tcelovsky/f33683c40d6fdb30fb9f738879115f5b)

[O(n²) Example 2 gist](https://gist.github.com/tcelovsky/aba19ebcfa797a4dc7eed889ceb932eb)

[O(log n) Example gist](https://gist.github.com/tcelovsky/dd455f417bd0eb09abd3db54e55b9f4a)

[Big-O Cheat Sheet](https://www.bigocheatsheet.com/)

[What is Big O Notation Explained: Space and Time Complexity](https://www.freecodecamp.org/news/big-o-notation-why-it-matters-and-why-it-doesnt-1674cfa8a23c/)

[Bubble Sort Algorithm](https://www.programiz.com/dsa/bubble-sort)

[Insertion Sort Algorithm](https://www.programiz.com/dsa/insertion-sort)

[Selection Sort Algorithm](https://www.programiz.com/dsa/selection-sort)
