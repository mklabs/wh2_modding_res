# Commands, Interfaces & Listeners

For today's lesson, the subject is three-fold: **commands**, **interfaces**, and **listeners**. We've looked at all three of these before - all in lesson #2 - but in this lesson, we'll be going way further in-depth so you might have a better grasp of all of those subjects.

#### Commands

A command, put *simply*, is a CA-defined Lua function that does some cool stuff that we can't do. We've seen a few already - `cm:transfer_region_to_faction()` and `cm:create_force()`. Commands are used to communicate between the Lua environment and the actual Total War Engine - the base C++ code that defines the majority of the game.

Through these commands, we can script a whole lot of stuff! From character spawns, to new effects applied to characters and factions and armies, to quest battles, to unlocking rites, and so on.

In big part, commands make or break what can be done with scripts. Commands are the driving force of most any script. At this point, though, you might be asking - "how the HECK do I find out what the commands are?" And that, my friend, is why you're reading this lesson. Or maybe you got lost when you were just trying to find funny videos about cats. Either way, next paragraph!

So, once more in the ["Tools & Resources"](./chapter_1.md) page on this site, there's a section within called "Scripting Resources". We'll look at, first, the "Scripting Doc (CM Functions)" file. Open it up!

Now, before we go on, one thing MUST be covered. **EVERY command in this document needs to have a prefix of `cm:`.**

In this document, let's take a look at the two commands that we've used already.

```
    Command: 	transfer_region_to_faction	
    Description: 	Transfer a region ownership to a faction.	
    Usage: 		transfer_region_to_faction("region", "faction")
```

This is relatively straightforward. The `Command:` line tells you what to affix to `cm:`, so this would be `cm:transfer_region_to_faction`. Since it's a *function call*, which we learned about just last episode and you wouldn't dare forget so soon, we add the brackets to it: `cm:transfer_region_to_faction()`.

We can see in `Usage:` that it takes TWO parameters. They seem to be strings, one of which says "region", the other "faction". It's easy enough to tell that means "the region key in string", and "the faction key in string". So, overall, the command call would look like this: `cm:transfer_region_to_faction("phoenix_gate", "eataine")`.

```
    Command: 	create_force	
    Description: 	create a navy or army at a position.  Final parameter specified whether the command goes via the command queue	
    Usage: 		create_force("faction_key", "unit_list", "region_key", x_position, y_position, exclude_named_characters, "id", true)
```

The same applies here. `cm:create_force()`, with a bunch of parameters. You might notice, however, that it's *completely wrong* compared to what we used before. Instead of `"id"`, we used `success_callback`, and there WAS no parameter after that. What the heck?

Well, in this situation, it pays to be careful. I advise you to use the CM Scripting Doc cautiously. It isn't fool-proof, and every change that CA has made over the years hasn't been recorded in there - there are commands listed that haven't been functional since Total War: Empire. This is when I point you back to the [Setup lesson](./chapter_3_1.md), if you haven't done that, and tell you to go do it. There are two hyper important tricks in there that will help you understand what to do in a situation like this:

1) Use the script dump! All you have to do is use "Find In Files ... " within Notepad++, type in the name of the command (ie. `create_force`), and wait for it to search all the files. It will come up with some hits, and you can read through CA scripts to see what they do and how it's used.

2) Use Kailua! It might take some time to fully understand the `ca_types.lua` file that we've made for TW:WH2, but it should be pretty easy to *read* it if you want to know the parameters of a function. Open `ca_types.lua`, within VSCode (or whatever), search for the name of the command again (`create_force`), and it will show you some neat stuff. I'll go over what it looks like now, just so you know:

```
--# assume CM.create_force: method(
--#     faction_key: string,
--#     unitstring: string,
--#     region_key: string,
--#     xPos: number,
--#     yPos: number,
--#     exclude_named_chars: boolean,
--#     callback: (function(CA_CQI))?
--# )
```

Each of these lines is a *parameter*, and it's split into a name and a type. The name is just a description of what should go there - a faction key, a region key, etc., - and the type defines, of course, the type!

There's a third example I'd like to go into, which is return types via the CA commands. It works just like regular return values (seen from the [last lesson](./chapter_3_1_7.md)), but sometimes the return value is vague, and it's really hard to read using CA's scripting doc. Again, utilize the script dump and Kailua. In Kailua, it'll look a little different - let's grab an example of a command with a return value.

```
--# assume CM.get_saved_value: method(valueKey: string) --> WHATEVER
```

*NOTE: `WHATEVER` is a beautiful keyword in Kailua that lets you not define the type of a value, instead of saying "return a number" it says "return literally whatever"*

Some functions in the ca_types files have a `-->` to their end; on the right side of that arrow is the return value. Easy enough to read!

And that's my explanation of CA commands! Hope you had as much fun as I did.

#### Interfaces

Similar to commands, script interfaces are a way to access some game data with some Lua. We looked at them previously - in the second lesson, we used a `FACTION_SCRIPT_INTERFACE`, which we got by using the command `cm:get_faction("faction_key")`.

Script interfaces are objects that we can apply some functions *to* (functions applied to an object are known as methods, but we'll talk about that terminology later). In our previous example, we did the following:

```
    local excess_faction = cm:get_faction(excess_name)
    if excess_faction:is_dead() == true then
```

Wherein, of course, `excess_name` was a local variable a bit further up. In this example, `cm:get_faction()` has a *return value* of a `FACTION_SCRIPT_INTERFACE`, which we assigned to the local variable `excess_faction`. `FACTION_SCRIPT_INTERFACES` have a method called `is_dead()`, which lets us read if the faction is, well... dead. But how did I know to use that? *gasp*

Also similar to commands, script interfaces are documented with a slightly-questionable CA file, though this one is typically waaaay more reliable, if a bit inconsistent with some naming. We can, again, grab it from the [Tools & Resources](./chapter_1.md) page, this time under the title "Scripting Doc (Events & Interfaces). Our big focus for now is the latter half of that name - Interfaces - though we'll be getting into events right in the next section!

