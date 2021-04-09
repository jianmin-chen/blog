---
layout: post
title: A Quick Refresher on Data Structures ~ Binary Search Trees and Tries
date: 2021-04-09 11:24:45 -0500
categories: data structures
tags: data structures
comments: true
---
<img src="https://images.idgesg.net/images/article/2020/03/jw_pt3_data_structure_algorithms_java_coding_programmer_2400x1600_davidgoh_akindo_gettyimages_531237630_473456596-100834801-large.jpg" alt="Computer image" style="display: block; margin: 0 auto;">

This is a third, final article explaining basic data structures! By the end of this article, you'll have(hopefully!) a understanding of binary search trees and tries, which are two tree structures that are pretty useful.

If you'd like to take a look at the first article, which focuses on linked lists, stacks, and queues, take a look [here]({% post_url 2021-02-01-a-quick-refresher-on-data-structures-~-linked-lists,-queues,-and-stacks %}). Once you're done with that, you might also like to take a look at the second [article]({% post_url 2021-02-19-a-quick-refresher-on-data-structures-~-hash-tables-and-dictionaries %}) in this series, which explains hash tables and dictionaries.

Anyways, let's get started now!

## What is a binary search tree?

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/da/Binary_search_tree.svg/180px-Binary_search_tree.svg.png" alt="Binary search tree(Wikipedia)" style="display: block; margin: 0 auto;">
<a href="https://en.wikipedia.org/wiki/Binary_search_tree" style="display: block; margin-top: 25px; text-align: center; width: 100%;">Pictorial depiction of a binary search tree(Wikipedia)</a>

The name of a binary search tree most likely gives you a idea of what it is, as long as you've heard of and understand binary search(check out [this]({% post_url 2021-01-30-a-quick-refresher-on-algorithms %}) article for a quick explanation if you don't know what binary search is). But before we get started on that, I'd like to discuss what a tree is.

### What is a tree?
We're going to be focusing on trees for the rest of this article, so it's pretty important that you know what trees are. No, not the trees you can see outside, but the data structure. Trees are extremely reminiscent of family trees. They have a starting node, typically called the **root** or **parent node**. This node, which has a value, commonly points to two other nodes(although it can point to any number of nodes), which are called it's **child nodes**. When depicted pictorially, these nodes can either be the **left node** or the **right node** of the parent node(take a look at the picture above and you'll understand). These nodes can point to two other nodes, and so on. Another way to view trees is to think of them as linked lists with nodes that point to one or more nodes.

Okay, back to binary search trees. Like I said before, the name used to describe these trees might ring a bell in your head. These trees contain nodes that are ordered in such a way that you can perform binary search on them. So searching a binary tree is **O(log n)**, basically.

Of course, you might also want to know the running time of inserting into a binary search tree. You might say, "Since we *do care* about the order of the nodes in a binary search tree, the running time of inserting into a binary search might be, eh, **O(log n)**, because we have to check the root node, compare it with the node we're adding, and then either head for the left or right node over and over again until we reach the end. Clearly, we'll only search need to search half the tree."

Well, that seems correct. But, are we really thinking in the worst case scenario? I dare say no. What if we have a **unbalanced** binary search tree?

<img src="https://qph.fs.quoracdn.net/main-qimg-61f940ac3024035312e258262c8945fe" alt="Unbalanced binary search tree" style="display: block; margin: 0px auto;">

Adding 60 to that binary search tree certainly isn't going to be O(log n). It's O(n), right? We would have to traverse all the nodes in the tree just to insert 60. So technically, the worst case running time of inserting into a binary search tree would be **O(n)**.

What's the point of using a binary search tree if inserting into one takes up more time then inserting into a more simpler data structure like a linked list? Well, let's adjust the above tree so it looks something like this:
~~~text
      30
     /  \
   20   40
  /        \
10         50
~~~
Now how long is it going to take to insert 60? That's right, O(log n)! The tree is now considered to be **balanced**, because the number of nodes on both sides are equal. That's better! Of course, the computer doesn't know how to balance a tree right away - it needs some algorithmic help. But that's a topic for another day...

## What is a trie?
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/be/Trie_example.svg/250px-Trie_example.svg.png" alt="Trie(Wikipedia)" style="display: block; margin: 0 auto;">
<a href="https://en.wikipedia.org/wiki/Trie" style="display: block; margin-top: 25px; text-align: center; width: 100%;">Pictorial depiction of a trie, which is used to make the words "a", "to", "tea", "ted", "ten", "i", and "inn"(Wikipedia)</a>

