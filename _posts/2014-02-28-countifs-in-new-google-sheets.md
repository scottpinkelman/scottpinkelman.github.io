---
layout: post
title: Using COUNTIFS in the New Google Sheets (February 2014)
description: Working with the COUNTIFS in the new google sheets.
modified: 2014-02-28
category: blog
tags: [data, google, spreadsheets]
---

Today I tried to convert a spreadsheet to the  [new version of Google Sheets](https://support.google.com/drive/answer/3541068?hl=en) that's supposed to be faster and better. As I was copying over formulas, I decided to try out a new formula that Google had rolled out in this new version of sheets. It's called COUNTIFS and it allows you do conditional counting based on mulitple sets of criteria. 

Previously, I had used the following formula to help build an easily copy-pasted set of cells for tracking users at a public computer lab. I wanted to check if a certain column had an entry in it, and count it if it was in a row with a certain date. The formula read: 

{% highlight html %}
=COUNTA( IFERROR( FILTER(N5:N300 ; SEARCH( E6 ; H5:H616 ) ) ) ) 
{% endhighlight %}

Using COUNTIFS, I was able to update it to a pleasant-looking:

{% highlight html %}
=COUNTIFS(H5:H300, E6, N5:N300, "\*")
{% endhighlight %}

Which basically says, moving backwards,: count if there's anything (this is the variable <code>\*</code>), in the N column IF in that row the value of H (a series of dates in my example) column is E6, the date we're building <em>Daily Total For</em>.

This isn't news for Excel experts, but might come in handy for those (like me) who are not.