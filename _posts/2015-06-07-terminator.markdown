---
layout: post
title:  "Terminator configuration"
date:   2015-06-07 19:55:11
categories: terminator configuration linux
---
Simple terminator configuration

{% highlight bash %}
~/.config/terminator/config
{% endhighlight %}


{% highlight bash %}
[global_config]
[keybindings]
  new_tab = <Primary>t
  copy = <Primary>c
  paste = <Primary>v
  next_tab = <Primary>Tab
[profiles]
  [[default]]
    scrollbar_position = hidden
    palette = "#073642:#dc322f:#859900:#b58900:#268bd2:#d33682:#2aa198:#eee8d5:#002b36:#cb4b16:#586e75:#657b83:#839496:#6c71c4:#93a1a1:#fdf6e3"
    background_image = None
    foreground_color = "#d8d3c4"
    scroll_on_output = False
    show_titlebar = False
    background_color = "#002b36"
    scrollback_infinite = True
[layouts]
  [[default]]
    [[[child1]]]
      type = Terminal
      parent = window0
    [[[window0]]]
      type = Window
      parent = ""
[plugins]
{% endhighlight %}

