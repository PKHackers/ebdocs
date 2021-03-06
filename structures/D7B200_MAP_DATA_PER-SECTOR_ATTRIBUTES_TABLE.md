# $D7B200 - Sector-based map attributes table

This table controls specific sector attributes for the map areas in EarthBound.  Every 2 bytes controls the data for an 8x4 tile area (or 'sector') on the map. This data includes graphics effects, the town map to use, exit mouse compatibility, Type-58 item usage, and other such components of the game.

Each entry is made up of two bytes:

## Byte 1

Byte 1 stores its data in binary, and contains most of the detailed information in the table.  An easy way to find the hex equivalent without converting from binary is to just find the sum of the bit numbers you want activated.  (I.E. 80, 40, and 08 - 11001000 - would equal 0x80 + 0x40 + 0x8 - 0xC8.)

[80 40 20 10 | 08 04 02 01]

* Bit 0x80 - Teleportation inhibitor.
    * [X---|----]
    * PSI Teleport may not be used in this sector if this bit is toggled on.

* Bit 0x40 - Unknown
    * [-X--|----]
    * As far as we can tell, this bit is unused.  We believe it may have been part of the town map section below.

* Bits 0x20, 0x10, 0x08 - Controls town map data.   There are 8 different possible settings to pick.
    * [--XX|X---]
    * 000 - No Map      100 - Fourside
    * 001 - Onett       101 - Scaraba
    * 010 - Twoson      110 - Summers
    * 011 - Threed      111 - No Map


* Bits 0x04, 0x02, 0x01 - Controls miscellaneous settings for the map.  There are, again, 8 possible settings for this data.
    * [----|-XXX]
    * 000 - No special settings
    * 001 - Area is indoors
        * Indoor areas indicate that you appear in your last position on the town map from when you went through a door. It is possible - though unknown - that setting 111 indicates frequent plus the effect of bit 0x01.  More testing is required to ascertain this.
    * 010 - Exit Mice will function
    * 011 - Lost Underworld sprites
    * 100 - Magicant sprites
    * 101 - Robotomized sprites
    * 110 - Super-frequent Magic Butterflies
    * 111 - Super-frequent Magic Butterflies


## Byte 2
Byte 2 is an item number.  Items in EB all have a specific item type number that indicates how they are used in the game.  Type 58 items are special in that they can not be used if the player is not in a specific area.  This byte controls that function.

To allow a player to use a type 58 item in a sector, you must program the item's number (displayed next to its name in PK Hack) into Byte 2.  See below for some examples of how this byte is used (as well as the other settings in general.)


Examples of default settings:
----------------------------------------------------
`00 00 = [0000|0000][00]` = Wide variety of places
   - No settings.

`00 AF = [0000|0000][AF]` = Deep Darkness
   - Item #AF (Hawk Eye) may be used in this area.

`00 B0 = [0000|0000][B0]` = Peaceful Rest Valley
   - Item #B0 (Bicycle) may be used in this area.

`03 00 = [0000|0011][00]` = Lost Underworld
   - Lost Underworld sprites are active.

`08 B0 = [0000|1000][B0]` = Onett
   - Onett shown in Town Map
   - Item #B0 (Bicycle) may be used in this area.

`10 B0 = [0001|0000][B0]` = Twoson & Happy Happy Village
   - Twoson shown in Town Map
   - Item #B0 (Bicycle) may be used in this area.

`18 00 = [0001|1000][00]` = Threed
   - Threed shown in Town Map

`20 00 = [0010|0000][00]` = Fourside
   - Fourside shown in Town Map

`28 00 = [0010|1000][00]` = Scaraba
   - Scaraba shown in Town Map

`30 00 = [0011|0000][00]` = Summers
   - Summers shown in Town Map

`80 00 = [1000|0000][00]` = Ness flashback house
   - Teleport inhibited

`81 00 = [1000|0001][00]` = Indoor areas, no map
   - Teleport inhibited
   - Indoor area

`82 00 = [1000|0010][00]` = Caves
   - Teleport inhibited
   - Exit mouse compatible

`84 00 = [1000|0100][00]` = Magicant
   - Teleport inhibited
   - Magicant sprites

`85 00 = [1000|0101][00]` = Devil's Machine, Cave in the Past
   - Teleport inhibited
   - Robotomized sprites
   - Indoor area

`89 00 = [1000|1001][00]` = Indoor areas, Onett
   - Teleport inhibited
   - Onett shown in Town Map
   - Indoor area

`8A 00 = [1000|1010][00]` = Giant Step caves
   - Teleport inhibited
   - Onett shown in Town Map
   - Exit mouse compatible

`91 00 = [1001|0001][00]` = Twoson Dept Store
   - Teleport inhibited
   - Twoson shown in Town Map
   - Indoor area

`99 00 = [1001|1001][00]` = Indoor areas, Threed
   - Teleport inhibited
   - Threed shown in Town Map
   - Indoor area

`99 AE = [1001|1001][AE]` = Zombie Relief Corps tent
   - Teleport inhibited
   - Threed shown in Town Map
   - Indoor area
   - Item #AE (Zombie Paper) may be used in this area.

`9A 00 = [1001|1010][00]` = Underground Road caves
   - Teleport inhibited
   - Threed shown in Town Map
   - Exit mouse compatible

`A1 00 = [1010|0001][00]` = Indoor areas, Fourside
   - Teleport inhibited
   - Fourside shown in Town Map
   - Indoor area

`A2 00 = [1010|0010][00]` = Fourside sewers
   - Teleport inhibited
   - Fourside shown in Town Map
   - Exit mouse compatible

`A9 00 = [1010|1001][00]` = Scarabian indoor areas
   - Teleport inhibited
   - Scaraba shown in Town Map
   - Indoor area

`B1 00 = [1011|0001][00]` = Indoor areas, Summers
   - Teleport inhibited
   - Summers shown in Town Map
   - Indoor area
