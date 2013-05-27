---
layout: post
title: "使用github page搭建博客记录"
description: ""
category: Geek
tags: [github, jekyll]
---
{% include JB/setup %}



在高中的时候，我经常写几句话，或者很长的文章（情书）在自己的纯空白草稿纸上，然后回头看看，写得很不错呢，那种感觉很好。


现在，用markdown、git、github、bitbucket和Jekyll-bootstrap的组合，可以在我的macbookpro上，重新得到纯空白草稿纸的感觉。


- 目标：搭建一个能让自己记录东西的地方。

- 原则：可以分享，也可以写私有的东西，界面可以个性化，不受拘束。

这三东西很好的满足了我的需求：markdown是纯文本，并且容易被其他工具排版成漂亮的界面，git可以管理我记录的东西，github给我一个免费的服务器，Jekyll-bootstrap给了我一些工具发布类容，bitbucket给我一个存放私有内容的地方。


最后瞎整的结果大概是这样的：

- 本地git库：master，public，private，分别对应所有内容、github托管内容，和私有内容。

  public会push到chencan.github.com的master分支(`git push github public:master`)，private会push到https://chencan@bitbucket.org/chencan/private-blog.git的master分支(`git push bitbucket private:master`)，
master只用来合并两边内容。                                                                                                 

- 主题我更喜欢zen，但the_minimum对tag支持更好，而其他主题我都不太喜欢，所以就选择了the_minimum。

另外，补上一些tip：

- 如何更改bitbucket或者github某一repository的默认branch

	这个都有设置的。
	
- 如何将本地的A分支，push到远程S上的B分支

	`git push S A:B`



