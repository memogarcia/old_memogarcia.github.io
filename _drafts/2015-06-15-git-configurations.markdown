---
layout: post
title:  "Git cheat sheet"
date:   2015-06-15 12:30:00
categories: git configuration
---

Change remote url to use git instead of https

{% highlight bash %}

git remote set-url origin git@github.com:user/repo.git

{% endhighlight %}


Update .gitignore to add more entries

{% highlight bash %}

git rm -r --cached .

{% endhighlight %}


Better logging

{% highlight bash %}

git log --oneline --graph

{% endhighlight %}


Check actual changes in a file

{% highlight bash %}

git log -p filename

{% endhighlight %}


Extract a file from another branch

{% highlight bash %}

git show some-branch:some-file.js

{% endhighlight %}


Create aliases for git commands

{% highlight bash %}

git config --global alias.l "log --oneline --graph"

{% endhighlight %}


Clone a specific branch

{% highlight bash %}

git clone -b branch repo

{% endhighlight %}


Get statistics about a repo

{% highlight bash %}

git shortlog -sne --since '3 years ago'

{% endhighlight %}
