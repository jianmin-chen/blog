---
categories: algorithms
date: 2021-01-30 04:20:45 -0500
layout: post
tags: algorithms
title: A Quick Refresher on Algorithms
---
![Graph image](https://cdn.pixabay.com/photo/2018/03/02/03/44/unordered-3192273__340.png)

If you're someone like me, you probably need to review your basic algorithms once in a while. This is the cheat sheet for that! This page contains explanations and code(in JavaScript) for basic searching algorithms:

* linear search
* binary search

and of course, explanations and code for basic sorting algorithms:

* selection sort
* bubble sort
* merge sort

We also talk about running time, so let's dive in!

## What is running time?
Okay, let's start off by talking about running time. Running time, simply put, is how fast or slow an algorithm is. This is so important in understanding whether an algorithm is good or bad, because it allows us to see how long an algorithm takes as given input grows larger and larger.

There are two ways to measure the running time of an algorithm - upper bound and lower bound.

### How do I measure the upper bound of an algorithm?
The upper bound of an algorithm is how long an algorithm takes to run, in the worst case scenario.

For example, let's take a sorting algorithm. If the algorithm is passed in a array that's entirely backwards(i.e the worst input), how long does the algorithm take given the length of the array?

In computer science, the upper bound of an algorithm is verbally stated as "big O of...", which is written as **O(*upper bound*)**.

### How do I measure the lower bound of an algorithm?
The lower bound of an algorithm is how long an algorithm takes to run, in the best-case scenario.

Once again, let's take a sorting algorithm. If the algorithm is passed in a array that's already sorted(i.e the best input), how long does the algorithm take given the length of the array?

In computer science, the lower bound of an algorithm is verbally stated as "big omega of...", which is written as **Ω(*lower bound*)**.

### Most common running times
Now that we've talked about upper and lower bound, I'd like to introduce some common running times that pop up often. Here's a list, where *n* is the length of the input given to an algorithm.

* **n squared**: Running time takes double the amount of *n*.

* **n log n**: Running time takes *n* iterations times *log n* iterations. This running time is typically applied to recursive algorithms, as well as *log n*, which you will see later. *Extra note: Whenever we talk about logarithms in this article, starting from here, we'll be talking about base 2 logarithms.*

* **n**: Running time takes *n* iterations. This is also typically called linear time, as the running time is linear compared to the length of input.

* **log n**: Running time takes *log n* iterations. This is typically found in recursive functions, where for example you might have to split a array in half again and again.

* **1**: Running time is in constant time. What this means when applied to an algorithm is that no matter the input, the algorithm always takes a constant amount of steps, whether that be big or small.

### A special case
You've probably read through a lot already - this will be the final blabber about running time, I promise! You can pat yourself on the back after this.

In certain situations, the running time of an algorithm might be the same in terms of upper and lower bound. In such a case, you can use one piece of notation to describe both bounds: "big theta of..." or **Θ(*upper and lower bound*)**.

## What are some basic searching algorithms?
In this article, we'll talk about two of them, linear search and binary search. Buckle up and get ready for a wild ride!

### What is linear search?
Linear search is the most basic of all searching algorithms. You literally loop through a array and find the given value. If the value isn't there, the algorithm will return false. Here's the code:
~~~ javascript
const linearSearch = (arr, val) => {
    for (let i of arr) {
        if (i == val) return true;
    }
    return false;
};
~~~
What's the upper and lower bound of this algorithm? Well, it's pretty obvious. In the worst case scenario where the value isn't in the given array, the algorithm has to search through the whole array before returning `false`, meaning the upper bound for this is **O(n)**. In the best case scenario where the value happens to be the first in the given array, the algorithm simply has to go one step. Therefore, the lower bound of this algorithm is **Ω(1)**.

### What is binary search?
Binary search is a more efficient way than linear search to search a array for a value. However, there's just one catch - the array has to be sorted.

This is because what binary search does is make use of recursion. It starts in the middle of the provided array and checks whether or not the middle is equal to the value being searched. If the middle value is actually bigger, the algorithm calls itself on the left half of the array, and searches it. Otherwise, if the middle value is smaller, the algorithm calls itself on the right half of the array, and searches it.

Of course, there's always a base case when it comes to recursion. If binary search gets passed a empty array, then the number must not exist. An implementation of this is shown below.
~~~ javascript
const binarySearch = (arr, val) => {
    if (!arr.length) return false;  // base case - array has nothing, so val doesn't exist in the orginal array
    let midPos = Math.floor(arr.length / 2);  // get position of middle element in array
    if (arr[midPos] > val) return binarySearch(arr.slice(0, midPos), val);  // search left half of array because current number is bigger than value
    else if (arr[midPos] < val) return binarySearch(arr.slice(midPos + 1, arr.length), val);  // search right half of array because current number is smaller than value
    return true;  // value was found
};
~~~
Now let's talk about the running time of this algorithm. In the worst-case scenario where the number we're searching for doesn't exist in the given array, the algorithm will keep splitting the array in half, over and over again. So for a array of size 8, it'll split 4 times. For a array of size 100, it'll split 50 times. See the pattern? In the worst case situation, the algorithm will split the given array *log n* times. Therefore, the upper bound of binary search is **O(log n)**.

What about the lower bound? Well, in the best case scenario the value provided might be smack in the middle of the array given. That means we only did one step, so the lower bound of binary search is **Ω(1)**.

## What are some basic sorting algorithms?
Now let's talk about sorting algorithms! In this article, we'll discuss three of them: selection sort, bubble sort, and merge sort.

### What is selection sort?


![Selection sort](https://upload.wikimedia.org/wikipedia/commons/9/94/Selection-Sort-Animation.gif)
<a class="img-link" href="https://en.wikipedia.org/wiki/Selection_sort">Selection sort visualization(Wikipedia)</a>

Selection sort is a pretty trivial sorting algorithm. When given a array, it goes through each item in the array, and keeps track of the smallest. Once it goes through the array fully, it swaps the current element with the smallest element. Obviously, this only puts one number into it's proper place. To fully sort any given array, selection sort has to do the above *n* times(hint hint!), as shown in the visualization above and the code below:
~~~javascript
const selectionSort = (arr) => {
    if (arr.length < 2) return arr;  // nothing to be sorted
    for (let i = 0; i < arr.length; i++) {
        let smallest = arr[i], smallestPos = i;  // variables used for keeping track of smallest number from ith item to last item in array
        for (let j = i + 1; j < arr.length; j++) {  // go through items after ith item
            if (arr[j] < smallest) {  // smaller number than current smallest number
                smallest = arr[j];
                smallestPos = j;
            }
        }
        // now swap
        arr[smallestPos] = arr[i];
        arr[i] = smallest;
    }
    return arr;
};
~~~
Once again, let's ask the same question: what's the running time of this algorithm?

If we think long and hard about it, the upper bound of selection sort would be **O(n squared)**. Why is that? Well, we know we have to go through the array *n* times. Each time, selection sort checks for the smallest element from the current element all the way to the end of the list. Therefore, the number of iterations selection sort takes can be summarized by something like `n + (n - 1) + (n - 2) + ...1`. This can be simplified, using some math, to something a little cryptic like `n squared / 2 + n / 2`.

This isn't the same as *n squared*, is it? But let's be honest, as the size of the array provided gets larger and larger, some of these values won't be relevant anymore. That's why in computer science, we can estimate running times based on the most dominant factor. In this case, *n squared* is the most dominant factor so the upper bound of selection sort can get simplified to **O(n squared)**.

Okay, what about the lower bound? In the best case scenario, selection sort would receive a completely sorted array. Sadly, selection sort doesn't have the capability to tell whether or not a provided array is sorted. Therefore, it would still go through the array anyways, meaning the lower bound of selection sort is also **Ω(n squared)**.

Since the upper and lower bound are the same, we can use a simpler piece of notation to describe the running time of selection sort, and that is **Θ(n squared)**.

### What is bubble sort?

![Bubble sort](https://upload.wikimedia.org/wikipedia/commons/c/c8/Bubble-sort-example-300px.gif)
<a class="img-link" href="https://en.wikipedia.org/wiki/Bubble_sort">Bubble sort visualization(Wikipedia)</a>

Bubble sort is a sorting algorithm derived from selection sort that allows for a better lower bound. Basically, it goes through the provided array *n* times, similar to selection sort. Each time, bubble sort goes through each item starting from the beginning, and checks whether or not the item next to it is smaller. If the next item is smaller, the two items get swapped. If we keep track of this swapping, we can assume the array is sorted as soon as we can't swap any more numbers, such as in the code below:
~~~javascript
if (arr.length < 2) return arr;  // nothing to be sorted
let swapped = false;
for (let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr.length - 1; j++) {
        if (arr[j] > arr[j + 1]) {
            let tmp = arr[j];  // temporary variable for swapping
            arr[j] = arr[j + 1];
            arr[j + 1] = tmp;
            swapped = true;
        }
    }
    if (!swapped) return arr;  // bubble sort didn't swap any values because all values are sorted already
    swapped = false;
}
return arr;
};
~~~
What's the upper and lower bound for bubble sort? Well, the upper bound is actually the same as selection sort, **O(n squared)**, since we go through the array *n* times and go through the array *n* times again inside those iterations. Not much better, clearly.

However, what about the lower bound? The best case scenario would obviously be if a already sorted array was passed. In such a scenario, bubble sort would only have to go through the provided array once to find out it can't swap anything, and as a result exit. So the lower bound of bubble sort is **Ω(n)**.

### What is merge sort?

![Merge sort](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e6/Merge_sort_algorithm_diagram.svg/300px-Merge_sort_algorithm_diagram.svg.png)
<a class="img-link" href="https://en.wikipedia.org/wiki/Merge_sort">Merge sort visualization(Wikipedia)</a>

Finally, we're at our final algorithm, merge sort. Merge sort is better than selection sort and bubble sort in terms of running time, especially since it takes advantage of recursion.

How does merge sort work? Well, when given an array, merge sort breaks the array down again and again by calling itself, then working from the smallest arrays up and "merging" them together with the help of a helper function known as `merge`. As the arrays get merged together, the original array becomes sorted.

Of course, to understand how merge sort works we also need to understand how it's helper, `merge`, works. `merge` takes two arrays and "merges" them together by constantly comparing the first element of each array and adding the smaller value to a final array, as well as removing the smaller value from it's respective array, through a `for` or `while` loop. If the items in one array have all been added to the final array, the rest of the items in the other array can be added, returning a sorted array. Take a look at the above visualization!

In the meantime, here's an implementation of merge sort in JavaScript:
~~~javascript
const merge = (leftArr, rightArr) => {
    let mergedArr = [];
    while (leftArr.length && rightArr.length) {  // still haven't run out of elements
        if (leftArr[0] < rightArr[0]) mergedArr.push(leftArr.shift());  // remove first element from leftArr and add it to mergedArr if left element is smaller
        else mergedArr.push(rightArr.shift());  // remove first element from rightArr and add it to mergedArr if right element is smaller
    }
    return [...mergedArr, ...leftArr, ...rightArr];  // return mergedArr and whatever leftover elements are in leftArr and rightArr
};

const mergeSort = (arr) => {
    if (arr.length < 2) return arr;  // base case
    let midPos = Math.floor(arr.length / 2);  // get position of middle element in array, which will be used to split the array in half
    let leftArr = mergeSort(arr.splice(0, midPos)), rightArr = mergeSort(arr);
    return merge(leftArr, rightArr);
};
~~~
Okay, last time I'm asking this: what's the running time of this algorithm?

Well, in the worst case scenario, merge sort would be provided with a completely backwards array, which means it would have to split the array entirely. We know from taking a look at binary search that the splitting of a array takes *log n* iterations. If you take a look at the visualization above, this is true - the amount of rows it takes to split the array of size 7 is 3 rows - pretty close to log 7(which is around 2.8). Then we have to merge together the split arrays, which would take *n* steps each iteration. As a result, the upper bound for merge sort is **O(n log n)**.

Continuing on, the best case scenario would be if the array passed to merge sort was completely sorted. Unfortunately, like selection sort, merge sort doesn't take a look at whether or not the array passed in to it is sorted. This means that the lower bound for merge sort is also **Ω(n log n)**, and the running time in general for merge sort is **Θ(n log n)**.

## Conclusion
That's it! I hope you got something out of this article. We talked about running time, basic searching algorithms(linear search and binary search), and finally basic sorting algorithms(selection sort, bubble sort, and merge sort). If you have any questions feel free to ask below. The code is located [here](https://github.com/jianmin-chen/blog-programs/blob/main/A%20Quick%20Refresher%20on%20Algorithms/algorithms.js).
