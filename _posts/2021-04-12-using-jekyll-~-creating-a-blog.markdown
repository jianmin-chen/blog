---
categories: jekyll blog
date: 2021-04-12 06:23:45 -0500
layout: post
tags: jekyll
title: Using Jekyll ~ Creating a Blog
---
<img src="https://repository-images.githubusercontent.com/65252/f2b7c780-70b6-11e9-85d2-f4bda8708a2d" alt="Jekyll logo">

Recently, I've been playing around with Jekyll, a static site generator. I've even gotten around to creating my own Jekyll [theme](https://github.com/jianmin-chen/techdoc), so I thought it would be interesting to talk about using Jekyll with GitHub Pages in a series of articles dedicated to the topic.

This is the first article in that series, and in this article, we'll talk about what Jekyll is and how to set up a basic blog with Jekyll. In the next article, we'll walk through how to switch out the theme of our blog, as well as deploying it to GitHub Pages. Let's get started!

## What is Jekyll?
Great question! Jekyll is a static site generator. To put it simply, Jekyll takes text files written in a format known as Markdown, and parses them into HTML files whenever a file on your site is accessed. Jekyll sites use HTML and CSS like every other website ever, but it's the parsing that makes it special and easy to use, especially for those who blog.

As an example, this blog uses Jekyll(and a GitHub Page theme known as [`minimal`](https://github.com/pages-themes/minimal) for those who are interested). Whenever I write a post, I write it in Markdown. For example, the raw text for the heading and paragraph above looks like this:
~~~markdown

## What is Jekyll?
Great question! Jekyll is a static site generator. To put it simply, Jekyll takes text files written in a format known as Markdown, and parses them into HTML files whenever a file on your site is accessed. Jekyll sites use HTML and CSS like every other website ever, but it's the parsing that makes it special and easy to use, especially for those who blog.
~~~
As you can see, the `<h2>`(header two) heading is represented by two hashes. When Jekyll parses this short snippet of Markdown, it'll convert the line into a header, i.e `<h2>What is Jekyll?</h2>`.

This is great for us, because all we need to do to write a post is to write some content in Markdown, place it inside a certain folder, and Jekyll will do the rest for us! Easy peasy, just like that.

## Setting Up a Blog
Hopefully, by now you understand what Jekyll is. Let's get into the fun part now - creating a basic blog! To do so, you'll need to install Ruby, which Jekyll relies on, and of course, Jekyll itself. This [page](https://jekyllrb.com/docs/installation/) contains detailed instructions for doing so.

Once you have Ruby and Jekyll installed, you can open up a command line and type in `jekyll new <path>`, where `<path>` represents the directory you want your blog to be stored in. This has the effect of setting up the files needed for your blog.

Now head over to that directory and take a look! You'll see a bunch of folders and files, organized somewhat like the following:
~~~
_posts
_config.yml
.gitignore
404.html
about.markdown
Gemfile
Gemfile.lock
index.markdown
~~~

## Viewing the Blog
<img src="https://raw.githubusercontent.com/jekyll/minima/master/screenshot.png" alt="Minima theme">
<a href="https://github.com/jekyll/minima" style="display: block; margin-top: 25px; text-align: center; width: 100%;">The default layout of your blog</a>

Okay, cool! There must be something we can do with all those files, like... viewing them. That's what this part is about! Whenever you'd like to preview your files, simply open up a command line, navigate to the directory your blog is in, and run:
~~~
bundle exec jekyll serve
~~~
This command will provide you with a link - something like *http://127.0.0.1:4000/blog/*. Visit that in your browser, and you should see something that looks like the above! If you do, pat yourself on the back - you've just created a basic Jekyll blog!

## Configuring Your Blog
I'm sure you had fun scrolling through the blog. It's kind of boring, though. For one, the title currently is "Your awesome title". You're probably wondering how to switch that out. It's easy! All the information, or configuration variables if you will, is stored in a file called `_config.yml`. By default, the file looks something like this:
~~~yml
title: Your awesome title
email: your-email@example.com
description: >-
  Write an awesome description for your new site here. You can edit this
  line in _config.yml. It will appear in your document head meta (for
  Google search results) and in your feed.xml site description.
baseurl: ""
url: ""
twitter_username: jekyllrb
github_username:  jekyll
theme: techdoc-jekyll-theme
plugins:
  - jekyll-feed
~~~
Change these variables to fit your needs, and experiment with them. Every time you change this file though, make sure to rerun `bundle exec jekyll serve`! Jekyll typically auto regenerates your content locally whenever you edit something, but the only exception is `_config.yml`.

Also, a extra note - you might already understand most of these variables, but here's what `baseurl` and `url` are for those interested.
* `url`: The starting URL of your blog. For example, since the URL of my blog is *https://jianmin-chen.github.io/blog*, the starting URL is *https://jianmin-chen.github.io*.
* `baseurl`: The ending path of your blog. Using the same example, the ending path of my blog would be */blog*.

## Writing Posts
Now we can get to the fun part - writing! Take a look at your directory again. There should be a folder called `_posts`. If you open that, you'll see some files - these are the default posts. Get rid of these, and then create a new file in that folder. Name this file `<year>-<month>-<day>-<title-split-by-dashes>.markdown`, and add the following header to it:
~~~
---
date: year-month-day hour:minute:second
title: <post title>
layout: post
categories: what categories this post belongs in
---
~~~
These are pretty self-explanatory. `date` represents the date the post was written, `title` represents the title of the post, `layout` represents the template that the post will use(e.g the home page uses a different template than a post because of it's content), and `categories` represents the categories the post falls into, separated by space.

This header, or a variation of it, should always be added to the beginning of your posts, because Jekyll uses it to display information about your post and determine a link for your post.

After you've got the header down, you can start writing away! Jekyll will automatically regenerate the blog for you when you save the file, so you can always reload your blog and preview it.

To write more posts, simply follow the steps above! If you're new to Markdown though, I would suggest looking at this quick reference [guide](https://kramdown.gettalong.org/quickref.html) to get a broad idea of writing in Markdown.

## Conclusion
Whew, that was quick! We talked about a lot in this post - what Jekyll is and how it works, as well as setting up a blog of your own. In the next article, we'll discuss changing the theme out for something more exciting, and deploy our blog to GitHub Pages! In the meantime, if you have any questions, feel free to ask below.
