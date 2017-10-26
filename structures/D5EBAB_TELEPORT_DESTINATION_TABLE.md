# $D5EBAB - Teleport Destination Table
Original Author: michael_cayer

Data Table for `[1F 21 XX]` control code


## [1F 21] TELEPORT DATA

Lying between $D5EBAB and $D5F2F2 is the data for all the [1F 21 XX] codes from 00 to E8. It seems anything from E9 to FF was never meant to be accessed by this code.

Finding entries: $D5EBAB + 0x8 * entry number. So entry 16 is $D5EBAB + 0x8 * 0x16; that is, $D5EC5B is the entry for 1F 21 16.

### Data structure

`AA AA BB BB CC DD EE EE`

- AA = X coordinate. This uses the same coordinate system as PSI Teleport.
- BB = Y coordinate.
- CC = What way to face after warping.
- DD = Warp style. This uses entries from $D01400; the effect list below remains for no real reason.
    - 00 = The screen goes black instantly and fades in, standard-door style.
    - 01 = Standard door-style warp.
    - 02 = Standard door-style warp, but the screen turns white instead of black.
    - 03 = Standard door-style warp.
    - 04 = Standard door-style warp, with a door sound.
    - 05 = Touching a hole warp - falling sound and the screen slides up.
    - 06 = Standard door-style warp.
    - 07 = Standard door-style warp, with a door sound.
    - 08 = Standard door-style warp, with a door sound.
    - 09 = Standard door-style warp, with a door sound.
    - 0A = Standard door-style warp.
    - 0B = ...Uhm, freaky. Try it for yourself, you'll see what I mean.
    - 0C = Standard door-style warp.
    - 0D = Standard door-style warp.
    - 0E = Standard door-style warp, but the screen turns white instead of black.
    - 0F = Standard door-style warp, but makes a weird sound.
    - 10 = Phase Distorter III-like warp.
    - 11 = Moonside style warp.
    - 12 = Moonside style warp.
    - 13 = Moonside style warp.
    - 14 = Moonside style warp.
    - 15 = Moonside style warp.
    - 16 = Moonside style warp.
    - 17 = Moonside style warp.
    - 18 = Moonside style warp.
    - 19 = Moonside style warp.
    - 1A = Moonside style warp.
    - 1B = Moonside style warp.
    - 1C = Standard door-style warp.
    - 1D = Standard door-style warp, but the screen turns white instead of black. Slower than 02.
    - 1E = Same as 1D. Makes a weird sound.
    - 1F = Ghost-filled tunnel warp.
    - 20 = Ghost-filled tunnel warp.
    - 21 = Touching a hole warp - falling sound and the screen slides up.
- EE = Unknown.
