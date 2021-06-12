---
categories: java interpreter
date: 2021-06-12 16:25:00 -0500
layout: post
tags: java interpreter
title: Writing a Brainf*ck Interpreter
---
![Brainf*ck example](https://upload.wikimedia.org/wikipedia/commons/b/b4/Hello_World_Brainfuck.png)

Have you ever heard of an esoteric programming language? If not, they're basically programming languages that have no real usage and are designed to be fun. Well, mostly obnoxious, really.

[Brainf*ck](https://en.wikipedia.org/wiki/Brainfuck) is one of the more well-known esoteric languages. A "Hello, world!" program in it might look something like the following:
```bf
>+++++++++[<++++++++>-]<.>+++++++[<++++>-]<+.+++++++..+++.[-]>++++++++[<++++>-] <.>+++++++++++[<++++++++>-]<-.--------.+++.------.--------.[-]>++++++++[<++++>- ]<+.[-]++++++++++.
```
It makes no sense, right? Well, us programmers love to fiddle with things that make no sense. And in this article, I'll be explaining how Brainf*ck works and how to write a basic interpreter for it in Java. Let's get started!

## How Brainf*ck Works
The first step in writing our interpreter is to understand how Brainf*ck works. The programming language operates in a series of cells, all of which are initially set to zero. Typically, the number of cells is set to 30,000, and the cells can wrap around.

Furthermore, it only contains eight commands - `>`, `<`, `+`, `-`, `.`, `,`, `[`, and `]`. Each of these commands have a different purpose:
1. `>`: Move to the next cell.
2. `<`: Move to the previous cell.
3. `+`: Increment the value of the current cell.
4. `-`: Decrement the value of the current cell.
5. `.`: Print the ASCII value of the current cell, if the cell's value can be converted.
6. `,`: Set the value of the current cell to the first character in given input.
7. `[`: Start a while loop, which keeps running while the current cell's value isn't zero.
8. `]`: Close a while loop, if the current cell's value is zero.

Believe it or not, these commands can be used together to create a full, working program! Therefore, Brainf*ck is Turing-complete, meaning that with enough time, you could write any program in it as you would in a more "normal" programming language like Java. For now, let's go back to the "Hello, world!" program from before:
```bf
>+++++++++[<++++++++>-]<.>+++++++[<++++>-]<+.+++++++..+++.[-]>++++++++[<++++>-] <.>+++++++++++[<++++++++>-]<-.--------.+++.------.--------.[-]>++++++++[<++++>- ]<+.[-]++++++++++.
```
I'll explain the first part of it - `>+++++++++[<++++++++>-]<.` so that you'll have a general idea of how Brainf*ck works. Let's say we have a group/array of 30,000 cells.

Clearly, the first character is `>`, so we need to move to the next cell, at index one.
```
[0][0][0][0][0]
    ^
```
The next nine characters are all `+`, so we need to increment the current cell by nine:
```
[0][9][0][0][0]
    ^
```
The tenth character is `[`, so we can start a while loop. Inside this while loop, we move back to the previous cell:
```
[0][9][0][0][0]
 ^    
```
The next eight characters are all `+`, so we need to increment the current cell by eight:
```
[8][9][0][0][0]
 ^  
```
Then, we have the character `>`, so we need to move back to the second cell at index zero again.
```
[8][9][0][0][0]
    ^
```
The next character is `-`, so we decrement the current cell by one:
```
[8][8][0][0][0]
    ^
```
Now, we have `]`. This should close the while loop, but because the value of the current cell isn't equal to zero yet, we have to run the while loop again. So we move to the previous cell again, increment it eight times, move back to the original cell, and subtract one from its value. Eventually, the cells will look like the following after eight more iterations:
```
[72][0][0][0][0]
     ^
```
This concludes the while loop. We're still on the second cell at index zero, so the program tells us to move to the previous cell - `<`.
```
[72][0][0][0][0]
 ^
```
Finally, the last character in our short snippet is `.`, meaning we need to print the ASCII value of the current cell. If you couldn't tell already, 72 = H. So `>+++++++++[<++++++++>-]<.` prints "H" to the console, the first character in "Hello, world!" Cool!

This is how Brainf*ck works in a nutshell. Feel free to try out the other character portions, and move on once you understand!

## Writing the Interpreter
Now we can get to the real work - writing the interpreter. This post will explain how to write one in Java, but you can basically follow along in any language.

### Logistics
To start, let's consider how the interpreter will work. Basically, it'll take in a command line argument representing the filename of a Brainf*ck program - for example, "file.bf". The program will read the contents of this file and store it in a string, which will be passed to a function that will parse the string character by character. Most of the characters will be generally easy to implement, with the exception of `[` and `]`. How will those work? It's easy to understand when you see Wikipedia's explanation:
> `[`: if the byte at the data pointer is zero, then instead of moving the instruction pointer forward to the next command, jump it forward to the command after the matching ] command.
>
> `]`: if the byte at the data pointer is nonzero, then instead of moving the instruction pointer forward to the next command, jump it back to the command after the matching [ command.

In other words, simply jump to the command after the matching while loop if a condition is met for both of these characters. For example, if the current character is "]" but the value of the current cell is nonzero, we just jump back to the command after the matching "[" to rerun the while loop over again. Easy!

Great, we've gotten the logistics of this program down. Now we can lay out the structure of our interpreter:
```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.*;

class Interpreter {
    private static Scanner input = new Scanner(System.in);
    private static int CELLS = 30000;
    private static int pointer = 0;
    private static byte memory[] = new byte[CELLS];

    private static void parse(String program) {
        // Parse program
    }

    public static void main(String args[]) {
        // Main code
    }
}
```
Let's start by reading the command line arguments and determining if a filename is provided:
```java
public static void main(String args[]) {
    if (args.length != 1) {
        System.out.println("Usage: java Interpreter [ filename ]");
        return;
    }
}
```
If a filename is provided, we can try reading it:
```java
try {
    // Attempt to read file
    File fileObj = new File(args[0]);
    Scanner reader = new Scanner(fileObj);
    String contents = "";
    while (reader.hasNextLine()) {
        contents = contents.concat(reader.nextLine());
    }
    reader.close();
    parse(contents);
    input.close();
} catch (FileNotFoundException e) {
    System.out.println("An error occurred while reading file.");
    e.printStackTrace();
}
```
As you can see, we attempt to read the file provided. If we're able to read it, we store its contents in a string aptly called `contents` and pass them to our `parse()` function/method, which we can get started on now:
```java
private static void parse(String program) {
    // Parse program
    int brackets = 0;
    for (int i = 0; i < program.length(); i++) {
    }
}
```
The first thing we do is set up a variable called `brackets`, which will be useful once we start jumping back and forth between `[` and `]`. For now, let's parse the other characters:
```java
if (program.charAt(i) == '>') {
    // Move pointer to the next cell
    if (pointer == CELLS - 1) {
        pointer = 0;
    } else {
        pointer++;
    }
} else if (program.charAt(i) == '<') {
    // Move pointer to the previous cell
    if (pointer == 0) {
        pointer = CELLS - 1;
    } else {
        pointer--;
    }
} else if (program.charAt(i) == '+') {
    // Add one to the current cell's value
    memory[pointer]++;
} else if (program.charAt(i) == '-') {
    // Subtract one from the current cell's value
    memory[pointer]--;
} else if (program.charAt(i) == '.') {
    // Output the ASCII value of the current cell's value
    System.out.print((char)(memory[pointer]));
} else if (program.charAt(i) == ',') {
    // Set the current cell's value to input value
    memory[pointer] = (byte)(input.next().charAt(0));
}
```
Finally, we can get to the hardest characters:
```java
else if (program.charAt(i) == '[' && memory[pointer] == 0) {
    // Skip to end of while loop
    i++;
    while (brackets > 0 || program.charAt(i) != ']') {
        if (program.charAt(i) == '[') {
            brackets++;
        } else if (program.charAt(i) == ']') {
            brackets--;
        }
        i++;
    }
} else if (program.charAt(i) == ']' && memory[pointer] != 0) {
    // Go back to beginning of while loop
    i--;
    while (brackets > 0 || program.charAt(i) != '[') {
        if (program.charAt(i) == ']') {
            brackets++;
        } else if (program.charAt(i) == '[') {
            brackets--;
        }
        i--;
    }
}
```
Let's tackle skipping to the end of a while loop first. If the current command is `[` and the value of the current cell is zero, it means we need to skip to the matching `]`. To do so, we first move to the next character, and run a while loop that skips to the matching `]` by checking to make sure that the number of opening brackets is equal to the number of closing brackets, and that the character is `]`.

As for going back to the beginning of a while loop, we need to move back one command, and run a while loop that goes back to the matching `[` by checking to make that the number of closing brackets is equal to the number of opening brackets while also making sure the character is `]`.

That's it for the program! Create another file for the Brainf*ck program you want to run, compile this program, and run it!

There are a bunch of cool programs you can try out [here](https://github.com/fabianishere/brainfuck/tree/master/examples). There are even Brainf*ck programs provided in it that print out the Mandelbrot Set(a fractal) to the console! Super, super cool.

## Conclusion
This article was definitely enjoyable to write. If you'd like to see the code in one place, head [here](https://github.com/jianmin-chen/blog-programs/tree/main/Writing%20a%20Brainf*ck%20Interpreter). If you have questions, ask below. Otherwise, have a good day!
