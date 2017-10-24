# PSI ANIMATION

0C3019 to 0CF67E = PSI animation arrangements.
    OTHER READING:
    [CCF04D_PSI_ANIM_CFG.md](../structures/CCF04D_PSI_ANIM_CFG.md)
    [CCF58F_PSI_ANIM_POINTERS.md](../structures/CCF58F_PSI_ANIM_POINTERS.md)

Compressed arrangements. The decompressed format is very simple - 1 byte / 8x8 tile.
There are 1024 bytes per frame: the first 64 (corresponding to the first two lines) and the last 64 (corresponding to the last two lines) are never seen. So the data is for 32x32, but you see 32x28. :P

PSEUDOCODE:

    while (counter < length of decompressed area divided by 1024) {
      while (y < 30) {
        while (x < 32) {
          get tile (y * 32 + x)
          draw at location 8*x, 8*y
          increment x
        }
        reset x
        increment y
      }
      y = 2 // Skip first two lines, used instead of 0-28 to make the "get tile" step non-evil
      increment counter
    }

Also located within this section: PSI tile graphics!

    $CCAC25 to $CCAF2E = PSI tile graphics.
    $CCB613 to $CCBAC7 = PSI tile graphics.
    $CCDB27 to $CCDDF8 = PSI tile graphics.
    $CCE31D to $CCE56C = PSI tile graphics.

And also, a table: 34 entries, 12 bytes per entry. See CF24D.txt for full details.
- `$CCF04D to $CCF1E4 = PSI animation table`

$CCF47F to $CCF58E = PSI graphics palettes.
- Standard palettes; BGR 15-bit.

$CCF58F to $CCF616 = PSI animation pointers.
- Standard four-byte pointers to the arrangements.