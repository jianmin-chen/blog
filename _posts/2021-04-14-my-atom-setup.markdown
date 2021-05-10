---
categories: others editors
date: 2021-04-14 09:49:45 -0500
layout: post
tags: others editors
title: My Atom Setup
---
<img src="/blog/assets/img/atom-editor.png" alt="My Atom editor">

What is something every programmer relies on to be productive? Well, actually, there's a long list of what we need to be productive. But if we don't include the necessities(a computer, access to the Internet, and of course, Stack Overflow), I think we all need one thing: a nice text editor, designed for programming.

I mean, surely you don't use Notepad, right? Or a literal text editor, like Microsoft Word. There are a lot of open-source text editors out there specifically designed for programming, so today, I'll be blabbering about what works for me: Atom + a couple of extra packages for maximizing productivity + a nice theme. Let's get started!

## Wait, wait, wait... Why don't you use Visual Studio?
That's actually a really good question! Most programmers use either Visual Studio or Atom, and I actually tried out Visual Studio when I first started programming.

However, Visual Studio just didn't cut it for me. It would take extremely long periods of time to load, and when it did load, I would have to bypass this screen requiring me to sign up for a account. It also looked extremely complicated, but that might have been because I was new to programming in general at the time. I eventually just gave up on using it, and used the noob-friendly tool known as Notepad++.

After I gained more experience in programming, I realized Notepad++ wouldn't work for me either. It was too simplistic, and I personally thought it looked like a text editor you would find in the 2000s(no offense, Notepad++). I surfed the Internet for a while on what text editor I could use besides these two, and I settled on Atom.

## My Atom Setup
Okay, now that I've finished ranting about my journey of choosing the right text editor, I'd like to talk about my Atom setup. That's the whole point of this post, right?

First things first: font and font size. I use Fira Code whenever I program; it's specifically designed for programming, and will convert symbols you use into a special symbol is applicable(for example, >= gets converted into the actual greater than or equal to symbol by default in Fira Code). The font size I use for editing code in 20 pixels.

I'm also the type of programmer who likes having long lines of code wrap rather than having to scroll, so I have `soft wrap` enabled. Otherwise, I use most of Atom's default settings when editing code.

### What Theme I Use in Atom
Now that we've talked about my editor preferences in Atom, let's talk about what theme I use. Inside Atom, there are two parts to a theme:
* The UI theme, which styles the tabs, status bar, tree view, and dropdowns of the whole application
* The syntax theme, which styles the text inside the editor depending on the programming language

For the UI theme, I use **Atom Material**, and for the syntax theme, I use **Atom Material Dark**. Atom Material also comes with it's own settings. For example, the theme colors I use in Atom Material are red as the primary color and teal as the secondary color.

### What Packages I Use in Atom
As for packages I use in Atom, there are a couple of them. Packages typically add some sort of functionality to Atom. When you install Atom, it comes with a collection of *core packages* that are installed by default. You can, however, install *community packages*, which add extra functionality. These are the community packages I have installed:
1. `file-icons`: This package adds icons to the files I have open and the files that are being shown in my tree view. You can find this package on [GitHub](https://github.com/file-icons/atom).
2. `platformio-ide-terminal`: This package is so useful! It adds a terminal to Atom so you can open up multiple terminals directly within Atom itself. You can find this package on [Github](https://github.com/platformio/platformio-atom-ide-terminal), as well.
3. `language-lua`: This adds syntax highlighting support for Lua in Atom. Most files in Atom typically get automatically highlighted for you, but Lua isn't supported by default. The source for this package is [here](https://github.com/FireZenk/language-lua).

## Conclusion
And that's that for my Atom setup! Atom is super useful for me personally - I can run the terminal in it, I can directly push to my GitHub repositories without using the command line, and I can edit, rename, and move files directly in it! Overall, it's super useful, and it's open source as well.
