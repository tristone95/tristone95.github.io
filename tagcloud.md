---
layout: page
title: Tags
permalink: /tagcloud/
---

<script>
  var query = location.search;
  query = query.substring(1);
  var kvs = query.split('&');
  var key;
  for (var i=0;i<kvs.length;i++){
    var kv = kvs[i];
    var k = kv.split('=');
    if (k.length == 2) {
      if (k[0] == 'key') {
        key = k[1];
      }
    }
  }
  if (key.indexOf('/') != -1) {
    key = '';
  } else {
    key = decodeURI(key);
  }
</script>

正在查看 "<script>document.write(key);</script>" 相关的文章

<div class="post">
  <div class="post-archive">
  {% for post in site.posts %}
    <!-- <h2>{{ post.date | date: "%Y" }}</h2> -->
    <ul class="listing" style="display: none;">
      <li>
      <span class="date">{{ post.date | date: "%Y/%m/%d" }}</span>
      <a href="{{ post.url | prepend: site.baseurl }}" key="{{ post.tags }}">
      {% if post.title %}
  		{{ post.title }}
  	  {% else %}
  		{{ site.page_no_title }}
  	  {% endif %}
  	  </a>
  	</li>
    </ul>
  {% endfor %}
  </div>
</div>

<script>
  window.onload=function() {
    var items = $('.post-archive a');
    for (var i=0; i<items.length; i++) {
      var item = items[i];
      if ($(item).attr('key').toLowerCase().indexOf(key.toLowerCase()) == -1) {
        $(item.parentElement.parentElement).remove();
      } else {
        $(item.parentElement.parentElement).show();
      }
    }
    if ($('.post-archive a').length == 0) {
      $('.post-archive').html('<font color="red">没有记录</font>')
    }
  }
</script>
