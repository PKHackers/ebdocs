# $E1CE08 - Title screen animation data

Pointer table located at 0x21D19D (2-byte format, base address 0x210200).  Data below demarked by said pointers. Pointer table denotes which data to use to render the letters, in order from left to right.  It does not have an effect on which letters appear on the screen first.

[08 CE 35 CE 62 C3 8F CE BC CE E9 CE 16 CF 43 CF 70 CF]

0x21D008 Format: AA BB BB CC DD
 AA = Absolute? Y location. Signed value. 00 seems to be middle of the screen. (signed means 80 is the sign bit, FF = -1) Appears to be measured in pixels.
 BB BB = Tile number used for this particular section.  The first block (Earth, and the left third of the o) runs from 0x0 to 0x59. The rest of the letters begin at 0x100 and go from there.  However, 0x3000 is added in all cases.  Why?  I dunno.  It just is.
 CC = Realative? X final location. Signed value. 00 seems to be middle of the screen. (signed means 80 is the sign bit, FF = -1) Appears to be measured in pixels. It is unknown what it is realative to. Note that since this is the final X location, greater values mean greater speed (of course 7f > 80 because it is signed)
 DD = Binary value. 0x80 = end of letter. 0x01 = more than one tile. Seemingly a 0x01 value will simply draw the tile stored directly to the right of the one in BB BB, as seen in the graphics. A 0x81 value will obviously be a synthesis of the two.

    0x21D008 | 08 CE - E entry:
    AA|BB BB|CC|DD
    10 52 30 04 00
    08 42 30 04 00
    08 40 30 F4 01
    00 32 30 04 00
    F8 22 30 04 00
    F8 20 30 F4 01
    F0 12 30 04 00
    E8 02 30 04 00
    E8 00 30 F4 81

    0x21D035 | 35 CE - A entry:
    AA|BB BB|CC|DD
    10 55 30 04 00
    08 45 30 04 00
    08 43 30 F4 01
    00 35 30 04 00
    F8 25 30 04 00
    F8 23 30 F4 01
    F0 15 30 04 00
    E8 05 30 04 00
    E8 03 30 F4 81

    0x21D062 | 62 CE - R entry:
    AA|BB BB|CC|DD
    10 58 30 04 00
    08 48 30 04 00
    08 46 30 F4 01
    00 38 30 04 00
    F8 28 30 04 00
    F8 26 30 F4 01
    F0 18 30 04 00
    E8 08 30 04 00
    E8 06 30 F4 81

    0x21D08F | 8F CE - T entry:
    AA|BB BB|CC|DD
    10 5B 30 04 00
    08 4B 30 04 00
    08 49 30 F4 01
    00 3B 30 04 00
    F8 2B 30 04 00
    F8 29 30 F4 01
    F0 1B 30 04 00
    E8 0B 30 04 00
    E8 09 30 F4 81

    0x21D0BC | BC CE - H entry:
    AA|BB BB|CC|DD
    10 5E 30 04 00
    08 4E 30 04 00
    08 4C 30 F4 01
    00 3E 30 04 00
    F8 2E 30 04 00
    F8 2C 30 F4 01
    F0 1E 30 04 00
    E8 0E 30 04 00
    E8 0C 30 F4 81

    0x21D0E9 | E9 CE - O entry:
    AA|BB BB|CC|DD
    08 40 31 FC 01
    F8 20 31 FC 01
    E8 00 31 FC 01
    10 5F 30 F4 00
    08 4F 30 F4 00
    00 3F 30 F4 00
    F8 2F 30 F4 00
    F0 1F 30 F4 00
    E8 0F 30 F4 80

    0x21D116 | 16 CF - U entry:
    AA|BB BB|CC|DD
    10 54 31 04 00
    08 44 31 04 00
    08 42 31 F4 01
    00 34 31 04 00
    F8 24 31 04 00
    F8 22 31 F4 01
    F0 14 31 04 00
    E8 04 31 04 00
    E8 02 31 F4 81

    0x21D143 | 43 CF - N entry:
    AA|BB BB|CC|DD
    10 57 31 04 00
    08 47 31 04 00
    08 45 31 F4 01
    00 37 31 04 00
    F8 27 31 04 00
    F8 25 31 F4 01
    F0 17 31 04 00
    E8 07 31 04 00
    E8 05 31 F4 81

    0x21D170 | 70 CF - D entry:
    AA|BB BB|CC|DD
    10 5A 31 04 00
    08 4A 31 04 00
    08 48 31 F4 01
    00 3A 31 04 00
    F8 2A 31 04 00
    F8 28 31 F4 01
    F0 1A 31 04 00
    E8 0A 31 04 00
    E8 08 31 F4 81