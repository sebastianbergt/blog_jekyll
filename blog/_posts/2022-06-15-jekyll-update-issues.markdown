---
layout: post
title:  "Jekyll Update Issues"
date:   2022-06-15 09:16:48 +0000
categories: jekyll update
---
`Gemfile.lock` outdated:

Fix it with:

`rm Gemfile.lock`

`bundle install`

`bundle exec jekyll serve`

[Found help here](https://talk.jekyllrb.com/t/installing-themes-requires-install-of-outdated-gems/3486/4)
