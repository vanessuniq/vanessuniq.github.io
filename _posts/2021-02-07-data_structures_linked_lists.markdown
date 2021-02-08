---
layout: post
title:      "Data Structures: Linked Lists"
date:       2021-02-07 23:54:00 -0500
permalink:  data_structures_linked_lists
---


![](data:image/png;base64,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)

Welcome back to my Data Structure learning series. Today we are going to learn** Linked Lists** and how to implement it in JavaScript. 

# What are Linked Lists?
Linked Lists  are simple, dynamic,  lightweight, and flexible data structre. Like arrays, elements are stored sequentially in the list; however,  elements are not stored in adjacent blocks of memory but rather in “nodes”  or objects that are connected via memory pointers. They are useful for easy addition or removal of arbitrary data at the head and tail of a  list, without re-indexing elements, preventing the worst performance case of an array. 

The first element or entry point of the linked list is called the **head**. Each element or node in the linked list uses a pointer to indicate the element that comes after it. The last element  points to **null**.  If the list is empty, the head will reference to **null**.

# Two Types of Linked Lists

## Singly Linked List

In singly linked list, we can only move forward through the list (but not backward), with each node pointing to the next one. The diagram below illustrates the singly linked list concept:

![](https://computersciencewiki.org/images/2/27/DataStructuresLinkedList.png)


## Doubly Linked List

Here, the nodes in the list have two pointers: one pointing to the previous node and another pointing to the next node

![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAfAAAABlCAMAAACFrkGQAAABhlBMVEX///8AAADk5OSsrKzLy8tdXV2/v7+4uLhPT08nJyc4ODju7u7d3d1vb2/S0tKCgoL5+fmysrKenp54eHiTk5NgYGA9PT1JSUmMjIwbGxsgICBERET0///w8PD7//9nZ2f///fR3ZX///EvLy/h+v+cxUyMwEPr7L/x6rBctkFEsUHz9dP9998+xKJLy7IVFRV33NT89diN49/Q6tXk7bIAp0DT+P/B1YjF8uoArleCwFhPtlPo9eu58/Hd8OEXxKiqyl8yv5ULpyYArm/O4rel4dQAoVKcxmHF5Mfk7c6ZyG/A36BGv3xivmnZ46ZDu29uxXpl0sIIsl5Et1cAtXeb5++00HW38v8AogAAozCx4cnN7OVCr2i504lUvmJ0vG86rCqX5M1607hqy6LZ6tR3yGd3x4mhz4yj26xMy5KF0at0u0qgynpRuH/o+tRbw3nH9tex2rGp5d2k2bxrvVhHzr1eyZwAs3xbsibF139Wrh5V1dN3tReI4unP7LgmyLff35WGzZjzEODrAAAKjUlEQVR4nO2c+UPaSh7AZwDxQARF8ShiiLFYiSJoECpeqBUtoIDiQT1qrdqKzye2tXb37dv3n+/Ek5wKxKPs9/NDcJpkmMwncw9FCAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD+H5iZfe4UAI/EyDsGDXNjon+dm3iWxACPj2uaCA8FEB2NMgjNR4ec5M+hBRBeqbjeBcffhwLsaCweYxKLS1wSTaZO+gaeO13AI+HiFhfTocDIcna+z0+NU3P9nsgKegslvFJxTTuQOxTIcByX8nt2OK7fHVmFNrxycb27bMP5pnye6ZlAfAn300dQpVcq1710emFtbYoZjqyPTjlH1kLffpcSPke6H+8CNyFWu9FkJrJKx28HL/SGU7OYKx46mkVs9pEiz219pz8wdHTDMR9ggyPLQa3EbG71U9tjFB9xkA4mUrMOjSJOcCtULn8ToqJaxfty8OycI9ddEzG5omXkuY9HwQ/jC8ffBhOxpY9zoV2t8m9z8HV+O5iLxQfci/uDC6FPjEYRu9JTjpwf7Q0xbJbKzqQ1e5NeDJ4Pn/2uJHWwuMvuZw8O+1JaGl/I9375sDf69SBJHZ07erVriTYH3bGT4NGng13nXIrxnWumxZV868/lXYtb/X+kDwdyXCxw/z2Ph7VT+zg9x3+cv08OL0d3VkdSA/TRrlZlhWfBj95OkxK+n098ift7tSvhmUGUCQVzx/uzw7HPSR+nXQlPek6W8j0/Do+ZCy7AxrXMjeJpxPWax+k5dmRiSdd5NMokuCTq+UvLyPcYxG446Gg0OxPwBamoZm34fJZ0PhiKRDyfJX1B0pZrFLEriUa4/MWXw1nP6Ock26fZm1QS1VineZwe0qnaISU78u7PrT/7VjNrmhr/7ZgPIvo0i04PZ+fz7Jlz5uxZ2/DHEF5RGKrLwvDc6RfzlMKthvJQ6G7w3ZC6WgHd3bWqiE536OVj1pkRMuOyqBLHyX5LBVAixvSSD3rhO5rsJwV+buDJRv1S4YbWmnKwK3xRUz3qLC/vcIN8zOY6hLwdLVV3tHTjwqCYlle4RvAPuEkpcyzIhk26AvTYqFPBgJsKg6b2O+F62+UHO5r+jlzLTC9HhL8dQD288NfPJNxiIQcj7rbX3WK34/aCoAQ7xoWn7e1Y4Ysw1llxW0MB9d32+gZl6ruw4HSTknAD7kZes+CfmpVScfPQJmHaFISjJtxsEUZlVO/k6nC1INxxJ7wb26zkg906DDGJG+F/P6/wOmzmH0lY5hVz44qOLkFQMas7vbgeC6tOe6tqzKKsNigJRw24qVaYirb7hAse0ar8iPqOV0UKbxSEC4Qj/WUdxY6OTQ6+FOGdzbhF8ki4WTWC9hZBULlsWWvNxQm3CaNqUM5qkpWlCTc28ZCnblLAQloSwa1FCq8tzJ0Gvu0gwj3T71+IcD7YZShNuFXfSKjuwo0KVJPavzThD2nfSxPe8pCoBbc+WLhSZEQ4mozFGHfEj3x9g0Q4cf36u2p6tUTcnBlwe2nCTQ/Ju9KEWy7pwmaLPG1V6sLno0PCoe+tcOslnaTgWWUhjXipwtsuU+btLkynHRsR25dHnsgyg0bWOe4Hg3o4jsu/5riQeJfhI1GN7a8KqSFPWFqVrjMROi3YpICuTll4ZnFrMY9E2MRtuFJWm7x1KsKpHHcSTwpOP7gN12OLTVG4J76VnhLNmknacEGD18XHFSYvXzjs5A/Xx7AjfPX3U2C0twp8d98nXJyyB7fhVVivKLxnKpsJidcUJMIVOm312Iu8ysKH11cQFRacfqhwIxlGKwt3cx/33oZWBafVOm3kpHrZeRZa76nSPTsiMQ8V3oqNVmXh/Q5+B44QWeFWqyTmGlJRqpTwjHTdS1a43iy+jBTwBjXhpBamfgrrDlXhzfgRVqrKw9p9X6ftYupI+IiywtukM4rYjjqVha9H1j+JvcgJ12Fpu28gQ18V4XPnkjvkhDdh+VdVXbi4f60mXOdtl/2GZ8Tacd+wjH7jHzkXPKKc8Dr5uktFeD/y7QyKLpcRrsMKta+K8BFJWyEnvBmb5eeYtRNuwi9mYv0mkdcTLyrC3SmG3+NagFS41Y7bZL9GVTi7kxRdLu201eM6aY1+lUhl4b6d843TH4LTEuGNTQpJvk94IiLc3KFapb8Ybqe0LPxTqwrPcOn0umCDq0S49ZVS70pZ+GsuzQ9RhEiEN+E6+YhVhSP2YGlfWMglwruV59pUhEdC6dBH4dUvRrj7ZNeB3ifRJvnwxcfobT/JiO2rkZBF+LRqwuk3g+PjOUH3SiLcrjh8Uu60hcfHxyXDEolwMn5QWLMw4SqTTnf3vcVOrWLcorDa0lLLr8PU3L7CBblDkUQLe/8vSHgvx62ghQk0OeUgw/4V9g1pMH19fv5Ug6jBVRPuXgzww5HCakwiHNstNnnMuIYcLbfXFje1arh/Xue2vi9WeC25uU5+IdWLu2trvbetbxlz6U9K73luAGXkhOtwh3CwUO5c+n103F5brPA2jM1G2W0GRtxKTtzldbHCG5txrcKQSblKl+PlCJ9mt4JywvUKWu4GpcUKN2GlO5SrdFmknbYGrNTRLXEu/SZdfC9dYSfIbyvccdGfkxFej7GlWYAZ25qb2+6WdYteLTN5sU32UuVOmywywzJivFH22rKFlzAOl+MlCWfj8Ql0kQqg3vXVgiq9AdcILy1xtewafhyua8eye4bKF04O8veULxzpu2SvffCM/iUm5Q0QT0sv6Vm7IhP8jqo4t+tk36TiU/M75OAgTyR8KTVYD9e1ypZDifAauatukd0AoZOveL1Vuvo7dDZcGBSja8KGwnCD2h4PM64S7ODCr9Q2eLXiusJglXRP2xNBZ52I2suSP4aGyJHaIx+XByf/SMbCS43Y0Hm3xtVpwm2dSgtgBGt7lbUg2HlbtsJhyRR2J241F2DzttvMKtRhwemq6+E9JRPzg5bMVbgSnlvaD0pibhPv6VLZ73W5JUwYlm/cnhereBxeHlexuCPrIb/oi7TZxJhZX1+WTJYay+OyTqN+Db5PPevPgJ4Hnb48rmIZ/oIuJnwbh46Zwzx75mA/qn9pEWT+hX79NX86i04PA+48ckvW0UuF+rmbWx4fXRvcjHBj//6L7lu9/57fEd9OhNMs024ZXo+kxhKp3cT0ad9GPHih3R6eTGRtyjF3Ppv5cXD+n3P0RrNfKlI/P31l6J0xX/ps4e/hfld/hf5C3JMOuKbCQ1GGjWapIQc9pMlzDscYJ0r0o/8un/4TuBg40q60ZL4jJ/+Twp4fh18dR4dTmv1wh/pFEsn2Bdits2jWtzUqbpEqBU/fIud3c8fRrf307NFqrzY5OLzMIOTqd3i2Fo8D7siUdqUlw1cWc4PIlYrvOkci4kXV0qEWiHB6O4DcSyd5tClZzKkUPOn8OHLHHL2p0+0V1/cebV5sml8MocNOFB4ad1B7GmZemI9rnBz2yBiDHqq4X9c/OqRKJ53qGOMJ/fM14OFSlfpiA9fQ/H86wm44kOf0jEEz0pEoAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAFQa/wNi9q1rvUKsNAAAAABJRU5ErkJggg==)


As shown in the diagram, with doubly linked list, we can move both forward and backward.

# Implementing a Singly Linked list in JS
We will be implementing how to create a list as well as the common linked list operations and time complexity for each: append, prepend, insert, remove,  and find a node, as well as getting the size of the list.

```js

//Let's start by constructing the node using JS Class
class Node {
    constructor(data) {
        this.data = data;
        this.next = null;
    }
}

/**
 * An instance of the Node class will have two properties: 
 * the data that will hold the value of the passed element, 
 * and the next property (initialized with null) that is the pointer to the next node.
 */

//Now let's implement the linked list class:
class LinkedList {
    constructor(data = null) {
        if (data !== null) {
            const node = new Node(data);
            this.head = node;
            this.tail = node;
            this.size = 1
        } else {
            this.head = null;
            this.tail = null;
            this.size = 0
        }
        //Having access to a head and tail makes appending and prepending, getting first and last node
        // operations constant time O(1). Also initializing the list with the size  makes getting the length
        // of the list a constant time operation as well
    };
    // Checking if the list is empty O(1)
    isEmpty() {
            return !!this.head;
            // or return this.size === 0
        }
        // Getting the first node of the list O(1)
    getFirst() {
        return this.head;
    };
    // Getting the last node of the list O(1)
    getLast() {
        return this.tail;
    };
    // Adding a node at the end of the list O(1)
    append(data) {
        const node = new Node(data);
        // if the list is empty, set the head = node
        if (this.isEmpty()) {
            this.head = node;
            this.tail = node;
        } else {
            this.tail.next = node;
            this.tail = node;
        }
        // increase the size
        this.size += 1;
        return node;
    };
    // Adding a node at the beginning of the list O(1)
    preppend(data) {
        const node = new Node(data);
        // if the list is empty, set the head = node
        if (this.isEmpty()) {
            this.head = node;
            this.tail = node;
        } else {
            // point the node next property to the list head then set the list head to the node
            node.next = this.head;
            this.head = node;
        };
        //increase the size
        this.size += 1;
        return node
    };

    // insert an element at a given position of the list O(n)
    insertAt(data, position) {
        // if position is greater than the size of the list, add to the end of the list
        if (position >= this.size) {
            this.append(data)
                // if position is index 0, preppend the data
        } else if (position === 0) {
            this.preppend(data)
        } else {
            const node = new Node(data);
            // start a counter
            let start = 0;
            // keep track of previous and current node
            let previous = null,
                current = this.getFirst();
            // iterate over the list until the given position is reached
            while (start < position) {
                previous = current;
                current = current.next;
                start++
            };
            // attach the current node to the pointer of the insert element
            node.next = current;
            // set the previous node to point to the insert element
            previous.next = node;

        }
        // increase the size
        this.size += 1;
    };

    // find an element and return its index O(n)
    indexOf(data) {
        // if the list is empty return -1
        if (this.isEmpty()) {
            return -1
        }
        // initialize the counter and the current node
        let count = 0,
            current = this.getFirst();
        // iterate over the list
        while (current) {
            // check if the current node data is equal to the given data and return the count if it is
            if (current.data === data) {
                return count
            };
            // if no match, move to the next node and increase the count
            current = current.next;
            count++;
        };
        // if the element is not found
        return -1
    };

    // remove an element from the list O(n)
    delete(data) {
        // return false if the list is empty
        if (this.isEmpty()) {
            return false
        };
        // track the current and previous node
        let previous = null,
            current = this.getFirst();
        // itertate over the list
        while (current) {
            // compare the given data to the current node data
            if (current.data === data) {
                if (previous) {
                    // if the previous node is not null
                    previous.next = current.next

                    if (!current.next) {
                        // if the current node is the last node, set the tail to the previous node as well
                        this.tail = previous
                    }

                } else {
                    // if the previous node is null, it means that the given data was found at the head of the list
                    this.head = current.next
                };
                this.size -= 1;
                return current;
            };
            // if there's no match
            previous = current;
            current = current.next;
        };

        // if the data not found
        return false;
    }


}

```

This is a quick cheat sheet summarizing the time complexity of singly linked list operations

![](https://lh4.googleusercontent.com/TKwGx_AoCAt0rJTBKzxrPg8OPn1Eg0GvxnjkVr_CDzhY1d87o9dIJT2WvFQH7Mh6d-Rhm1epAvus8wSwuzh05LIp_G3a4fV3xVnfRSn3FKniT0BQNnk9pu71gLkFNQlrW27BTHZH)

> ## Final Note
> Linked list are best when we need to efficiently insert or delete items from a list, and when we need a flexible list size.  Unlike arrays, it does not require re-indexing other elements in the list. On the other hand, linked lists by themselves aren’t as good for indexing as arrays and they use more memories as it  they need extra space to store the pointers. Additionaly, search operations are slow with linked lists as there is no direct access to data (no keys/indexes). We must iterate through the list sequentially to perform a lookup.
> 

That's all I have on linked list. I hope you found it helpful. 

Until next time,

Happy learning / coding!!


