---
layout: post
title: "使用github page搭建博客记录"
description: ""
category: Geek
tags: [github, jekyll]
---
{% include JB/setup %}



在高中的时候，我经常写几句话，或者很长的文章（情书）在自己的纯空白草稿纸上，然后回头看看，写得很不错呢，那种感觉很好。


现在，用markdown、git、github、bitbucket、Jekyll和Jekyll-bootstrap的组合，可以在我的macbookpro上，重新得到纯空白草稿纸的感觉。

计算机就是这样，你必须把机器得弄得复杂点，才能得到你真正想要的，机器只会执行指令，你得设计出足够多的指令，并且他们足够好方便组合使用，他才能足够智能。

markdown是一种格式，能让你轻松写出好看的文本。git好像在这里没啥用，它主要是为了能把内容发布到github上，github呢，能提供免费的主机，bitbucket也一样，不过可以存放私有内容。Jekyll可以帮你生成静态网页。Jekyll-bootstrap嘛，是一个Jekyll基础上的扩展，可以帮你生成网页，定制主题等。


最后瞎整的结果大概是这样的：

- 本地git库：master，public，private，分别对应所有内容、github托管内容，和私有内容。

  public会push到chencan.github.com的master分支(`git push github public:master`)，private会push到https://chencan@bitbucket.org/chencan/private-blog.git的master分支(`git push bitbucket private:master`)，
master只用来合并两边内容。                                                                                                 

- 主题我更喜欢zen，但the_minimum对tag支持更好，而其他主题我都不太喜欢，所以就选择了the_minimum。

记录一些tip：

- 如何更改bitbucket或者github某一repository的默认branch

	这个都有设置的。
	
- 如何将本地的A分支，push到远程S上的B分支

	`git push S A:B`



