---
title: "How To Install Hugo, Add A Theme, & Deploy to GitLab Pages"
description: "I have had such a wonderful time using Hugo and I really do enjoy all of the things it allows me to do with ease. Even though there are plenty of fantastic guides for installing and configuring Hugo, I wanted to try my hand. Most guides or tutorials out there focus on deploying to Github or just don't cover the topic at all."
summary: "I have had such a wonderful time using Hugo and I really do enjoy all of the things it allows me to do with ease. Even though there are plenty of fantastic guides for installing and configuring Hugo, I wanted to try my hand. Most guides or tutorials out there focus on deploying to Github or just don't cover the topic at all."
categories: ["Software","Tools","Web","How To"]
tags: ["Hugo","Website","Tutorial","Development","Gitlab"]
#externalUrl: ""
date: 2023-11-27
draft: false
author:
  - El Khalidi
---

I have had such a wonderful time using Hugo and I really do enjoy all of the things it allows me to do with ease. Even though there are plenty of fantastic guides for installing and configuring <a target="_blank" href="https://gohugo.io/">Hugo</a>, I wanted to try my hand. Most guides or tutorials out there focus on deploying to Github or just don't cover the topic at all. 

I hope you find this informative, let's get started. 🏃

## Installing Hugo

I currently use Arch Linux so the commands and instructions below will be for that system. If you are using something else then you can <a target="_blank" href="https://gohugo.io/installation/">go here</a> and return to the section for creating your first project and continue from there.

### Step 1: Update System Packages

Ensure your system is up to date by running:

```bash
sudo pacman -Syu
```

### Step 2: Install Hugo

Installing Hugo on Arch, and many other systems out there, is as simple as running a single command in the terminal. Like so:

```bash
sudo pacman -S hugo git go
```

### Step 3: Verify Installation

Check if Hugo installed correctly:

```bash
hugo version
```

## Create A New Project

Let's go ahead and make a new Hugo website. We will create it and move into the directory, like so:

```bash
hugo new site mywebsite
cd mywebsite
```

## Adding A Hugo Theme

### Step 1: Choose a Theme

Welcome to the fun part, choosing the template for your website. You can find a theme you like on the <a target="_blank" href="https://themes.gohugo.io/">Hugo Themes</a> page.

### Step 2: Add the Theme to Your Project

Add the theme as a submodule:

```bash
git init
git submodule add <theme-git-url> themes/<theme-name>
```

### Step 3: Configure the Theme

Edit your site's configuration file (config.toml or config.yaml) to set the theme:

```toml
theme = "<theme-name>"
```

There will most likely be extra configuration needed depending on the theme you have selected. So please refer to the themes documentation before continuing.

## How To Organize Content

I am using a theme called <a target="_blank" href="https://themes.gohugo.io/themes/blowfish/">Blowfish</a> so I am going to use that as the example of how it recommends setting up your project. Even though you may need to setup things a little different for different themes most things should be the same.

Here is a general overview of how my folder structure looks for my project:

```md
.
├── assets
│   └── img
│       └── author.jpg
├── config
│   └── _default
├── content
│   ├── _index.md
│   ├── about.md
│   └── posts
│       ├── _index.md
│       ├── first-post.md
│       └── another-post
│           ├── aardvark.jpg
│           └── index.md
└── themes
    └── blowfish
```

Now would be a great time to go ahead and add some starter content for your website. So maybe take a pause on trying to get your site deployed and make sure you have something good to look at.

## Deploying to GitLab Pages

### Step 1: Create a GitLab Repository

Create a new blank repository for your Hugo site on GitLab.

### Step 2: Initialize Git and Add Remote

Inside your Hugo project directory:

```bash
git remote add origin <gitlab-repo-url>
```

### Step 3: Create CI YAML File

You now need to create the file to let Gitlab know what to do so your website gets built and works. It should look like this, or at least similar:

```yaml
#
# Before using this .gitlab-ci.yml:
#
# - This example uses the latest Docker image, but you might want to use the
#   exact version to avoid any broken pipelines.
#   All available Hugo versions are listed under https://gitlab.com/pages/hugo/container_registry.
# - Read about the difference between hugo and hugo_extended
#   https://gitlab.com/pages/hugo/-/blob/main/README.md#hugo-vs-hugo_extended.
# - To change the theme, see
#   https://gitlab.com/pages/hugo/-/blob/main/README.md#use-a-custom-theme.
#
image: registry.gitlab.com/pages/hugo/hugo:latest

variables:
  GIT_SUBMODULE_STRATEGY: recursive

test:
  script:
    - hugo
  except:
    - master

pages:
  script:
    - hugo
  artifacts:
    paths:
      - public
  only:
    - master

```

### Step 4: Commit and Push Changes

```bash
git add .
git commit -m "Initial commit"
git push -u origin master
```

### Step 5: Configure GitLab Pages

- Go to your GitLab repository.
- Navigate to Settings > General > Visibility.
- Make sure the option for Pages is set to _Anyone With Access._

### Step 6: Access Your Site

Once the <a target="_blank" href="https://docs.gitlab.com/ee/user/project/pages/">GitLab Pages</a> pipeline finishes building and deploying, access your site at:

```
https://<username>.gitlab.io/<repository-name>
```

## Congratulations! You're Done!

All that's left for you to do is link your domain to the repo, which the options for can be found under Deploy > Pages. Now the instructions on this vary wildy depending on where you have your domains through. So if you need help there just search _"how to setup dns for gitlab pages for (insert domain provider)"_ on <a target="_blank" href="https://google.com/">Google.</a>

You've successfully installed Hugo, added a theme, hopefully added some quality content, and deployed your site to GitLab Pages.

Hope this has helped you. Thank you for taking the time to read! 👍
