# Chivalry Training Script

## Overview
This script automates chivalry skill training by continuously casting appropriate chivalry spells based on your current skill level and optionally managing mana through meditation.

## How It Works
- Runs in a continuous loop until stopped
- Checks your current Chivalry skill level each iteration
- Casts the appropriate spell for your skill range:
  - **Below 60**: Divine Fury (15 mana)
  - **60-70**: Enemy Of One (20 mana)
  - **70-90**: Holy Light (10 mana)
  - **90-115**: Noble Sacrifice (20 mana)
- Automatically meditates if your mana is below the required amount (configurable)

## Configuration
- `USE_MEDITATION`: Set to `True` to enable auto-meditation, `False` to disable it

## Usage
1. Ensure you have enough reagents for the spells you'll be casting
2. (Optional) Edit `USE_MEDITATION` in the script to enable/disable auto-meditation
3. Run the script
4. The script will continuously train until you stop it
5. Stop the script using the Legion Scripting interface

## Requirements
- Chivalry skill
- Meditation skill (if using auto-meditation)
- Appropriate reagents for chivalry spells
- Sufficient tithing points

## Notes
- The script uses `while not API.StopRequested:` for the main loop with a small pause to prevent CPU overload
- This is the standard pattern for continuous Legion scripts
