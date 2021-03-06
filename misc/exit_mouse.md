# Exit Mouse documentation

## Pre-mouse use

`[1F 68] is used to activate an Exit Mouse return link.  When used, the game locks into memory your current coordinates.`
`[1F 69] deactivates the link and teleports you to the coordinates stored in memory.`

`$C9B10B: [1F 63 12 B1 C9 00 02] (On screen refresh, hop to $C9B112)`
`$C9B112: [05 0A 03 1F 68 02] (Unset flag 0A 03, then set Exit Mouse return hotspot)`

## Upon using Exit Mouse

`$EF9EF4: [1B 00 01 1D 22 1B 02 9F 9F EF 00 06 0A 03 E2 9F EF 00 07 B4 00 1B 03 02 A0 EF 00 07 B5 00 1B 03 7E A0 EF 00 07 85 02 1B 03 7E A0 EF 00 07 BE 01 1B 03 29 A0 EF 00 07 86 02 1B 03 29 A0 EF 00 07 87 02 1B 03 29 A0 EF 00 07 88 02 1B 03 29 A0 EF 00 07 A3 02 1B 03 02 A0 EF 00 07 B6 02 1B 03 7E A0 EF 00 07 B7 02 1B 03 7E A0 EF 00 06 FE 02 B7 A0 EF 00 1F 41 05]@(The mouse found the way out and waved for you to follow.)[03 1F 69 1B 01 1D 0F 00 00 05 00 02 1F 41 06 08 60 DE C7 00 02]`

    1B 00 01 - ?
    1D 22 - Sets True if current location is Exit Mouse-incompatible
    1B 02 9F 9F EF 00 - If true, say "The Exit Mouse cannot do its job unless it is in a hole, cave, or some place with an exit!"
    06 0A 03 E2 9F EF 00 - If 0A 03 is set*, say "At this point in time, the mouse is asleep."
    07 B4 00 - Set T/F based on B4 00
    1B 03 02 A0 EF 00 - If false, say "You shouldn't let the mouse go until the pizza has arrived."
    07 B5 00 - Set T/F based on B5 00
    1B 03 7E A0 EF 00 - If false, say "Please stick around until Escargo Express arrives.  So... do not use the Exit Mouse!"
    07 85 02 - Set T/F based on 85 02
    1B 03 7E A0 EF 00 - If false, say "Please stick around until Escargo Express arrives.  So... do not use the Exit Mouse!"
    07 BE 01 - Set T/F based on BE 01
    1B 03 29 A0 EF 00 - If false, say "If you release the mouse now, it might be stepped on by the approaching customer.  So... it's not a good idea to release the mouse now."
    07 86 02 - Set T/F based on 86 02
    1B 03 29 A0 EF 00 - If false, say "If you release the mouse now, it might be stepped on by the approaching customer.  So... it's not a good idea to release the mouse now."
    07 87 02 - Set T/F based on 87 02
    1B 03 29 A0 EF 00 - If false, say "If you release the mouse now, it might be stepped on by the approaching customer.  So... it's not a good idea to release the mouse now."
    07 88 02 - Set T/F based on 88 02
    1B 03 29 A0 EF 00 - If false, say "If you release the mouse now, it might be stepped on by the approaching customer.  So... it's not a good idea to release the mouse now."
    07 A3 02 - Set T/F based on A3 02
    1B 03 02 A0 EF 00 - If false, say "You shouldn't let the mouse go until the pizza has arrived."
    07 B6 02 - Set T/F based on B6 02
    1B 03 7E A0 EF 00 - If false, say "Please stick around until Escargo Express arrives.  So... do not use the Exit Mouse!"
    07 B7 02 - Set T/F based on B7 02
    1B 03 7E A0 EF 00 - If false, say "Please stick around until Escargo Express arrives.  So... do not use the Exit Mouse!"
    06 FE 02 B7 A0 EF 00 - If FE 02 is set**, say "For some reason, the exit mouse isn't being agreeable."
    1F 41 05 - ?
    @(The mouse found the way out and waved for you to follow.)[03]
    [1F 69] - Return player to previous area and unset link.
    [1B 01 1D 0F 00 00 05 00 02 1F 41 06 08 60 DE C7 00 02]

*  Flag FE 02 is set when Starman DX is defeated and unset when you talk to Tony.

** Flag 0A 0E is inexplicably set in a block of flags unset during the game over screen.
