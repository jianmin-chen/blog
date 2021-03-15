---
layout: post
title: A Quick Refresher on Data Structures ~ Linked Lists, Queues, and Stacks
date: 2021-02-01 01:14:45 -0500
categories: data structures
tags: data structures
comments: true
---
<img src="https://images.idgesg.net/images/article/2020/03/jw_pt3_data_structure_algorithms_java_coding_programmer_2400x1600_davidgoh_akindo_gettyimages_531237630_473456596-100834801-large.jpg" alt="Computer image" style="display: block; margin: 0 auto;">

If you're someone like me, besides reviewing your [basic algorithms]({% post_url 2021-01-30-a-quick-refresher-on-algorithms %}) you might also need to review your basic data structures. This article, along with 2 others soon to come, intend to do exactly that! In this article, I'll include explanations and code(in JavaScript) for three basic data structures:

* linked lists
* queues
* stacks

Okay, let's get started!

## What are data structures?
Data structures are exactly what they sound like they are - structures used to store data. For example, if you've used arrays or dictionaries before, you know they're used to store data in a accessible way. There are a bunch more data structures out there, including some we'll talk about today.

Before we get started however, I want to nail down some terminology that will make it easier for you to understand the implementations of the data structures we discuss. One of these terms, which we'll use often throughout this article, is **node**. **node** is used as a term of art in computer science to describe a element containing one or more values, and is often depicted pictorially as a rectangular shape.

Okay, let's get started now!

## What are some basic data structures?
There are many basic data structures out there. Some of the most commonly used and implemented ones are linked lists, queues, and stacks, which are the ones we'll talk about today. Let's dive straight in!

### What is a linked list?

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/6d/Singly-linked-list.svg/408px-Singly-linked-list.svg.png" alt="Linked list(Wikipedia)" style="display: block; margin: 0 auto;">
<a href="https://en.wikipedia.org/wiki/Linked_list" style="display: block; margin-top: 25px; text-align: center; width: 100%;">Pictorial depiction of a linked list(Wikipedia)</a>

A linked list is, as you guessed it, a list. You probably are aware of what a list is; it's a array, or a collection of nodes already implemented in most programming languages where you can access each value by their position, which starts from zero.

Lists are great. However, lists have fixed sizes, in most programming langauges. Even in programming languages with lists that have dynamic sizes(e.g JavaScript or Python), when a item is inserted into a list the list has to be extended under the hood. And to do so, the computer has to copy the elements in one list into a new one, usually by allocating extra memory. So what is the running time of extending a list to fit a new item? Well, we have to go through `n` iterations to copy all the items in a list. Then, to create a list with a added item, we have to set up more memory, and add the new item in. Therefore, we can estimate that the running time is somewhere around **Θ(n)**, no matter where we are adding the new item.

Linked lists come to the rescue in such a situation! These lists are made up of nodes that contain two pieces of information: it's own value, and a variable that contains the next node or the location of the next node(known as a address in computer science). The above picture represents this. Once the list gets to the last item, there are no more nodes, so the address to the next node is nothing, or `null`.

You might be thinking, "Well, it's still going to take O(n) iterations to add an item to a linked list, since we have to start from the first node, go to the next node by following the address, and so on! Hmph. Not much better than a list." Well, what if we don't care about the order of the nodes in our linked list? In that case, we can add new nodes to a linked list at the beginning rather than the end, which would still take **O(n)** iterations when using a list. To do so, we can create a temporary variable with the address of the first node in a linked list. Then, we can add the new node to the linked list, and have this new node point to the temporary variable. The running time of this then becomes **O(1)**, or constant time - so much better than a list!

Okay, let's take a break from the talking and actually code something. To implement a linked list, we need to implement a node first, and that's actually pretty simple:
~~~javascript
class Node {
    constructor(val=null, nextNode=null) {
        this.val = val;  // own value
        this.nextNode = nextNode;  // pointer to next node
    }

    getInfo() {
        const nextVal = this.nextNode ? this.nextNode.val : null;  // if there is a next node, set nextVal to the next node's values; otherwise, set nextVal to null
        return `Value: ${this.val} | Next Node: ${nextVal}`;
    }
}
~~~
Then we can implement a linked list:
~~~javascript
class LinkedList {
    constructor(val=null) {
        this.startNode = val == null ? null : new Node(val);  // if val is null, startNode will start out as null; otherwise, startNode will be a node containing val
        this.len = val == null ? 0 : 1;  // either 0 or 1 element in linked list, depending on whether list was initialized with a value or not
    }

    print() {
        for (let node = this.startNode; node != null; node = node.nextNode) {  // start from starting node and keep on jumping to next node until we reach the last node
            console.log(node.getInfo());
        }
        console.log("\n");
    }

    addNode(val=null, pos=this.len) {  // default position to add a node is at the end of the linked list
        if (pos < 0 || pos > this.len) throw Error(`Attempt to add node at position ${pos} of linked list, which has a length of ${this.len}`);
        else if (pos == 0) {  // special corner case for adding to beginning of linked list
            let tmp = this.startNode;  // temporary variable that stores current linked list
            this.startNode = new Node(val, tmp);  // let new node point to original linked list, joining the two linked lists together
        }
        else {  // otherwise, we're adding to middle of list or end of list
            let node = this.startNode;
            for (let i = 0; i < pos - 1; i++) {  // start from starting node and keep on jumping to next node until we reach node before pos
                node = node.nextNode;
            }
            let tmp = node.nextNode;  // temporary variable that stores nodes that current node points to
            node.nextNode = new Node(val, tmp);  // let current node point to new node, which points to the rest of the nodes
        }
        this.len++;  // new item was added
    }

