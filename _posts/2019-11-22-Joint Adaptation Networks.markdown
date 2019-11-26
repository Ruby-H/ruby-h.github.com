---
layout: post
title:  "Joint Adaptation Networks"
date:   2019-11-22 19:46:21 +0800
tags: JAN
color: rgb(255,90,90)
cover: '../assets/JAN.png'
subtitle: 'Joint distribution adaptation'
---
摘要：

众所周知，深度网络能成功将可迁移的特征从源域适应到不同的目标域。本文提出了联合适应网络（joint adaptation networks，JAN），该网络基于提出的联合最大均值差异（joint maximummean discrepancy，JMMD）基准来学习对齐跨域的多个特定于域的层的联合分布，从而训练迁移网络。文章采用对抗训练测略，来最大化JMMD，从而使源域和目标域的分布更加可分。

详细正文：

$a^2 + b^2 = c^2$
You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
