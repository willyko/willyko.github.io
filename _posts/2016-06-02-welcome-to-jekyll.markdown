---
layout: post
title:  "Alpacado.new()"
date:   2016-06-02 15:33:33 -0700
categories: alpacado update
comments: true
---
This is the creation of Alpacado Software Solution.  This is an umbrella group name for multiple projects that I have in mind.

A quick background on the name "ALPACADO".  It's a cross of Alpaca and Avocado. Period. 

So this site is built on jekyll following exactly as the website stated: 

{% highlight shell %}
jekyll new landing-page
cd landing-page
jekyll serve
{% endhighlight %}

Thanks to Jekyll.  Please check out the following resources 
 [Jekyll docs][jekyll-docs] 
[Jekyll’s GitHub repo][jekyll-gh]

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll

Next I guess I should introduce the environment I am currently using.
Having attended Polyglot Unconference 2016 @ Vancouver, I attempted to
make my environment for like home (dotfiles).

So I am currently using a Macbook Air and using fish shell
`brew install fish`

For fish plug-in manager, I chose fisherman. Also I strongly suggest
metro for the powerline look
`fish metro`

Next I use rvm to handle ruby versions
[RVM doc][rvm-docs]

[rvm-docs]: https://rvm.io

For text editors I am using VIM.  Yes that's right.
`brew install macvim`

Oh and for VIM I am now using Neobundle for plugins. 

So I did indeed hit some complication with all this python versioning,
vim, powerline vimrc, bash, fish...
There's a very nice guide that solved my problem
[Redemption in a Blog - Installing Powerline on OSX][powerline-guide]

[powerline-guide]: https://blog.codefront.net/2013/10/27/installing-powerline-on-os-x-homebrew/

Then there's more problem with RVM versioning and fish and vim...
[fish integration][fish-integration]

[fish-integration]: https://rvm.io/integration/fish

The problem comes when your rvm's current ruby version doesn't match
with the ruby version vim supports...  A temporary solution is to set
rvm ruby version manually to match that of vims 
a quick checkup is to see if the following two matches
`vim --version`  (very last few lines after linking) vs. 
`which ruby`

