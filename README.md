# entitylump showtriggers

so this is something i made while playing around with sourcemod's new entitylump module added in 1.12. so this won't support any sourcemod version older than 1.12. it is a sourcemod plugin for visualizing and debugging triggers in source engine games. handy for mappers or server owners, or just surfers. this plugin lets you:

- toggle visibility individual triggers by targetname, or classname.
- change colours of individual triggers by targetname, or classname.
- search for specific triggers by targetname or classname.
- identify triggers by pointing your crosshair at them
- easily view a list of all triggers currently on the map
- view entity info like outputs and other properties for any trigger
- save and load profiles so you can easily reload your colour / trigger visibility setup
- share profiles with others

## main commands

| command | description |
|---------|-------------|
| `!st` or `!showtriggers` | opens the main menu |
| `!stsearch [term]` | search triggers by name/class (empty = show all) |
| `!identifytrigger` | opens settings for whatever trigger you're looking at |
| `!sthelp` | get help |

![4](https://github.com/user-attachments/assets/0fb918c4-1caf-4772-bd4d-7fa2063968fd)

## menu system
  
the main menu (`!st`) gives you access to:

1. **toggle by type** - enable/disable entire trigger categories by classname
2. **search / find triggers** - find specific triggers and view their properties, toggle visibility, etc.
3. **my enabled triggers** - manage triggers that you have toggled on
4. **display settings** - opacity and display mode (todo)
5. **commands & help** - quick reference

![1](https://github.com/user-attachments/assets/e66b5087-32a1-4d50-81f9-6964b4b8cdd6)

## trigger info

when you select a trigger, you can view detailed info including:
- classname and targetname
- entity index and hammerid
- spawnflags
- world position and bounding box
- parent entity (if any)
- all outputs and their targets
- command to change its colour

![3](https://github.com/user-attachments/assets/bcc6d024-ddc9-473d-90ca-cd5feb8016b6)

## color commands and supported trigger types
i will add more soon, but currently we support 4 entities, with some special cases for `trigger_multiple`:
  
### **1.** set color for a trigger by classname. types are:
```
0 = trigger_multiple
1 = trigger_push  
2 = trigger_teleport
3 = trigger_teleport_relative

sm_stcolor <type> <r> <g> <b> [a]
```

### **2.** set color for special trigger_multiple subtypes:
```
0 = normal
1 = gravity
2 = anti-gravity
3 = base velocity

sm_stcolor_special <type> <r> <g> <b> [a]
```
  
### **3.** set a custom color for a specific trigger entity. 
```
// the search menu will generate this link for you with the ent index

sm_stcolor_ent <entity_index> <r> <g> <b> [a]
```

### **4.** set opacity for all trigger types at once.  
```
sm_stopacity <0-255>
```

![2](https://github.com/user-attachments/assets/72313521-5726-49b5-b772-fe968ae98a33)

## display commands

todo: currently only solid. want to add beam outlines at some point.  

## profile commands

profiles let you save and load your trigger display settings per-map. you can also share and copy other peoples.  

```
sm_stprofile_save <name> [public]
sm_stprofile_load <name> [public] [map]
sm_stprofile_list [public] [map]
sm_stprofile_copy <name> <frommap> [public] [newname]
```
  
## how to install

all color preferences are saved via clientprefs cookies, so they persist across sessions. profiles are stored in a sqlite database `triggerprefs.sq3`. **make sure you add to your `addons/sourcemod/configs/databases.cfg`**:

```
	"triggerprefs"
	{
		"driver"			"sqlite"
		"host"				"localhost"
		"database"			"triggerprefs"
		"user"				"root"
		"pass"				""
	}
```

the database will autogenerate  

## notes

- outline mode is disabled in this version (the beam rendering was causing issues)
- individual trigger settings are stored by hammerid, so they'll persist across map reloads but not across different maps
- the search function checks both targetname and classname
- profiles are map-specific by design
