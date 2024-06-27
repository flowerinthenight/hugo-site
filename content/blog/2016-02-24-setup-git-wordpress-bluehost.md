---
title: "How I setup `git` for my WordPress installation in BlueHost"
description: "2016-02-24"
categories: ["Tech"]
categories: ["Tech"]
date: "2016-02-24"
paige:
  feed:
    hide_page: true
tags: ["git", "wordpress", "bluehost"]
weight: 1
---

I'm not sure if this is the "proper" way to do it. Well, it sort of works for me at the moment so I thought I'll share it here. I did use FTP at first (using FileZilla) but I didnt really like the workflow.

* Host: BlueHost shared account
* Client: Windows 10

## Prerequisites

* SSH/Shell Access should be enabled. I enabled this from my cPanel -> SSH/Shell Access menu.

## Server/Host side

* SSH to BlueHost host. By default, my website was installed inside `~/public_html` folder. I created a new folder named `~/www-checkout`. This will be my new "live" website.
* I copied everything from `~/public_html` to `~/www-checkout`.

```sh
$ cp -rv ~/public_html/ ~/www-checkout/
```

* I renamed my `~/public_html` to `~/public_html_original`. Just for backup.

```sh
$ mv ~/public_html ~/public_html_original
```

* Then I created a symbolic link named `~/public_html` that points to `~/www-checkout`.

```sh
$ ln -s ~/www-checkout ~/public_html
```

* Then I created my main git repository folder named `~/www.git`.

```sh
$ mkdir www.git
$ cd www.git
$ git --bare init
```

* Then I added a `post-receive` script inside hooks folder that will do a checkout to `~/www-checkout` every time the repository is updated.

```sh
$ cd hooks
$ touch post-receive
$ chmod 755 post-receive
```

### Contents of post-receive (edited using vim)

```sh
#!/bin/sh
GIT_WORK_TREE=~/www-checkout git checkout -f
```

* Then I created a new "work" folder named `~/www-work` for my initial commit, cloned the still empty git repository, then copied the contents of `~/www-checkout`.

```sh
$ cd && mkdir www-work
$ cd www-work
$ git clone ~/www.git
```

* Before the commit, I deleted the files that I thought should not be included in the source.
  * wp-content/plugins
  * wp-content/upgrade
  * wp-content/uploads
* (Optional) Edit source files.
* Commit source files.

```sh
$ git add --all
$ git commit -a -m "Initial commit."
$ git push -u origin master
```

Thats it. Now to the client side.

## Client side

* I installed Git for Windows from here.
* The I created a working folder, right-click -> Git Bash Here menu (I checked the context menu during installation, which by default, was already checked anyway).
* Clone the repository.

```sh
$ git clone <username>@<domain>:~/www.git
```

I now have a working source copy in my Windows client machine!

## Disadvantages?

One thing I can think of is that every edit goes directly to live folder. So I dont have a sort of "staging" server for me to test before putting changes to live. Both a blessing and a curse, I think. But then again, this is just a simple blog, using a free theme, so I dont think I will be doing any massive changes to the source code anyway.

<br>
