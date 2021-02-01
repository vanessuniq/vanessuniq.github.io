---
layout: post
title:      "Data Structures: Arrays"
date:       2021-02-01 00:43:31 +0000
permalink:  data_structures_arrays
---


![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQyYLBL8eSDAKXqHHq2u_c1EiGB3YUzyRL9Vg&usqp=CAU)

In this post, we will be learning the theory behind Arrays as well as how to implement them in JavaScript.

**Arrays** are simple and intuitive types of data structures. They represent a list of elements of any type grouped together in a central location of memory. The elements are ordered so that they can be accessed with a numerical index. This characteristic helps us to efficiently read and insert data into an array.

Arrays in JS are **dynamic** , meaning that they automatically adjust their size (increase or decrease) when adding or removing an element. This property guarantees a better performance by executing costly operations only when necessary. Some operations that can be performed on arrays include **inserting an item, removing an item, updating an item, finding an item, sorting, reversing or looping over an array, and copying part or an entire array**.

## Time complexities of operations on array

**Constant Time Complexity O(1)**: Because elements are sequentially ordered in array, it allows for fast lookups (accessing and updating an element using index) and fast adding and removing elements at the end of the array.

**Linear Time Complexity O(n)**: removing, inserting, or searching a random element in an array is done in linear-time. Removing a given element (if not at the end of the array) requires shifting all elements coming after the removed item over by one to fill the gap; inserting a new element requires reindexing all element coming after the inserted element; and finding a value in an unsorted list  requires iterating over the entire array in the worst case scenario.

**Logarithmic Time Complexity O(log n)**: searching an element in a sorted array can be done using binary search. Binary search algorithm works by initially checking the value present in the center of the list. If the value of the element it is looking for is lower than the one found, it repeats the process on the left half of the set. If it’s bigger, it checks the right half. This process is repeated until the element is found. 



**Log-Linear Time Complexity O(n log n)**: sorting an array falls into this category.

Below are the common methods to perform the above operations on an array in JavaScript :

```js

const fruits = ['mango', 'banana', 'orange', 'raisin'];
// Accessing element (set/get) or fast lookup O(1)

// get banana
console.log(fruits[1]); // banana
// set or update banana with guava
fruits[1] = 'guava' // guava
console.log(fruits); // (4) ['mango', 'guava', 'orange', 'raisin']

// Adding and removing item at the end O(1)
fruits.push('apple') //adding apple to the end of fruit
console.log(fruits); // (5) ['mango', 'guava', 'orange', 'raisin', 'apple']

fruits.pop(); //removes the last element of fruits and returns it
console.log(fruits); // (4) ['mango', 'guava', 'orange', 'raisin']

// Inserting an item with unshift() or splice () O(n)

/* The tradeoff of sequential arrangement of the array is that if we want to insert an item at
    any position other than the end of the array, we have to to shift and re-index each item after it 
    in the array 
*/
fruits.unshift("cherry"); // adding an item at the beginning of the array
console.log(fruits); // (5)  ['cherry', 'mango', 'guava', 'orange', 'raisin']

fruits.splice(1, 0, 'clementine'); // add clementine at index one, shifting the other elements
console.log(fruits) // (6)  ['cherry', 'clementine', 'mango', 'guava', 'orange', 'raisin']

// Removing an item with shift() or splice () O(n)
/* similar to insertion, it requires the re-indexing of elements comming after the deleted item */

fruits.shift() // removes and returns the first element of the array
console.log(fruits) //(5) ['clementine', 'mango', 'guava', 'orange', 'raisin']

fruits.splice(1, 1); // removes the element at index 1 (mango)
console.log(fruits) // (4) ['clementine', 'guava', 'orange', 'raisin']

// Iteration methods O(n)

// forEach(): execute a callback fuction on elements
fruits.forEach(fruit => console.log(fruit)) // will print every fruit in fruits array

// map(): copy or transform elements of an array and returns a new array
fruits.map(fruit => fruit + 's') // => (4) ["clementines", "guavas", "oranges", "raisins"]

// find(): search the array and return the first element meeting the condition given in the callback function
fruits.find(fruit => fruit === 'orange') // will return orange if found in the array, otherwise returns undefined

// filter(): search the array and returns a new array containing all the elements meeting the given condition in the callback function. Returns an empty array if none found.
fruits.filter(fruit => fruit[2] === 'a') // => (2) ["guava", "orange"] (fruit which third character is 'a')

// reduce(): applies a reducer function to elements and return a single value
fruits.reduce((string, fruit) => string + fruit[0], ''); // => "cgor" (this accumulate the first character of each fruit to a single string)

// some(): returns true if at least one element meets the condition given in the callback, otherwise false
fruits.some(fruit => fruit[0] === 'b'); // => false (no fruit in the array starts with b)

// every(): returns true if all elements meet the condition given in the callback, otherwise false
fruits.every(fruit => fruit[0] === 'c'); // => false

// reverse(): will reverse the order of elements in an array
fruits.reverse(); // => (4) ["raisin", "orange", "guava", "clementine"]
console.log(fruits); // (4) ["raisin", "orange", "guava", "clementine"]

// copying elements of array into a new array O(n)
const copy1 = fruits.concat();
const copy2 = [...fruits];
const copy3 = fruits.slice();
console.log(copy1); // (4) ["raisin", "orange", "guava", "clementine"]
console.log(copy2); // (4) ["raisin", "orange", "guava", "clementine"]
console.log(copy3); // (4) ["raisin", "orange", "guava", "clementine"]

// sorting an array O(n log n)

fruits.sort((a, b) => a.length - b.length); // (4) ["guava", "raisin", "orange", "clementine"]
// ordered the fruit by string length, smallest to largest.

```

Here is a summary of the time complexity of Arrays' operations:

![](https://lh6.googleusercontent.com/lXK3Y3YfZ1JejnVRsrrfM3-tJpvZgpNdJBZ_lYQvqw6YQ1EML0OEO8e1TUlUhLRT96JMTO_8LAv-zikezvtpug08Ym1BNd_1XCgOzw-0e8EmwZf15xK-5da-n8XO_dRutLbTjjuU)

> ## Final Note
> Arrays are great for fast retrieval of data in an ordered list with fast indexing. Adding and removing the last item is also fast. However, if you need to search items in an unsorted list, or frequently remove and insert elements in a list, an array might not be the most efficient data structure to use.


I hope you find this a helpful summary.  Let's take sometimes to digest it...

Until next time,

Happy learning / coding!!
