---
title: Repository
description: Best Practices
published: true
date: 2024-03-08T16:28:19.400Z
tags: veeam
editor: markdown
dateCreated: 2024-03-08T16:28:16.745Z
---

# Forum


> Is it ok for Veeam Backup to be on same server as the repository?
> 
> {.is-danger}


> The best way would be to turn that physical server into a Linux hardened repository so you can take advantage of immutability. Run your VBR as a VM on your prod hosts.
> 


> Gostev
> 
> 
> VBR running as a VM on production hosts it protects would be a bad idea though. As when those host or their storage will go down, so will VBR. And then we're talking hours before you can even start your restores.
> {.is-warning}


> #1 rule of backup is that a backup system should not rely on a production system it protects in any way.
> {.is-info}





Absolutely right. OP didn’t mention another site - so if he loses all prod hosts, he’s probably waiting for new hardware before restores could take place. For a single site and only 1 other server, the hardened repo would offer the best protection, which leaves VBR only able to run as a VM in prod.


OP, if you have more hardware or preferably another site - then run VBR from the other site, with a hardened repo at prod, and another repo at DR for copy jobs (preferable immutable as well)



Ideally you want to separate your repository from any other services. If your physical server acting as the repository has some sort of issue or crash, all that data is potentially lost. Introducing other roles on top of a storage repository increases the likelihood of that happening.

> The Veeam server itself could be a simple VM. As long as you backup the config file the Veeam server can be recreated if ever needed.
{.is-info}




> Having the Veeam server as a physical machine makes protecting it easier. It should be standalone, not in domain. This is harder when the Veeam server is a VM. Then use copy jobs to copy the data to a immutable repository.
{.is-success}




I used one of my Repo servers as my VBR servers as recommended by a partner, works OK, but I would not do it again this way. When it comes to hardware refresh I will look to move VBR to another server, probably Phys (outside of any environment it protects).



I have it both ways. I have Veeam (with a repository) on a physical server, not on the domain, and I have another immutable Linux repository setup.

Do both.




