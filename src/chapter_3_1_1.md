# Hello World!

As with basically every programming tutorial on the globe, I'll begin where they all do - printing a message that says "Hello World!"

I'd like to quickly note that as this tutorial series progresses, I'm going to be helping less and less and explaining more and more. I don't want to hand-hold you through the entire project, and each lesson will include at least one "challenge", where I give you an objective and tell you to go do it. Self-guided teaching is important, and once you're done this tutorial series, you'll be expected to be at least remotely competent and able to work this stuff out independently. I highly recommend you stick to the challenges and give them a shot, they'll benefit you if you really want to get good with Lua.

Alright, within your text editor of choice, create a new file. Within that file, type the following block:
```
function testing_init()
    out("Hello World!")
end
```

This is our first look at functions, which I'll go much further in detail about next lesson. "testing_init()" is a function, which does the "out("Hello World!") line when it's called. Likewise, "out("Hello World")" is a function of its own, and it prints the text within the brackets to a log.txt file, which CA uses for error-checking scripts and printing out details. We're using it for this first tutorial, but we'll see it many more times as we go on.

Save it, and save as "testing_init.lua" within your modding folder on your PC. Boom, your first Lua file. And you did it *all* by yourself!

Open up RPFM, and create a new packfile. Within the packfile, make the following script directory: "script/campaign/mod". Within the "/mod" folder, add your testing_init.lua file. Save it as whatever you'd like.

I mentioned that the "out()" function is a CA function that prints text onto a log.txt file, but I missed a detail - on retail, that log file is never made. So, what we need to do is go and download/enable the [Script Debug Activator][https://steamcommunity.com/sharedfiles/filedetails/?id=1271877744&searchtext=script+debug+activator] mod, by theSniperDevil, which enables the creation of that log file.

Now go grab your new .pack file, and drop it into Steam/steamapps/common/Total War WARHAMMER II/data (which will here-to-for be referred to as **/data folder**). Enable the .pack file with the mod manager of your choosing, load up the game, and load up a campaign.

In your game folder (Steam/steamapps/common/Total War WARHAMMER II), you should see a new file - script_log_DDMMYY_HHMM.txt. Open that up with a text editor, use find to search for "Hello World". If it's there, it worked, and if it isn't, something went fatally wrong! Go back and try again.

#### Post-Credits Explanations

At this point, I'd like to quickly cover how CA loads and runs script mods for modders. There is a mod-script loader built into TW:WH2, and it's pretty simple for us to use. We can load our mods automatically in different game modes, using the following paths:
* Campaign (Mortal Empires): script/campaign/main_warhammer/mod
* Campaign (Vortex): script/campaign/wh2_main_great_vortex/mod
* Campaign (either): script/campaign/mod
* Battle (excluding quest-battles): script/battle/mod
* Frontend: script/frontend/mod

And lastly, there's one path that is available during all game modes, and loads before any of the other paths (so if you load campaign, and have a script in the following directory, it will be loaded before any in the script/campaign/mod directory). I must warn, though, that this directory should only be used if you're being very smart about what's within, making sure it's all game-mode-agnostic, or making sure you're checking for the game-mode within
* Lib: script/_lib/mod

Within each of these directories (barring the battle one), if you have a function named the same thing as the .lua file (like testing_init from our last example), it will be automatically called. We'll talk more about what this means, and some alternatives to this method that I prefer, but for now we'll stick with it. Remember, if the **function name** matches the **file name** of the script, then it'll be run, except when it's in the /battle directory.

#### Challenge

And time for your challenge. We're going to start from the top. Make a brand new pack, and a brand new script file, with a new name. Have this print out the text "Goodbye World!" on to the log file, as before. This time, however, make it print while in the frontend game mode (post-startup menus to select between Custom Battle/Campaign and what not).