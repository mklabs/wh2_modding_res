# Tolerant Tables

##### What's a tolerant table?

**A tolerant table is an arbitrarily defined table that will not crash when a *referenced* key is missing in a certain column.** This is particularly relevant when you are overriding new content from another mod and it turned out that you do not need to port two or more tables just to provide background references for those keys. Instead, you need just one table for the value to edit. 

It may also be a trap leading to stuff not working in your own mod while also not crashing to desktop.

## Registry

Here is a list of such tables and streamline overrides. We encourage you all to report any new findings, preferably in alphabetical order. When reporting new tables, please include the date, because we do not know if or when this kind of behavior can abruptly change CA-side, and older dates can be considered suspicious when you have weird crashes after a patch.

1. **ancillary effects junction**
	 - tolerates missing *ancillary* as of 2nd November 2018

2. **building culture variants**
	 - tolerates missing *building level* and *culture* (*likely sc and faction as well*) as of 26th March 2019.

3. **building effects junction**
	- tolerates missing *building level*, but not *effect* as of 20th August 2018.

4. **building units allowed**
	- tolerates missing *building level* as of 20th August 2018.

5. **campaign stances factions junctions**
	- tolerates missing *faction* as of 9th December 2018.

6. **cdir events dilemma/incident/mission option junctions**
	- tolerates missing *value* and *dilemma*, even in the assembly kit, as of 8th December 2018.

7. **cdir military generator unit qualities**
	- tolerates missing *unit* as of 12th Orctober 2018.

8. **effect bonus value ids unit sets**
	- tolerates missing *effect* as of 5th Orctober 2018.

9. **effect bundles to effects junction**
	- tolerates missing *effect* as of 18th August 2018. 

10. **faction rebellion units junctions**
	- tolerates missing *faction* as of 2nd November 2018.

11. **frontend faction effects junctions**
	- tolerates missing *effect scope* as of 1st January 2019.

12. **land units**
	- tolerates missing *missing weapon*
	- tolerates missing *historical+short descr, mount, attribute group* as long as the unit is not selected in-game, as of April 2019.
	- tolerates missing *engine*, but the supposed engine will not show up in battle, as of 12th November 2018.

13. **land units to unit abilities junction**
	- tolerates missing *land unit* as of 20th August 2018.

14. **main units**
	- tolerates missing *campaign mount* as of 12th Orctober 2018.
	- tolerates missing *ui_groupings* on startup sometimes, and will crash on clicking Custom Battles otherwise.

15. **melee weapons**
	- tolerates missing *contact effect phase* as of 9th November 2018.

16. **projectile bombardments**
	- tolerates missing *projectile type* as of 12th November 2018.

17. **special ability phase stat effects**
	- tolerates missing *phase* as of 9th November 2018.

18. **special ability groups to units junction**
	- tolerates missing *unit* as of 20th Orctober 2018.

19. **technology nodes**
	- tolerates missing *faction* as of 1st January 2019.

20. **unit set unit attribute junction**
	- tolerates wrong *attribute* as of 20th Orctober 2018.

21. **unit set to unit junction**
	- tolerates missing *main unit* as of 20th August 2018.

22. **unit variants**
	- tolerates missing *land unit* as of 26th March 2019.

23. **units to groupings military permissions**
	- tolerates missing military group as of 12th December 2018.

24. **ui unit bullet point unit overrides**
	- tolerates wrong *unit* as of 27th February 2019.
