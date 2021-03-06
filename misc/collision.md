# Map Collision Data

Map Collision is controlled by a great variety of data blocks, includin a pointer table to a pointer table.  Scary.  Well, here it is:

Involved ranges:
- 180200 to 18914F = Map tile collision data
- 189150 to 18F25D = Map tile collision pointer table
- 2F137B to 2F13CA = Map tile properties pointers

Now, we're going to work through these backwards, because it's going to make a BIT more sense.

- Map tile properties pointers formatting: AA AA AA 00

Another no-frills pointer table.  There are 20 pointer here, one for each graphical tileset.  These pointers link into the Map tile collision pointer table.

- Map tile collision pointer table formatting: AA AA

This is a large string of 2-byte pointers.  The pointer value is 0x(AAAA) + 0x180200.  Each pointer represents one tile.  The game seems to be programmed so only used tiles have pointers - there are no null ones for the unused tiles at the end of each set.  Still, it's just a huge string, so if you encounter the first unused tile, it will just read the next collision pointer in line - typically the first of the next tileset.

- Map tile collision data formatting:
`AA BB CC DD EE FF GG HH II JJ KK LL MM NN OO PP`

This is the data that actually determines collision.  The data for one tile exists in a 16 byte chunk.

    AA|BB|CC|DD
    --+--+--+-- <-- Each byte indicates the permeability status of
    EE|FF|GG|HH     one minitile *position* within the tile.  This
    --+--+--+--     is all very dynamic and nothing is fixed based
    II|JJ|KK|LL     upon graphical data.
    --+--+--+--
    MM|NN|OO|PP

Known collision values
- 00 = No collision
- 01 = Potentially related to transparent graphics
- 08 = Shallow water
- 80 = Can't walk through
- 90 = Potentially related to door object