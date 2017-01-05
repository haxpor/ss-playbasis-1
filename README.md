# ssp-1

A #1 sharing session with awesome Playbasis Team at Playbasis Office.
Topics revolve around **automation**.
Agenda is as follows.

1. Gitflow via commandline
2. Jekyll example (Webhook on Github, build commands, automation)
3. Automate iOS build workflow with Fastlane (Android also applies, see the link below).
4. Testing with Jasmine on native-sdk-js
5. UI Testing on iOS

# Introduction

This repository consists of 2 git submodules

1. [ssp1-jekyll](https://github.com/haxpor/ssp1-jekyll)
2. [ssp1-fastlane](https://github.com/haxpor/ssp1-fastlane)

With above set up, this repository acts as an umbrella repository that contains all other repositories or information.
Thus when asked to refer to sharing session #1, then just head to repository name *ssp-1*.

# How to

1. Clone this repository to your system via `git clone git@github.com:haxpor/ssp1.git`.
2. Go to repository folder via `cd ssp1`.
3. Execute `git submodule update --init`.
4. You're done. Refer to [ssp1-jekyll](https://github.com/haxpor/ssp1-jekyll), and [ssp1-fastlane](https://github.com/haxpor/ssp1-fastlane) on further instruction on how to make use of each of them.

# Reference

* [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)
* [Jekyll](http://jekyllrb.com/)
* [Jasmine](https://jasmine.github.io/)
* [Fastlane](https://fastlane.tools/)
