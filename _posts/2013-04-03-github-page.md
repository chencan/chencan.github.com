---
layout: post
title: "使用github page搭建博客记录"
description: ""
category: Geek
tags: [github, jekyll]
---
{% include JB/setup %}

一直羡慕自己搭建博客的人，可以自定制自己的页面，多有个性。

后来接触了trac，准备用此来搭建博客，但发现需要部署到公网上，是要钱的，                                                     
就放弃了。

感谢github和Jekyll-bootstrap让我如愿以偿。

- 目标：搭建一个能让自己记录东西的地方。

- 原则：可以分享，也可以写私有的东西，界面可以个性化，不受拘束。

最后瞎整的结果大概是这样的：

- 本地git库：master，public，private，分别对应所有内容、github托管内容，和私有内容。

public会push到chencan.github.com的master分支，private会push到https://chencan@bitbucket.org/chencan/private-blog.git的master分支，
master只用来合并两边内容。                                                                                                 

- 主题我更喜欢zen，但the_minimum对tag支持更好，而其他主题我都不太喜欢，所以就选择了the_minimum。

