# Building Buildings

### So you want to build a building? 

Buildings are a great feature in the game, and editing them can allow for a lot of fun new mechanics! New building chains can allows new units, new effects, and new landmarks. The best news of all is that most ideas with building chains are possible.

### Vocabulary

I want to first cover some vernacular that we will be using throughout this tutorial. These terms are conceptual but critical to understanding what Creative Assembly is manipulating in their game files.

- **Key**: A key is an identifier, a definition point in the database.
- **Id**: An id is similar to a key in that it is an identifier, except it is a randomly generated number used in certain tables of the database.
- **Level**: A level is each individual building that you can construct in the game. They can be one-off or individual, or they can be upgraded into other buildings. The Empire tier 1 barracks, tier 2 barracks, and the tier 3 barracks are each a level.
- **Chain**: A chain is much like it sounds - a direct chain of buildings. In the images, below. each vertical set of buildings that can be upgraded is a chain. The tier 1 barracks, the tier 2 barracks, and the tier three barracks are each a level.
- **Building Availability Set (BAS)**: A building availability set is the link between building chains and the faction/culture/subculture that can build them.
- **Superchain**: A superchain can be seen as a collection of various chains from different races/factions, which can save data space when linking to slot templates (the only way to link buildings to locations). For instance, all settled factions except for the Wood Elves (due to their unique building style) have their standard infantry building linked to the `wh2_main_sch_military1_barracks` superchain, which is available at all settlements. It consolidates the entirety of your buildings into one package.
- **Set**: A set is more or less a visual effect that conveys a category. As you'll see in the images below, "Military Recruitment" and "Infrastructure" are examples of buildings sets, which lump together various different building chains into a specific group.
- **Slot Template**: The database (db) classification of various locations, and the union between buildings and regions. Slot templates allow you to lock buildings to specific locations (for example, landmarks). However, ***not every settlement has a unique slot template.*** For instance, Altdorf has the slot templates `wh_main_special_altdorf_primary` and `wh_main_special_altdorf_secondary`, while Bibali has the basic "wh_main_human_minor_primary_coast" slot template. As we see in an unmodded game, Altdorf has unique buildings in, while Bilbali does not, so we can infer that Creative Assembly had no need to add in a custom slot template for the latter.

![test][picture_1]
![test][picture_2]

### Getting Started

First off, you'll want to get your idea(s) in order. You want to make a building, but what will this building do? Where can it be built? Is this building locked to certain cultures/factions? How many levels will it have? Having an answer to each of these questions will help you carry on through the tutorial.

Now, you'll want to open up Rusted Pack File Manager (RPFM) and the Assembly Kit. RPFM is where you'll be peforming all of your work - the Assembly Kit is more of less going to be used as a glorified search engine.
 
Open up RPFM, go up to Packfile -> New Packfile. Right click this new packfile (called unknown.pack) -> Add -> Add from Packfile.
The "Select Packfile" window will open, go to wherever you installed Warhammer 2 -> click on the data folder -> and then find and select the data.pack to open.

On the right hand side, the "Add from Packfile" Mode will open, and you'll see a file hierarchy of the different folders and tables within the pack. Look for the folder marked "db" and expand it. There will be a list of subfolders. Scroll through, expand the following subfolders and add the following tables from the data.pack:
- building_chains_availability_sets_tables
- building_chains_tables
- building_culture_variants_tables
- building_effects_junctions_tables
- building_instances_tables
- building_levels_tables
- building_set_to_building_junctions_tables
- building_superchains_tables
- building_units_allowed_tables (Only needed if you will be linking units to the building)
- building_upgrades_junction_tables (Only needed if you will have more than one level)
- slot_template_to_building_superchain_junctions_tables

Once you have added each table, click the "Exit 'Add from Packfile' Mode" at the top center of the screen.

You will also have to add a few files for the localisation of the building mod, so let's do that now. As before, right click the packfile and add from packfile, this time grabbing the local_en.pack to open.
Scroll through and add the following tables from local_en.pack:
- building_chains__.loc
- building_culture_variants__.loc
- building_short_descrptions_texts__.loc

