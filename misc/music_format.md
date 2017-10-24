# The Format of Music Blocks

Created by michael_cayer on Mar 30 2005, 21:28 EST
Last updated by michael_cayer on Mar 30 2005, 21:28 EST

Music blocks are pointed at from a pointer table at 0x4FB47. This table
is odd internally; the addresses are ordered <high> <low> <middle>. For
example, C58000 would be represented as C5 00 80.

These blocks consist of:
 - Two bytes that indicate a length, hereafter called "len".
 - Two bytes that indicate an address in the SPC700's memory, hereafter
   called "spcroot".
 - A block of bytes of length "len", which is dumped into the SPC700's
   memory, starting at the address "spcroot".

For example:
`05 00 00 48 vv ww xx yy zz`

  Len is "05 00", or 0x5.
  Spcroot is "00 48", or $4800.
  $4800 = vv, 4801 = ww, and so on to 4804 = zz.

# The Format of the Block of Bytes

The block of bytes is composed of byte pairs that indicate addresses in
the SPC700's memory. These pairs continue until a 00 00 is reached.
One special code is FF 00, which indicates a jump.

This is the start of the block of bytes for the Department Store theme,
which is dumped to $4800 in the SPC700's memory:

    $4800 = 2E 48
    $4802 = 0E 48
    $4804 = 1E 48
    $4806 = 3E 48
    $4808 = FF 00 <-- Loop code. Jump back...
    $480A = 02 48 <-- ...to 4802.
    $480C = 00 00 <-- End

These values are pointers to blocks of 16 (0x10) bytes, which in turn are
pointers, one for each of the eight sound channels. Returning to the
Department Store theme example, the game would go to $482E in SPC memory,
retrieve the eight pointers, and parse them simultaneously. It would then
continue to 480E's pointers, then 481E and 483E in turn. Finally, it would
jump back to $4802 and parse 480E's pointers again, creating a loop.

The values pointed to in the groups of eight pointers are parsed as described
in [hotelspcstuff.md](hotelspcstuff.md).