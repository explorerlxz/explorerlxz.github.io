---
layout: post
title:  "Make your uniq markdown blog"
date:   2017-07-22
---

## Hide your code/text blocks

Add these line to `_include/header.html` file

{% highlight html %}
  <script>
    function ishidden(odiv){
      var vdiv = document.getElementById(odiv);
      vdiv.style.display = (vdiv.style.display == 'none')?'block':'none';
    }
  </script>
{% endhighlight %}

In your blog, use these line to hide your code/text:

{% highlight html %}
  <div onclick="ishidden('X')">有东西藏起来了!</div>
  <div id="X" style="display:none;">啊，被发现了</div>
{% endhighlight %}


Maybe I should learn more about html and css to make my blog better.








## Reference

 - [用markdown折叠/展开代码](http://nyhtn.leanote.com/post/ea9dee43df01)
