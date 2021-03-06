# Enemy Placement

Involved databanks:
- 101A80 to 10BA7F = Enemy Placement data
- 10BA80 to 10BDAB = Enemy Placement groups pointer table
- 10BDAC to 10C80C = Enemy Placement groups data
- 10C80D to 10D72F = [1F 23 XX XX] Battle Entries Pointer Table
- 10D74C to 10E1B3 = [1F 23 XX XX] Battle Entries

- Enemy Placement Data Formatting: AA 00

The data here is stored in two-byte increments.  Each two bytes represents a 2-tile block (horizontally arranged) for which placement is indicated.  The value for AA indicates a particular enemy group that will appear in the block.  That value can range from 00 (none) to 0xCB - the value being an entry in the Enemy Placement groups pointer table at $D0B880.

- Enemy Placement Group Data Formatting: XX XX XX 00

This is a standard, no frills 4-byte pointer table.  It links to the Enemy Placement groups data blocks.  Each entry in this table is equivalent to a byte value in the Enemy Placement Data.

## Enemy Placement Group Data Formatting
`AA AA ?? ?? [(BB CC CC) (DD EE EE)]`

Now this is the good shit.  As far as we can tell, this appears to be some sort of probability table for how the enemies appear.  More on how that works in a second.

`[40 00 0A 00 03 08 00 02 09 00 02 0A 00 01 0B 00 0F 00 00]`
(This is an example of the said code.)

- `AA AA` stores an event flag value.  This is used to toggle between the BC and DE enemy probability cluster subgroups.  Don't worry, just one more second.  In this case, the flag used is 0x40.

- ?? ?? is an unknown value.  It COULD - but doesn't seem to - indicate the number of probability clusters.  It's a possiblity, however, that it indicates a coordinate set within the 2-tile area.  In this case, the data is 0A 00.

- `[(BB CC CC) (DD EE EE)]`: Enemy Probability Clusters
Okay, this is the real meat of this table.  There are two groups of these clusters, the BB CC CC subgroup and the DD EE EE subgroup.  It's a bit confusing until you actually look at some examples...  bear with me for a second.  Each individual cluster indicates a particular enemy that will appear in this group, and the probability that it will do so.

`CC CC` and `EE EE`, I should mention first, indicate the enemy to be fought.  This is indicated via that `[1F 23]` battle table.  Convenient, no?  :P

`BB` and `DD` indicate the appearance probability for this enemy.  This probability acts BB/8.  Now, the sum of the probabilites of all of the clusters must add up to 8.  Confused?  Don't worry.  See, it goes like this:

This enemy placement group has four possibilities in its `(BB CC CC)` subgroup.

`A 3/8 chance of Skate Punk/Yes Man Jr. (03 08 00)`
`A 2/8 chance of Skate Punk/Pogo Punk.  (02 09 00)`
`A 2/8 chance of Pogo Punk/Yes Man Jr.  (02 0A 00)`
`A 1/8 chance of all three Shark types. (01 0B 00)`

This constitutes the whole of the `[BB CC CC]` subgroup.  See, the total appearance chances add up to 8/8.  Now, next up is the second subgroup - the `[DD EE EE]` subgroup.  This indicates the enemy probability appearances when the main group's flag is toggled.

`A 15/8 chance of no enemies.           (0F 00 00)`

It seems a bit odd...  I don't remember the chance exceeding 8.  But still, this indicates something important.  If the flag above (0x40) is set, the game will use this subgroup for enemy appearances.

This group, in case you're wondering, is the primary enemy placement for the Sharks around Onett.  If flag 0x40 is set, they don't show up any more.  Now you know why... maybe.

## [1F 23 XX XX] Battle Entries Pointer Table Formatting
`AA AA AA 00 BB BB CC DD`

[1F 23 XX XX] is the battle-initating control code.  It also is used for enemy identification in the placement groups.

- `AA AA AA 00` is a pointer to a block in the `[1F 23 XX XX]` Battle Entries.  Simple, no frills pointer.
- `BB BB` indicates an event flag.  When this flag is set, this enemy will always run from you.
- `CC` is flag usage.  Assuming there's a flag in use:
    - 00 = Run away while the flag is unset.
    - 01 = Run away while the flag is set.
- `DD` seems to indicate the size of the battle "letterbox."  Values range from 00 to 03.

## [1F 23 XX XX] Battle Entries Formatting:
`(AA BB BB) FF`

These entries are as simple as it gets.  `(AA BB BB)` is repeated as many times as one desires.  FF is exactly that - 0xFF and indicates the end of the `[1F 23]` group.

`AA` is a numeric value, indicating how many enemies there are.
`BB BB` is also a numeric value, for the enemy that will appear.  It uses the standard PK Hack listing.