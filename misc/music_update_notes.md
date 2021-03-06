# MUSIC NOTES (pun unintentional)

This is a note file; stuff here is largely speculative. Stuff gets more accurate the farther down you go.

081D: Confirmed as comparing against a pointer to a channelset - checking for the end, it would seem. (BNE $0841)
0841: Copy the pointers in the channelset to $0030 + Y, where Y = [0-F].

From here it seems to divide, one way being 0860, the other 095F. $0047 changes. A LOT. Traces confirm this. I think it holds powers of two, related  to which pointer-within-channelset is being parsed...

I suspect 0860 is the end case, in which case, 095F is the pointer-within-channelset parser. Alternately, it could be that 0860 is the note case, and 095F does commands. Yes, rampant speculation, whatever. More research needed.

Assuming this to be true...
 0963: BPL. Less-than-0x80 cases. Duration?
 0966: SBC. Subtract 0xCA...

There is a small loop at 099D.

If I'm reading this right, it does stuff for one pointer-within-channelset at a time.
And 095F does the parsing stuff that needs to be known. O_o

What is stored at $0211+X? It's written to at $095F.
What is stored at $005F? It's read from in the $095F routine...
Something is multiplied by six and tossed to $14? O_o?

Maybe another trace is in order. Trace to $0750, then step via F7 the rest of the way. SPCtool is your friend.

There are a lot of short routines in the $0A00s.

There are calls to $0955, which seems to increment the $0030 value (and $0031 as appropriate, every 256 turns.) It returns with A and Y equal to the address as it was before the increment.

$0330, $0350... what are these?

Look at $0B12. It seems odd.

    0B12: 3F 55 09    CALL     $0955
    0B15: 8D 08       MOV      Y, #$08
    0B17: CF          MUL      YA
    0B18: 5D          MOV      X, A
    0B19: 8D 0F       MOV      Y, #$0F
    0B1B: F5 88 0E    MOV      A, $0E88+X
    0B1E: 3F 49 07    CALL     $0749

Multiply the current location by 0x8. Transfer the low order of this result to X.
Y, the higher order becomes 0xF. Then A = $0E88+X...
(Current location * 0x8) mod 0x100... 32 possibilities `[00-1F]`.
So $0E88-$0EA7 is some kind of one-byte-per-entry table.
Then call $0749... this sets filter designation for each voice. Good. Now what IS that? :P

        8  9  A  B  C  D  E  F  0  1  2  3  4  5  6  7
      ------------------------------------------------
0E88 | 7F 00 00 00 00 00 00 00 58 BF DB F0 FE 07 0C 0C
0E98 | 0C 21 2B 2B 13 FE F3 F9 34 33 00 D9 E5 01 FC EB

--------------------------------------------------------------------------------------------
X is (2 * channel number), where channel number is (0-7).

0070+X: Duration
0071+X: Duration * Style / 0x100 (if this resolves to 0, it equals 0x1)

0200+X: Duration
0201+X: "Style" (see freq_tsv.txt)
0210+X: Volume


## MUSIC FORMAT - Parser at 0882

01-7F: These can be standalone duration changes, or duration changes with "style" and volume changes mixed in.
       Two forms:
       - `[01-7F] [80-FF]`: The `[80-FF]` is another code; stop parsing here, and go parse that as normal. It's probably a note.
       - `[01-7F] [00-7F]`: Now we get into the fun stuff.
           - Bits 4-6 of the argument set "Style".
           - Bits 0-3 of the argument set volume.
       You may note "Style" in quotes all the time. That's because I don't know what it is. It's described that way in freq_tsv.txt.

80-C7: Notes. The code is scary and I haven't looked at it much.
       It uses a table at $0EBD in some way, maybe messing with that would produce interesting results...

## Music works something like this...

1. Get a channelset.
2. Parse a channel, handling all non-note codes, finally terminating with a note code of some arbitrary duration.
3. Go to the next channel.
4. Repeat step 2 for that channel; continue until all channels have been handled. Iterate back to the start and continue looping around through the channels...
5. Check if the note on the current channel has concluded with each pass. If so, parse until the next note.
6. When all channels have no notes remaining, get the next channelset.

