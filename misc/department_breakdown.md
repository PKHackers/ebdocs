# Department Store ($C4F947 entry 0x14) Breakdown

Start at $EE1EFE.

The first word (FD 04) is length (0x4FD).
The second word (00 48) is the location in SPC700 memory to dump it ($4800).
  - Since .spcs have a 0x100 byte header, this would be at 0x4900 in the appropriate .spc

Now, parse words until you get a 0x0000.

    2E 48
    0E 48
    1E 48
    3E 48
    FF 00 <-- This shows up a lot. A special significance is theorized. Maybe "jump back to" <nextword>?
    02 48
    00 00

Analyze these, with $EE1EFE + 4 (+4 due to length and base location) as 4800.
$EE1F02 = 4800...

--- Up to here is pretty definite. How to use the stuff below is still unclear... ---

                                                           SOUND CHANNELS? (Not sure)
                                                      1    2    3    4    5    6    7    8
    ---------------------------------------------------------------------------------------
    482E: C54A DC4A E64A F04A FA4A 054B 104B 0000 = 4AC5 4ADC 4AE6 4AF0 4AFA 4B05 4B10 0000
    480E: 4E48 7C48 BE48 DF48 1E49 5349 6049 0000 = 484E 487C 48BE 48DF 491E 4953 4960 0000
    481E: 7549 BD49 E949 234A 4C4A 714A 994A 0000 = 4975 49BD 49E9 4A23 4A4C 4A71 4A99 0000
    483E: 1B4B 0000 2B4B 0000 484B 7C4B B44B 0000 = 4B1B 0000 4B2B 0000 4B48 4B7C 4BB4 0000

    NOTE: 487C - 4800 = 7C. $EE1F02 + 7C = 2E217E. When played with Music001, this sounds Departmentish. :o

What follows is rampant speculation.
The columns above are the channels. For a given time/bytespan, the game reads from one row, all columns (ex. 4AC5 4ADC 4AE6 4AF0 4AFA 4B05 4B10 0000). Then it goes to the next (484E 487C 48BE 48DF 491E 4953 4960 0000) and the next (4975 49BD 49E9 4A23 4A4C 4A71 4A99 0000) and next (4B1B 0000 4B2B 0000 4B48 4B7C 4BB4 0000). It then hits FF00, and jumps back to 4802 - which does (484E 487C 48BE 48DF 491E 4953 4960 0000), the second one, again.

It makes sense, in an infernal, doomish way.

Not entirely correct... keep experimenting...