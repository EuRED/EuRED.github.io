---
layout: post
title: Knowing the site generated time by Jekyll on GitHub Pages
date: 2015-01-05T17:08:17+05:45
tags: jekyll, github pages
---

If you are using Jekyll to generate your blog on GitHub pages, you might want too know the last extact site generated time as the Jekyll generate and keep the files on `_site`.

I found the following tricks to do it.

Here's the code:

{% highlight text %}
{% raw %}
{{ site.time }}
{% endraw %}
{% endhighlight %}

To know generated time according to your locale timezone you have to add your timezone in your `_config.yml`:

{% highlight text %}
{% raw %}
timezone:    Asia/Kathmandu #Add your own locale timezone
{% endraw %}
{% endhighlight %}

You can add/custumize this in the end of your site source code as following:

{% highlight text %}
{% raw %}
<!-- Proudly Hosted on GitHub | Generated {{ site.time }} | Revision {{ site.github.build_revision }} -->
{% endraw %}
{% endhighlight %}

Following is the generated example format:

{% highlight text %}
{% raw %}
<!-- Proudly Hosted on GitHub | Generated 2015-01-05 15:53:08 +0545 | Revision e7a8fa260c4381526ecbbc12d5bbeb00f3077eeb -->
{% endraw %}
{% endhighlight %}

Happy Jekyll'ing! :sunglasses: