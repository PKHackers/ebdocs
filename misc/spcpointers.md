    <Penguin> 0x1F7 - $00F7 - What song is playing
    <Penguin> 0x1F9 - $00F9 - Sound effect to be played(see the sfx list for which sound is which)

    <Penguin> $2E4A - $47FF: Music Data pointer table (points to the pointer table for music data)
    <Penguin> Note: SPC pointers are different than EB's pointers. it's simply +0x100, if you're looking in an spc, and they're only 2 bytes instead of 3, due to the SPC only having 64K of memory.
    <Penguin> the first 16 bytes of music data is a table.
    <Penguin> 8 entries, each 2 bytes long.
    <Penguin> they're pointers that tell which part of a song is played.
    <Penguin> They each point to another table, which also has 8 entries, 2 bytes each.
    <Penguin> These pointers tell the program which music data to use for each channel.


    <Penguin> if any of the pointers at the beginning of the music data's header point to an area inside the header, that means to loop that part and any parts after it.
    <Penguin> EX: (0x4900) C9 48 B9 48 A9 48 99 48 02 48 00 00 would mean to loop parts "B9 48", "A9 48", and "99 48" over and over again.
    <Penguin> and a "0000" should mark the end of said header.
