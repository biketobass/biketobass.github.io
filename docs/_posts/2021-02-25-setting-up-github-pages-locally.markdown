---
layout: post
title: "Setting up Github Pages Locally"
date: 2021-02-25 15:15:00 -0400
categories: computer software-installation
tags: jekyll github repository
---

For my inaugural post, I'm recording the steps I took to set up this site using GitHub Pages from initial installation to first commit of a blog post. My goal was to end up with a site that I could build and test locally before having it go live on GitHub.

To start, I followed the steps in the GitHub documentation for [creating a GitHub pages site](https://docs.github.com/en/github/working-with-github-pages/creating-a-github-pages-site-with-jekyll), moving through the prerequisites and creating a repository for the site. In the *Creating your site* section, I followed the instructions but wanted to make a few notes:

* In step 3, when I ran `git init REPOSITORY-NAME`, git created an initial branch with the name 'master.' I wanted the branch to be named 'main.' Instead of changing just that branch's name, I deleted the new repository and first ran `git config --global init.defaultBranch main` which changes git's global settings so that the initial branch is always called main. You can set the name to anything you want.

* In step 6, I created a docs folder and cd'd into it and proceeded to move through steps 7, 8, and 9.

* In step 10, I initially added the wrong GITHUB-PAGES-VERSION as specified [in GitHub's list of Dependency Versions](https://pages.github.com/versions/). I inadvertently used the Jekyll version rather than the github-pages version.

* After I ran `bundle update` in step 12, I changed the Markdown processor to GFM in the _config.yml file by adding the line `markdown: GFM` after the `url:""` line.

* In step 13, I finally got to the point of being able to test my site locally which was the primary reason I was going through this process to begin with. The [instructions here](https://docs.github.com/en/articles/testing-your-github-pages-site-locally-with-jekyll) were straightforward, but the first time I ran `bundle exec jekyll serve`, the command failed due to a webrick load error. I fixed it by running `bundle add webrick` in my docs folder. The issue is discussed [in this forum](https://github.com/jekyll/jekyll/issues/8523).

* To create this blog post, I created a new post file in the _posts directory as explained [later in the documentation](https://docs.github.com/en/github/working-with-github-pages/adding-content-to-your-github-pages-site-using-jekyll). **Note that if you specify a date in the YAML front matter that is in the future, it will not display on your site until time catches up to your post.**

* I ran the command in step 14 from the top level directory of my repository.

* Step 15 gave me some trouble. When I ran `git push -u origin BRANCH`, I received the error: `error: failed to push some refs to` my repository. After some digging I realized that it's because when I originally created the repository on GitHub, I made some changes and committed them on GitHub. Those changes never got integrated with the new repository I created locally which caused git to throw the error.

* To fix the problem, I ran `git pull --rebase origin main`, but that only solved part of the problem. When I first ran `git push -u origin main`, I received the message, "Everything up-to-date."

* To fix this I had to use `git add`, `git commit`, then `git push` as follows from my top level repository directory:
  - `git add docs`
  - `git commit -m "commit message"`
  - `git push -u origin main`

* In step 16 I chose the docs folder as my publishing source and the site worked!
