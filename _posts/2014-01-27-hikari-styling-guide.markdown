---
layout: post
title:  "On the correct computation of the f-stop"
date:   2018-10-14 22:00:00
---

#### Heading 4
the correct way


{% raw %}
  $$a^2 + b^2 = c^2$$ --> note that all equations between these tags will not need escaping! 
{% endraw %}

{% highlight c++ %}
var separate = function(css) {
  var lines = css.split('\n');
  var docs, code, line, blocks = [];
  while (lines.length) {
    docs = code = '';

    while (lines.length && checkType(lines[0]) === 'single') {
      docs += formatDocs(lines.shift());
    }

    if (lines.length && checkType(lines[0]) === 'multistart') {
      while (lines.length) {
        line = lines.shift();
        docs += formatDocs(line);
        if (checkType(line) === 'multiend') break;
      }
    }
    while (lines.length && (checkType(lines[0]) === 'code' || checkType(lines[0]) === 'multiend')) {
      code += formatCode(lines.shift());
    }
    blocks.push({ docs: docs, code: code });
  }
  return blocks;
};
{% endhighlight %}
