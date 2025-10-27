# Legion Scripting Converter

## Project Overview
This project converts game scripts from various scripting systems to the Legion Scripting API.

## Key Files
- `API.cs`: C# class providing all Legion Scripting access points
- `API.py`: Auto-generated file from API.cs for IDE autocomplete
- `assistantdocs/`: Contains documentation for various scripting systems

## Legion Scripting API
Legion Scripting is a scripting system for an FNA game using IronPython, allowing players to script in-game functionality.

## Conversion Guidelines

### Primary Goal
Convert scripts from other scripting languages/systems into Legion Scripting API calls.

### Reference Documentation
- Use files in `assistantdocs/` folder for documentation on source scripting systems
- Always examine API.cs and API.py to understand available Legion Scripting methods

### Conversion Process
1. Analyze source script to understand functionality
2. Identify equivalent Legion API methods in API.cs/API.py
3. Convert syntax and method calls to Legion format
4. Maintain original script logic and behavior
5. Test converted script for compatibility
6. Place finished conversions inside converted folder in subfolders named after the script

### Important Notes
- Focus on accurate functional conversion, not literal translation
- Preserve script intent and behavior
- Use Legion API methods whenever possible
- Document any limitations or differences in conversion

- Put finished files in converted directory
- In Legion scripting, ShareVar are for variables used across different scripts. Avoid for single scripts. Prefer python vars where possible.
- PersistentVars are saved to disk, SharedVars are only in session
- In Legion scripting, API.Pause is in seconds, most other scripting systems use ms
- While loops should be while not API.StopRequested:

### UI and Callback Handling
- `API.ProcessCallbacks()` is only needed if you registered click callbacks with `API.AddControlOnClick()`
- Use proper positioning methods: `.SetPos(x, y)`, `.SetX(x)`, or `.SetY(y)` instead of direct `.X` or `.Y` assignment
- Main loop pattern for scripts with click callbacks:
  ```python
  while running:
      API.ProcessCallbacks()  # Only needed for registered click events
      API.Pause(0.1)  # Small pause to prevent CPU overload
  ```
- Always add import API to scripts, *unless* you create a script that will only be imported. Import only scripts should be prefaced with _ like _myhelper.py
- Gumps usually have a serial, and replygump is the same as clicking a button. Buttons always have a button parameter(number)
- Context menu numbers, Gump reply/button numbers, Gump serial, npc serial, and item serial numbers will be the same
- Always add an instruction.md file with an overview and a guide(if needed) on how to use the script
- Always add a problems.md file with parts of the script you were not sure on converting or were unable to convert.
- ClassicAssist Print function is API.SysMsg in legion.
- Hue (ushort) are the same across scripting languages
- For finding all items you can use uint max value( 4294967295 ) in place of a container serial in ItemsInContainer or FindType with uint max value as it's graphic. This will match all items.
- Player weight can be accessed with API.Player.Weight and MaxWeight
- Target.TargetResource is the equivelent of API.TargetResource
- A players backpack is accessed via API.Backpack
