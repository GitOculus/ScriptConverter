# UO Steam (UOS) Scripting Documentation

*Last updated: April 2014*  
*Authors: Diego Alcântara, Diogo Palma*

## Table of Contents

1. [Introduction](#introduction)
2. [Syntax](#syntax)
3. [Object Inspector](#object-inspector)
4. [Layers](#layers)
5. [Commands](#commands)
   - [Abilities](#abilities)
   - [Actions](#actions)
   - [Agents](#agents)
   - [Aliases](#aliases)
   - [Conditions](#conditions)
   - [Gumps](#gumps)
   - [Journal](#journal)
   - [Lists](#lists)
   - [Main](#main)
   - [Others](#others)
   - [Spells](#spells)
   - [Targeting](#targeting)
   - [Timers](#timers)

## Introduction

UOS scripting language is a "command based" language that is easy to use and requires very basic programming knowledge. Its power and flexibility comes from its commands, which are documented in this guide.

### Syntax

#### Commands and Parameters

Commands use a specific parameter format:
- **Mandatory parameters**: shown in parentheses `(parameter)`
- **Optional parameters**: shown in brackets `[parameter]`
- **Choices**: separated by slashes `/` (e.g., `'front'/'back'`)

**Important Notes:**
- Text parameters can be written without quotes, in single quotes, or double quotes
- Use double quotes for text containing apostrophes
- Use quotes for compound text like "star fruit"
- Always using double quotes is the recommended best practice

**Examples:**
```
// Correct usage
pushlist 'fruits' 'apple'
pushlist 'fruits' grape 'front'
pushlist 'fruits' 'lemon' 'back'
pushlist 'fruits' 'star fruit'
pushlist 'fruits' "japan's melon" 'front'

// Incorrect usage
pushlist
pushlist 'fruits'
pushlist 'fruits' star fruit
```

#### Control Structures

**Conditional Statements:**
```
if (statement)
elseif (statement)
else
endif
```

**Loops:**
```
while (statement)
endwhile

for (value)
endfor

for (start) to (end)
endfor

for (start) to ('list name')
endfor

for (start) to (end) in ('list name')
endfor
```

**Keywords:**
- `break` - Exit loop
- `continue` - Continue to next iteration
- `stop` - Stop macro execution
- `replay` - Restart current macro
- `not (statement)` - Logical NOT
- `(statement) and (statement)` - Logical AND
- `(statement) or (statement)` - Logical OR

**Example Loop:**
```
// Repeat 10 times
for 10
    // Send a lovely message in game
    msg "I love UOS!"
    pause 1500
endfor
sysmsg "End of for loop"
```

#### Symbols

**@ Symbol (Prefix):**
- Suppresses command warnings or outputs
- Example: `@findtype 0x12 0 0 0 10` (suppresses "type not found" warnings)

**! Symbol (Suffix):**
- Usage varies by command
- For targeting functions: disables targeting queue
- Example: `target! 'self'` (disables targeting queue)

#### Comments

Use `//` prefix for single-line comments:
```
// This is a comment
msg "Hello World!" // This is also a comment
```

## Object Inspector

The Object Inspector helps you identify object properties needed for scripting. Access it via the "Object Inspector" button on the Macros tab.

**What is an object?**
An object is essentially a game item or mobile. The Object Inspector shows properties like:
- Serial (unique identifier)
- Graphic (item type ID)
- Color
- Position (x, y, z)
- Amount
- Layer (for equipped items)

## Layers

Equipment layer numbers for use with commands like `equipitem` and `findlayer`:

| Layer | Item Type |
|-------|-----------|
| 1 | Right Hand |
| 2 | Left Hand |
| 3 | Shoes |
| 4 | Pants |
| 5 | Shirt |
| 6 | Head |
| 7 | Gloves |
| 8 | Ring |
| 9 | Talisman |
| 10 | Neck |
| 11 | Hair |
| 12 | Waist |
| 13 | Inner Torso |
| 14 | Bracelet |
| 16 | Facial Hair |
| 17 | Middle Torso |
| 18 | Earrings |
| 19 | Arms |
| 20 | Cloak |
| 21 | Backpack |
| 22 | Outer Torso |
| 23 | Outer Legs |
| 24 | Inner Legs |
| 25 | Mount |
| 26 | Shop Buy |
| 27 | Shop Restock |
| 28 | Shop Sell |
| 29 | Bank |

## Commands

### Abilities

#### Fly and Land
Start or stop flying.
```
fly    // Start flying
land   // Stop flying
```

#### Set Ability
Toggle or enforce primary, secondary, stun or disarm ability.
```
setability ('primary'/'secondary'/'stun'/'disarm') ['on'/'off']

// Examples
@setability 'primary'
setability 'secondary'
setability 'primary' 'on'
@setability 'secondary' 'off'
```

### Actions

#### Attack
Attack a specific mobile by serial or alias.
```
attack (serial)

// Example
if findtype 0x12 0 0 0 5
    autotargetobject 'found'
    virtue 'Honor'
    attack 'found'
endif
```

#### Clear Hands
Unequip character's hands.
```
clearhands ('left'/'right'/'both')

// Example
if not clearhands 'right'
    sysmsg 'Unable to clear hands, item not found.'
else
    sysmsg 'Right hand cleared.'
endif
```

#### Click Object
Perform a single click on a specific serial.
```
clickobject (serial)

// Example
if findtype 0xbd2 0 'ground'
    clickobject 'found'
endif
```

#### Use Type
Use a specific item type (graphic). Can be queued unless using `!` suffix.
```
usetype (graphic) [color] [source] [range or search level]
clearusequeue

// Examples
usetype! 0xe21 'any' 0x40116650  // Use bandage from specific container
usetype 0xe21 'any' 'ground' 2   // Use bandage from ground in 2 tile range
```

#### Use Object
Use a specific object by serial or alias. Can be queued unless using `!` suffix.
```
useobject (serial)
clearusequeue

// Examples
useobject! 0x40116650
useobject 'myObject'
```

#### Move Item
Move an item serial or type from source to destination.
```
moveitem (serial) (destination) [(x, y, z)] [amount]
moveitemoffset (serial) 'ground' [(x, y, z)] [amount]
movetype (graphic) (source) (destination) [(x, y, z)] [color] [amount] [range or search level]
movetypeoffset (graphic) (source) 'ground' [(x, y, z)] [color] [amount] [range or search level]

// Examples
moveitem 0x40116650 'backpack'
moveitem 'righthand' 'backpack'
movetype! 0xeed 'backpack' 'ground' 1950 50 0  // Move 100 gold, disallow stacking
```

#### Movement
Move your character in given directions.
```
walk (direction)
turn (direction)
run (direction)

// Examples
walk "North, East, East, West, South, Southeast"
turn "Northeast"
for 10
    run "South"
endfor
```

#### Use Skill
Use a skill by name.
```
useskill ('skill name'/'last')

// Example
if hits != maxhits
    if yellowhits 'self'
        useskill 'Spirit Speak'
    elseif not poisoned 'self'
        cast 'Heal' 'self'
    else
        cast 'Cure' 'self'
    endif
endif
```

#### Bandage Self
Shortcut to use bandage and automatically target self.
```
bandageself

// Example with timer
if not timerexists 'bandageSelf'
    settimer 'bandageSelf' 2000
endif
if hits != maxhits
    if timer 'bandageSelf' >= 2000
        bandageself
        settimer 'bandageSelf' 0
    endif
endif
```

#### Toggle Hands
Arm and disarm an item.
```
togglehands ('left'/'right')

// Example
togglehands 'right'  // Equip/unequip right hand
```

### Aliases

#### System Aliases
Predefined aliases available in all macros:

| Alias | Description |
|-------|-------------|
| `backpack` | Player backpack |
| `bank` | Player bank |
| `enemy` | Current enemy |
| `friend` | Current friend |
| `ground` | World ground |
| `last`/`lasttarget` | Last targeted mobile |
| `lastobject` | Last used object, item or mobile |
| `lefthand` | Player equipped left hand item |
| `mount` | Current mount |
| `righthand` | Player equipped right hand item |
| `self` | Player character |

#### Alias Management
```
setalias ('alias name') [serial]
unsetalias ('alias name')
findalias ('alias name')
promptalias ('alias name')

// Examples
setalias 'pet'                    // Prompt for new pet
setalias 'oldObject' 0x40116650   // Set to specific serial
setalias 'newObject' 'oldObject'  // Copy from another alias

if not findalias 'weapon'
    promptalias 'weapon'
endif
```

### Conditions

#### Contents
Check amount of items inside a container.
```
if contents (serial) ('operator') ('value')
endif

// Example
if contents 'backpack' > 10
    sysmsg 'More than 10 items inside backpack!'
endif
```

#### In Region
Check if item or mobile is in specific region type.
```
if inregion ('guards'/'town'/'dungeon'/'forest') [serial] [range]
endif

// Examples
if inregion 'town'
    msg 'bank'
endif

if innocent 'enemy' and inregion 'guards' 'enemy' 10
    cancelautotarget
endif
```

#### Skills
Check local player skill value.
```
if skill ('name') (operator) (value)
endif

// Example
if skill 'Necromancy' >= 99
    cast 'Vampiric Embrace'
elseif skill 'Necromancy' >= 75
    cast 'Lich Form'
else
    cast 'Horrific Beast'
endif
```

#### Player Attributes
Check various player attributes:

**Coordinates:** `x`, `y`, `z`  
**Resistances:** `physical`, `fire`, `cold`, `poison`, `energy`  
**Status:** `str`, `dex`, `int`, `hits`, `maxhits`, `diffhits`, `stam`, `maxstam`, `mana`, `maxmana`  
**System:** `usequeue`, `dressing`, `organizing`  
**Others:** `followers`, `maxfollowers`, `gold`, `hidden`, `luck`, `tithingpoints`, `weight`, `maxweight`, `diffweight`

```
if hits <= maxhits
    bandageself
    if diffhits > 30
        autotargetself
        cast 'Greater Heal'
    endif
endif

if not hidden
    useskill 'Hiding'
endif
```

#### Object Attributes
Check attributes for mobiles and items:

**All objects:** `serial`, `graphic`, `color`, `x`, `y`, `z`  
**Items only:** `amount`  
**Mobiles only:** `name`, `dead`, `direction`, `hits`, `maxhits`, `diffhits`, `flying`, `paralyzed`, `poisoned`, `mounted`, `yellowhits`, `war`  
**Notoriety:** `criminal`, `enemy`, `friend`, `gray`, `innocent`, `invulnerable`, `murderer`

```
if poisoned
    autotargetself
    cast 'Cure'
endif

if graphic 'enemy' == 401
    msg 'Hey pretty!'
endif
```

#### Find Object
Search for an item by serial or alias.
```
if findobject (serial) [color] [source] [amount] [range]
endif

// Example
if findobject 'righthand'
    clearhands 'right'
endif
```

#### Find Type
Search for an item type and set alias "found".
```
if findtype (graphic) [color] [source] [amount] [range or search level]
endif

// Example
if findtype 0xe21 'any' 'backpack' 20
    moveitem 'found' 'ground' 1250 489 0
else
    buy 'Bandages'
endif
```

#### Distance and Range
Check distance or range between character and another object.
```
if distance (serial) (operator) (value)
endif
if inrange (serial) (range)
endif

// Examples
if distance 0x40116650 <= 2
    moveitem 0x40116650 'backpack'
endif

if inrange 'friend' 10
    miniheal 'friend'
endif
```

#### Count Type
Compare amount of item type inside a container.
```
counttype (graphic) (color) (source) (operator) (value)

// Examples
if counttype! 0x14f0 'any' 'backpack' == 2  // Ignore stacked amounts
    sysmsg '2 bank checks found!' 86
endif

if counttype 0xf0c 'any' 'backpack' > 10    // Consider stacked amounts
    sysmsg 'More than 10 heal pots!' 86
endif
```

### Gumps

#### Wait For Gump
Wait for a gump from server.
```
waitforgump (gump id/'any') (timeout)

// Example
useobject! 0x491093
waitforgump 0x1ec8c837 5000
```

#### Reply Gump
Reply to a server gump.
```
replygump (gump id/'any') (button) [option] [...]

// Example
useobject! 0x491093
waitforgump 0x1ec8c837 5000
replygump 0x1ec8c837 1
```

#### Gump Checks
```
if gumpexists (gump id/'any')
endif

if ingump (gump id/'any') ('text')
endif

// Examples
if gumpexists 'any'
    sysmsg 'There is at least 1 gump'
endif

if ingump 0x1ec8c837 'Home'
    replygump 0x1ec8c837 2
endif
```

### Journal

#### In Journal
Check for text in journal with optional source.
```
if injournal ('text') ['author'/'system']
endif

// Example
if @injournal 'outside the protection' 'system'
    // Do something...
    @clearjournal
endif
```

#### Clear Journal
```
clearjournal
```

#### Wait For Journal
Check for text in journal until found or timeout.
```
waitforjournal ('text') (timeout) ['author'/'system']

// Example
waitforjournal 'too far away' 5000 'system'
```

### Lists

#### List Management
```
createlist ('list name')
removelist ('list name')
clearlist ('list name')
if listexists ('list name')
endif
if list ('list name') (operator) (value)
endif
```

#### List Operations
```
pushlist ('list name') ('element value') ['front'/'back']
poplist ('list name') ('element value'/'front'/'back')
if inlist ('list name') ('element value')
endif

// Examples
createlist 'sample'
pushlist 'sample' 'apple'
pushlist 'sample' 'grape' 'front'

if inlist 'sample' 'apple'
    sysmsg 'List contains apple!'
endif

// Use ! suffix for unique elements or case-sensitive checks
pushlist! 'sample' 'unique_item'  // Only add if not already present
inlist! 'sample' 'Apple'          // Case-sensitive check
```

### Messages

#### Send Messages
```
msg ('text') [color]
headmsg ('text') [color] [serial]
partymsg ('text')
guildmsg ('text')
allymsg ('text')
whispermsg ('text')
yellmsg ('text')
sysmsg ('text')
chatmsg ('text')
emotemsg ('text')

// Examples
sysmsg 'Hello World!'
partymsg "What's up?"
msg 'Hi'
headmsg 'Hi' 26  // Red overhead message
```

### Spells

#### Cast
Cast a spell by ID or name.
```
cast (spell id/'spell name'/'last')

// Examples
cast 'Magic Arrow'
waitfortarget 650
target 'enemy'

// Automated target
autotargetobject 'enemy'
cast 'Lightning'
```

#### Healing Spells
Managed casting with automatic disruption checking.
```
miniheal [serial]
bigheal [serial]
chivalryheal [serial]

// Examples
miniheal          // Heal self
bigheal 'friend'  // Heal friend
chivalryheal      // Chivalry heal self
```

### Targeting

#### Wait For Target
```
waitfortarget (timeout)

// Example
cast 'Explosion'
waitfortarget 2500
target! 'enemy'
```

#### Direct Target
```
target (serial) [timeout]
targettype (graphic) [color] [range]
targetground (graphic) [color] [range]
targettile ('last'/'current'/(x y z)) [graphic]
cleartargetqueue

// Examples
cast 'Heal'
waitfortarget 250
target 'friend'  // Queued

usetype! 0x26ac
waitfortarget 500
target! 'enemy'  // Not queued (suffix !)
```

#### Automated Target
Setup automatic targeting before action.
```
autotargetlast
autotargetself
autotargetobject (serial)
autotargettype (graphic) [color] [range]
cancelautotarget

// Example
cancelautotarget
autotargetself
cast 'Greater Heal'

autotargetobject 'enemy'
cast 'Explosion'
```

### Timers

#### Timer Management
```
createtimer ('timer name')
settimer ('timer name') (value)
removetimer ('timer name')
if timerexists ('timer name')
endif
if timer ('timer name') (operator) (value)
endif

// Example
if not timerexists 'sample'
    createtimer 'sample'
endif

if timer 'sample' > 10000
    settimer 'sample' 0
endif
```

### Main Commands

#### Pause
Insert a pause/wait in milliseconds.
```
pause (timeout)

// Examples
pause 1000  // 1 second
pause 500   // 0.5 second
```

#### Play Macro
Run a specific macro by name (case sensitive).
```
playmacro 'name'
```

#### Resynchronize
Resynchronize game data with server (wait 0.8 seconds between requests).
```
resync
```

### Other Commands

#### Virtues
```
virtue ('honor'/'sacrifice'/'valor')

// Example
if @findtype 0x12 0 0 0 5
    autotargetobject 'found'
    virtue 'Honor'
endif
```

#### Context Menu
```
contextmenu (serial) (option)
waitforcontext (serial) (option) (timeout)
```

#### Properties
```
waitforproperties (serial) (timeout)
if property ('name') (serial) [operator] [value]
endif

// Example
waitforproperties 'ring' 5000
if property 'Faster Casting Recovery' 'ring' == 3
    moveitem 'ring' 'backpack'
endif
```

## UOS Script Examples

### Basic Healing Loop
```
while not dead
    if hits < maxhits - 30
        if poisoned 'self'
            cast 'Cure'
            waitfortarget 5000
            target 'self'
        else
            bandageself
        endif
        pause 1500
    endif
    pause 500
endwhile
```

### Auto-Attack with Honor
```
if findtype 0x12 0 0 0 5  // Find ettin in 5 tiles
    autotargetobject 'found'
    virtue 'Honor'
    attack 'found'
endif
```

### Resource Gathering
```
while @findtype 0x19b9 'any' 'ground' 1 1  // Find logs on ground
    moveitem 'found' 'backpack'
    pause 1000
    if weight > maxweight - 50
        msg 'bank'
        pause 2000
        // Move logs to bank
        while @findtype 0x19b9 'any' 'backpack'
            moveitem 'found' 'bank'
            pause 1000
        endwhile
        break
    endif
endwhile
```

### Smart Targeting
```
unsetalias 'smart'
if targetexists 'harmful'
    setalias 'smart' 'enemy'
endif
if targetexists 'beneficial'
    setalias 'smart' 'friend'
endif
if targetexists
    setalias 'smart' 'last'
endif

if @findalias 'smart' and inrange 'smart' 10
    target! 'smart'
endif
```

## Key Differences from Other Scripting Systems

1. **Command-based**: UOS uses simple command syntax rather than complex programming constructs
2. **Built-in queuing**: Many commands have automatic queuing systems with `!` suffix to bypass
3. **Automatic targeting**: Rich set of automated targeting commands
4. **Agent integration**: Direct integration with UOS agents (organizer, vendor, etc.)
5. **Simple syntax**: Minimal programming knowledge required
6. **Case sensitivity**: Command names are case-insensitive, but some parameters are case-sensitive

This documentation provides a comprehensive reference for UOS scripting syntax and commands, useful for both writing new UOS scripts and converting them to other scripting systems like Legion API.