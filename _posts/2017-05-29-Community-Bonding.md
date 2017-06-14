---
layout: post
title: "Community Bonding period ends"
date: 2017-05-29
---

Hey!
During the Community Bonding Period, Gedare provided detailed instructions as 
to what needs to be done during the summer.

* Begin to code as per our mentor's instructions and our proposal
* Ask them for preffered form of communication
* Introducing ourselves to the community with a mail on users mailing list
* Adding our project to [Tracking Table](https://devel.rtems.org/wiki/GSoC/2017#StudentsSummerofCodeTrackingTable)
* Adding a wiki page
* Writing blog posts
* Attending all weekly meetings

We had a meeting during the Community Bonding Period and we discussed what we 
were up to and what we had done. 

I had been busy with exams and relocating since my college got over. But got a 
chance to look at the macro files and communicate with Chris and Kuan about the
format of INI files during the second half of the Community Bonding period.

We discussed why INI would be a better choice over YAML for the configuration 
files. Using PyYAML would add a dependency in the tool that would be difficult 
to handle across all hosts we support. Chris had already pushed helper code for
handling INI format. I studied it and asked the questions I had. We finalized 
on the format for ini files for different kinds of macro files.

We discussed specifics for configuration files which need user-specific settings 
as well as other details about ConfigParser would parse the section:values.
Thank you Chris for patiently answering the countless stupid questions I ask.

I was asked to come up with a ReST documentation for the INI format.

