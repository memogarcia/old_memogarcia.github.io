---
layout: post
title:  "[Snippet] Disable FileSystem redirection on Windows"
date:   2015-06-09 15:23:00
categories: python windows
---

When a 32 bit program runs on a 64 bit operating system the paths
to C:/Windows/System32 automatically get redirected to the 32 bit
version (C:/Windows/SysWow64), if you really do need to access the
contents of System32, you need to disable the file system redirector first.


{% highlight python %}

import ctypes


class DisableFileSystemRedirection:
    """
    When a 32 bit program runs on a 64 bit operating system the paths
    to C:/Windows/System32 automatically get redirected to the 32 bit
    version (C:/Windows/SysWow64), if you really do need to access the
    contents of System32, you need to disable the file system redirector first.
    """

    _disable = ctypes.windll.kernel32.Wow64DisableWow64FsRedirection
    _revert = ctypes.windll.kernel32.Wow64RevertWow64FsRedirection

    def __enter__(self):
        self.old_value = ctypes.c_long()
        self.success = self._disable(ctypes.byref(self.old_value))

    def __exit__(self, type, value, traceback):
        if self.success:
            self._revert(self.old_value)

{% endhighlight %}


usage:

{% highlight python %}

with DisableFileSystemRedirection():
	pass

{% endhighlight %}