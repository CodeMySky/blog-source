---
title: 'How to Create a Blog Using Hexo'
date: 2016-01-17 15:11:08
tags: [Hexo]
---
[Hexo](https://hexo.io "Hexo") is a fast, simple & powerful blog framework built on Node.js. By using Hexo, I avoided a lot of frustration building a blog while concentrate myself on blog.

<!-- more -->
{% blockquote %}
Warning: Please go to [Hexo](https://hexo.io "Hexo") to get the most updated guild.
{% endblockquote %}
# Get Everything Ready
## Install Node.js
[Node.js](https://nodejs.org "Node.js") is a JavaScript runtime. Hexo is built on Node.js. Node.js can also be used with [Express](http://expressjs.com "Express.js") to create more sophisticated websites.

Please download and install Node.js.

## Install Git
[Git](http://git-scm.com/download/ "Git") is used to publish the blog. Git is also a very well-known code version control tool. By using Hexo, you don't need a lot of knowledge of git.

Please download and install Git.

## Install Hexo


Install Hexo using npm (Node Package Manager).

```bash
$ sudo npm install hexo-cli -g
```

# First Hexo Blog

Now I'm going to create my first blog.

```bash
$ hexo init blog
$ cd blog
$ npm install
```

## File Structure
There are several important files and folders in the `blog` folder,

### \_config.yml
Blog settings can be changed here.

### scaffolds
It contains the templates of posts, normally there is no to change it.

### source
It contains all original posts

### themes
It contains all themes installed, different themes can be switched. [Hexo Themes](https://hexo.io/themes/) provides plenty of themes to choose.

### public
It does not exist at the beginning, but it will appear after your first deployment. It is all auto generated, all its changes will be overwritten in the next deployment.

## Setup Pages
### Github Pages
[Github Pages](https://pages.github.com) provides free host for the blog. I already have an account on Github, then I created a repository naming `<username>.github.io`. In this way Github can recognize its your personal websites.

### GitCafe Pages
[GitCafe Pages](https://gitcafe.com/GitCafe/Help/wiki/Pages-相关帮助) also provides free host for the blog, but it is faster to access in China. I created a repository name `<username>`. In this way GitCafe can recognize its your personal websites

### Binding Custom Domain
If you have a personal domain, it is very easy to direct the domain to the blog by setting a `CNAME` pointing to `<username>.github.io`.

In China, [DNSPod](https://www.dnspod.cn) provides free DNS service and can direct the domain to either GitCafe or Github according to visitor's IP, which can provide faster access to all visitors.

### Change Setting in \_config.yml
Deploy segment is in the end on `_config.yml`.

```bash
deploy:
- type: git
  repo: git@github.com:<username>/<username>.github.io.git
- type: git
  repo: git@gitcafe.com:<username>/<username>.git
```

Hexo also supports deployment to [Heroku](https://heroku.com) and other places. Detailed instructions can be found [here](https://hexo.io/docs/deployment.html)

## Basic Hexo Commands
### new

```bash
$ hexo new [layout] "<title>"
```

Create a new article. `layout` maybe `post`, `draft`, if no `layout` is provided, Hexo will use the `default_layout` defined in `_config.xml`

### generate
```bash
$ hexo generate
```

Generate static files.

### server
```bash
$ hexo server
```

Starts a local server, to serve the blog

### deploy
```bash
$ hexo deploy
```

Deploy the blog.

### clean
```bash
$ hexo clean
```

Clean up cache file to avoid weird problems.
