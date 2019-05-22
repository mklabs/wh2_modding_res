# Fun Stuff

For our second lesson, we're going to go right into something more complicated, and I'll give you a few challenges within this. I'll voice this note right at the beginning: **you won't understand every single thing I use here**, and that's literally the point. This second lesson is a quick, semi-complicated script to show you some of the possibilites of scripting, with some intro knowledge on new concepts, and the next few lessons following will go right back into the bare basics.

First up - challenge time!

#### Challenge 1

Your first challenge is to make a new .pack file, with a single Lua file named "example_script.lua" that will load exclusively in the Eye of the Vortex campaign. Create a blank function within the Lua file, named as we addressed in the last lesson.

#### What We're Doing

The project here is to build a slightly solid script that affects a good amount of change. What we'll have by the end of this lesson is a script that gives Lothern control over the Phoenix Gate at the start of a new campaign, and will spawn a new army automatically when the Cult of Excess faction is destroyed.

#### Transfer Region on Campaign Start

First thing's first, let's grant the region to them! It's important to note *when* we call *what* in our scripts. For things we want to do only once, on game start, we can use a nice CA command, `cm:is_new_game()`. All that command does is check if it's the very first turn, and the game hasn't been saved yet. We'll use that within an "if" statement, as follows:

```lua
function example_script()
    if cm:is_new_game() == true then
        -- it's a new game, so it'll run this stuff here
    else
        -- it isn't a new game, so it'll run this stuff here
    end
end
```

You might be wondering the point of those "--" bits. Those dashes - two are necessary - allow you to comment your script, which I will be doing quite a bit and try to make a habit of with all scripts. Everything on that line after the "--" is ignored, and allows you to write whatever you want. It's really helpful when you go and look back over very old scripts and are wondering why the heck you did something a certain way. I've been there many times.

You might be wondering about the "if cm:is_new_game() == true then" bit, too. More or less, when you use the command "cm:is_new_game()", it returns a value of true/false, depending on whether or not it's currently a new game. We can read that value, like I show above, and go down different branches of our script depending on the answer.

In a future tutorial, I'll be showing how to find these commands and use them, but for now, blind faith will make this tutorial go quicker and easier.

On our "if cm:is_new_game() == true" branch, we'll be transfering the region over to Lothern. The command we need to use is `cm:transfer_region_to_faction(region_name, faction_name)`. region_name and faction_name are both database keys, easy enough to find using the Assembly Kit.

```lua
function example_script()
    if cm:is_new_game() == true then
        -- it's a new game, so give Lothern the Phoenix Gate region
        cm:transfer_region_to_faction("wh2_main_vor_phoenix_gate", "wh2_main_hef_eataine")
    else
        -- it isn't a new game, so it'll run this stuff here
    end
end
```

#### Challenge 2

This one's pretty simplistic, but it'll get harder, don't worry. For now, I would like you to give Lothern a second new region, and to output some text if it isn't a new game.

#### Are They Dead Yet?

Alright, next step - we need to create something that checks if the Cult of Excess faction is dead, before we create the thing that'll spawn the new army.

And no, I won't give you the answers to that challenge above! Figure it out, chap!

First, a new concept - listeners. One of the lessons very soon, after the basics, goes over listeners in big depth, but suffice it to say: listeners are love, listeners are life. They allow you to "listen" for different things happening in the game - like a battle, a region being captured, a character dying - and to "trigger" a function afterwards. It's the mechanic that powers things like Legendary Lord/Rite unlocks, the Chaos Invasion in big part, post-battle Chivalry or Infamy gains, and a whooole lot more!

We're going to use a listener that "listens" for Lothern's turn to start, and it'll *check* to see if the Cult of Excess faction is dead-o-rama. If they are, it'll *trigger* the spawn of the new army.

```lua
function example_script()
    if cm:is_new_game() == true then
        -- it's a new game, so give Lothern the Phoenix Gate region
        cm:transfer_region_to_faction("wh2_main_vor_phoenix_gate", "wh2_main_hef_eataine")
        -- the second region you should've added in the challenge!
    else
        -- it isn't a new game, so it'll run this stuff here
        -- the output you should've added in the challenge!
    end

    core:add_listener(
        "MyFirstListener",
        "FactionTurnStart",
        function(context)
            -- is it Lothern's turn?
            return context:faction():name() == "wh2_main_hef_eataine"
        end,
        function(context)
            -- check to see if the Cult of Excess is living!
        end,
        true
    )
end
```

Again, don't fear - I'll cover all this in way more depth, and this is the last time I'm gonna give this warning or clarification. Just accept it.

To check if they're dead yet, we have to *get* the faction object, and run a quick check on if they're living or not. We can do that with the CA command `cm:get_faction(faction_name)`. That returns what CA calls a "FACTION_SCRIPT_INTERFACE". You can run a lot of functions on a faction object, a "FACTION_SCRIPT_INTERFACE", as one of them is `is_dead()`. You can see more if you go to the scripting-doc.html file in Tools & Resources, and search for "FACTION_SCRIPT_INTERFACE".

