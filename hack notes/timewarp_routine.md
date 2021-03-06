Map (32 X 80 sectors) is divided into 8 eras for the time travel purposes of the game.
Each era is 16 X 20 sectors in size.

[1F D8 XX] - Store current location to memory, and store XX value ("target era") to memory.  Then make modifications to the current location in memory based on that XX value.  Changes are made for the purposes of jumping from the same location in one era to the other.

Routine is as follows:

- Lock current coordinates as in the unchanged [1F D8].
- Check value of current era against value of target era for neccessary X-changes.
    - If current is less than five, and target is less than five:
        - Do nothing.
    - If current is less than five, and target is more than four:
        - Add 16 sectors (h) to location x-value, and add 4 to current era.
    - If current is more than four, and target is less than five:
        - Drop 16 sectors (h) from location x-value, and drop 4 from current era.
    - If current is more than four, and target is more than four:
        - Do nothing.
    - If current is equal to target:
        - Do nothing.
- Check value of current era against value of target era for neccessary Y-changes.
  BEGIN LOOP
    - If current is less than target:
        - Add 20 sectors (v) to location y-value, and add 1 to current era.
    - If current is more than target:
        - Drop 20 sectors (v) from location y-value, and drop 1 from current era.
    - If current is equal to target:
        - End loop process.

Ex. - Ness is at a given point in era one.  Using the time machine, he targets era seven as his destination.  [1F D8 05] is triggered, and the fun ensues.

- Coordinates are locked.
- Current era (#1) is less than five, and target era (#7) is more than four.  Add 16 sectors (h) to location x-value.  Add 1 to current era (now #5.)
- Current era (#5) is less than target era (#7).  Add 20 sectors (v) to location y-value.  Add 1 to current era (now #6.)
- Current era (#6) is less than target era (#7).  Add 20 sectors (v) to location y-value.  Add 1 to current era (now #7.)
- Current era (#7) is equal to target era (#7).  End loop process.

The space-time destination coordinates have now been set, and the teleport may begin.