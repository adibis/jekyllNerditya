---
layout: post
title: "Exploring i3 Window Manager"
category: code
---

My personal laptop is a 6+year old Thinkpad T420. I installed Arch linux on it after I bought it and have been using the same install since. Over the years I have tried so many different things on it at it had Gnome, KDE, Cinnamon and XFCE desktop environments installed while I only used XFCE exclusively for the last three years or so.

During the holidays I was spending more time on the personal laptop than usual when I noticed that it was taking too long to boot and was considerably slower than what I remembered. I started investigating and realized that gdm, apparently for over 2 years now, has been [behaving weird and moving graphical instance on TTY2][1] location while keeping login screen on TTY1. People reported seeing [heavier resource usage][2] and slowness as a result. In the resulting obsession, I ended up ditching GDM for [lightDM][lightDM] and also moving to [i3wm][i3wm] as my window manager. Though not significant, I've noticed a decent impovement in the responsiveness of the system as a result.

i3wm is a [tiling window manager][tiling]. Tiling window managers automatically divide the desktop space and arrange all programs without overlapping on each other as opposed to traditional approach of stacking them. Moving from XFCE to i3 was a little painful at first but after a couple of days of tweaking, I have most of my workflow ported over successfully. Here's what I have been using for the last week.

![i3wm workflow]({{ site.url }}/public/img/exploring-i3-window-manager/i3wm-workflow.png)

You can see me writing this blog post in the screenshot above. The bar at the top of the screen has virtual workspaces on the left, browers, terminals, file manager and music -- each on its own space. On the right is information that I find relevant such as audio controls, network and battery status, etc. This bar is an i3 extension called [bumblebee-status][bumblebee]. Another neat little thing is [i3lock-next][i3lock-next] that blurs the current view and locks the screen as can be seen below.

![i3lock-next]({{ site.url }}/public/img/exploring-i3-window-manager/i3lock-next.png)

The complete configuration file is available [here][etc] in the git repository. Enjoy!

[1]: https://bugzilla.gnome.org/show_bug.cgi?id=747339
[2]: https://bbs.archlinux.org/viewtopic.php?id=196776
[lightDM]: https://wiki.archlinux.org/index.php/LightDM
[i3wm]: https://i3wm.org/
[tiling]: https://en.wikipedia.org/wiki/Tiling_window_manager
[bumblebee]: https://github.com/tobi-wan-kenobi/bumblebee-status
[i3lock-next]: https://github.com/owenthewizard/i3lock-next
[etc]: https://github.com/adibis/etc/tree/master/i3
