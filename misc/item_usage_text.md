This area seems to load the pointers for item usage text. It's organized by the item type.

## TOP LEVEL: $C1AFDC

    <Michael1> Type 0n = Store 0x1 at $02/24, load the root of battle effects to $0A/0C, get the 1Dth byte of an item entry (battle effect presumably), multiply by 12 and add four for the text pointer, stash it in $06/08 and $26/28.
    <Michael1> Type 1n = Load the address of "The ___ is one of the items that can be equipped." to $06/06 and $26/28.
    <Michael1> Type 2n = Same code as Type 0n.
    <Michael1> (0n, 1n, 2n = 0x00 + n, 0x10 + n, 0x20 +n for item type.)
    <Michael1> This code works with text, evidently.
    <Michael1> Type 3n = $04 is transferred to X and decremented, then used to get one of four values (01, 02, 04, 08) from a microtable at 45AAB. ($04 = 01-04 evidently). The accumulator is ANDed by the 1Cth byte of the chosen item. If nonzero, go to B0AF... (I think this is usage checking.)
    <Michael1> B09A = can't use, B0AF = can use, under current theory...
    <Michael1> B09A: Load "___ could not use the ___ very well." to $06/08 and $26/28.
    <Michael1> B0AF: Continue parsing.
    <Michael1> B28C = Common point code goes to after loading a text pointer to $06/08 and $26/28.

## SUB LEVEL: $C1B0B2

    <Michael1> Continued parsing, Type C: Same as 0n and 2n.
    <Michael1> Continued parsing, Type 4: Load "The ___ can't be used here." to $06/08 and $26/28.
    <Michael1> Continued parsing, Type 8: Even more parsing.

## LOW LEVEL: $C1B117

    <Michael1> Final parsing, Type 0: Same as 0n and 2n.
    <Michael1> Final parsing, Type 1: Same as 0n and 2n.
    <Michael1> Final parsing, Type 2: A bunch of code to deal with the Bicycle, and the 0n/2n ASM + a loading of "The ___ can't be used here." for the other items.
    <Michael1> Final parsing, Type 3: C1B1F2.txt