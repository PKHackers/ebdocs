# $CADCA1 - Battle Background Data Table

17-byte entries
`[AA BB CC DD EE FF GG HH II JJ KK LL MM NN OO PP QQ]`



- AA - Graphics and arrangement.
    - Corresponds to a graphics pointer from the $CAD7A1 table, as well as an arrangement pointer from the $CAD93D table.

- BB - Palette.
    - Corresponds to a palette pointer from the $CADAD9 table.

- CC - Bits per pixel.
    - Always 04 or 02.

- DD - UNKNOWN
    - (Related to the direction/style of cycling?)
    - This byte gets stored into $7EADD7, which is parsed at $C2C988.
    - Code branches:
        - 01 = $C2CA3B
        - 02 = $C2C9A5
        - 03 = $C2CAD9
        - Otherwise, $C2CBA5.



- EE - Palette cycle #1: First $7E0240 palette.
- FF - Palette cycle #1: Last $7E0240 palette.
- GG - Palette cycle #2: First $7E0240 palette.
- HH - Palette cycle #2: Last $7E0240 palette.

    - The format of all of these is the same. Some examples:
        - 08 --> 2 * 08 = 10 --> $7E0240 + 10 --> Palette at $7E0250
        - 0F --> 2 * 0F = 1E --> $7E0240 + 1E --> Palette at $7E025E
    - Typical values are between 00 and 0F.


II - Palette changing speed.
    - High values are slow and low values are fast.
    - 00 means no cycling.

JJ - Scrolling movement #1.
KK - Scrolling movement #2.
LL - Scrolling movement #3.
MM - Scrolling movement #4.
    - The scrolling movement bytes use the [$CAF258](CAF258_BG_SCROLLING_TABLE.md) table.

- NN - Distortion #1.
- OO - Distortion #2.
- PP - Distortion #3.
- QQ - Distortion #4.
    - The distortion bytes use the [$CAF708](CAF708_BG_DISTORTION_TABLE.md) table.

The scrolling movements and distortions are cycled through in some way. Details are still being researched.