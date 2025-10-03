# Conversion Notes and Potential Issues

## Conversions Made
1. **Skill Check**: `Player.GetSkillValue('Chivalry')` → `API.GetSkill('Chivalry').Value`
2. **Spell Casting**: `Spells.CastChivalry('Divine Fury')` → `API.CastSpell('Divine Fury')`
3. **Mana Access**: `Player.Mana` → `API.Player.Mana`
4. **Skill Usage**: `Player.UseSkill('Meditation')` → `API.UseSkill('Meditation')`
5. **Pause Timing**: `Misc.Pause(5000)` (milliseconds) → `API.Pause(5)` (seconds)
6. **Main Loop**: Added `while not API.StopRequested:` loop for continuous training
7. **Optional Meditation**: Added `USE_MEDITATION` configuration variable to make meditation optional
8. **CPU Prevention**: Added small `API.Pause(0.1)` at end of loop to prevent CPU overload

## Enhancements
1. Made meditation optional via configuration variable
2. Added continuous loop pattern using `while not API.StopRequested:` (standard Legion pattern)
3. Added small pause to prevent CPU overload

## Potential Issues
None identified. The conversion is straightforward with direct equivalents in the Legion API.

## Important Notes
- **Main Loop Pattern**: Legion scripts should use `while not API.StopRequested:` with a small pause (0.1s) to prevent CPU overload
- This is the recommended pattern for continuous Legion scripts

## Testing Recommendations
1. Verify spell names are correct in Legion (especially capitalization)
2. Confirm tithing points are available for chivalry spells
3. Test mana threshold logic works as expected
4. Test the stop functionality works correctly