That's it for importing! In the future, you may need more tables such as effects if you're going in to that, however this tutorial will not be covering that action.

### Inputting Data

The very first thing that I recommend is to go into the building_superchains table and delete all rows. If you have not changed around any hotkeys (which you can do by the way by going to Packfile -> Preferences), you can delete all rows in the table by clicking an entry in the table -> `CTRL+A` to select all in the table -> `DELETE`. 
 
 Let's add in a new row by pressing `CTRL+SHIFT+A`. From here, we can add in your new key. It can be named whatever you want, but we advise that you rename it to something that's not already a vanilla key or looks like it could be a vanilla key. You don't want to assume vanilla keys, as it will make the game freak out. Depending on what I'm making, I either prefix with vandy_ or prussian_, but you could go with whatever works for you. **Just don't assume vanilla keys.**

 I recommend writing an easy key, one that you can copy over through various tables and use interchangeable. If I were to make a new horde building for the Empire, I would probably call it "vandy_empire_horde_main" if it were the primary building, or "vandy_empire_horde_barracks" if it were the barracks. You could also easily do "empire_main" and "empire_barracks", but keeping a unique prefex on all of your work helps with organization.
 
 After that, go the slot_template_to_building_superchains_junctions. Don't delete anything just yet! Here is where we actually want to link the building we are making to a location or locations. There are three columns we want to worry about: building_superchains, id, and slot_templates.
 
 **Note:** If the building or building chain is not in here, then it will not show up in any settlements.
 
 If you are making this into a horde building, you would assign it to the horde_primary slot for the main building, or horde_secondary for any others.
 
 If you are making this into a settled building, you would have to have to input your building to all the different slot_templates. The four main slot_templates are 
 1. wh_main_human_major_primary (a province capital's main building)
 2. wh_main_human_major_secondary (any other building in a province capital)
 3. wh_main_human_minor_primary (a regular settlement's main building)
 4. wh_main_human_minor_secondary (any other building in a regular settlement)
 
Remember, there are also unique slot_templates for all different resources, all unique locations, several new resources made for Warhammer 2, and some older and antiquated slot_templates such as "wh_main_dwarf_orc", used for the Regional Occupation system in Warhammer 1 that is currently unused in Warhammer 2.

I'll let you in on a little secret - typing out all the slot_templates is unnessary and tedious. Open up the Assembly Kit real quick, go into the slot_template_to_building_superchains_junctions table and open up the Search function. Change the column filter to "building_superchain" and type in "wh2_main_sch_military1_barracks." This will only show the junctions between the barracks superchain and all slot templates that the barracks superchain is available in. Thus, this superchain includes all beginner military superchains for each race - such as the Cemetary for the Vampire Counts. Grab the entire Slot Templates column from the Assembly Kit and paste it into the corresponding table in your mod back in RPFM. Secondly, take your building_superchain key and paste it in each entry of the column. 

One last step - we need to assign unique id numbers to each of these entries. Now, Creative Assembly used random generated id numbers for their entries, and there are hundreds of thousands of entries within these game files. Thus, you probably can't use 1 or 10 or 100 for any of these entries. I recommend being safe and using 8 digit (1906015) id numbers for each superchain entry.

If you did this right, this action should now apply that superchain's availability to each and every settlement on the map. You'll have to repeat this step for each new building chain. 

**Note:** There is only the need for one superchain for a link of buildings you can upgrade, but you will need one superchain for each separate link of buildings.

### Building The Building

Now that the boring part is done, we can move into the nitty-gritty! Next step is in building_chains_tables. Most of this table does not actually do much, yet we still need it. Again, `CTRL+A` to select all, then `DELETE` to delete all previous entries in this table. There are 9 coluymns in this table but we are only going to be worried about 3 of them: key, chain category, and building superchain. In the first column, "key", input a unique key for your building chain. I recommend using the same key as the superchain, but you do not have to! Skip over to chain category. For this column, you're either going to enter in "military" or "money" as an entry. Ask yourself: Is this building or building chain that I am making going to be used for recruitment or combat capabilities? If so, enter in "military", otherwise input "money". Lastly, enter in your building_superchain key in the building superchain column. Voila, another table completed!

Next up, you want to make a new_building_instances key. This table determines how many of a certain building we can have in a settlement. Delete all those previous entries, make a new row. You're going to want to input your building_chain key in the first column. For the second column, "1" is the most common value, but if you wanted to add in a set of, say, three buildings, and allow the player to only choose **two** of them at once, you would enter "2". Keep in mind that this is only for a **single settlement**, not an entire province.

After this we will click on over into the building_levels_tables, which will be each individual link in the building chain. Much as a Cemeetary upgrades into a Graveyard, you can have your building upgrade into another building.

**Note:** If your building only has one level, like a landmark, you still need this table.

For the "level_name", type in your building_chain name with the sufix "_1", and increase it for each further step in the chain. The "chain" column should have you put in your unique building_chain key. For "level", enter 0 for the first level, then increase by one as you go up the chain. Every building chain MUST start with a building at level 0, this does not mean that you can build your building when the settlement is a ruin! From here, a lot of stuff is just details - how many turns does your building take to build, how much does it cost to construct, does your building have an upkeep cost (which is a negative value taken every turn from your treasury), and a whole string of things that are no longer used for this game. 

After upkeep, scroll to "can_convert" and put a check in each of your building entries. Without this, the buildings cannot be destroyed. Insert the building_instance_key that you made earlier into the "building_instance_key" column. Scroll over again to the "can_be_damaged" and put a check in each of your building entries, so that your building can be damaged after bring razed or sacked.

***Note:*** The Development Point Cost column is a modifier to make the building cost Population Surplus. This can be used for any building, but it is seen in the game through main settlement and horde buildings only.

We are not done yet in here! The next column, Primary Slot Building Building Building Level Requirement, determines what level the main building needs to be at to construct this building. If you input "1", then this building can be constructed at a Tier 1 settlement. Conversly, if you input a "3", then this building can only be constrcuted at a Tier 3 settlement. Each separate level needs to have a different primary slot requirement! Lastly, check off each entry in the Visible in UI column and head on over to building_culture_variants_tables. 

This is where you build what your building looks like - its name, its icon, its description. You can use this table to change how it looks between cultures/factions as well. 
For now, however, delete all previous entries like usual, add a new row, and input your building chain entries in the Building column, so i.e. "vandy_empire_barracks_1" to "vandy_empire_barracks_3". Next, enter the culture, subculture, or faction key of the group you're making this for in each entry - use the Assembly Kit to search for that! For instance, my building was for the Empire, so I entered "wh_main_sc_emp_empire". The Description column is not used, just put in "wh_main_PLACEHOLDER". For the Artpiece column, enter either the name of the vanilla icon or import your own. The UI location is ui/buildings/icons/%.png. At the end, check off for each entry the display_tooltip.

**Note:** This table needs either a "faction" entry or a "culture" entry. You can use both, but it needs at least one to function.

### Only a few more db tables to go!

From here, we have to make this building do stuff, link it to who can built it, and edit where it shows up in the buildings panel, finishing off with flavor text for all of it in game.

Go to building_chain_availability_sets_tables. **(You should know by now to clear out these existing entries and making new rows for your own entries by now, so I'll stop saying it)** This table is used to link the bulding to who can build it. The first column we input, you guessed it, the building_instances key, so i.e. "vandy_main_empire_barracks". Continuing with the example, I am making my buildings for the Empire, so I enter "wh_main_bas_emp" for the second column. If your building is not supposed to be bult by the Empire, the Assembly Kit will help you find the availability set you want to use.

**Note:** If you wanted to, you can make your own custom BAS, but it is not always required unless you're making a new culture or subculture. if you tie your building to an existing BAS - say, wh_main_bas_emp but then only give a culture variant to the Midenheim faction, then only Middenheim will be able to build it. This is very useful for excluding certain factions from buildings and much more flexible (and simpler) than using a custom BAS.

Let's now open up building_set_to_building_junctions_tables. This is where the building will show up in the Building Panel in-game, whether it's in "Military Recruitment", "Military Support", "Infrastructure", or "Defense." Building sets are culture specific, so once more let's open up the Assembly Kit and look in this table for what building set you need. I entered my building in "wh_main_set_empire_military_support", so it will show up in-game under the Military Support category in the Building Panel. Find your key and enter in the new building. I recommend entering the building_instances key instead of the building_levels.

### Whoo-hoo!
**You now have a useless icon that costs money**

Let's give it something to do - go into the building_effects_junctions tables, and let's add in an effect. My building for this example is going to add +10 Public Order for this tutorial (even though we've been running with a barracks example... -pw), so I will go back to the Assembly Kit and search for a public order effect in the vanilla buildings and see how they add it.

Looks like I have to add my building in here, then use the effect key "wh_main_effect_public_order_base" under the Effect column, link "province_to_province_own" in the Effect Scope column, and add in my own value. I'm giving it +10 Public Order, so I'll enter "10" in for the value. The Value Damaged and the Value Destroyed columns convey the amount of effect to give when the building is damaged and when the building is ruined, respectively.

### Localisation, or fleshing out what the heck you made

Localisation files can be annoying and fickle, so let's get through this as smoothly as we can. Open the building_culture_variants.loc and make sure you add one new row for each "level" you made after you cleared out the previous entries.

**The format needs to be as follows:** "building_culture_variants_name_[name_of_your_building_1][culture][subculture][faction]"

Whatever culture or subculture or faction you linked in the culture_variants table NEEDS to be formatted properly here or it will not pop up.

**My entry would look like this:** "building_culture_variants_name_vandy_main_test_building_1wh_main_sc_emp_empire"

In the localised string, I input "Test Building." This will be the name of each individual level. In building_short_description_texts.loc, you ONLY ned the name of the building, and the description you want to pop underneath the name. I input "building_short_description_texts_short_description_vandy_main_test_building_1", with a localised string that says "This is a test building!"

Lastly, in building_chains.loc we can enter the chain name that pops up under the name of the building. I input: "building_chains_chain_tooltip_vandy_main_test_building_1" with the localised string "Testing!" for the entry.

### You Did it!
![test][picture_3]

Your first building or building chain is completed! Go on, pat yourself on the back. There are plenty of creative options with buildings - you can introduce penalties and consequences to new building chains, or create interesting links of, say, two different buildings that can both get to level 3, but you can only get one to level 4 or 5. It is possible to link units to buildings, either vanilla or custom; link new technologies, and even create band new landmarks.

### Troubleshooting

Something happened and now you are getting a crash.That happens and it's okay, we can fix it. There's a couple things that you can check off the list to figure out where it's coming from.
1. Typos in the .loc files, or the absence of them, will not cause a crash. It just means that the text that you wrote will not show up.
2. Typos in the data/tables will **100%** cause a crash. If you reference a non-existent key in one of the above tables, like linking a misspelled superchain to the wh_main_human_major_secondary slot template, the game will crash on start-up. This is because when the game is loading up the data, it cannot establish or identify that non-existent key in the ways it knows how and cannot continue.
3. Missing information can sometimes cause issues, for example, if you did not link a culture_variant for your levels.
4. Empty rows! This is a fun one that can also cause unintended results. Make sure you go through each table and delete any rows that you do not need.
5. Did you assume a vanilla key would work before verifying it exists? The Total War Franchise was developed and coded by people, and if we know anything about people, it's that consistency is more an ideal than a standard at times. Naming schemes, formats, and even data entries themselves have been changed and removed from patch to patch. Go back and verify that effect scope from the Assembly kit, it could be the issue.
6. Please rename your tables and your mod. There is no reason to not rename your tables and your mod, and problems can ensue otherwise. Oh, and please make sure you are using unique id's for these tables! If you use vanilla ones/id's from other mods, then you're ensuring incompatibility.

Thanks for reading!


[picture_1]: images/2_1-01.png
[picture_2]: images/2_1-02.png
[picture_3]: images/2_1-03.png