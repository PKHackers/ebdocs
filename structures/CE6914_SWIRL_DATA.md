# $CE6914 - Swirl Data

$CE6914 controls battle swirl animation entries.

`[AA BB (CC) DD (EE) FF (GG) 00]`

- AA = 01 or 04. If 01, the game will drop down a line every time a line entry is processed. As far as I know, 01 doesn't handle things with spaces. 04 drops down if the next line is further to the left than the current, or on `[FF 00]`. Also affects CC and GG.
- BB = Distance from the top to start drawing from.
- CC = Not really sure. If AA is 01, put `[FF 00]` here. If AA is 04, put `[FF 00 FF 00]` here.
- DD = Number of lines to draw, plus 0x80.
- EE = Lines. See the more complete description below.
- FF = Unknown; I think it's space at the bottom to have clear. I'll have to check. In the meantime, use 64.
- GG = Same as CC.
- 00 = End.

## LINE DOCUMENTATION: AA = 01

`[XX YY]`

- XX = Start of horizontal line, measured in pixels from the left. 0x00 is far left, 0x80 is midscreen, 0xFF is far right.
- YY = End of horizontal line. Uses the same coordinate pattern as above.

After each line entry, the game jumps down a line; 01 can't handle stuff with spaces.
There's also a way that lines can be processed repeatedly. Don't know how it works yet though (example is seen in frame 1 of the normal battle swirl).

## LINE DOCUMENTATION: AA = 04


`[XX YY FF 00]`

- XX = Start of horizontal line. Same old coordinate pattern.
- YY = End of horizontal line. Same old coordinate pattern.
- FF 00 = Jump down a line.

This pattern is used for no-spaces stuff. But if you need gaps, you'll need to use the pattern below.

`[WW XX YY ZZ]`

- WW = Start of horizontal line #1.
- XX = End of horizontal line #1.
- YY = Start of horizontal line #2.
- ZZ = End of horizontal line #2.

For example, `[60 70 90 A0]`. This would draw a line from 0x60 to 0x70, and other on the same line from 0x90 to 0xA0. 0x71 through 0x8F would be untouched. After this it automatically jumps down a line. You can't have more lines on the same line - for example, `[60 61 70 71 80 81]` would draw a line from 0x60 to 0x61, 0x70 to 0x71, drop down a line automatically and THEN draw 0x80 to 0x81. This kills most attempts at lettering, and is also the reason that pkswirl.ips is animated.