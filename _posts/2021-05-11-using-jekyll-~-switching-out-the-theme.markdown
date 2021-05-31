---
categories: jekyll blog
date: 2021-05-11 08:51:00
layout: post
tags: jekyll
title: Using Jekyll ~ Switching out the Theme
---
![Jekyll logo](https://repository-images.githubusercontent.com/65252/f2b7c780-70b6-11e9-85d2-f4bda8708a2d)

In the [previous](https://jianmin-chen.github.io/blog/jekyll/blog/2021/04/12/using-jekyll-~-creating-a-blog.html) article of this series, I introduced what Jekyll is and how to set up a basic blog with it. If you followed to the end of it, you would have gotten a simple, minimalistic blog set up.

However, it's a bit boring. What if you want to switch out the theme? And what if you wanted to deploy it online for the world to see? In this article, we're tackling those two things.

## Switching out the Theme

![TechDoc theme preview](https://raw.githubusercontent.com/jianmin-chen/jekyllthemes/master/thumbnails/techdoc.png)
<a class="img-link" href="https://github.com/jianmin-chen/techdoc">TechDoc theme</a>

The first thing we're going to do is switch out the theme. Now, obviously, you're going to have to know what theme you want to switch out the current one with. There are a bunch out there - no kidding. For the sake of this article, we'll use TechDoc, a Jekyll theme I created, as the example.

Before we go on, let me tell you that every theme has different layouts, or templates, for pages. As an example, the Minima theme you're currently using has four layouts, which you can find in the `_layouts` folder: `default.html`, `home.html`, `page.html`, and `post.html`. In addition, the default page of your Jekyll website uses the layout `home`, which references `home.html`. However, if the theme you're using doesn't have a `home` layout, then you're going to get a error(or at the very least, a warning). This is why it's important to take a look at the theme you're using and it's documentation before you start using it.

Having said all that, the TechDoc theme contains only two layouts: `default.html` and `post.html`. This means there won't be a `home` layout, so we need to do a couple of things before we can install the theme:
1. Replace the the the header in `index.markdown`, which is the default page of your Jekyll website. Currently, it's something like this:
~~~markdown
---
layout: home
---
~~~
But since TechDoc doesn't have the `home` layout, we can just switch it out for this instead:
~~~markdown
---
layout: post
---
~~~
2. Delete some unnecessary files. TechDoc is a theme designed for writing technical documentation, so the `about.markdown` file is unnecessary, unless you really want it. Delete it, and if you don't want Jekyll's default posts, delete those too.

The next step is to actually get the theme up and running. Jekyll is written in Ruby, which uses a file called `Gemfile` to keep track of the dependencies and install them using a package known as Bundler. This means we need to edit that file. Currently, there are going to be two lines in it that we need to edit. The first line is `gem "jekyll", "~> <version number, e.g 4.2.0>"`. This points to a specific version of Jekyll, which will cause an error when used with TechDoc or any theme in general. Strip the end off so that it's just `gem "jekyll"`.

The other line we need to remove is the line pointing to the Minima theme, which we won't be using anymore. The line you need to find is along the lines of `gem "minima", "~> 2.5"`. Removing it will effectively remove the Minima theme. Now, to get our theme on, we need to replace the line you just deleted with `gem "techdoc-jekyll-theme"`. This points to the TechDoc theme, specifically the gem/module for the theme that can be found on [rubygems.org](https://rubygems.org).

Before we install the theme, there's one last thing we need to do - edit the `_config.yml` file. As mentioned in the previous article, this file contains your configuration variables that get pasted onto your website when it's generated - for example, your name, or your social media links. TechDoc has these too. The list of variables that are recommended for TechDoc are:
~~~yaml
author: <your name>
baseurl: <the location of your blog on your website, e.g /blog>
github_username: <your GitHub profile, or any other social media profile link>
library_name: <the name of your library, or the title of your blog>
library_link: <link to the source code for your library>
library_description: <description of your library or your blog>
url: <your website's URL>
theme: techdoc-jekyll-theme
~~~
TechDoc was designed for writing technical documentation for programming, so these variables might not fit your goals. That's fine! After you read this article, you'll be able to switch the theme out with one of your own choosing. However, always remember that whatever theme you use, you must at least include a **`theme`** variable, as this points to the theme Jekyll will be using to generate your website.

Finally, we can install the theme! Fire up a terminal, navigate to the folder where your blog or website is stored at, and run `bundle install`. There shouldn't be any errors. If there are, check your dependencies and make sure they match the theme's dependencies.

## Write!
If there were no errors, everything should work perfectly! If you head over to the local version of your blog or website, you should see the TechDoc theme, rather than the Minima theme. Great job! Try writing some test posts(explained in the [previous](https://jianmin-chen.github.io/blog/jekyll/blog/2021/04/12/using-jekyll-~-creating-a-blog.html) article), and pat yourself on the back! You have one last step left, and that's making your website publicly available.

## Deploying to GitHub Pages
There are a couple of options for deploying your blog/website. One of the more easier(and free!) ones is using GitHub Pages, which supports Jekyll-created websites. It's simple, really. The first step is to add the following to your `_config.yml` file:
~~~yaml
plugins_dir:
    - jekyll-remote-theme
remote_theme: jianmin-chen/techdoc
~~~
These lines tell Jekyll to use the plugin `jekyll-remote-theme`. What this plugin does is enable GitHub Pages to search for the theme located at `remote_theme`(which should point to a GitHub repository), and use it in your blog or website. Make sure to remove `theme: techdoc-jekyll-theme` too because it can cause issues when GitHub attempts to determine the theme to use.

The next step is log into your GitHub account - you can create one if you haven't done so already. Now, you need to create a GitHub repository for your Jekyll blog or website. There are two options for naming it:
1. *Your website has a `baseurl`.* In this case, you can simply name it as the `baseurl`.
2. *Your website doesn't have a `baseurl`.* In this case, you'll have to name the repository you're creating `<your GitHub username>.github.io`.

Once you've got that nailed down, you need to upload your blog to the GitHub repository. To do this, there are a couple of commands you need to run in the command line. I'm assuming you already have **Git**, a command line tool for interacting with GitHub, but if you don't, you'll need to install [it](https://git-scm.com/downloads).

Now that you've got Git installed, you need to upload your blog to the repository you just created. First, open the command line, navigate to the directory of you blog, and get the repository on your computer:
~~~
git config --global user.name "<your GitHub username>"
git config --global user.email "<your GitHub email>"
git clone https://github.com/<username>/<repository name>.git
~~~
Then, push all your files to the repository:
~~~
git add .
git commit -m "<commit message>"
git push -u origin master
~~~
Almost there! The last step is to head over to your repository, and turn on GitHub Pages in the `Settings` tab using the branch `master`.

Assuming there are no errors, you should be good to go! Check out your blog, website, documentation, or whatever it is you just created. You've got your own little space on the Internet! :)

## Conclusion
In this article, you learned how to switch out the theme of your Jekyll website, and how to deploy a Jekyll website to GitHub Pages. Now, whenever you add a post or make a change to your website locally, all you need to do to publish it to GitHub is open the command line, navigate to the directory where your blog is stored at, and run the following commands:
~~~
git add .
git commit -m "<commit message>"
git push -u origin master
~~~
And that's it! If you have any questions, fire below. Thanks for reading!
