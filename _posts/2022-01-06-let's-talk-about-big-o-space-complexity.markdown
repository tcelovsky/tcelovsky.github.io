---
layout: post
title: Let's Talk About Big-O - Space Complexity
date: 2022-01-06 10:00
permalink: let's_talk_about_big_o_space_complexity
tags: [Big-O, Algorithms]
# featured_image_thumbnail:
featured_image: assets/images/posts/let's-talk-about-big-o/insung-yoon-w2JtIQQXoRU-unsplash.jpg
image_caption: true
photographer_name: insung yoon
photographer_name_url: https://unsplash.com/@insungyoon
image_source: Unsplash
image_source_url: https://unsplash.com/
featured: true
hidden: true
---

_Big-O notation isn't something that we worry too much about while learning to code. After all, our primary goal is to learn how to code. It's no secret that in programming there can be multiple ways to correctly do something, for example, in Javascript there are multiple ways to write functions (think arrow functions vs regular functions), and once we've mastered the basics, it's time to focus on optimization. This is where Big-O notation will enter your vocabulary and you'll want to understand what it actually is. This article is a brief introduction to the Big-O notation._

### What is Big-O?

Big-O (pronounced "big-oh") is a theoretical measure of the execution of an algorithm; it tells us how much time or memory is needed to execute a function when given the input of size _n_. In this case, _n_ represents the number of items that will be analyzed by the given function and algorithm is the function itself.

Big-O notation is the language used to talk about how much time the algorithm takes to run (time complexity) or how much memory is used to run it (space complexity). In this article we are going to focus primarily on the space complexity. See [here](<[http://tatyanacelovsky.com/let's_talk_about_big_o](http://tatyanacelovsky.com/let's_talk_about_big_o)>) for the discussion on time complexity.

#### What is Space Complexity?

Space complexity of an algorithm refers to the amount of memory space required to solve the algorithm with respect to the size of the input. It is the memory required by an algorithm until it executes completely.

Similar to [time complexity](<[http://tatyanacelovsky.com/let's_talk_about_big_o](http://tatyanacelovsky.com/let's_talk_about_big_o)>), space complexity is expressed in Big-O notation.

Big-O notation can express the best, worst or average-case scenario of an algorithm’s growth. However, as software engineers, we are primarily concerned with the worst-case scenario. We want to know the largest possible amount of memory space that will be required to run an algorithm with an input of size n.

The amount of memory space required to execute an algorithm is affected by all the variables, constants, inputs, data structures, etc., that are utilized in an algorithm. Please note that space complexity can be further affected by variables such as compiler, programming language and even the machine running the algorithm, but Big-O does not take these into account.

**O(1) or Constant Complexity**

Any variables declared within a function or passed into a function will always take up the same amount of memory space, even if they are later re-assigned. That’s because memory space for any given variable is allocated at the time it is initialized, so any change in the value of that variable does not change the amount of memory space allocated to it. This means that space complexity of constants and variables is O(1) or constant space complexity.

**O(n) or Linear Complexity**

Any arrays that are either passed to the function or built up within a function will require memory space to be allocated to each element in that array. This means that space complexity of an array is O(n) or linear space complexity, where n is the number of elements of the array.

### Calculating Big-O

Remember that Big-O notation gives the worst-case scenario of an algorithm’s growth rate relative to the size of the input and does not take into account variables such as processors or language used to write the function. Similarly, Big-O is not concerned with any constants that may be a part of an algorithm as these make very little difference in the overall space complexity approximation.

#### O(1) or Constant Complexity

Take a look at the below code example:

<script src="https://gist.github.com/tcelovsky/34038e0b815b3978d03a157e8a779907.js"></script>

Here we have three variables passed to the function, one variable initialized and then re-assigned within the function. Memory space is allocated at the time of initialization of our variable and the amount of memory space needed to later re-assign a value to it does not change. Therefore, space complexity of this example is O(1) or constant complexity.

#### O(n) or Linear Complexity

Take a look at the below code example:

<script src="https://gist.github.com/tcelovsky/af276675d6c562f4e7e8febe4d7a43e6.js"></script>

Here we have an array passed to the function, one variable is initialized, then we loop through the passed array and re-assign the value of our variable. Memory space is allocated for each item of the array, space complexity of this is O(n), where n is the number of elements in the array. Memory space needed for initialization and later re-assignment of a variable is O(1), or constant space, as discussed above.

Total space complexity of the above example is O(n+1), but remember that Big-O is not concerned with any constants, so we drop the 1 and are left with space complexity of O(n) or linear complexity.

### Space/Time Complexity Tradeoffs

How do we know when our algorithm is efficient? It seems the answer is simple: our algorithm is efficient when it is fast and takes up the least amount of memory possible. Unfortunately, in reality things are not so simple. Often times increasing speed will lead to increased memory consumption or vice-versa. Therefore, we should not aim for the best time and space efficiency, but rather find middle ground that will satisfy our requirements.

### Conclusion

Having a good understanding of Big-O notation provides a more well-rounded context when designing algorithms. This article is only an introduction and if you'd like to read more on this topic, I would recommend [Big-O Cheat Sheet](<[https://www.bigocheatsheet.com/](https://www.bigocheatsheet.com/)>) and [What is Big O Notation Explained: Space and Time Complexity](<[https://www.freecodecamp.org/news/big-o-notation-why-it-matters-and-why-it-doesnt-1674cfa8a23c/](https://www.freecodecamp.org/news/big-o-notation-why-it-matters-and-why-it-doesnt-1674cfa8a23c/)>)

## Resources 

[O(1) Example gist](https://gist.github.com/tcelovsky/34038e0b815b3978d03a157e8a779907)

[O(n) Example gist](https://gist.github.com/tcelovsky/af276675d6c562f4e7e8febe4d7a43e6)

[Let's Talk About Big-O - Time Complexity](http://tatyanacelovsky.com/let's_talk_about_big_o)

[Big-O Cheat Sheet](https://www.bigocheatsheet.com/)

[What is Big O Notation Explained: Space and Time Complexity](https://www.freecodecamp.org/news/big-o-notation-why-it-matters-and-why-it-doesnt-1674cfa8a23c/)
