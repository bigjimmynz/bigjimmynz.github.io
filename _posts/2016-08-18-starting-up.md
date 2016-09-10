---
layout: post
title: Starting up...
date: '2016-08-18T03:22:00.001-07:00'
author: James Morgan
tags:
modified_time: '2016-08-18T03:33:23.172-07:00'
thumbnail: https://1.bp.blogspot.com/-ZJ3-PtCGQto/V7WMVqTrmhI/AAAAAAAAAZM/sYzrAjYk-tQ1iwX8gKGp70n8NpsBOknKQCLcB/s72-c/devops_all_the_things.jpg
---

I should welcome all you readers. I originally started this on my personal blog. But have
decided to organise myself more and devote a specific blog to my technology work and learning. My
original blog will stay as a personal project and thoughts output.

To give you some background, I started off in Information Technology as a basic level systems
admin. Building servers and keeping office networks running. Through the years I have progressed
through skills and seniority levels. Ending up team leading a group of sysadmins in the SaaS
sector for a multinational enterprise. That lead me to my current focus.

<img style="float: right;"  src="https://1.bp.blogspot.com/-ZJ3-PtCGQto/V7WMVqTrmhI/AAAAAAAAAZM/sYzrAjYk-tQ1iwX8gKGp70n8NpsBOknKQCLcB/s200/devops_all_the_things.jpg" />
These days I am a DevOps Engineer. There's lots of debate into what DevOps is, but really it's an
evolution of things that have been done before, what is being done now, and what new technologies
will come out of the industry. It's the automation of systems building and management, the deployment
of configuration and applications, and the ability to scale all of those systems and processes.
Very much designing things to be as hands off as possible. If you are a systems admin that has
scripted installs of servers or configurations, you have been doing DevOps. If you are a developer
that has automated the testing and deployment of an application, you have been doing DevOps. And
it progresses from there. Really there are many other places that probably define it better than
I can. But it's where I am now, and I quite enjoy the tech and the logistics behind designing
and implementing things in this space.

<img style="float: left;" border="0" height="195" src="https://3.bp.blogspot.com/-sv67Tbhid6s/V7WLXSG0EWI/AAAAAAAAAZA/4DiJ0d__Msw5GjJbTpJoFJweGQdJNnDPgCLcB/s200/6a01bb07ae7c9b970d01b8d1985065970c-600wi.png" width="200" />
The whole premise behind this, is designing a way that your systems, infrastructure or applications
can be created as code. That way it can be version controlled for changes, and can be repeated quickly
and reliably. Systems use to be built carefully and have a long-life, requiring long-term maintenance,
which allows the possibility of config drift. That drift often being the bane of a support person.
Now it is all about being able to destroy a problematic server and recreate it's state quickly
(usually minutes, maybe hours).<br /><br />And what have I been doing? I have been upskilling in the
processes and tools required to do all this. My ultimate goal would be to implement and run large
scale infrastructure, that is in some ways self-healing and scalable.

**Amazon Web Services(AWS)** was my first point of learning (I started working with it a couple years
ago though). The sheer breadth of services they can provide is impressive. Which is likely why companies
like Netflix use them. You could build your entire infrastructure using them, and have pretty much
everything you need. And if you don't want to manage systems, they provide tools to deploy services
and apps without having to know about servers or operating systems.

To orchestrate all my work I have been using [Ansible](https://www.ansible.com) and [Puppet](https://puppet.com), (Ansible being the much preferred option for me). Both
are a scripting frameworks to automate tasks and configurations. With an account from AWS, I could
start out with nothing and build out a systems stack that an be deployed within minutes. And taken
down just as fast.<

There are other tools out there like the previous mentioned that can so the same or similar things.
It all comes down to your usage or business case. Generally picking them involves costs, and
how easily they might be supported. But can be personal preference too. Don't get caught up in
trying to pick between them. A good understanding of your requirements will lead to the correct
answer. In the future that may change, but that's a step to be taken at a future time.

Now using all the great tools is one thing. But if things aren't designed or planned correctly,
it will still go horribly wrong. In days past an application was built and "thrown over the fence"
to the Ops/Support team. Now it's about understanding and planning the entire process from coding
to deployment and management. A company is generally all working towards one goal. Therefore is a
team and should work together. Fractures between internal teams can make things infinitely harder.
So in the modern environment it is in everyones best interest to have all team members involved
with support, or deployments. That way the end result is understood by all, and troubleshooting
should be easier.<br /><br />Thats probably enough rambling from me. I'm sure there are others than
can go into more detail better than I can. I'm just trying to get better at what I do. I do hope what
I end up putting here can help people out. And if someone provides info that I can learn from, all
the better. Take care, and automate all the things.
