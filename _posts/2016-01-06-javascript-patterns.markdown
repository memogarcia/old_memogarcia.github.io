---
layout: post
title:  "[Snippet] Javascript patterns"
date:   2016-01-05 19:55:11
categories: javascript patterns
---

Module Design Pattern

helps encapsulate code and provide public and private methods

{% highlight javascript %}

var HTMLChanger = (function () {
    var contents = 'contents'

    var changeHTML = function() {
      var element = document.getElementById('attribute-to-change');
      element.innerHTML = contents;
    }

    return {
      callChangeHTML = function() {
        changeHTML()
        console.log('lol')
      }
    };
})();

HTMLChanger.callChangeHTML()

{% endhighlight %}


Revealing Design Pattern

{% highlight javascript %}

var Exposer = (function() {
  var privateVariable = 10

  var privateMethod = function() {
    console.log('privateMethod')
  }

  var methodToExpose = function () {
    console.log('expose this method')
  }

  return {
    publicMethod = methodToExpose
  };
})();

Exposer.publicMethod()

{% endhighlight %}
