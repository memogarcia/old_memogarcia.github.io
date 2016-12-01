---
layout: post
title:  "[Snippet] Jquery show and hide items"
date:   2015-06-11 15:23:00
categories: js jquery
---

Hide and show items according to dropdown selection

{% highlight html %}

<select id="action">
  <option value="backup">Backup</option>
  <option value="restore">Restore</option>
</select>

<input type="text" id="backup_input" name="backup_input" value="backup"/>
<input type="text" id="restore_input" name="restore_input" value="restore"/>

<script src="http://code.jquery.com/jquery-1.11.3.min.js"></script>

<script type="text/javascript">

$(document).ready(function(){
	$("#action").change(function() {
		if ($("#action").val() == 'backup') {
			$("#restore_input").hide();
		}
		else {
			$("#restore_input").show();
		}
	});

	$("#action").change();
});
</script>


{% endhighlight %}