---
title: TJCTF - Gamer W challenge writeup
date: 2020-05-27 11:41:00 +0530
categories: [Technical, CTF-writeups]
tags: [ctf, writeups, wasm-game, cetus-extension]
---

![TJCTF 2020](/assets/img/posts/tjctf-gamerw-writeup/TJCTF.png "TJCTF2020")

This is a writeup for the `Gamer W` challenge in the recently held [TJCTF 2020](https://tjctf.org/). 

![Challenge Question](/assets/img/posts/tjctf-gamerw-writeup/chall-question.png "Gamer W")

The challenge required us to cheat in a web assembly game using the [Cetus](https://github.com/Qwokka/Cetus) browser extension.

![Challenge Image](/assets/img/posts/tjctf-gamerw-writeup/chall-img-1.png "Game Image")

I cloned the extension repository from GitHub and loaded it in chrome using developer mode.

> **Note:** Instructions to setup the extension in either Chrome or Firefox can be found in its GitHub README or Wiki.

As soon as we enter the game, we are given instructions on how to move our character (<kbd>W</kbd>, <kbd>A</kbd>, <kbd>S</kbd>& <kbd>D</kbd>) and attack (<kbd>X</kbd>&<kbd>C</kbd>). We are also provided with a shop on the home screen which allows us to upgrade the traits of our character in exchange for gold coins.

![Challenge Image](/assets/img/posts/tjctf-gamerw-writeup/chall-img-2.png "Game Shop")

I went ahead and played the game without thinking of how to cheat. I killed 4 skulled goons and then the main boss appeared. He was impossible to kill, as he spread out his attacks (impossible to dodge) and I was dead before long. 

So, now I had to find a way to cheat and defeat the final boss.

By looking at the game, we can figure out that there are two main variables which we can modify to cheat:
- The Health variable
- The Gold Coins variable

Using Cetus's search feature, I searched for the addresses storing the values of health and gold coins.

![Challenge GIF](/assets/img/posts/tjctf-gamerw-writeup/chall-health-search.gif)

I froze the value in the address storing the health and gave myself a whole lot of gold coins by changing the value in the address holding the amount of gold coins. Now that I was wealthy and invincible 😈, I maxed out my character from the store and went to face the boss again.

![Challenge Image](/assets/img/posts/tjctf-gamerw-writeup/chall-max.png "Maxed out Shop")

However, as I was getting close to killing him, the boss drank something (a potion), which gave him regenerative health. If I was somehow able to give him a hit before the potion took its effect, he teleported and put me behind a barricade, making it impossible for me to reach him.

This is the place where I was stuck for a long time, I understood that I had to do two things:
1. Be able to kill the boss even with his regenerative potion
2. Kill him before he teleports out of my reach

I tried a few things like checking whether there was a way to change the position coordinates of my character, getting hold of the address that stored the health of the boss, using the speed hack in Cetus to attack quickly, etc. None of them worked out, so I started looking for other things I could manipulate.

I looked at the items in the store again and realized that if I could change the `swing damage` from my weapon to a very high value, I could kill the boss quickly even with his regenerative powers! Using Cetus' search feature again, I got hold of the address storing my weapon's swing damage value and increased it.

![Challenge Image](/assets/img/posts/tjctf-gamerw-writeup/chall-cetus-addresses.png "Cetus addresses")

>**Note:** The value of swing damage resets if we enter the shop. So after manually changing its value through Cetus, don't enter the shop.

Now when I went and faced the boss, I could kill him and the game ended.

![Challenge GIF](/assets/img/posts/tjctf-gamerw-writeup/chall-play.gif)

I was presented with the flag for the challenge at the end screen.

![Challenge Image](/assets/img/posts/tjctf-gamerw-writeup/chall-flag.png "Challenge Flag")