---
layout: post
title: "Bulls and Cows"
category: code
---

When I started learning C programming back in 2003, the first big program I wrote was Bulls and Cows. I didn't even know the game back then and my neighbor mentioned it to me a good starting exercise. I did it and was exalted and I spent the next week playing it hundres of times. Programming exercise aside, I still think it's a very good game and I play it once in a while when I want to jerk my analytical skills in motion. This time though, instead of downloading one of the existing games from the Play Store, I decided to try my hand at writing the game again - as a webapp.

My intentions were different this time around. I did not want to learn to program the logic behind the game. I wanted to use this as a way to learn to develop mobile ready web based games. Finally, after spending a couple of nights on it, I have the beta version out.

![Main Menu]({{ site.url }}/public/img/bulls-and-cows/bulls-and-cows-01.png)

I am still testing it on all my friends' devices who would let me install it. It looks pretty decent - I think - on my phone. I've been adding features to the basic game to make it a bit interesting. Currently it supports:

- Three different number lengths: 4, 5 and 6.
- Four different levels based on maximum guesses.

I am planning to add more features as I find time. The planned features are:

- Ability to use 0 in the numbers (and allow 0 in starting position).
- A timed game mode with limited time and unlimited guesses.

![Game Over]({{ site.url }}/public/img/bulls-and-cows/bulls-and-cows-02.png)

Entire source code is available under the MIT license on [GitHub](https://github.com/adibis/bullsandcows). The user interface and the game modes still need some heavy testing. Once that is done, I should be able to publish it on the Google Play Store.
