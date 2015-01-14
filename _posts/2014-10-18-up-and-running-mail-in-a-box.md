---
layout: post
title: Up and Running With Mail-in-a-Box
description: Impression on setting up my own mail server with Mail-in-a-Box
modified: 2014-10-18
category: blog
tags: [email, privacy]
---

## Intro

I’ve been trying to learn a lot more about data security and internet privacy, which lead to the [Mailpile project](http://mailpile.is). I’ve set that up with a Gmail account, but I wanted to try to try to “own my data” a little bit more, so I’ve decided to host my own email with [Mail-in-Box](https://mailinabox.email). Running a mail server seems only <em>slightly</em> over my head, so it’s time to learn something new!

Mail-in-a-Box has a [great setup guide](https://mailinabox.email/guide.html) and I'm not going to pretend to duplicate that. I want to write up my experience as additional reading for people who don’t have much experience with the linux command line, or are perhaps on the fence about trying somehting like this. Personally, I find reading multiple accounts of installing software helps me feel confident about starting a project, and prepared for when things go wrong. And lucky for you, gentle reader, several things went wrong in the process! 

## VPS

Mail-in-a-Box recommends a VPS with at least 1G of ram. Importantly, nothing else should be on your machine. I already have an account with [Digital Ocean](http://digitalocean.com), so I decided to go with them. If you starting from scratch you can see Digital Ocean’s great documentation on [getting started with Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04).

The first thing I do is login to my Digital Ocean account panel and select a new droplet with Ubuntu 14.04 64 bit on it. It automatically adds my SSH keys, so I don't need to be sent a root user password over email.

I look through the mail in a box documentation and read that the droplet must be the same name as the hostname. I go to the control panel and rename it, but there’s a message that reads:

{% highlight bash %} 
This will update the displayed hostname of your droplet and the automatically generated PTR
record. It will not however modify your actual hostname inside of the system.
{% endhighlight %}

I consider going through with it, but I have a vague feeling that this is going to be an issue. More importantly, it’s so damned easy to destroy and create DO droplets, so I decide to start over. This time I name it <em>box.mydomain.com</em>.

60 seconds later I’m back on track. I run through the [initial server setup](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04) and disable root login (after all, what’s the point of hosting your own email if your root login gets compromised. Not so private, huh?)

I login with root and bam, I get this error: 

{% highlight bash %} 
 @ WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED! 
 IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY! 
 Someone could be eavesdropping on you right now (man-in-the-middle attack)! 
 It is also possible that a host key has just been changed. The fingerprint
 for the RSA key sent by the remote host is
{% endhighlight %}


[A little research](https://www.digitalocean.com/community/questions/how-can-i-get-rid-of-warning-remote-host-identification-has-changed) tells me that since I had already generated an RSA key for this server IP address in my first droplet, I needed to clear the last line of my known_hosts file in order to access the second droplet. I decided to clear it out manually. On my locallymachine I do:

{% highlight bash %} 
cd ~/.ssh
nano know_hosts
{% endhighlight %}

and found my new droplet’s IP in the last line. I deleted the whole line <code>CTRL+K</code>, and closed and saved with <code>CTRL+x</code>.

## Domain

I’m able to login, where I run Ubuntu updates. At this point, I read through the documentation again and see that I made an error when I registered my domain. When I bought the domain, I had automatically put the Digital Ocean nameservers, thinking I was cleverly saving myself some time. Not so. The Mail-in-a-Box needs to be set up with “glue” records pointing to the IP from the hostnames. The Mail-in-a-Box [DNS instructions](https://mailinabox.email/guide.html#domain-name-configuration) explain it more clearly than I will.

## Installing Mail-in-a-Box

Finally, the big moment comes to install Mail-in-a-Box with the following command

{% highlight bash %} 
curl -s https://mailinabox.email/bootstrap.sh | sudo bash
{% endhighlight %}

I follow the instructions and everything is smooth. Mail-in-a-Box recognizes my domain. 

I hit a snag when I set up an admin password with spaces:

{% highlight bash %} 
Okay. I'm about to set up me@mydomain.com for you. This account will also
have access to the box's control panel.
password:
{% endhighlight %}

I enter my password, but it tells me:

{% highlight bash %} 
Passwords cannot contain spaces.

FAILED: tools/mail.py user make-admin me@mydomain.com
-----------------------------------------
That's not a user (me@mydomain.com).
-----------------------------------------
alias added
added alias hostmaster@box.mydomain.com (=> administrator@box.mydomain.com)
added alias postmaster@box.mydomain.com (=> administrator@box.mydomain.com)
added alias admin@box.mydomain.com (=> administrator@box.mydomain.com)
{% endhighlight %}

I run the setup script again, but the password part fails again. I end up finding the forums, where I see that I can access user controls from the command line with the following:

{% highlight bash %} 
sudo tools/mail.py
{% endhighlight %}

which returns

{% highlight bash %} 
Usage:
  tools/mail.py user  (lists users)
  tools/mail.py user add user@domain.com [password]
  tools/mail.py user password user@domain.com [password]
  tools/mail.py user remove user@domain.com
  tools/mail.py user make-admin user@domain.com
  tools/mail.py user remove-admin user@domain.com
  tools/mail.py user admins (lists admins)
  tools/mail.py alias  (lists aliases)
  tools/mail.py alias add incoming.name@domain.com sent.to@other.domain.com
  tools/mail.py alias add incoming.name@domain.com 'sent.to@other.domain.com, multiple.people@other.domain.com'
  tools/mail.py alias remove incoming.name@domain.com
{% endhighlight %}

With these commands, things are straightforward. I add the email address I want:

{% highlight bash %} 
sudo tools/mail.py user add me@mydomain.com
{% endhighlight %}

And wait for it to ask for a passowrd. I then reset the admin password. 

{% highlight bash %} 
tools/mail.py user password me@box.mydomain.com [password here]
{% endhighlight %}

I end up not making my email me@mydomain.com an admin, and leaving what was set up initially (me@box.mydomain.com) 

That’s about it! All of the interface and instructions within Mail-in-a-Box are pretty clear. If you hit a snag that I managed to avoid, or want to learn more about the project, check out the [Mail-in-a-Box forums](https://discourse.mailinabox.email/).