Okay, let's move on to tries now. As you might already realize, data structures tend to build atop one another. Tries build on top of trees and arrays to form a tree made of arrays.

Although it doesn't show it for simplicity, the nodes in the image above are items in arrays, which point to one another to form words. We start out with what seems like a empty node - really a node containing a array of size 26. This array contains the letters A through Z, but only three of them are used - "t", "a", and "i".

"a" doesn't point to anything; in other words, it contains a `null` pointer. Therefore, we stop there, and the word "a" is formed. But there's also a number 15. Why? It's there to make finding "a" easily. If we need to find "a", we can simply use a reference to it - 15. Then, we can start going through our tree, most likely using a hashing function(explained in the second [article]({% post_url 2021-02-19-a-quick-refresher-on-data-structures-~-hash-tables-and-dictionaries %}) in this series). Using a hashing function, we know that "a" is at index zero of this first array. Okay, now that we're there, we can see if 15 is there, which will indicate that we've found "a". It is, so perfect, we've found "a"! And that's searching a trie in a nutshell.

On the other hand, "t" and "i" point to arrays of their own. Let's start with "i" since it's easy. In fact, it's a good example of where using numbers to reference words becomes super useful. "i" is already a word, so we have the number 11. We could end there, but what if we're trying to find the word "in"? Well, in that case, we can go farther and take a look at the array "i" points to. In this array, we have a "n", and the number 5, indicating the end of the word "in". That's why we use numbers - to understand where the word we're trying to find ends. If we wanted to add "inn" to this trie, we could have the array we're currently at point to another array, containing the letter "n" and a number reference.

Hopefully by now you can understand how the rest of the words, all starting with the letter "t", might be formed. We have the letter "t" in our first array(remember, the one containing "a" and "i"), and this letter/element in array points to two letters in another array - "o" and "e". The letter "o" forms the word "to", while the letter "e" goes further and points to another array, which contains the last letters of the words "tea" and "ted". If we wanted to add "ten", we could use the letter "n" in this array and leave a number reference there.

Now that you have a basic understanding of what tries are, let's take a look at why anyone would want to use them. I mean, it's pretty obvious - for large amounts of data, they would take up a insane amount of memory! But there are some clear advantages of using a trie. How long would finding a item in a trie take, in terms of running time? Well, as long as we have the word we're trying to find, a number reference, and a working hashing function, we can do it in **O(1)**.

For example, let's use our above example again and try to find "in". "In" is two letters long, and is represented by 5. Here's a general list of the steps we would need to take:
1. The first letter, "i" is at location 8 of the array in the first node, so the first letter is nailed down. Is our number reference, 5, there? Nope, so let's move on.
2. Going further down the rabbit hole, we arrive at a second node, containing another array. Let's find "n" in this array - location 12. We finished the second letter! Okay, is 5 there? Yes, it is! We've found "in".

So it took us around two steps to find "in" - in other words, it took us `<amount of letters in "in">` steps to find "in". If we wanted to find "inn", we would need three steps. See a pattern here? We know the amount of steps it'll take for us to find something in a trie. Therefore, finding something in a trie is always O(1).

Of course, we'd also like to take a look at inserting into a trie. The general steps for inserting into a trie might go like this, using "code" and the above trie as an example:
1. We need to start our word by finding the location of the first letter, "c", in the array of the first node in our trie. Where is that? That's at location 2 of the first array, so we mark something there, most likely a pointer that doesn't point to anything yet.
2. Now we need to find the second letter, "o". Well, we can't find "o" - because "c" doesn't point to anything yet. So we can create a new node that we can point to. "o" in the new array of this node will be at position 14. Great! We've got "o".
3. We're getting closer. We need to insert "d", because "o" doesn't point to anything yet. The process to do this is similar to step two.
4. Finally, we can repeat what we're doing to insert "e". This time, we also need to add a number reference. How about 0? It's not in our list of number references yet, so we can use it.

Okay, that was four steps. Once again, a pattern is starting to emerge. *Inserting into a trie takes `<amount of letters in item being added>` steps*, which is constant time, meaning inserting into a trie is **O(1)** as well.

## Conclusion
Well, that was a lot, especially the explanation on tries! If the idea of a trie is still a bit fuzzy for you, that's fine - you can ask whatever questions you have below! And if you're really up for a challenge, try programming them!

{% include disqus.html %}
