---
title: "Deleting stale computer invitations"
layout: default
date: 2015-05-29 09:29:29 PDT
categories: casper
comments: true
tags: jamf capser jss security
---

I recently discovered a small, tiny, minuscule, security flaw with the JSS that I never knew about. Computer invitations allow the quickadd package, and the web enrol to get your computer joined to your JSS. Every time a quickadd package gets generated, a computer invitation with unlimited expiry is created as well. 

We need to think about why these invitations may be problematic. What would happen if someone were to get a hold of these invitations and enrol their own machine in your JSS? Could they get access to active directory? Could they get access to an expensive software catalog? Are you able to detect rogue machines on your network?
Maybe this isn't an issue, maybe your JSS is behind a firewall. 

If your JSS is on the Internet these could present a huge security risk.

Thankfully you can delete these guys if you want to. If you go to ;
{% highlight bash %}
https://<my.jss.com>:8443/api/#!/computerinvitations/findComputerInvitations_get
{% endhighlight %}

Hit the "Try it out!" button and look at the results.  
You'll see a list of computer invitations. Some will have an "Unlimited expiry". In my mind there should only be one of those, that goes with one quickadd package, and the rest should be deleted. 

To delete one we use the API to delete the ID of the invitation.  
{% highlight bash %}
curl -sfku <user>:<pass> "https://<my.jss.com>:8443/JSSResource/computerinvitations/id/<id to delete> -X DELETE"
{% endhighlight %}

So far this is the only way I can figure out how to do it.

