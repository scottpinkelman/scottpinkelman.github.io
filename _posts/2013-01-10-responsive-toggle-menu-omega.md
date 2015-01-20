---
layout: post
title: Responsive Toggle Menu in Omega 3.x
description: "How to make a responsive toggle menu in Omega 3.x"
modified: 2014-01-27
category: blog
tags: [responsive design, drupal, omega theme, menus]
---

<section id="table-of-contents" class="toc">
  <header>
    <h3>Contents</h3>
  </header>
<div id="drawer" markdown="1">
*  Auto generated table of contents
{:toc}
</div>
</section><!-- /#table-of-contents -->

## Notes
* This was made with Omega 7.x-3.1
* This only works for first level menu items. See [this tutorial](http://ivanchaquea.com/creating-responsive-menu-omega-subthemes.html) for multi-level menus
* There is a [demo](http://demos.scottpinkelman.com/menus/)


## What you'll need
* An HTML5 Omega subtheme
* A text editor
* Optional: If you want to make a hip 'Navicon' instead of the word Menu, you'll need the [@font-your-face module](http://drupal.org/project/fontyourface) and a icon from somewhere like [IcoMoon](http://icomoon.io/)

## Modifying Omega Menu Template File

Once you've created an Omega HTML5 subtheme, you'll want modify the menu template in our subtheme. First copy __sites/all/themes/omega/omega/templates/region--menu.tpl.php__ to
__sites/all/themes/SUBTHEME_NAME/templates__
Open it up with a text editor. It should look like this:

{% highlight php %}
<div<?php print $attributes; ?>>
  <div<?php print $content_attributes; ?>>
    <?php if ($main_menu || $secondary_menu): ?>
    <nav class="navigation">
      <?php print theme('links__system_main_menu', array('links' => $main_menu, 'attributes' => array('id' => 'main-menu', 'class' => array('links', 'inline', 'clearfix', 'main-menu')), 'heading' => array('text' => t('Main menu'),'level' => 'h2','class' => array('element-invisible')))); ?>
      <?php print theme('links__system_secondary_menu', array('links' => $secondary_menu, 'attributes' => array('id' => 'secondary-menu', 'class' => array('links', 'inline', 'clearfix', 'secondary-menu')), 'heading' => array('text' => t('Secondary menu'),'level' => 'h2','class' => array('element-invisible')))); ?>
    </nav>
    <?php endif; ?>
    <?php print $content; ?>
  </div>
</div>
{% endhighlight %}

We'll modify the template so that we have a mobile menu toggle, and a role attribute in our main navigation menu. We'll add the the mobile menu toggle on line 4, and adjust our tag in line 5:

{% highlight html %}
<a href="#menu" class="menu-link">Menu</a>     /*This line is added*/
    <nav id="menu" role="navigation">      /*This line is modified*/
{% endhighlight %}

So __region--menu.tpl.php__ will look like this:

{% highlight php %}
<div<?php print $attributes; ?>>
  <div<?php print $content_attributes; ?>>
    <?php if ($main_menu || $secondary_menu): ?>
    <a href="#menu" class="menu-link">Menu</a>
    <nav id="menu" role="navigation">
      <?php print theme('links__system_main_menu', array('links' => $main_menu, 'attributes' => array('id' => 'main-menu', 'class' => array('links', 'inline', 'clearfix', 'main-menu')), 'heading' => array('text' => t('Main menu'),'level' => 'h2','class' => array('element-invisible')))); ?>
      <?php print theme('links__system_secondary_menu', array('links' => $secondary_menu, 'attributes' => array('id' => 'secondary-menu', 'class' => array('links', 'inline', 'clearfix', 'secondary-menu')), 'heading' => array('text' => t('Secondary menu'),'level' => 'h2','class' => array('element-invisible')))); ?>
    </nav>
    <?php endif; ?>
    <?php print $content; ?>
  </div>
</div>
{% endhighlight %}

## Javascript

Next we'll [add some javascript](https://drupal.org/node/304255) was used in the CodePen: 

{% highlight javascript %}

(function ($) { 
    $(document).ready(function($){
      $('body').addClass('js');
      var $menu = $('#menu'),
        $menulink = $('.menu-link');
       
    $menulink.click(function() {
      $menulink.toggleClass('active');
      $menu.toggleClass('active');
      return false;
    });});
}(jQuery));

{% endhighlight %}

I generally add it using the 'Theme's Info File' method (this caches it). I do that by creating a folder in my subtheme (e.g. __sites/all/themes/SUBTHEME_NAME/js/menu.js__) and then adding 'scripts[] = js/menu.js' to SUBTHEME_NAME.info) but you can do it however you like.

## CSS

Finally, we'll copy over the CSS to the global.css file.

{% highlight css %}
a.menu-link {
float: right;
    display: block;
    padding: 1em;
}
nav[role=navigation] {
    clear: both;
    -webkit-transition: all 0.3s ease-out;  
    -moz-transition: all 0.3s ease-out;
    -ms-transition: all 0.3s ease-out;
    -o-transition: all 0.3s ease-out;
    transition: all 0.3s ease-out;
}
.js nav[role=navigation] {
    overflow: hidden;
    max-height: 0;
}
nav[role=navigation].active {
    max-height: 15em;
}
nav[role=navigation] ul {
    margin: 0;
    padding: 0;
    border-top: 1px solid #808080;
}
nav[role=navigation] li a {
    display: block;
    padding: 0.8em;
    border-bottom: 1px solid #808080;
}
/*End Codepen Code */
/*Reseting system style causing gray lines to show up in Toggle Menu */
nav[role=navigation] ul.inline li {
    padding: 0;
}
{% endhighlight %}

Then we'll add this to __SUBTHEME_NAME-alpha-default.css__

{% highlight css %}
a.menu-link {
   display: none;
}
.js nav[role=navigation] {
    max-height: none;
}
nav[role=navigation] ul {
    margin: 0 0 0 -0.25em;
    border: 0;
}
 
nav[role=navigation]  li {
    display: inline-block;
    margin: 0 0.25em;
}
nav[role=navigation] li a {
    border: 0;
}
{% endhighlight %}

## Customize 

Clear your cache and check it out!
If you want to use a [navicon](http://css-tricks.com/three-line-menu-navicon/) instead of the word "menu" you can alter the menu template accordingly. I've used the [Icon font module](http://drupal.org/project/iconfonts) to display an icon font navicon. See CSS Tricks' ["HTML For Icon Font Usage](http://css-tricks.com/html-for-icon-font-usage/)

<div markdown="0"><a href="http://demos.scottpinkelman.com/menus/" class="btn">View Demo</a></div>