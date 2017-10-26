# $D01400 - Screen Transition Config
Original Author: michael_cayer

This pattern is used for the "warp style" of `[1F 21 xx]` data entries, and the "style" byte of door destinations.

-------------------------------------------------------------------------------------

`[AA BB CC DD EE FF GG HH II JJ KK LL]`

- AA = Speed (start); higher numbers = longer duration. 0xFF = ~16 seconds.
- BB = $CE6914 animation to use (start).
    - 00 = N/A
    - 01 = Normal battle swirl
    - 02 = Phase Distorter ring
    - 03 = Boss battle swirl
    - 04 = Shield effect (ex. Shield, PSI Shield)
    - 05 = Enemy PSI
    - 06 = Giygas Battle phase switch
- CC = $CE6914 animation options (start).
    - 00 = Show once
    - 80 = Show repeatedly, going successively faster while time remains
    - There are probably more options...
- DD = Fade style.
    - 00 = Fade to black
    - 31 = ???
    - 64 = Fade to white
    - There are probably more options...
- EE = Direction for "slide" movements (tunnel ghosts, holes, Warp Men). 00 = North; 08 = Northeast; 10 = East; and so on (0x08 = 45 degrees). Unsure of effect of values above 3F, though they probably just wrap around to 00.
- FF = E8 for ghosts and 00 for everything else; no changes were seen on corruption, so this one is still unknown.
- GG = "Slide" speed - 00 is no slide (normal doors, Phase Distorter), higher values mean faster slide movement.
- HH = Sound effect (start). Uses the `[1F 02 xx]` listing. In a hole warp, this would be the "dropping" sound.
- II = Speed (end). See AA.
- JJ = E6B14 animation to use (end). See BB.
- KK = E6B14 animation options (end). See CC.
- LL = Sound effect (end). Uses the `[1F 02 xx]` listing. In a hole warp, this would be the "landing" sound.

## ENTRIES

    0x00 - 00 00 00 00 00 00 00 00 00 00 00 00
    0x01 - 14 00 00 00 00 00 00 00 14 00 00 00
    0x02 - 14 00 00 64 00 00 00 00 3C 00 00 00
    0x03 - 14 00 00 00 00 00 00 00 14 00 00 00
    0x04 - 14 00 00 00 00 00 00 00 14 00 00 09
    0x05 - 1E 00 00 00 00 00 10 11 14 00 00 2E
    0x06 - 14 00 00 00 00 00 00 00 14 00 00 70
    0x07 - 14 00 00 00 00 00 00 08 14 00 00 09
    0x08 - 14 00 00 00 00 00 00 08 3C 00 00 09
    0x09 - 14 00 00 00 00 00 00 08 3C 00 00 09
    0x0A - 14 00 00 00 00 00 00 08 14 00 00 00
    0x0B - 64 05 00 31 00 00 00 00 64 05 00 00
    0x0C - 14 00 00 00 00 00 00 70 14 00 00 00
    0x0D - 14 00 00 00 00 00 00 70 14 00 00 70
    0x0E - 14 00 00 64 00 00 00 00 14 00 00 00
    0x0F - 14 00 00 00 00 00 00 12 14 00 00 00
    0x10 - FF 02 80 64 00 00 00 00 3C 02 01 00
    0x11 - 3C 00 00 00 26 00 0C 0C 14 00 00 00
    0x12 - 3C 00 00 00 07 00 0C 0C 14 00 00 00
    0x13 - 3C 00 00 00 3B 00 0C 0C 14 00 00 00
    0x14 - 3C 00 00 00 3D 00 0C 0C 14 00 00 00
    0x15 - 3C 00 00 00 3D 00 0C 0C 14 00 00 00
    0x16 - 3C 00 00 00 07 00 0C 0C 14 00 00 00
    0x17 - 3C 00 00 00 05 00 0C 0C 14 00 00 00
    0x18 - 3C 00 00 00 30 00 0C 0C 14 00 00 00
    0x19 - 3C 00 00 00 16 00 0C 0C 14 00 00 00
    0x1A - 3C 00 00 00 26 00 0C 0C 14 00 00 00
    0x1B - 3C 00 00 00 1B 00 0C 0C 14 00 00 00
    0x1C - 14 00 00 00 00 00 00 0C 14 00 00 00
    0x1D - 78 00 00 64 00 00 00 00 64 00 00 00
    0x1E - 64 00 00 64 00 00 00 64 3C 00 00 00
    0x1F - 64 00 00 00 10 E8 07 60 14 00 00 53
    0x20 - 64 00 00 00 30 E8 07 60 14 00 00 53
    0x21 - 1E 00 00 00 00 00 10 11 14 00 00 00