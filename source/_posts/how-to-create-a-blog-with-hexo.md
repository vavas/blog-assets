title: How To Create a Blog With Hexo
date: 2016-08-21 11:38:08
tags:
  - hexo
---

![Hexo logo](https://pbs.twimg.com/profile_images/476729162707644418/mQZOTo9f.png)

So you want to write about some cool stuff you are doing but don't want to waste time configuring a bunch of things? Well, a **really great** solution for this is to use the static site generator [Hexo](https://hexo.io/). It's **incredibly simple** how fast you can have a blog up and running with this great tool.

**Let's learn how!**

<!-- more -->

## Installing Hexo
**Before** install Hexo, you should have Node and NPM installed in your machine. In order to install Node, I recommend you to use [nvm](https://github.com/creationix/nvm) to manage your Node & NPM installation.

Now it's time to install Hexo. You just need to:

```sh
npm install hexo-cli -g
```

## Creating your blog
After install Hexo, let's create our blog with the Hexo CLI tool. To do this, type the following commands in your terminal:

```sh
hexo init <blog-name> && $_
```

Replace `<blog-name>` with the name of the folder where your files will be located.

> `$_` is a shortcut for `cd <blog-name>`

## Finding and Installing a New Theme
If you want to change the default theme, you just need to go [here](https://hexo.io/themes/) and find a new one you prefer.

I'm current using the [NexT](https://github.com/iissnan/hexo-theme-next) theme. I'll show you how to install this theme, but the procedure is almost equal for all themes.

In the root folder of your blog, type the following command:

```sh
git clone https://github.com/iissnan/hexo-theme-next themes/next
```

Now you need to tell Hexo which theme you want to use. To do this you need to open the `_config.yml` in your root directory and change the `theme` property value to `next`.

## Hexo Plugins
To generate all files and features that your blog will have (Archive section, RSS Feed, Browsersync, etc) you need to install some additional modules, or hexo plugins. I'm currently using the following modules:

**package.json** 
```js
"dependencies": {
	"hexo": "^3.1.0",
	"hexo-browsersync": "^0.2.0",
	"hexo-deployer-git": "0.0.4",
	"hexo-generator-archive": "^0.1.4",
	"hexo-generator-category": "^0.1.2",
	"hexo-generator-feed": "^1.2.0",
	"hexo-generator-index": "^0.1.2",
	"hexo-generator-sitemap": "^1.1.2",
	"hexo-generator-tag": "^0.1.1",
	"hexo-renderer-ejs": "^0.1.0",
	"hexo-renderer-marked": "^0.2.4",
	"hexo-renderer-stylus": "^0.3.0",
	"hexo-server": "^0.1.2"
}
```

When we install Hexo, we have by default the following plugins installed:

```js
"dependencies": {
	"hexo": "^3.2.0",
	"hexo-generator-archive": "^0.1.4",
	"hexo-generator-category": "^0.1.3",
	"hexo-generator-index": "^0.2.0",
	"hexo-generator-tag": "^0.2.0",
	"hexo-renderer-ejs": "^0.2.0",
	"hexo-renderer-stylus": "^0.3.1",
	"hexo-renderer-marked": "^0.2.10",
	"hexo-server": "^0.2.0"
}
```

So let's install the missing ones:

```sh
npm i -S hexo-{browsersync,deployer-git,generator-feed,generator-sitemap}
```

**Awesome!** You can test your brand new blog now typing these commands in your terminal:

```sh
hexo g && hexo s
```

> This is a shortcut for `hexo generate` and `hexo server`

## Configuring Your Blog
Now that you have your site up and running, you need to configure it with your personal information. We have two configurations files to change, `_config.yml` in the root of your project, and `_config.yml` located in the `themes/next` folder. Just open those files and put your personal information on them!

If you have some doubts about it, look my config files [here](https://github.com/ericdouglas/blog-assets/blob/theme/next/_config.yml) and [here](https://github.com/ericdouglas/hexo-theme-next/blob/master/_config.yml).

## Writing Your First Article
Now you are almost done with your blog setup. It is time to write your first article.

To generate a new article file, use the following command:

```sh
hexo new post "name of the post"
```

This file will be located at `source/_posts`. More information about *writing* can be found [here](https://hexo.io/docs/writing.html).

## Hosting Your Blog
You can host your blog for free in GitHub. You have two "hosting options" to do this:

### https://username.github.io
If you want your blog to be found in this type of address, for example: `https://ericdouglas.github.io`, you need to create a repository on GitHub with that exactly name: `<username>.github.io`. **In this type of repository, you should deploy your files to the `master` branch**.

### https://username.github.io/repository-name
If you want your blog to be found in this type of address, for example: `https://ericdouglas.github.io/blog`, you need to create a repository on GitHub with that exactly name: `blog`. **In this type of repository, you should deploy your files to the `gh-pages` branch**.

## Enabling HTTPS on GitHub
To enable HTTPS on your blog, go to the **Settings** tab in your repository, scroll down the page and click on **Enforce HTTPS**.

![Settings tab](http://i.imgur.com/6FZo5TY.png)

![Enforce HTTPS](http://i.imgur.com/YEQ1SKT.png)

## Adding a favicon to Your Blog
Just go to the [Realfavicongenerator.net](http://realfavicongenerator.net/) site and generate your favicon. After this, put the `favicon.ico` in the `source` folder.

## Generate and Deploy
**This is the final step**! Let's generate the static files and deploy those ones to GitHub. To do this use the following commands:

> **OBS**: The information about where Hexo should deploy your files should be stated in the `_config.yml` at the root of your project.

```sh
hexo d -g
```

> Shortcut for `hexo generate` and `hexo deploy`

**It's done!** Now you have a great blog fully set up, just write some cool stuff there and share with us :D

> If you found something wrong, you can contribute to this article [here](https://github.com/ericdouglas/blog-assets/blob/theme/next/source/_posts/how-to-create-a-blog-with-hexo.md).
