---
layout: post
title: Google Sheets Column Sorter with Google Apps Scripts
description: Ready-to-use script to sort columns in Google Sheets -- for the non-coder
modified: 2014-11-30
category: blog
tags: [data, google apps scripts, spreadsheets, google]
---


I recently wrote a [simple two column sorter](https://gist.github.com/sco-tt/b3f07c1882ac698afc74) for Google Sheets in Google Apps Scripts. While there is an [example of the Range.sort function on Stackoverflow](https://stackoverflow.com/questions/12205908/sort-ranges-in-an-array-in-google-apps-script), it's for a slighly more complicated use case. Mine is for a very simple case: you want to do a two column sort, you have a header, and you'd like to access it from the Google Sheets mney. You can see it in action [here](https://docs.google.com/spreadsheets/d/1kXFj_m5_JoNI7gSVT9yVECFNE3dH60bSZnNOCmQYl2c/edit?pli=1#gid=0). (Note: you'll need to be signed in to a Google account and authorize the app to run in order to use the sort function.)

Something to keep in mind about two column sorting: the first column you sort by should be less specific than the second. In my example above, it's for cities where multiple rows have the same value. 

I hope that this can be used by non-technical people who need this functionality, so I'll call attention to the five variables you'll need to customize the script. They're in the sort() function:

{% highlight js %} 
 /**  Variables for customization:
  
  Each column to sort takes two variables: 
      1) the column index (i.e. column A has a colum index of 1
      2) Sort Asecnding -- default is to sort ascending. Set to false to sort descending
  
  **/
 
  //Variable for column to sort first
  
  var sortFirst = 2; //index of column to be sorted by; 1 = column A, 2 = column B, etc.
  var sortFirstAsc = true; //Set to false to sort descending
  
  //Variables for column to sort second
 
  var sortSecond = 3;
  var sortSecondAsc = false;
  
  //Number of header rows
  
  var headerRows = 1; 
 
  /** End Variables for customization**/

{% endhighlight %}

For each column you'll be sorting, you'll need to enter the column index (i.e. Column B has an index of 2, Column C and index of 3) for <b>sortFirst</b> and the <b>sortSecondAsc</b> variables. You'll use <b>sortFirstAsc</b> and <b>sortSecondAsc</b> and whether or not you want to sort it ascending. If you put false, that column will sort descending. Finally, enter the number of header rows in the <b>headerRows</b> variable so they can be excluded from the sort. Make sure these lines end with a semicolon and you're good to go.

<div markdown="0"><a href="https://gist.github.com/sco-tt/b3f07c1882ac698afc74" class="btn">View on Github</a></div>



