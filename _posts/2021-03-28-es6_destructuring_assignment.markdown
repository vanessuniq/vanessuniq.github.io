---
layout: post
title:      "ES6 Destructuring Assignment"
date:       2021-03-28 21:49:08 +0000
permalink:  es6_destructuring_assignment
---


You might be familiar with the ES6 destructuration, but do you know all it features? Up until now, I was only familiar with ES6 destructuring syntax to extract values from objects and arrays. Here I will cover all possible ways you can use this special syntax introduced in ES6.

Destructuring assignment is a syntactic sugar for neat assignment of values taken directly from an object. Below are the different ways ES6 destructuring syntax can be used:

## Extract Values from an Object

consider the following object: 

```js
const person = {firstName: "Vanessa",  lastName: "Learn", age: 26};
```

Before ES6, to retrieve the values of an object and assign them to a variable, we will do it as follow:

```js
const firstName = person.firstName;
const age = person.age;
```

ES6 destructuring syntax  makes the assignment's statements simpler and neater:

```js
const {firstName, age} = person;
```

The `firstName` and `age` variables will be created and assigned the values of their respective values from the person object. This works as long as the variables we are creating  and the object's properties have the same name.

## Extract Values from an Object and Assign them to New Variables Name
It might happen that you need to create new variable's name as opposed of using the name derived from the object's property. This can be done as follow:

```js
const person = {name: "Vanessa Learn", age: 26};

// assigning the name and age values from person object to userName and userAge variable
const {name: userName, age: userAge} = person;

console.log(useName); // "Vanessa Learn"
console.log(useAge); // 26
```

The above assignment code can be read as "get the value of `person.name` and assign it to a new variable named userName" and so on.

## Extract Values and Assign Variables from Nested Objects

It is also possible to destructure values from nested objects. Consider the following object:

```js
const subjects = {
  math: {
	  passingGrade: "C or higher",
		topic: "Algebra"
	}
};
```

Now let's extract the `grade` and `topic` values of `subjects.math` and assign them to variables with the same name:

```js
const { math: { passingGrade, topic } } = subjects;

console.log(passingGrade); // "C or higher"
console.log(topic); // "Algebra"
```

we can also assign an object properties' values to variables with different names as follow:

```js
const { math: { PassingGrade: validGrade , topic: subjectTopic } } = subjects;
```

## Extract Values from Arrays and Assign to Variables
Destructuring arrays is as simple as destructuring objects.

```js
const [a, b] = [1, 2, 3, 4, 5, 6];
console.log(a, b); // 1, 2
```

Each variable in the array on the left is assigned a value in the array on the right having the same index or position. As in the above example, the variable `a` is assigned the first value (1) of the array, and `b` is assigned the second value(2) of the array. 
The value at any index in an array can also be accessed with destructuring, using commas to reach the desired index:

```js
const numbers = [1, 2, 3, 4, 5, 6];

// assigning the value at index 0 and 3 to the variable a and b respectively
const [a, , , b] = numbers;

console.log(a,b); // 1,4
```

## Pass an Object as a Function's Parameters
It is possible to destructure an object in a function argument. Taking the following example:

```js
const fullName = (personObject) => `${personObject.firstName} ${personObject.lastName}`;

const person = {firstName: "Vanessa", lastName: "Learn"};

fullName(person); // will return "Vanessa Learn"
```

The `fullName` function can be refactored with ES6 destructuration as followed:

```js
const fullName = ({firstName, lastName}) => `${firstName} ${lastName}`;

const person = {firstName: "Vanessa", lastName: "Learn"};

fullName(person); // will return "Vanessa Learn"
```

When the object `person` is passed to the `fullName` function, the values of `firstName` and `lastName` properties are destructured from the function parameter for use within the function.

Now you know a little more about ES6 destructuring syntax and hope it can help you refactor your codes to make it more readable.

Happy Coding/ Learning!!!
