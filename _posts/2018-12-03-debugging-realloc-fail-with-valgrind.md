---
layout: post
title: "Debugging realloc() failure with valgrind"
category: code
---

I've been trying to solve the [advent of code][advent of code] puzzles this year. Mainly to improve my python but I'm also using it as an opportunity to implement the same logic in C. The C implementations are actually helping me learn more than the python code.

I was working on the [day 3][day 3] problem in C when I hit a problem with realloc() failing. The code ran fine otherwise and if I allocated a huge buffer to start with (basically never calling realloc()) then it gave correct answers. The realloc() call kelp failing.

I spent a few minutes trying to figure out manually what was going wrong before giving up to try and find a better way. I had heard about [valgrind][valgrind] but never used it. I decided to give it a go. You can find the wrong code on my [github repo][github repo].

I compiled the code with a simple ```gcc puzzle.c``` and ran it with valgrind using ```valgrind ./a.out``` and there it was:

```
==23715== Invalid write of size 8
==23715==    at 0x10938A: main (in /home/aditya/Git/aoc_2018/day_3/a.out)
==23715==  Address 0x4a35830 is 0 bytes after a block of size 1,280 alloc'd
==23715==    at 0x4839D7B: realloc (vg_replace_malloc.c:826)
==23715==    by 0x1093CC: main (in /home/aditya/Git/aoc_2018/day_3/a.out)
```

Plain and simple. There is a write outsie the allocated space. It was still hard to figure out why and where. That's when I headed to the freenode IRC channel for C. There someone mentioned using ```-g3``` to compile my code and sure enough:

```
==24391== Invalid write of size 8
==24391==    at 0x10938A: main (puzzle.c:70)
==24391==  Address 0x4a35830 is 0 bytes after a block of size 1,280 alloc'd
==24391==    at 0x4839D7B: realloc (vg_replace_malloc.c:826)
==24391==    by 0x1093CC: main (puzzle.c:74)
```

The line#70 was the culprit. So I was writing to an uninitialized location from here:

```
fabrics[total_fabrics] = fabric;
```

Sure enough, my check to allocate more memory was missing one important thing. I had it set to only reallocate when total_items were more than 32 while my counting started at 0. I was writing to location 32 which is undefined. The fix was simple:

```
if(total_fabrics > (int)realloc_counter) => if(total_fabrics >= (int)realloc_counter)
```

That was it. The code was working again, this time reallocating the buffers as and when required. Happy coding!

[advent of code]: https://adventofcode.com
[day 3]: https://adventofcode.com/2018/day/3
[valgrind]: http://www.valgrind.org/
[github repo]: https://github.com/adibis/aoc_2018/tree/e926cad93a2e9574090d9c7837425e09efd650d4
