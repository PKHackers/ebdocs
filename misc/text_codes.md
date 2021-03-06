|Code                        |Routine|Description                                                                      |
|----------------------------|-------|---------------------------------------------------------------------------------|
|`00                        `|$C18A04|Drop to next line                                                                |
|`01                        `|$C18A0B|UNKNOWN                                                                          |
|`02                        `|$C18B0A|Signal end of block or string                                                    |
|`03                        `|$C18A1D|Wait with prompt, brief pause in battle                                          |
|`04 XX XX                  `|$C14265|Toggle flag $XXXX on                                                             |
|`05 XX XX                  `|$C142AD|Toggle flag $XXXX off                                                            |
|`06 XX XX YY YY YY YY      `|$C142F5|If flag $XXXX is set, jump to address $YYYYYYYY                                  |
|`07 XX XX                  `|$C1435F|Set binary flag with the status of flag $XXXX                                    |
|`08 XX XX XX XX            `|$C143D6|Parse text at $XXXXXXXX and continue here                                        |
|`09 XX YY YY YY YY         `|$C141D0|Multiple-entry pointer table (Jump to address)                                   |
|`0A XX XX XX XX            `|$C14103|Jump to text at address $XXXXXXXX                                                |
|`0B XX                     `|$C14558|Set binary flag to true if number in memory == $XX                               |
|`0C XX                     `|$C14591|Set binary flag to true if number in memory != $XX                               |
|`0D XX                     `|$C145EF|Copy from memory $XX into argumentative memory                                   |
|`0E XX                     `|$C1461A|Input $XX into a memory bank                                                     |
|`0F                        `|$C18A81|UNKNOWN                                                                          |
|`10 XX                     `|$C14EAB|Pause for ~0.02 sec. * $XX                                                       |
|`11                        `|$C18A8F|Treats text displayed by [1C (07|0C) XX] as a menu                               |
|`12                        `|$C18AAA|Clear current line of text                                                       |
|`13                        `|$C18AB0|Wait without prompt                                                              |
|`14                        `|$C18ABA|Wait with prompt in battle                                                       |
|`18 00                     `|$C17954|Closes most recently used text window                                            |
|`18 01 XX                  `|$C143C2|Open window                                                                      |
|`18 02                     `|$C1795E|UNKNOWN                                                                          |
|`18 03 XX                  `|$C143CC|Change parsing focus to window $XX                                               |
|`18 04                     `|$C17976|Close all windows                                                                |
|`18 05 XX YY               `|$C14509|Display text $XX pixels over and $YY lines down                                  |
|`18 06                     `|$C17987|Clear contents from current window                                               |
|`18 07 XX XX XX XX YY      `|$C1528D|Check for $XX inequality to memory                                               |
|`18 08 XX                  `|$C15529|UNKNOWN                                                                          |
|`18 09 XX                  `|$C1554E|UNKNOWN                                                                          |
|`18 0A                     `|$C1799B|Open wallet window and display current cash on hand                              |
|`18 0D XX XX               `|$C15B46|UNKNOWN                                                                          |
|`19 02                     `|$C178F7|Load a text string into memory (end with [02]                                    |
|`19 04                     `|$C17A7E|UNKNOWN                                                                          |
|`19 05 XX YY YY            `|$C1506F|Inflict ailment YY YY on character $XX                                           |
|`19 10 XX                  `|$C1597F|Return the character number of the person in party position $X                   |
|`19 11 XX                  `|$C14723|UNKNOWN                                                                          |
|`19 14                     `|$C147CC|UNKNOWN                                                                          |
|`19 16 XX YY               `|$C17A96|UNKNOWN                                                                          |
|`19 18 XX                  `|$C15007|UNKNOWN                                                                          |
|`19 19 00 00               `|$C15384|Sets number of used item into active use                                         |
|`19 1A XX                  `|$C15B0E|UNKNOWN                                                                          |
|`19 1B XX                  `|$C15C36|UNKNOWN                                                                          |
|`19 1C XX YY               `|$C15FF7|UNKNOWN                                                                          |
|`19 1D XX YY               `|$C16080|UNKNOWN                                                                          |
|`19 1E                     `|$C17AE3|UNKNOWN                                                                          |
|`19 1F                     `|$C17AF3|UNKNOWN                                                                          |
|`19 20                     `|$C17B0D|UNKNOWN                                                                          |
|`19 21 XX                  `|$C16143|UNKNOWN                                                                          |
|`19 22 XX XX YY YY         `|$C168A0|Store to memory the direction from character $XX to object $YY                   |
|`19 23 XX XX YY YY         `|$C16947|Store to memory the direction from TPT entry $XX to object $YY                   |
|`19 24 XX XX YY YY         `|$C16A7B|Store to memory the direction from generated sprite $XX to object $YY            |
|`19 25 XX                  `|$C16F9F|UNKNOWN                                                                          |
|`19 26 XX                  `|$C17037|UNKNOWN                                                                          |
|`19 27 XX                  `|$C1776A|UNKNOWN                                                                          |
|`19 28 XX                  `|$C14819|UNKNOWN                                                                          |
|`1A 00                     `|$C1463B|UNKNOWN INCOMPLETE                                                               |
|`1A 01                     `|$C1467D|UNKNOWN INCOMPLETE                                                               |
|`1A 04                     `|$C17BA5|UNKNOWN INCOMPLETE                                                               |
|`1A 05                     `|$C1549E|UNKNOWN INCOMPLETE                                                               |
|`1A 06 XX                  `|$C14EB5|Displays shop window $XX                                                         |
|`1A 07                     `|$C17BC9|Related to Escargo Express stored goods window                                   |
|`1A 08                     `|$C17BDD|UNKNOWN INCOMPLETE                                                               |
|`1A 09                     `|$C17BF4|UNKNOWN INCOMPLETE                                                               |
|`1A 0A                     `|$C17C0B|Open non-working phone window                                                    |
|`1A 0B                     `|$C17C1F|UNKNOWN                                                                          |
|`1B 00                     `|$C17C70|Copy all memory to storage                                                       |
|`1B 01                     `|$C17C76|Copy all storage to memory                                                       |
|`1B 02 XX XX XX XX         `|$C17C7C|If binary flag is set 0, jump to $XXXXXXXX                                       |
|`1B 03 XX XX XX XX         `|$C17CBA|If binary flag is set 1, jump to $XXXXXXXX                                       |
|`1B 04                     `|$C17CF8|Swap working and argumentary memory                                              |
|`1B 05                     `|$C17D36|UNKNOWN                                                                          |
|`1B 06                     `|$C17D5A|UNKNOWN                                                                          |
|`1C 00 XX                  `|$C140F9|Text and window background color effects                                         |
|`1C 01 XX                  `|$C140B0|Display statistical data value $XX                                               |
|`1C 02 XX                  `|$C14FD7|Display character name $XX                                                       |
|`1C 03 XX                  `|$C1488D|UNKNOWN                                                                          |
|`1C 04                     `|$C17E5F|Open party HP/PP status windows                                                  |
|`1C 05 XX                  `|$C146BF|Display item name $XX                                                            |
|`1C 06 XX                  `|$C146DE|Display teleport destination name $XX                                            |
|`1C 07 XX                  `|$C145CA|Display $XX strings centered horizontally (see [19 02])                          |
|`1C 08 XX                  `|$C143B8|Display text graphic $XX                                                         |
|`1C 09 XX                  `|$C140EF|UNKNOWN                                                                          |
|`1C 0A XX XX XX XX         `|$C153AF|Displays $XXXXXXXX in decimal                                                    |
|`1C 0B XX XX XX XX         `|$C15573|Displays $XXXXXXXX in decimal as money                                           |
|`1C 0C XX                  `|$C15BA7|Display $XX strings vertically (see [19 02])                                     |
|`1C 0D                     `|$C17E9F|Display action user name                                                         |
|`1C 0E                     `|$C17EC6|Display action target name                                                       |
|`1C 0F                     `|$C17EED|UNKNOWN                                                                          |
|`1C 11 XX                  `|$C140CF|UNKNOWN                                                                          |
|`1C 12 XX                  `|$C161D1|Print PSI name $XX                                                               |
|`1C 13 XX YY               `|$C173C0|UNKNOWN                                                                          |
|`1C 14 XX                  `|$C1516B|UNKNOWN                                                                          |
|`1C 15 XX                  `|$C151FC|UNKNOWN                                                                          |
|`1D 00 XX XX               `|$C14C1E|Add item $YY to character $XX's inventory and return $YY if successful           |
|`1D 01 XX YY               `|$C14C86|Remove item $YY from character $XX's inventory                                   |
|`1D 02 XX                  `|$C148AC|Set binary flag true if character $XX does not have free space in their inventory|
|`1D 03 XX                  `|$C14CEE|Set binary flag true if character $XX has free space in their inventory          |
|`1D 04 XX YY               `|$C14D24|Sets binary flag true if char $XX does not have item $YY                         |
|`1D 05 XX YY               `|$C14D93|Sets binary flag true if char $XX has item $YY                                   |
|`1D 06 XX XX XX XX         `|$C15C85|Add $XX dollars to ATM balance                                                   |
|`1D 07 XX XX XX XX         `|$C15D6B|Remove $XX dollars from ATM balance                                              |
|`1D 08 XX XX               `|$C148E9|Add $XX dollars to wallet balance                                                |
|`1D 09 XX XX               `|$C1494A|Remove $XX dollars from wallet balance                                           |
|`1D 0A XX                  `|$C14EF8|Return price of item                                                             |
|`1D 0B XX                  `|$C14F33|Check permanance of item                                                         |
|`1D 0C XX XX               `|$C150E4|UNKNOWN                                                                          |
|`1D 0D XX YY YY            `|$C17058|Set binary flag true if character $XX has ailment YY YY                          |
|`1D 0E XX YY               `|$C15659|Add item $YY to character $XX's inventory and return number of items held        |
|`1D 0F XX XX               `|$C156DB|UNKNOWN                                                                          |
|`1D 10 XX XX               `|$C1575D|UNKNOWN                                                                          |
|`1D 11 XX XX               `|$C157CD|UNKNOWN                                                                          |
|`1D 12 XX XX               `|$C158A5|UNKNOWN                                                                          |
|`1D 13 XX XX               `|$C158FE|UNKNOWN                                                                          |
|`1D 14 XX XX XX XX         `|$C159F9|Sets binary flag true if the player currently holds $XX in the wallet            |
|`1D 15 XX XX               `|$C15BCA|UNKNOWN                                                                          |
|`1D 17 XX XX XX XX         `|$C15E5C|Sets binary flag true if the player currently holds $XX in the ATM               |
|`1D 18 XX                  `|$C16124|UNKNOWN                                                                          |
|`1D 19 XX                  `|$C16172|Sets binary flag true if there are $XX party members                             |
|`1D 20                     `|$C180A2|Check same user/target                                                           |
|`1D 21 XX                  `|$C161F0|Generate random number from 0 to $XX                                             |
|`1D 22                     `|$C180DC|Set binary flag if current sector is Exit mouse-compatible                       |
|`1D 23 XX                  `|$C17708|UNKNOWN                                                                          |
|`1D 24 XX                  `|$C17274|May relate to returning cash earned since last phone call to dad                 |
|`1E 00 XX YY               `|$C149B6|Character $XX recovers HP by YY% of max                                          |
|`1E 01 XX YY               `|$C14A03|Character $XX loses HP by YY% of max                                             |
|`1E 02 XX YY               `|$C14A50|Character $XX recovers $YY HP                                                    |
|`1E 03 XX YY               `|$C14A9D|Character $XX loses $YY HP                                                       |
|`1E 04 XX YY               `|$C14AEA|Character $XX recovers PP by YY% of max                                          |
|`1E 05 XX YY               `|$C14B37|Character $XX loses PP by YY% of max                                             |
|`1E 06 XX YY               `|$C14B84|Character $XX recovers $YY PP                                                    |
|`1E 07 XX YY               `|$C14BD1|Character $XX loses $YY PP                                                       |
|`1E 08 XX YY               `|$C16A01|Character $XX becomes level $YY                                                  |
|`1E 09 XX YY YY YY         `|$C1744B|Character $XX gains $YYYYYY EXP                                                  |
|`1E 0A XX YY               `|$C17523|Boost IQ in character $XX by $YY                                                 |
|`1E 0B XX YY               `|$C17584|Boost Guts in character $XX by $YY                                               |
|`1E 0C XX YY               `|$C175E5|Boost Speed in character $XX by $YY                                              |
|`1E 0D XX YY               `|$C17646|Boost Vitality in character $XX by $YY                                           |
|`1E 0E XX YY               `|$C176A7|Boost Luck in character $XX by $YY                                               |
|`1F 00 XX YY               `|$C14751|Play music track $YY ($XX is unused)                                             |
|`1F 01 XX                  `|$C147A0|Stops music ($XX is unused)                                                      |
|`1F 02 XX                  `|$C147AB|Play sound effect $XX                                                            |
|`1F 03                     `|$C18428|Return music setting to sector default                                           |
|`1F 04 XX                  `|$C17254|Toggles sound for text printing (?)                                              |
|`1F 05                     `|$C1843C|UNKNOWN                                                                          |
|`1F 06                     `|$C18446|UNKNOWN                                                                          |
|`1F 07 XX                  `|$C1741F|Apply audio effect $XX                                                           |
|`1F 11 XX                  `|$C15F71|Add character $XX to party                                                       |
|`1F 12 XX                  `|$C15F91|Remove character $XX from party                                                  |
|`1F 13 XX YY               `|$C163FD|Turn character $XX to direction $YY                                              |
|`1F 14 XX                  `|$C1646E|UNKNOWN                                                                          |
|`1F 15 XX XX YY YY ZZ      `|$C16744|Generate sprite $XXXX with movement pattern $YYYY using style $ZZ                |
|`1F 16 XX XX YY            `|$C16490|Turn TPT entry $XXXX to direction $YY                                            |
|`1F 17 XX XX YY YY ZZ      `|$C16509|Generate TPT entry $XXXX with movement pattern $YYYY using style $ZZ             |
|`1F 18 XX XX XX XX XX XX XX`|$C16582|UNKNOWN                                                                          |
|`1F 19 XX XX XX XX XX XX XX`|$C165AA|UNKNOWN                                                                          |
|`1F 1A XX XX YY            `|$C165D2|Generate sprite $YY near TPT entry $XXXX                                         |
|`1F 1B XX XX               `|$C1662A|Delete 1F 1A sprite near TPT entry $XXXX                                         |
|`1F 1C XX YY               `|$C1666D|Generate sprite $YY near character $XX                                           |
|`1F 1D XX                  `|$C166DD|Delete 1F 1C sprite near character $XX                                           |
|`1F 1E XX XX YY            `|$C167D6|Make TPT entry $XXXX disappear via style $YY                                     |
|`1F 1F XX XX YY            `|$C1683B|Delete 1F 15-generated sprite $XXXX via style $YY                                |
|`1F 20 XX YY               `|$C14DFB|PSI Teleport to destination $XX using method $YY                                 |
|`1F 21 XX                  `|$C14E8C|Teleport to 0x15EDAB teleport coordinate entry $XX                               |
|`1F 23 XX XX               `|$C16FD1|Trigger battle with 0x10D74C enemy group entry $XXXX                             |
|`1F 30                     `|$C184C2|Change to normal font                                                            |
|`1F 31                     `|$C184C2|Change to Mr. Saturn font                                                        |
|`1F 40 XX                  `|$C172BC|UNKNOWN                                                                          |
|`1F 41 XX                  `|$C172DA|Call special event $XX                                                           |
|`1F 50                     `|$C184D4|Disable controller input                                                         |
|`1F 51                     `|$C184DA|Re-enable controller input                                                       |
|`1F 52 XX                  `|$C144A3|Number selector with XX digits                                                   |
|`1F 60 XX                  `|$C15494|UNKNOWN                                                                          |
|`1F 61                     `|$C184EC|Simultaneously commences all prepared movements                                  |
|`1F 62 XX                  `|$C169F7|UNKNOWN                                                                          |
|`1F 63 XX XX XX XX         `|$C16DE8|Jump to $XXXXXXXX after the next screen refresh                                  |
|`1F 64                     `|$C184FE|Purge all NPCs from party                                                        |
|`1F 65                     `|$C18505|Purge first NPC from party                                                       |
|`1F 66 XX YY ZZ ZZ ZZ ZZ   `|$C1711C|Activate hotspot $XX with address $ZZZZZZZZ                                      |
|`1F 67 XX                  `|$C17233|Deactivate hotspot $XX                                                           |
|`1F 68                     `|$C18518|Store current coordinates into memory                                            |
|`1F 69                     `|$C18527|Teleport to coordinates stored by 1F 68                                          |
|`1F 71 XX YY               `|$C15C58|Character $XX realizes special PSI $YY                                           |
|`1F 81 XX XX               `|$C14F6F|UNKNOWN                                                                          |
|`1F 83 XX XX               `|$C1583D|UNKNOWN                                                                          |
|`1F 90                     `|$C18594|UNKNOWN                                                                          |
|`1F A0                     `|$C185A9|TPT entry being spoken to/checked faces downwards                                |
|`1F A1                     `|$C185B3|UNKNOWN                                                                          |
|`1F A2                     `|$C185BD|UNKNOWN                                                                          |
|`1F B0                     `|$C185DA|Save the game in the slot selected at the beginning                              |
|`1F C0 XX YY YY YY YY      `|$C16308|Multiple-entry pointer table (Reference address)                                 |
|`1F D0 XX                  `|$C163A7|UNKNOWN                                                                          |
|`1F D1                     `|$C185ED|Returns a numeric value based on the proximity of a magic truffle TPT entry      |
|`1F D2 XX                  `|$C17304|Summon photographer to location $XX                                              |
|`1F D3 XX                  `|$C17440|Trigger timed event $XX from the 0x15F845 table                                  |
|`1F E1 XX YY ZZ            `|$C166FE|Change map palette to that of tileset $XX, pallet $YY at speed $ZZ               |
|`1F E4 XX XX YY            `|$C16B2B|Change direction of 1F 15-generated sprite $XXXX to $YY                          |
|`1F E5 XX                  `|$C16BA4|Lock player movement                                                             |
|`1F E6 XX XX               `|$C16BAF|Delay appearance of TPT entry $XXXX until end of text parsing                    |
|`1F E7 XX XX               `|$C16BF2|UNKNOWN                                                                          |
|`1F E8 XX                  `|$C16C35|UNKNOWN                                                                          |
|`1F E9 XX XX               `|$C16C40|UNKNOWN                                                                          |
|`1F EA XX XX               `|$C16C83|UNKNOWN                                                                          |
|`1F EB XX YY               `|$C16CC6|Make character $XX disappear via style $YY                                       |
|`1F EC XX YY               `|$C16D14|Make character $XX appear via style $YY                                          |
|`1F ED                     `|$C1863E|UNKNOWN                                                                          |
|`1F EE XX XX               `|$C16D62|UNKNOWN                                                                          |
|`1F EF XX XX               `|$C16DA5|UNKNOWN                                                                          |
|`1F F0                     `|$C1864E|Activate bicycle                                                                 |
|`1F F1 XX XX YY YY         `|$C16EBF|Apply movement pattern $YYYY to TPT entry $XXXX                                  |
|`1F F2 XX XX YY YY         `|$C16F2F|Apply movement pattern $YYYY to sprite $XXXX                                     |
|`1F F3 XX XX YY            `|$C17325|Generate sprite $YY near sprite $XXXX                                            |
|`1F F4 XX XX               `|$C1737D|Delete 1F F3 sprite near sprite $XXXX                                            |
|`15 XX                     `|$C18804|Use compressed string $XX from bank 0                                            |
|`16 XX                     `|$C1885E|Use compressed string $XX from bank 1                                            |
|`17 XX                     `|$C188B7|Use compressed string $XX from bank 2                                            |