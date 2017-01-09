# ssp-1

A #1 sharing session with awesome Playbasis Team at Playbasis Office.
Topics revolve around **automation**. This repository contains related document files.

Agenda is as follows.

0. Git general commands
1. Gitflow via commandline
2. Jekyll example (Webhook on Github, build commands, automation)
3. Automate iOS build workflow with Fastlane (Android also applies, see the link below).
4. Testing with Jasmine on native-sdk-js
5. UI Testing on iOS

# Introduction

This repository contains another 2 repositories working together for you to access each example project.

1. [ssp1-jekyll](https://github.com/haxpor/ssp1-jekyll)
2. [ssp1-fastlane](https://github.com/haxpor/ssp1-fastlane)

# Narrative Explanation

## Git general commands

Use `git` command such as

* `git status` - check current status of git repository
* `git clone <url>` - clone remote git into current directory
* `git branch` - list all local branches
* `git branch -v` - list all remote branches
* `git branch <branch-name>` - branch off from current branch into <branch-name>
* `git checkout <branch-name>` - move to <branch-name>
* `git add -A` - add (stage) all files
* `git commit` - commit (will enter into vim editing for commit message with free-feature of character limiting for title)
* `git push origin <branch-name>` - push the commit to target <branch-name>
* `git diff` - show the differences (changes) of current commit working point to previous commit

## Git aliased commands

We can add aliased command at global level for ease of use.
We can add a new aliased command by adding it into `~/.gitconfig` file in `[alias]` section.

* `git tree`

   ```
   [alias]
       tree = log --graph --decorate --abbrev-commit --pretty=format:'%C(yellow)%h %C(white)%>(12)%ad %Cgreen%<(7)%aN%Cred%d %Creset%s' --branches --date=relative
   ```

   **Usage**
   `git tree`

## Jekyll example

Follow the following step

We will set up some sort of basic automatic system for automatic build on our server.
Developers or client can make changes of the Jekyll site on development system, see via `jekyll serve` whether everything is alright before making it live, then commit & push the code; in which we've set up Webhook on Github's repository page.

Setting up Webhook will allow us to know which event is happened, and we can react to it appropriately. Such as `push` event in our case.
Then we write a NodeJs script to build a resulting Jekyll with changes made from development system to be live on server.

### Create a new repository and Jekyll site

* Create a new git repository with repository name as you desire.
* Clone the directory to your local system via `git clone <url>`.
* Go to cloned directory.
* Execute `gem install jekyll` to install Jekyll from Ruby Gem
* Execute `jekyll new my-new-site` to create a new website
* Execute `cd my-new-site` go to created website directory
* Execute `jekyll serve` to serve on localhost for development and debugging
* Then make some changes either via theme (`_sass/_base.scss`, `_sass/_layout.scss`) or creating a new post in `_posts`.
* Open browser at `localhost:4000` (normally port 4000, if not see the output from `jekyll serve`). You will see changes made on website.
* We will stage all changes and push it onto github via `git add -A`, then `git commit -m "Make some changes"`, finally `git push origin master`.

### Set up Webhook on Github

* Go to github repository page
* Click on Setting icon
* Click on Webhooks icon on the left panel
* Click on "Add webhook" button.
* Enter your github password to continue.
* Enter Payload URL, and Secret as `https://your-domain.com/test-ssp1` and `thisissecret` respectively. 
* *DO NOT* click on "Add webhook" green button just yet.
* Finish *Set up NodeJS script on server* section then go back to this section, and click on "Add webhook" green button.
* You will notice that Github has sent you a ping message. You should see a green correct icon on that packet. That means everything done now.

### Set up NodeJS script on server

We will create a NodeJS script to listen on such port, then configure apache to support *Reverse proxy* thus forward the data to our script.
With this set up, you can set Payload URL on *Set up Webhook on Github* section without specifying port as we can use normal http/https port to operate. It's cleaner.

* On your server, create a new folder which we name it as `webhook` on `~/webhook`. Note that it's not neccesary to create such a folder on accessible web via apache or anything i.e. not necessary to be on `/var/www/...`.
* Grab the code from [Skeleton Github Webhook for NodeJS](https://gist.github.com/haxpor/ad3dd6f099c6b8bd3c80ab8fb54b836b) and save it on your server as `github-webhook.js`.
* Execute `npm install fs`, `npm install http`, `npm install github-webhook-handler`, and `npm install child_process`.
* Create a directory `repo` via `mkdir repo`.
* Go to `repo` directory by `cd repo`.
* Clone our Jekyll repository by executing `git clone <url>`.
* Modify the script for its `path`, and `secret` to be `/test-ssp1`, and `thisissecret` respectively.
* Set up Reverse Proxy to apache2 by editing its config file to have 
   
   ```
   ProxyPass /test-ssp1 http://localhost:9500/test-ssp1
   ProxyPassReverse /test-ssp1 http://localhost:9500/test-ssp1
   ```

   Port here is the same as we have inside the script. If you already have https supported on your apache, thus it's good as we will have higher security for free when set up in this way. But it's not mandatory.
* Restart apache2 via `service apache2 restart`.
* Start our NodeJs script via `node github-webhook.js >> log-server.log 2>&1 &`. This means start script and write its output into `log-server.log` file separated from the log file we've set inside the code. We also start it in the background process.

### Make changes on Jekyll site and Push for automatic build

* Make some changes on Jekyll.
* Push the code to github.
* The script we've set up will do its work pulling in the latest source code, and build a Jekyll site onto directory that can be accessible from outside.
* All done.

# Reference

* [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)
* [Jekyll](http://jekyllrb.com/)
* [Jasmine](https://jasmine.github.io/)
* [Fastlane](https://fastlane.tools/)
