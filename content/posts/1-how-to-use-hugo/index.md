---
title: "Blogging with Hugo"
date: 2025-05-02T19:55:59-04:00
modified: 2025-05-02T22:55:59-04:00
draft: false
cover:
    image: https://gohugo.io/images/hugo-logo-wide.svg
    alt: "Hugo Logo"
    hidden: false
summary: "Quick notes about how to make a post within Hugo framework"
tags: ["Hugo", "Blog",]
---

Hugo isn't a new framework, but it's new to me. Here's the quickest way to make a post:

## Step by step
1. `hugo new content content/posts/{my-new-post}`
2. Write in the new .MD that was made at that path
    - Add Tags
    - Make sure the data is write
    - Draft to false when ready to publsih
    - Add a cover if possible
    - Add a summary
3. `hugo build`
4. `git add .`
5. `git push`

At this point the GitHub action should take the new content and push it to the site https://nathanemb.github.io/codetrails/.

## Theme
Currently we're using the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme. It's nice and gives a decent amount of customizability. I tried the profile mode but it felt like while it looked cool, overall it was not really useful, all this is supposed to be is a notepad for me, and maybe a way for someone to view my resume.

## Files
If putting images in blog post, make your blog post a folder, name the md `index.md` and then put the images in the folder. This colocation will make the blog content more easily transferrable if we ever want to switch to another framework or theme.

An alternative location is to use the static folder, and you can link normally from there as well. This won't transfer as well, but can be nice if you need to reference from `hugo.yaml`.

## hugo.yaml
This is where all the Hugo specific configuration goes, [this yaml](https://github.com/adityatelange/hugo-PaperMod/wiki/Installation#sample-hugoyml) shows all of the possible options for PaperMod. It's simultaneously overwhelming, and still lacks some customization options I wish that it had.