```lua
function example_script()
    if cm:is_new_game() == true then
        -- it's a new game, so give Lothern the Phoenix Gate region
        cm:transfer_region_to_faction("wh2_main_vor_phoenix_gate", "wh2_main_hef_eataine")
        -- the second region you should've added in the challenge!
    else
        -- it isn't a new game, so it'll run this stuff here
        -- the output you should've added in the challenge!
    end

    core:add_listener(
        "MyFirstListener",
        "FactionTurnStart",
        function(context)
            -- is it Lothern's turn?
            return context:faction():name() == "wh2_main_hef_eataine"
        end,
        function(context)
            -- define a local variable that can be used to shorthand the faction's name
            local excess_name = "wh2_main_def_cult_of_excess"

            -- define a local variable to access the FACTION_SCRIPT_INTERFACE for Cult of Excess
            local excess_faction = cm:get_faction(excess_name)

            -- check if they're dead, and if they are ... 
            if excess_faction:is_dead() == true then
                -- ... spawn that army!
            end
        end,
        true
    )
end
```

Sweet! This is our first look at variables and the keyword "local", which defines where that variable exists and can be accessed. More on this later on!

#### Spawn That Aaaaarmy!

We're coming up close to being done here! All we've gotta do left is spawn the army, and all I've gotta do is challenge you a couple more times!

The next and penultimate CA command we'll be using is `cm:create_force(faction_key, army_list, region_key, x_position, y_position, exclude_named_chars, success_callback)`. The parameter list looks slightly daunting, but if you take some time to critically think, the only strange thing should be the very last parameter - success_callback. Luckily for you, we won't even be using that one, really.

```lua
function example_script()
    if cm:is_new_game() == true then
        -- it's a new game, so give Lothern the Phoenix Gate region
        cm:transfer_region_to_faction("wh2_main_vor_phoenix_gate", "wh2_main_hef_eataine")
        -- the second region you should've added in the challenge!
    else
        -- it isn't a new game, so it'll run this stuff here
        -- the output you should've added in the challenge!
    end

    core:add_listener(
        "MyFirstListener",
        "FactionTurnStart",
        function(context)
            -- is it Lothern's turn?
            return context:faction():name() == "wh2_main_hef_eataine"
        end,
        function(context)
            -- define a local variable that can be used to shorthand the faction's name
            local excess_name = "wh2_main_def_cult_of_excess"

            -- define a local variable to access the FACTION_SCRIPT_INTERFACE for Cult of Excess
            local excess_faction = cm:get_faction(excess_name)

            -- check if they're dead, and if they are ... 
            if excess_faction:is_dead() == true then
                -- define a few local variables that we'll use in the command to save some space!
                local army_list = ""
                local region_key = "wh2_main_vor_straits_of_lothern_lothern"
                local x_position = 516
                local y_position = 430

                -- ... spawn that army!
                cm:create_force(
                    excess_name,
                    army_list,
                    region_key,
                    x_position,
                    y_position,
                    true,
                    function(cqi)
                    end
                )
            end
        end,
        true
    )
end
```

Those of you critically-thinking will notice that I kept a completely blank string for `army_list`. Guess what!

#### Challenge 3

Create the army list! It's one single string, with unit keys separated by commas, NO spaces within the string. Give it a shot, I dare you.

HINT: If you're hitting a wall, global search CA scripts! I won't tell you **what** to search.

#### Packaging and Testing

Alright, with the `army_list` string expanded, this mod should be fully functional. There is one definite bug that I know will exist, and I'll point out now - if there's a character already standing on (516, 430) game coordinates, the army will not spawn. Explaining how to avoid so would take more time than I want to spend on this lesson, so deal with it.

Your final task - start testing! If it doesn't work, here's a quick tip: use the out(text) function! You can use it throughout scripts to take a look at what's triggering and what isn't, like so:

```lua
    if cm:is_new_game() == true then
        -- it's a new game, so give Lothern the Phoenix Gate region
        out("It's a new game!")
        cm:transfer_region_to_faction("wh2_main_vor_phoenix_gate", "wh2_main_hef_eataine")
        out("Added Phoenix Gate")
        -- the second region you should've added in the challenge!
        out("Added the second region!")
    else
        -- it isn't a new game, so it'll run this stuff here
        out("It's not a new game!")
        -- the output you should've added in the challenge!
    end
```

And that's it, yo. Next up, we'll get down to the bare bones and get some great understanding! If some of this stuff didn't make sense, keep pushing on through the next few lessons where I break it down to the bit and hopefully give you a more fuller knowledge on the language. I gave you a glimpse at some story-telling; now let's go back and learn the alphabet, grammar, and colloquialisms.