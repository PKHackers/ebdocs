# Minitile Properties

EBisumaru(@gmail.com)

Each minitile can have a variety of properties. This crappy excuse for a table represents all the possible properties that they can have.

|0x80	|0x40	|0x20	|0x10	|0x08	|0x04	|0x02	  |0x01   |
|-----|-----|-----|-----|-----|-----|-------|-------|
|Solid|??   |??   |Door |Water|Sweat|Layer 2|Layer 2|

I think 0x40 is some sort of alternate solid bit. Suffice it to say that it's not used in the game.
0x08 and 0x04 can be combined for the deep water property seen in the DD.
You can get all the properties you want simply by adding the values in hex.

The game engine automatically makes lines between similar tiles, so don't worry about diagonals. Say you wanted something like this:
    #|#|#|/                                  80|80|80|00
    #|#|/|   For the properties, you'd put   80|80|00|00
    #|/| |                                   80|00|00|00
    /| | |                                   00|00|00|00


Enabling Layer 2 is confusing, so I'll explain that too. For the top two rows of minitiles, you'll only use the 0x01 bit. All minitiles below them using layer 3 need both layer 2 bytes. So all minitiles in the following tile property diagram will have layer 2 enabled:

    01|01|01|01
    01|01|01|01
    03|03|03|03
    03|03|03|03

Also,

    00|01|00|00
    00|01|01|01
    00|03|01|01
    00|03|03|03

works. If you're placing another tile under it on the map, you need 03s in all minitile that are directly under another Layer 2 minitile.
Like this.

    01|01|01|01
    01|01|01|01
    03|03|03|03
    03|03|03|03

    03|03|03|03
    03|03|03|03
    03|03|03|03
    03|03|03|03
