# ARRANGEMENT DATA

Arrangement data is compressed

- $E15455 to $E1549D = Nintendo arrangement
- $E14EC1 to $E14F29 = APE arrangement
- $E15174 to $E151E7 = HAL arrangement

- $C0EEBB to $C0EEC4 = Pointer to Nintendo arragnement
- $C0EF13 to $C0EF1C = Pointer to APE arrangement
- $C0EF6A to $C0EF73 = Pointer to HAL arrangement


Arrangement data is exactly 2 kilobytes long. Each two bytes represent one 8x8 tile.

    [XX YY]
    XX: Tile to use
    YY: Binary switch
    00000000 - use 1st palette
    00000010 - Unknown
    00000100 - Use 2nd palette
    00001000 - Use 3rd palette
    00010000 - Use 5th palette
    00100000 - Unknown
    01000000 - Flip horizontally
    10000000 - Flip vertically

Notes: There is a 4th palette in the data but it's all black. Telling a tile to use two different palettes equals doom.