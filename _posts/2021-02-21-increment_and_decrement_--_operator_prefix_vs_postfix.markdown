---
layout: post
title:      "Increment (++) and Decrement (--) Operator Prefix vs. Postfix"
date:       2021-02-22 03:36:18 +0000
permalink:  increment_and_decrement_--_operator_prefix_vs_postfix
---


We all love shortcuts in programming like the ones we are about to cover. The increment operator `++` and the decrement operator `--` is the fastest way to  increase or decrease the value of a variable by 1 respectively, in languages like JavaScript, Java, C++, etc. Less typing, Happy us :).

Let's look at an example to see how they work:

```java

// Suppose we have the following integer variable

in num = 0;

++num;     // num is now 1
num++;    // num is now  2
--num;     // num went back to  1
num--;    // num is back to 0

```

This is too easy Wriiiigth.... Where is the catch? Well an important slight  difference I recently learned about these operators when they are used as prefix vs postfix.

## So What is the Difference? Prefix ++num vs. Postfix num++

Up until now, these looked like a syntactic preference to me. However, there's a difference in the return value of each expression. 

* When the ++ operator is used as prefix like in `++num`,  the value of num is incremented by 1 then, it returns the new value.
* When  the ++ operator is used as postfix like in `num++`, the original value of num is returned first then, num is incremented by 1.

The same logic goes for the` -- ` operator , with the exception that the value is decreased by one.

Let's see the use of `--` as prefix and postfix in Java :

```java

class Decrement {
    public static void main(String[] args) {
        int num1 = 10, num2 = 10;

        // num1 is displayed
        // Then, num1 is decreased to 9.
        System.out.println(num1--);

        // num2 is decreased to 9
        // Then, num2 is displayed
        System.out.println(--num2); 
    }
}

```

The output of this code will be:

```

10
9

```

Now in your program if you want to use the return value of the decrement or increment operator, you know how to avoid getting non desired output.

That's it for this post! Now, spread your newfound knowledge to the world!

Until next time, 

Happy Coding!!!