-------------------------------------------------------------------------------------------

Is FF an actual code? It doesn't look like it. Further notes will use E0-FE... if it IS a code, adjustments required will be minor.

E0-FE references a table: 0BE3 to 0C20.
Function: 0B23 and 0B24 + (current byte * 2 mod 0x100)
 So FE -> 0B23 and 0B24 + FC;
    FD -> 0B23 and 0B24 + FA;
    ...
    E0 -> 0B23 and 0B24 + C0.

Also, there's a reference to another table: 0C21 to C3F
Function: 0BC1 + (current byte mod 0x80).
 So FE -> 0BC1 + 7E;
    FD -> 0BC1 + 7D;
    ...
    E0 -> 0BC1 + 60.

...these programmers were brilliant. Scary, but brilliant. Values from those tables above are pushed onto the stack, then a RET instruction is parsed... which uses the stack to know where to go back to.

The 0BE3-0C20 table indicates where to go (address).
The 0C21-0C3F table indicates number of arguments.
Y and A hold the first argument of the code, or 0000 (if there are 0 arguments).

Given this...

|Code|Address|Args|
|----|-------|----|
| E0 |  095F |  1 |
| E1 |  09B8 |  1 |
| E2 |  09D6 |  2 |
| E3 |  09FD |  3 |
| E4 |  0A09 |  0 |
| E5 |  0A24 |  1 |
| E6 |  0A29 |  2 |
| E7 |  0A3B |  1 |
| E8 |  0A40 |  2 |
| E9 |  0A52 |  1 |
| EA |  0A55 |  1 |
| EB |  0A59 |  3 |
| EC |  0A65 |  0 |
| ED |  0A86 |  1 |
| EE |  0A8F |  2 |
| EF |  0AAC |  3 |
| F0 |  0A14 |  1 |
| F1 |  0A68 |  3 |
| F2 |  0A6C |  3 |
| F3 |  0A82 |  0 |
| F4 |  0AA8 |  1 |
| F5 |  0ACF |  3 |
| F6 |  0B03 |  0 |
| F7 |  0B0A |  3 |
| F8 |  0AE2 |  3 |
| F9 |  0B94 |  3 |
| FA |  0B72 |  1 |
| FB |  0B75 |  2 |
| FC |  0B79 |  0 |
| FD |  0B7E |  0 |
| FE |  0B7F |  0 |

E5:
 Y = argument, A = 0x0.
 Copy YA to $0058.
 End.

E6:
 Move arg1 into $005A.
 Move arg2 into $005B.
 <<< not yet worked out >>>
 End.

E7:
 Y = argument, A = 0x0.
 Copy YA to $0052.
 End.

E9:
 Copy argument into $0050.
 End.

EA:
 Copy argument into $02F0+X.
 End.

ED:
 Copy argument into $0301+X.
 Set $0300+X to 0.
 End.

EF:
 Move target address into $0240+X, $0241+X.
 Move iters into $0080+X.
 Move return location into $0230+X, $0231+X.
 Copy the target address to the current address.
 IMPLIED: Only one level of EF jumps is supported; no EF jumps within EF jumps.

F4:
 Copy argument into $0381+X.
 End.

F6:
 Null out $0060-63.
 Set bit 5 of $0048.
 End.

FA:
 Copy argument into $005F.
 End.

------------------------------------------------------------

    0050   : Pitch of entire song
0052 + 0053: Speed of entire song
0058 + 0059: Volume of entire song
    005F   : ??? of entire song

(These are all words; channel (0 = X as 0), (1 = X as 2), (2 = X as 4), (3 = X as 6), etc.)
0070+X: Duration of current note on channel X
0080+X: Number of iterations of subroutine to go on channel X
0230+X: Address to return to when finished subroutine stuff on channel X
0240+X: Address of subroutine on channel X
02F0+X: Pitch of channel X
0300+X: Volume of channel X
0380+X: Frequency of channel X? (see hotelspcstuff2.txt, F4 code)