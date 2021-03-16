## Faster Than Light (FTL) 

CODE: [https://github.com/danjaaron/FTL-Hacks.git](https://github.com/danjaaron/FTL-Hacks.git)

[Download the standalone trainer (.exe) here](/FTLScrapTrainer.exe)

FTL is a great indie game in which the player controls a spaceship and flies across the universe while fleeing from a rebel fleet. 

It's sort of like The Oregon Trail in space, with some RTS elements. I highly suggest you check it out.

As the player flies across the universe, they need to stop at stores to buy more fuel as well as weapons, crew members, etc.

Unfortunately I kept dying before I could pick up any of the really cool weapons. 

So, I decided to target the in-game currency with Cheat Engine, to see if I could hack my way into infinite moneys.

This was actually fairly easy and is a great first target for anyone who wants to get started with Cheat Engine.

Below is a demo of the work-in-progress trainer:

<iframe width="560" height="315" src="https://www.youtube.com/embed/XZAXIxBawkE" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

I scripted with Lua in Cheat Engine to make a cheat table, and then a full standalone trainer. 

The tl;dr is that I scanned for the memory address which matched the value of my scrap (currency) while I bought and sold things at the store, then I scanned for pointers to that address, then I disassembled that region in memory, and then I wrote a script to alter the "mov" assembly instruction which I dissassembled. This instruction was called any time my scrap changed, and so I guessed that maybe it was moving an updated balance into my player wallet. So instead of moving the correct value, I edited the instruction to read "mov ...., (float) 99999". This causes my wallet to be maxed out any time I have any kind of encounter which might change the balance, including random encounters as well as interactions with the shop (buying / selling).  

The great thing about Cheat Engine is that it's easy to extend this trainer in the future to add checkboxes for more cheats, which I'll probably do in the future. 

