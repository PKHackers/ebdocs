    /\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\
    \                        ._____.                              \
    /                  /-----|TABLE|------\                       /
    \                  | $CF0000 = Start  |                       \
    /                  | $CF264E = Finish |                       /
    \                  \------------------/                       \
    /                        ._____.                              /
    \         /--------------|STATS|---------------\              \
    /         | Total Bytes:       9806 (264E hex) |              /
    \         | Total Entries:     841  (349  hex) |              \
    /         | Total Entry Bytes: 9251 (2423 hex) |              /
    \         | Total Gap Bytes:   555  (22B  hex) |              \
    /         \------------------------------------/              /
    \                                                             \
    /                        .______.                             /
    \       /----------------|Format|------------------\          \
    /       | [AA AA AA AA BB BB CC CC DD DD EE]       |          /
    \       |------------------------------------------|          \
    /       | AA = pointer                             |          /
    /       | BB = flag                                |          /
    \       | CC = Y                                   |          \
    /       | DD = X                                   |          /
    \       | EE = style (using 1F 21 teleport styles) |          \
    /       \------------------------------------------/          /
    \                         .____.                              \
    / /-----------------------|GAPS|----------------------------\ /
    \ | OFFSET  to OFFSET    TYPE              LENGTH           | \
    / |---------------------------------------------------------| /
    \ | $CF0580 to $CF05AC - sign pointers     0x2C bytes long. | \
    / | $CF05D8 to $CF05F8 - sign pointers     0x20 bytes long. | /
    \ | $CF0624 to $CF0648 - unknown           0x24 bytes long. | \
    / | $CF0997 to $CF09B7 - unknown           0x20 bytes long. | /
    \ | $CF0ACA to $CF0AD2 - unknown           0x08 bytes long. | \
    / | $CF0DDF to $CF0DE7 - unknown           0x08 bytes long. | /
    \ | $CF0E34 to $CF0E58 - unknown           0x24 bytes long. | \
    / | $CF0EF2 to $CF0EF6 - unknown           0x04 bytes long. | /
    \ | $CF127C to $CF1280 - unknown           0x04 bytes long. | \
    / | $CF13A9 to $CF13AD - unknown           0x04 bytes long. | /
    \ | $CF1B74 to $CF1B80 - unknown           0x0C bytes long. | \
    / | $CF1B8B to $CF1BA3 - unknown           0x18 bytes long. | /
    \ | $CF1DB3 to $CF1DD3 - unknown           0x20 bytes long. | \
    / | $CF1DE9 to $CF1DF5 - unknown           0x0C bytes long. | /
    \ | $CF1F4A to $CF1F6C - unknown           0x22 bytes long. | \
    / | $CF20E2 to $CF20F7 - unknown           0x15 bytes long. | /
    \ | $CF21E9 to $CF2299 - Dungeon Man signs 0xB0 bytes long. | \
    / | $CF23B7 to $CF23C3 - unknown           0x0C bytes long. | /
    \ | $CF25A7 to $CF25BF - unknown           0x18 bytes long. | \
    / \---------------------------------------------------------/ /
    \/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/

THANKS TO MICHAEL, BA AND ANYONE ELSE WHO PUT UP WITH MY CURSING AND SCREAMING THROUGH OUT THIS THING

THANKS TO MICHAEL FOR FIGURING OUT THE FLAG BYTES!