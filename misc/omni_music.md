# Map Music and Events

Involved databanks:
- CF58F1 to CF5A38 = Map music/event flag correlations pointer table
- CF5A39 to CF61DF = Map music/event flag correlation table

Pointer table formatting: `AA AA`

2 byte pointer table, nothing fancy.  The base address is $CF0000. (0xAAAA + $CF0000 = Address)  These pointers represent musical values as seen in the map editor.

Correlation table formatting: AA AA BB 00

`AA AA` = Event flag upon which to play a different music.
`BB`    = Music value to play.
`00`    = Terminator.

This table is just kind of... put together.  There's no real division between different entries.  However, there is a terminator value.  At the end of each string of possible tracks, there is a default musical value where the flag is 00 00.  This default value stops the game from reading possible flags any farther than that.

Music value 1 as code example:

    49 80 5D 00 = Event 49 00 (End game) - Because I Love You
    74 81 BB 00 = Event 74 01 (Praying?) - Give us strength
    04 80 1F 00 = Event 04 00 (Recorded Giant Step) - Your Sanctuary, after recording
    03 80 1E 00 = Event 03 00 (Defeated Titanic Ant) - Your Sanctuary, before recording
    0C 82 1E 00 = Event 0C 02 (Unknown) - Your Sanctuary, before recording
    78 82 29 00 = Event 78 02 (Beginning of game?  W/Pokey?) - Alien Investigation
    6B 80 04 00 = Event 6B 00 (Unknown) - No music
    68 80 2E 00 = Event 68 00 (After sunrise) - Normal Onett music
    12 80 72 00 = Event 12 00 (Buzz Buzz in party) - Onett at night w/ Buzz
    77 81 1D 00 = Event 77 01 (Unknown) - Onett at night 1
    01 82 97 00 = Event 01 02 (Unknown) - Onett after meteor 1
    00 00 79 00 = Default Music Track - Onett Intro
