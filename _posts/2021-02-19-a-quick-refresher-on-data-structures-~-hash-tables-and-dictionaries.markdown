---
layout: post
title: A Quick Refresher on Data Structures ~ Hash Tables and Dictionaries
date: 2021-02-19 07:57:45 -0500
categories: data structures
tags: data structures
comments: true
---
<img src="https://images.idgesg.net/images/article/2020/03/jw_pt3_data_structure_algorithms_java_coding_programmer_2400x1600_davidgoh_akindo_gettyimages_531237630_473456596-100834801-large.jpg" alt="Computer image" style="display: block; margin: 0 auto;">

Another article on data structures! In the last [one]({% post_url 2021-02-01-a-quick-refresher-on-data-structures-~-linked-lists,-queues,-and-stacks %}), we talked about linked lists, stacks, and queues. In this one, we'll build off that article and talk about hash tables and it's child, dictionaries.

This article provides explanations and code for both data structures in JavaScript, so feel free to follow along!

## What is a hash table?

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/7d/Hash_table_3_1_1_0_1_0_0_SP.svg/1200px-Hash_table_3_1_1_0_1_0_0_SP.svg.png" alt="Hash table(Wikipedia)" style="display: block; margin: 0 auto;">
<a href="https://en.wikipedia.org/wiki/Hash_table" style="display: block; margin-top: 25px; text-align: center; width: 100%;">Pictorial depiction of a hash table(Wikipedia)</a>

Good question! A hash table is basically a array of linked lists. That doesn't sound so special at first, but what makes a hash table special is that it allows you to add and get values from it typically in **O(1)** time. How does that work? Well, when you want to add a value to a hash table, the hash table uses a function called `hash()` to get the respective position of that value in the array. Basically, it converts that value to a number somehow that represents the position of that value in the hash table.

An example might be helpful here - we'll also use this example later on in our implementation of a hash table. Let's say that we want to create a hash table containing peoples' names. How might we go about implementing such a table? Well, let's start with a array containing twenty-six linked lists(remember, a hash table is just a array of linked lists). If we want to add "Amanda" to this hash table, we're going to need to *hash* it. To do so, we can:
1. take the first letter, which is "A"
2. make that first letter lowercase, so that "A" is now "a"
3. get the ASCII value of that lowercase letter, which is 97
4. subtract 97 from that value, which results in the final value of 0

So "Amanda" belongs in the first linked list of the array. If you added "Brian", it would be second, and so on. Do you see the pattern? By *hashing* these values, we're putting them alphabetically in our hash table.

Continuing on from this example, what if we tried to add "Andy"? Well, we would get the position of "Andy" in our hash table, which is index 0. But index 0 already contains "Amanda"! Fear not. We can simply add "Andy" to the beginning of the linked list that is at index 0, thus proving that adding to a hash table is always **O(1)**, a constant number of steps, if you implement it correctly.

However, this brings up a issue with getting "Amanda". What if we added more names that started with the letter "A"? In that case, we would have to traverse all those names just to get to "Amanda". Previously, I had told you that getting a value from a hash table was **O(1)**. Well, that's the case occasionally - if the item you're trying to find is the only one in a linked list. Most of the time, it's actually **O(n)**.

The only way to solve this problem is to make a sacrifice. If we give up more memory, we can improve the running time of getting to "Amanda". What if we alter our hashing function so we check the first two letters of each provided name? This means that for each letter, we'll have to use 26 linked lists, each one pertaining to a certain set of two letters(e.g "aa", "ab", etc.), which results in a total of 676 linked lists. Insane! But because of this, "Amanda" and "Andy" can each go in their own linked list, improving the running time of getting them.

Now let's actually go about implementing such a hash table:
~~~javascript
class HashTable {
    constructor() {
        this.table = [];
        for (let i = 0; i < 26; i++) {
            this.table.push(new LinkedList());  // add to this.table a new linked list, which will represent a letter in the alphabet
        }
    }

    print() {
        for (let list of this.table) {
            if (list.len == 0) console.log("No nodes in current bucket\n");
            else list.print();
        }
    }

    hash(chr) {
        return chr.charCodeAt() - 97;  // return ASCII value minus 97 to get actual position in this.table
    }

    add(val) {
        if (val.length == 0) throw Error("Attempt to add a empty value to hash table");
        let pos = this.hash(val[0].toLowerCase());  // val has at least a length of one, so we can just pass the lowercase version of the first letter in val to hash()
        this.table[pos].addNode(val, 0);  // adding val to the beginning of the current linked list is actually quicker than adding it to the end, hence the 0
    }
}
~~~
Note that this code is assuming you already have a `LinkedList()` class somewhere. If you don't, the previous [article](https://jianmin-chen.github.io/blog/data/structures/2021/02/01/a-quick-refresher-on-data-structures-~-linked-lists,-queues,-and-stacks.html) provides the code! You can also find a implementation of a linked list [here](https://github.com/jianmin-chen/blog-programs/blob/main/A%20Quick%20Refresher%20on%20Data%20Structures/part-one.js).

## What is a dictionary?

<img src="https://developers.google.com/edu/python/images/dict.png" alt="Dictionary(Google)" style="display: block; margin: 0 auto;">
<a href="https://developers.google.com/edu/python/dict-files" style="display: block; margin-top: 25px; text-align: center; width: 100%;">Pictorial depiction of a dictionary(Google)</a>

Dictionaries are data structures based off of hash tables. They're extremely similar to dictionaries in the real world in that they can store a word, typically called a *key* in computer science, and a definition, typically called a *value*. As you might already see, they're pretty useful - you can actually find them in most high level programming languages!

In fact, JavaScript already has them, and they come with a bunch of features, as they were designed to be objects(object-oriented programming might be something I discuss in another article); it's actually conventional to call them objects, so that's what I'll do for the rest of this article. There are two straightforward ways of creating them:
~~~javascript
let dict = {};
dict = Object.create(null);  // dict is still the same
~~~
To add a key and a value to a JavaScript object, you can once again use two different options:
~~~javascript
dict["a"] = ["Andy", "Amanda"];
dict.b = ["Brian"];
~~~
As mentioned before, JavaScript objects come with many features. Although there are too much to discuss thoroughly in one article, here's an example of what JavaScript offers that loops through the keys and values of an object at the same time:
~~~javascript
for (let [key, value] of Object.entries(dict)) {
    console.log(`Key: ${key}, value: ${value}`);
}
~~~
If you want a more in-depth article on what JavaScript objects have to offer, feel free to ask in the comments below!

## Conclusion
That's the end of this article! Play around with the hash table, and try to devise a hashing function of your own. If you're confused on anything, ask about it below! The code for the hash table discussed can be found [here](https://github.com/jianmin-chen/blog-programs/blob/main/A%20Quick%20Refresher%20on%20Data%20Structures/part-two.js).

{% include disqus.html %}