    removeNode(pos=this.len - 1) {  // default position of node to be removed is position of last node in linked list
        if (this.startNode == null || pos >= this.len) throw Error(`Attempt to remove node at position ${pos} of linked list, which has a length of ${this.len}`);
        else if (pos == 0) {  // special corner case for removing first node
            let tmp = this.startNode.nextNode;  // temporary variable that stores current linked list
            delete this.startNode;  // delete the first node
            this.startNode = tmp;  // new first node is node after original first node
        }
        else {  // otherwise, we're removing from middle of list or end of list(a special corner case)
            let node = this.startNode;
            for (let i = 0; i < pos - 1; i++) {  // start from starting node and keep on jumping to next node until we reach node before pos
                node = node.nextNode;
            }
            let tmp = node.nextNode.nextNode;  // temporary variable that stores nodes that node after current node points to
            delete node.nextNode;  // delete node after current node, which will delete the node at pos
            node.nextNode = tmp;  // let current node point to tmp, which points to the rest of the nodes
        }
        this.len--;  // an item was removed
    }
}
~~~
Whew, that must be a lot to take in! Let's go through each of the methods. First, the class has a `constructor()` method, which sets up two variables: one for the first node, and one to keep track of the length of the linked list.

Then, we have a `print()` method, which uses a `for` loop to traverse the linked list by following the addresses, sort of like following breadcrumbs, and printing out the contents of each node it encounters.

Afterwards, we have a `addNode()` method, which does exactly as it's name says. It takes two parameters, `val` and `pos`, and creates a new node at `pos` with the value `val` as long as `pos` exists. If you look at the code again, you may notice that we have a corner case for where `pos` equals 0 - i.e adding to the beginning of our linked list - meaning that our linked list is zero-indexed just like a normal list is.

Last but certainly not least, we have a `removeNode()` method, which takes one parameter, `pos` and removes the node at that position as long as that node exists.

That's all for linked lists! As you'll see later on, linked lists are very useful for implementing other data structures.

### What is a queue?

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/52/Data_Queue.svg/300px-Data_Queue.svg.png" alt="Queue(Wikipedia)" style="display: block; margin: 0 auto;">
<a href="https://en.wikipedia.org/wiki/Queue_(abstract_data_type)" style="display: block; margin-top: 25px; text-align: center; width: 100%;">Pictorial depiction of a queue(Wikipedia)</a>

A queue is basically a data structure following the rules of a queue in real life. For example, a line in a restaurant is a queue. If two people get in line, the first person gets out first. It would obviously be very unfair if the second person got out first, right?

This is exactly the concept a queue data structure follows - FIFO, or *First In, First Out*. It stores nodes just like a list would, but removes the nodes that get added first in order to adhere to the previously mentioned mantra. While we're discussing such terminology, let me also include that there are specific terms for adding and removing from a queue: "enqueue" and "dequeue".

Okay, how can we implement a queue? We could just use a list, but let me propose a more elegant way: inheriting from the `LinkedList` class we created before! By doing this, we reduce the number of lines of code needed to implement a queue, in a elegant way. In fact, this is all you need to implement a queue if you happened to have a linked list implementation already:
~~~javascript
class Queue extends LinkedList {  // inherit all properties/methods from LinkedList
    constructor(val=null) {
        super(val);
    }

    addNode(val=null) {  // also known as enqueue - naming this as addNode allows it to override the addNode() method of LinkedList so nodes can only be added to end of queue
        super.addNode(val);
    }

    removeNode() {  // also known as dequeue - naming this as removeNode allows it to override the removeNode() method of LinkedList so nodes can only be removed from beginning of queue
        super.removeNode(0);
    }
}
~~~
Neat, right?

### What is a stack?

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b4/Lifo_stack.png/350px-Lifo_stack.png" alt="Stack(Wikipedia)" style="display: block; margin: 0 auto;">
<a href="https://en.wikipedia.org/wiki/Stack_(abstract_data_type)" style="display: block; margin-top: 25px; text-align: center; width: 100%;">Pictorial depiction of a stack(Wikipedia)</a>

Finally, let's discuss a stack. Now that we know what a queue is, a stack is actually very easy to understand. It's basically the opposite of a queue! Instead of following the acronym FIFO, it instead follows the acronym LIFO: *Last In, First Out*. It's like taking a plate from a a stack of plates - you don't take the bottom plate, which happens to be the first plate, do you? No, you normally would take the top plate, which happens to be the last plate.

Some extra information to chew on again: to add a node to a stack, you "push" it, and to remove a node, you "pop" it.

Continuing on, this also means the code for implementing a stack is short and sweet, especially if we take advantage of our linked list implementation:
~~~javascript
class Stack extends LinkedList {  // inherit all properties from LinkedList
    constructor(val=null) {
        super(val);
    }   

    addNode(val=null) {  // also known as push - naming this as addNode allows it to override the addNode() method of LinkedList so nodes can only be added to top of stack
        super.addNode(val);
    }

    removeNode() {  // also known as pop - naming this as removeNode allows it to override the removeNode() method of LinkedList so nodes can only be removed from top of stack
        super.removeNode();
    }
}
~~~

## Conclusion
That's it! Try out some of these implementations. You can even add extra features to them, such as changing the value of a node in a linked list. If you have any questions to ask, fire below! The code is located [here](https://github.com/jianmin-chen/blog-programs/blob/main/A%20Quick%20Refresher%20on%20Data%20Structures/part-one.js).

{% include disqus.html %}
