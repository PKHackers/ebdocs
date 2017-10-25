# $EF126B - Tileset animation properties table
As linked to via the related pointer table (0x2F141B)

## ENTRY PATTERN

[ZZ] (AA BB CC DD EE FF GG HH)

ZZ = Number of AA-HH clusters to follow
AA = Frames of animation per cycle
BB = Time per frame (1/60 second format)
CC = ? - Vertical graphics offset (shift tile up C/2 pixels)
     Has an effect on the sequence used (horizontal rather than vertical?)
DD = ? - Increasing values delete an increasing amount of map tiles
EE = ? - Vertical graphics offset (shift tile up C/2 pixels - 0x20 = full tile)
FF = ? - Changes the tiles being used?
G? = Tile number in static tileset to start overwriting with animation effects
?G = Vertical graphics offset (shift tile down G pixels)
HH = ? - Changes the tiles being used?

### Entry 00 (Underworld)
`$EF126B [05]`

    04 0C 60 00 20 00 10 00
    04 0F 60 00 A0 01 40 00
    04 0F 80 00 20 03 70 00
    04 15 E0 00 20 05 B0 00
    04 15 00 01 A0 08 20 01

### Entry 01 (Onett)
`$EF1294 [02]`

    04 13 40 01 20 00 10 00
    02 14 40 00 20 05 B0 00

### Entry 02 (Twoson)
`$EF12A5 [00]`

    NO DATA

### Entry 03 (Threed)
`$EF12A6 [00]`

    NO DATA

### Entry 04 (Fourside)
`$EF12A7 [00]`

    NO DATA

### Entry 05 (Magicant)
`$EF12A8 [01]`

    08 0A 00 01 20 00 10 00

### Entry 06 (Outdoors)
`$EF12B1 [02]`

    04 0A 80 01 20 00 10 20
    04 15 40 02 20 06 D0 00

### Entry 07 (Summers)
`$EF12C2 [02]`

    05 31 80 02 20 00 10 00
    04 12 20 01 A0 0C 50 01

### Entry 08 (Desert)
`$EF12D3 [01]`

    04 14 E0 03 20 00 10 00

### Entry 09 (Dalaam)
`$EF12DC [00]`

    NO DATA

### Entry 10 (Indoors 1)
`$EF12DD [00]`

    NO DATA

### Entry 11 (Indoors 2)
`$EF12DE [00]`

    NO DATA

### Entry 12 (Stores 1)
`$EF12DF [01]`

    03 05 40 02 20 00 10 00

### Entry 13 (Caves 1)
`$EF12E8 [01]`

    08 03 40 00 20 00 10 00

### Entry 14 (Indoors 3)
`$EF12F1 [00]`

    NO DATA

### Entry 15 (Stores 2)
`$EF12F2 [00]`

    NO DATA

### Entry 16 (Indoors 4)
`$EF12F3 [06]`

    02 1F 40 00 20 00 10 20
    02 1F C0 00 A0 00 30 00
    02 1F 40 00 20 02 90 20
    02 1F 40 01 A0 02 B0 00
    02 19 40 00 20 05 50 21
    02 19 C0 00 A0 05 70 01

### Entry 17 (Winters)
`$EF1324 [01]`

    04 0F A0 02 20 00 10 00

### Entry 18 (Scaraba)
`$EF132D [01]`

    04 13 00 02 20 00 10 00

### Entry 19 (Caves 2)
`$EF1336 [01]`

    06 14 E0 01 20 00 10 00
