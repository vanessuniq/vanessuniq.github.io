---
layout: post
title:      "Operators and Flow Control in Java"
date:       2021-03-01 01:37:04 +0000
permalink:  operators_and_flow_control_in_java
---


In this post, we will be learning different types of operators in Java and how to use them. 
Operators are used to perform operations on variables and values. There are five types of Java operators: assignment, arithmetic, relational, logical, bitwise and unary operators.

The following code template will show the most used operations with examples on how to use them.

```java

public class Operators {
    public static void main(String[] args) {

        // Arithmetic Operators
        /**
         * 
         *   Operator	Operation
         * 
             +	        Addition
             -	        Subtraction
             *	        Multiplication
             /	        Division
             %	        Modulo Operation (Remainder after division)
         */
        // the modulo operator is mainly used with int data type; and the division of two integers
        // results in an integer. 

         int a = 10, b = 8;

         System.out.println("a + b = " + (a + b));
         System.out.println("a - b = " + (a - b));
         System.out.println("a * b = " + (a * b));
         System.out.println("a / b = " + (a / b));
         System.out.println("The remainder of a / b is " + (a % b));

         // Assignment operators
         // create variables
        int num = 4;
        int var;

        // assign value using =
        var = num;
        System.out.println("var using =: " + var);

        // assign value using =+
        var += num;
        System.out.println("var using +=: " + var);

        // assign value using =*
        var *= num;
        System.out.println("var using *=: " + var);

        // Relational operators used for comparison. returns true or false

        // == operator
        System.out.println(a == b);  // false

        // != operator
        System.out.println(a != b);  // true

        // > operator
        System.out.println(a > b);  // true

        // < operator
        System.out.println(a < b);  // false

        // >= operator
        System.out.println(a >= b);  // true

        // <= operator
        System.out.println(a <= b);  // false
        
        // Logical operators: used to check two expressions. it results in true or false

        // && operator
        System.out.println((5 > 3) && (8 > 5));  // true

        // || operator
        System.out.println((5 < 3) || (8 > 5));  // true

        // ! operator
        System.out.println(!(5 == 3));  // true
        System.out.println(!(5 > 3));  // false

        // Unary operators

        int result1, result2;

        // original value
        System.out.println("Value of a: " + a);

        // increment operator
        result1 = ++a;
        System.out.println("After increment: " + result1);

        System.out.println("Value of b: " + b);

        // decrement operator
        result2 = --b;
        System.out.println("After decrement: " + result2);

        // instanceof operator: to check if an object is an instance of a class.
        // the syntax is objName instanceof className
        String myString = "love";
        System.out.println(myString instanceof String);

        // Ternary operator: conditional operator shorthand for if-then-else statement.
        // it must evaluate an expression on the left. Syntax: Expression ? a : b;
        int c = a < b? a : b;
        System.out.println(c);
    }
}
```

### Code Output
```java
a + b = 18
a - b = 2
a * b = 80
a / b = 1
The remainder of a / b is 2
var using =: 4
var using +=: 8
var using *=: 32
false
true
true
false
true
false
true
true
true
false
Value of a: 10
After increment: 11
Value of b: 8
After decrement: 7
true
7
```

## Flow Controls

Java code  statements  are usually executed from top to bottom, in the order that they appear. Flow controls are used to break or change the flow of execution by introducing decision making or looping, pushing the program to execute specific blocks of code based on the given conditions.

There are several flow controls in Java, some of which' syntax are shown and explained below:

```java
// utility to ask user for input and read it
import java.util.Scanner;

public class FlowControl {
    public static void main(String[] args) {
        int num = 8;
        // if-else-if statement: used to execute a block based on wether the given condition is true or not
        if (num < 0) { 
            System.out.println("The variable num is a negative number");
        } else if (num > 0) {
            System.out.println("The variable num is a positive number");
        } else {
            System.out.println("The variable num is zero");
        }
        
        // switch statement: execute a specific block code per case
				
        char operator;
				
        Double double1, double2, result = 0.0;
				// define input variable
        Scanner input = new Scanner(System.in);
				
        // ask user for input
        System.out.print("Choose an operator (+, -, *, /): ");
				// read input
        operator = input.next().charAt(0);

        System.out.print("Enter your first number: ");
        double1 = input.nextDouble();

        System.out.print("Enter your second number: ");
        double2 = input.nextDouble();
        
				// execute each block based on the matching case
        switch (operator) {
            case '+':
                result = double1 + double2;
                break;

            case '-':
                result = double1 - double2;
                break;

            case '*':
                result = double1 * double2;
                break;
                
            case '/':
                result = double1/double2;
                break;

            default:
                System.out.println("Unknown operator: thus, the following is not a valid operation.");
                break;
        }
        System.out.println(double1 + " " + operator + " " + double2 + " = " + result);

        /**
         * Note: The Java switch statement only works with:
         * 
         * Java Primitive data types: byte, short, char, and int
         * Java Enumerated types
         * Java String Class
         * Java Wrapper Classes: Character, Byte, Short, and Integer.
         */

        // for loop
        int sum = 0;
        for (int i = 1; i <= num; i++) {
            sum += i;
        }
        System.out.println(sum);

        // for-each loop to iterate through arrays
        int[] arr = {2,4,5,8,10};

        for (int i : arr) {
            System.out.print(i);
        }

        // while loop
        // ask user for numbers and add them until the input number is negative
        int sum1 = 0;
        System.out.print("Sum = " + sum1 +". Enter a number to add to Sum: ");
        int inputNum = input.nextInt();

        while (inputNum >= 0) {
            sum1 += inputNum;
            System.out.print("Sum = " + sum1 +". Enter a number to add to Sum: ");
            inputNum = input.nextInt();
        }
        System.out.println("The sum of all positive number you entered is " + sum1);
        input.close();
    }
}

```

### Code Output
```java
The variable num is a positive number
Choose an operator (+, -, *, /): +
Enter your first number: 34
Enter your second number: 23
34.0 + 23.0 = 57.0
36
2 4 5 8 10
Sum = 0. Enter a number to add to Sum: 10
Sum = 10. Enter a number to add to Sum: 49
Sum = 59. Enter a number to add to Sum: 30
Sum = 89. Enter a number to add to Sum: -20
The sum of all positive number you entered is 89
```

That's all for this post, you can learn more about Control Flow Statements in Java [here](https://www.scaler.com/topics/java/control-flow-statements-in-java/). Now take time to digest what you've learned and do not hesitate to leave a feedback.

Until next time,

Happy learning / coding !!!!
