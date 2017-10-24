# $7E8650 - Window Stats Table

0x52 BYTE STRUCTURE

Seems to be ordered by windows-opened; 7E8650 is the first (oldest) window, 7E86A2 is the second, etc. Messing around with this can cause freezing.

- 00 - 03: ?
- 04 - 05: Window entry in 7E88E4 table.
- 06 - 07: Text box's top left corner: X coordinate
- 08 - 09: Text box's top left corner: Y coordinate
- 0A - 0B: Text box width
- 0C - 0D: Text box height
- 0E - 0F: In the current text box, the X coordinate where text is being drawn.
- 10 - 11: In the current text box, the Y coordinate where text is being drawn.
- 12: [1C 09 XX] writes a value to this...
- 13: ?
- 14: Related to graphics minitiles? (Setting it to 02 gets you status effect symbols, etc)
- 15: Font selector. Above 04 is glitched.
    - 00 = Normal
    - 01 = Saturn
    - 02 = Small Font #1
    - 03 = Small Font #2 (has some glitching...)
    - 04 = Coffee Sequence
- 16: ?
- 17 - 1A: Working memory
- 1B - 1E: Argumentary memory
- 1F - 20: Secondary memory
- 21 - 24: Working memory storage
- 25 - 28: Argumentary memory storage
- 29 - 2A: Secondary memory storage