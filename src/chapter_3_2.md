# Listeners

### Can You Hear Me?

A listener is an important script command that, simply, waits for something to happen - or, *listens* for something to happen - in the game, and then triggers something else. Using a listener, we can listen for an event such as the creation of an army, the occupation of a city, or even a battle, and then we use the information from that situation, check for something specific, and run our code if the event matches our criteria. For example: we may be looking for Mannfred to occupy Altdorf. In this case we'd have to use a listener to "listen" for a city being occupied, but we don't want to listen for just *any* city being occupied. In our example, we have to listen for Altdorf being occupied, and for Mannfred doing the occupying. The listener will listen for every single time a city is occupied, and once those two conditions are met - *Mannfred* occupying *Altdorf*... Clue, anyone? - then our listener will trigger some code. In layman's code, our listener will *do a thing*!

Overall, it is a very valuable connection between Lua and game data, and any scripter should understand the in's and out's of a listener.

Let's break down the basics of a listener. Here is it, at its most basic form:

```lua
core:add_listener(
	"listener_name",
	"event_name",
	boolean1,
	function(context)
	end,
    boolean2
)
```

- "listener_name": Can be whatever you want it to be! (Keep in mind if you share the same listener_name as another listener, and that name is called with core:remove_listener("listener_name"), one of those listeners will be removed, and it'd be hard to tell which would be. Use unique names!)
- "event_name": This must be one within a set of Events that CA defines. You can view this [list here][events_and_interfaces]. We'll cover this list more in depth later on.
- boolean1: This is the "conditional" boolean. If it is set to true, the function to execute will execute; if it is set to false, the function will not execute.
- function: This is the "callback" function, which only runs if the conditional boolean returns as true. Can be whatever valid function you want! Note that it MUST have the format of "function(context) --\[\[....\]\] end,"
- boolean2: This is the "persistence" boolean. If it is set to true, the listener will continue listening. If it is set to false, the listener will be removed from the game data.


### CA Events

Just kidding.

Let's talk more in detail about the Events I mentioned.

As previously clarified, Events are predefined objects that are used exclusively in tandem with Listeners. A full list of Events, and all of their methods, and all of the interfaces, can be found [here][events_and_interfaces]. Let's go over how to use this document!

At the top of the document is a long list of Events, each clickable to divert to another part of the document. Each of these Events have specific usage. It should be noted here that documentation on some events is rather light, ie. when to use certain ones versus others, but a lot can be found out by looking at vanilla scripts or other mods. ((NOTE: Flesh out a full Events page to detail each Event))

I want to start a new function after the player takes over Altdorf. After searching the term "Occup", the event "GarrisonOccupiedEvent" is highlighted. This looks promising! After clicking, the available functions are "garrison_residence" and "character", which seem to be the region that was occupied, and the character that led the assault, in order.


### Conditional Coding

Remember "boolean1" from up above? If that is set to true/false, it's not really effective code. We can replace that true/false with a function that returns a true/false value, and that way we can actually check the event, see if a specific thing happened in that event, and then spit out a boolean to trigger the callback function.

```lua
	function(context)
		return context:[thing we want to check]
    end,
```

In essence, the above is the foundation for what all effective conditional functions in a listener should be. Context, in the context of listeners, is an object which contains all the data that occurred within the Event. But let's back up. Remember on the document, we found GarrisonOccupiedEvent? Let's go back to it and click it again. We see two options, as mentioned before - "garrison_residence" and "character". As our intention is to find a specific region being occupied - Altdorf - we need to use that path.

At this point, our WIP code looks like the following:

```lua
	function(context)
		return context:garrison_residence()
    end,
```

For the conditional function, you always want to start with "return context:", and the next method can only be one of the methods that is accessible by that Event - which is where this document comes SO in handy. For other examples, click around the different Events listed at the top. Each "Function" underneath each Event after clicking it is a valid method for the context of that Event. The "RegionSelected" event only has access to a "region" function, which means the only valid method is to use "context:region()". "DilemmaChoiceMadeEvent", however, has access to "faction", "choice", "campaign_model", and "dilemma", so those four are all valid methods.

***NOTE: Yes, you have to add "()" to the end of the function's name.***

Back to our example - you'll see the garrison_residence has "GARRISON_RESIDENCE_SCRIPT_INTERFACE" beneath it. You'll see this, too, is clickable. Much like context, the garrison residence script interface is a big object that contains all the data about that garrison residence. All of the functions listed below it are valid methods, meaning we can continue our chain of information! Our goal is to find a region with the key "wh_main_reikland_altdorf", after all.

Looking in the "GARRISON_RESIDENCE_SCRIPT_INTERFACE", I don't see a "key" or "name". Matter of fact, the most likely path is "region()". That brings our code to...

```lua
	function(context)
		return context:garrison_residence():region()
    end,
```

After checking the `REGION_SCRIPT_INTERFACE`, we finally see the "name" method, which allows us to fully check our context. Our line of code is finally built to:

```lua
	function(context)
		return context:garrison_residence():region():name() == "wh_main_reikland_altdorf"
    end,
```

But that's not all! All this does is to check if the Altdorf region is being occupied, but it says nothing about Mannfred! We'll go back to the very beginning - find the GarrisonOccupiedEvent on the document, and return to its functions, which were garrison_residence() and character(). We want to build an "and" sentence for our conditional, so that it only triggers the function when Altdorf is occupied **AND** when it's occupied by Mannfred. Let's try to plot our trajectory a little. We should try to find a script_interface that allows us to reference an agent_subtype key, which is the cleanest and most mod-friendly way to search for Mannfred. Looking at CHARACTER_SCRIPT_INTERFACE, I see two functions - character_subtype() and character_subtype_key(). The latter is used to return a string - ie., it doesn't check for a specific subtype key, but it will tell us what the characters subtype key is. We need to *check* for a specific subtype key, "vmp_mannfred_von_carstein".

```lua
	function(context)
		return context:garrison_residence():region():name() == "wh_main_reikland_altdorf" and context:character():character_subtype("vmp_mannfred_von_carstein")
    end,
```

Note here that the first line of context, that ends in ":name()", needs to have an equal-to check because we don't want to return the *name* of the region, but we want to return a value of true or false if the name of the region is equal to the string we define - "wh_main_reikland_altdorf".

Further note, one might be wondering what the "return" part of this whole deal is. Essentially, a return statement stops the current block of code - ie., the function(context) - and, if something is designated, it returns a value. In this case, it is returning a boolean, which is how we managed to turn an at-first boolean into a function.

A strong conditional function is very important to writing good listeners, as listeners can be very performance-heavy if there are many triggering, and the more that are whittled away via a strong conditional, the better performance will be. Be mindful of your conditionals!

### Callback Functions

At this point in the walkthrough, we have about 60% of our listener completed! It's not bad, let's take a quick look at what we actually have written:

```lua
core:add_listener(
	"MannfredOccupiesAltdorf",
	"GarrisonOccupiedEvent",
	function(context)
		return context:garrison_residence():region():name() == "wh_main_reikland_altdorf" and context:character():character_subtype("vmp_mannfred_von_carstein")
	end,
    function(context)
        -- code to execute
    end,
    boolean2
)
```

I'd like to point out a couple quick notes. To start off - yes, your listener_name can be anything you want, as long as it's a string. You need a comma after *every argument* in the command, of which there are five that are detailed way at the top - listener_name, name, conditional boolean, callback, and persistence boolean. The code above is written the way it is because that's a common application of it, and allows it to be easily read, but realistically this can all be written on one line. In no world is that optimal, though.

Alright, so we are on the function to execute block, also known as the callback function. Here, we have much more freedom than the conditional function, as we can do whatever is needed with this block. For instance, we can have a callback function that triggers an event (like a dilemma or a message), or grants a unit/experience/ancillary, or unlocks something like a Rite or a Legendary Lord. For our instance, I want to just give Mannfred's army a new unit and a new effect bundle.

***NOTE: For our sake, we're going to assume the new unit and new effect bundle are already defined.***

Looking up in our [commands document][events_and_interfaces], I see a grant_unit_to_character to command. Checking vanilla usage, I see it takes two args - a char_lookup_str and a unit_key string. The unit_key string - which refers to the main_units_tables - is, in our situation, "tutorial_big_bat". To find a char_lookup_str, we have to first find the character's cqi.

*For more information on char_lookup_str and cqi, go to [page link here that doesn't yet exist]*

Thankfully for us, we can go back to where we were before and use the context of the Event to find all the info from before. Looking at the "CHARACTER_SCRIPT_INTERFACE", we see a direct link to cqi, with the method "cqi()". For this, we define our local values as such:

```lua
	local char_cqi = context:character():cqi()
    local char_lookup_str = cm:char_lookup_str(char_cqi)
```

With this, we find the CQI first of the character, and then apply that to find the character's lookup string, which is the type needed for grant_unit_to_character.

Before we apply that command though, we should find the arguments we need for apply_effect_bundle_to_force(), which is our command that will give an effect bundle to Mannfred's army. We first need our effect bundle; we'll use "tutorial_free_hair". The second argument is a military force CQI, and our third argument is a number of turns. We'll make it five turns long.

To find the military force CQI, we use something similar as above. We will go from character to military force, and then find the CQI there. We can use something like the following:

```lua
    local mf_cqi = context:character():military_force():command_queue_index()
```

Note that, in this situation, we couldn't use "cqi()". That's one reason why it's so important to check vanilla application and the documents. If we used "cqi()", it would've caused the listener to abort.

Alright, now we have all we need to build our callback function! Let's mush it all together real quickly.

```lua
function(context)

	local char_cqi = context:character():cqi()
	local char_lookup_str = cm:char_lookup_str(char_cqi)
	local mf_cqi = context:character():military_force():command_queue_index()
	
	cm:grant_unit_to_character(char_lookup_str, "tutorial_big_bat")
	cm:apply_effect_bundle_to_force("tutorial_free_hair", mf_cqi, 5)

end,
```

And that's a quick walkthrough on how to assemble your callback function! Please note, you can have whatever you want here, and there's nothing saying you *must* restrict yourself to the context of the event. As an example, I can do the same thing without context, like so:

```lua
function(context)

	local mannfred_faction = cm:get_faction("wh_main_vmp_vampire_counts")
	local mannfred = mannfred_faction:faction_leader()
	
	local char_cqi = mannfred:cqi()	
	local char_lookup_str = cm:char_lookup_str(char_cqi)
	local mf_cqi = mannfred:military_force():command_queue_index()
	
	cm:grant_unit_to_character(char_lookup_str, "tutorial_big_bat")
	cm:apply_effect_bundle_to_force("tutorial_free_hair", mf_cqi, 5)

end,
```

This takes a little bit longer than using context, but it is a solution to applying commands to objects that aren't involved in the context. So if, for example, you wanted to give all Vampire factions aside from Mannfred's own an `effect_bundle` that increases their diplomatic relations with Mannfred after he conquers Altdorf, you can. The world is your oyster when it comes to listeners.

### Persistence

One last thing, and it's gonna be much quicker than the last two. The final boolean - persistence - can be set to true, or false. True means that the listener will fire more than once; false means that, once the entire code is run through, the listener is removed from the game state and won't be called again. This only gives two options - once, or infinite. There is a third option, by using core:remove_listener("listener_name"). At whichever point this code is called, the listener with the matching "listener_name" is removed from the game state, and won't be called again. A remove_listener statement like this can be used after enabling a counter that listens for the listener firing five times, or to cancel the listener if the turn is beyond 100, or much else.

Keep in mind that, to "fire", only the conditional needs to return true. If there's more if statements in your callback function, and they fail, but your persistence is set to false, the listener will remove itself.

```lua
core:add_listener(
    "my_listener", 
    "GarrisonOccupiedEvent", 
    true, 
    function(context) 
        if this_is_done then
            --stuff happens
        end;
    end, 
    false
)
```

So this listener will cancel after one use if "GarrisonOccupiedEvent" is triggered, regardless of whether or not "this is done" returns as true or false.

[events_and_interfaces]: https://cdn.discordapp.com/attachments/531219831861805069/569627247217082387/scripting_doc.html