# SPC File Format

|Offset |Size |Description                                                            |
|-------|-----|-----------------------------------------------------------------------|
|0x00000|32   |File Header : SNES-SPC700 Sound File Data v0.30                        |
|0x00021|3    |26,26,26                                                               |
|0x00024|1    |Version #(/100)                                                        |
|0x00025|2    |PC Register value                                                      |
|0x00027|1    |A Register Value                                                       |
|0x00028|1    |X Register Value                                                       |
|0x00029|1    |Y Register Value                                                       |
|0x0002A|1    |Status Flags Value                                                     |
|0x0002B|1    |Stack Register Value                                                   |
|0x0002C|2    |Empty area                                                             |
|0x0002E|32   |SubTitle/Song Name                                                     |
|0x0004E|32   |Title of Game                                                          |
|0x0006E|16   |Name of Dumper                                                         |
|0x0007E|32   |Comments                                                               |
|0x0009E|7    |Date of SPC Dumped in decimal (DD/MM/YYYY)                             |
|0x000A5|4    |unused                                                                 |
|0x000A9|4    |Time in seconds for the spc to play before fading                      |
|0x000AC|4    |Fade out time in milliseconds(Ascii if > 48*256, else decimal)         |
|0x000B0|32   |Author of Song                                                         |
|0x000D0|1    |Default Channel Disables (0 = enable, 1 = disable)                     |
|0x000D1|1    |Emulator used to dump .spc file (0 = UNKNOWN, 1 = ZSNES, 2 = SNES9X)   |
|0x000D2|10   |Unused                                                                 |
|0x000DC|36   |Reserved For Future Use (Please Keep 0 for now)                        |
|0x00100|65536|SPCRam (where samples, music data, and music program are stored)       |
|0x10100|1    |Channel 1 Left Volume                                                  |
|0x10101|1    |Channel 1 Right Volume                                                 |
|0x10102|1    |Channel 1 Pitch Low                                                    |
|0x10103|1    |Channel 1 Pitch High                                                   |
|0x10104|1    |Channel 1 Instrument                                                   |
|0x10105|1    |Channel 1 ADSR 1                                                       |
|0x10106|1    |Channel 1 ADSR 2                                                       |
|0x10107|1    |Channel 1 Gain                                                         |
|0x10108|1    |ENVX                                                                   |
|0x10109|1    |VALX                                                                   |
|0x1010A|2    |Unknown                                                                |
|0x1010C|1    |Master Volume Left                                                     |
|0x1010D|1    |Echo (in binary, 1 bit for each channel)                               |
|0x1010E|1    |Unknown                                                                |
|0x1010F|1    |Channel 1 Filter                                                       |
|0x10110|1    |Channel 2 Left Volume                                                  |
|0x10111|1    |Channel 2 Right Volume                                                 |
|0x10112|1    |Channel 2 Pitch Low                                                    |
|0x10113|1    |Channel 2 Pitch High                                                   |
|0x10114|1    |Channel 2 Instrument                                                   |
|0x10115|1    |Channel 2 ADSR 1                                                       |
|0x10116|1    |Channel 2 ADSR 2                                                       |
|0x10117|1    |Channel 2 Gain                                                         |
|0x10118|5    |Unknown                                                                |
|0x1011C|1    |Master Volume Right                                                    |
|0x1011D|2    |Unknown                                                                |
|0x1011F|1    |Channel 2 Filter                                                       |
|0x10120|1    |Channel 3 Left Volume                                                  |
|0x10121|1    |Channel 3 Right Volume                                                 |
|0x10122|1    |Channel 3 Pitch Low                                                    |
|0x10123|1    |Channel 3 Pitch High                                                   |
|0x10124|1    |Channel 3 Instrument                                                   |
|0x10125|1    |Channel 3 ADSR 1                                                       |
|0x10126|1    |Channel 3 ADSR 2                                                       |
|0x10127|1    |Channel 3 Gain                                                         |
|0x10128|5    |Unknown                                                                |
|0x1012C|1    |Echo Volume Left                                                       |
|0x1012D|1    |Pitch Modulation (binary)                                              |
|0x1012E|1    |Unknown                                                                |
|0x1012F|1    |Channel 3 Filter                                                       |
|0x10130|1    |Channel 4 Left Volume                                                  |
|0x10131|1    |Channel 4 Right Volume                                                 |
|0x10132|1    |Channel 4 Pitch Low                                                    |
|0x10133|1    |Channel 4 Pitch High                                                   |
|0x10134|1    |Channel 4 Instrument                                                   |
|0x10135|1    |Channel 4 ADSR 1                                                       |
|0x10136|1    |Channel 4 ADSR 2                                                       |
|0x10137|1    |Channel 4 Gain                                                         |
|0x10138|5    |Unknown                                                                |
|0x1013D|1    |Echo Volume Right                                                      |
|0x1013E|1    |Noise Enable (binary)                                                  |
|0x1013F|1    |Channel 4 Filter                                                       |
|0x10140|1    |Channel 5 Left Volume                                                  |
|0x10141|1    |Channel 5 Right Volume                                                 |
|0x10142|1    |Channel 5 Pitch Low                                                    |
|0x10143|1    |Channel 5 Pitch High                                                   |
|0x10144|1    |Channel 5 Instrument                                                   |
|0x10145|1    |Channel 5 ADSR 1                                                       |
|0x10146|1    |Channel 5 ADSR 2                                                       |
|0x10147|1    |Channel 5 Gain                                                         |
|0x10148|5    |Unknown                                                                |
|0x1014C|1    |Key-On (Enable channel, Binary)                                        |
|0x1014D|1    |Echo Enable (Binary)                                                   |
|0x1014E|1    |Unknown                                                                |
|0x1014F|1    |Channel 5 Filter                                                       |
|0x10150|1    |Channel 6 Left Volume                                                  |
|0x10151|1    |Channel 6 Right Volume                                                 |
|0x10152|1    |Channel 6 Pitch Low                                                    |
|0x10153|1    |Channel 6 Pitch High                                                   |
|0x10154|1    |Channel 6 Instrument                                                   |
|0x10155|1    |Channel 6 ADSR 1                                                       |
|0x10156|1    |Channel 6 ADSR 2                                                       |
|0x10157|1    |Channel 6 Gain                                                         |
|0x10158|5    |Unknown                                                                |
|0x1015C|1    |Key-Off (Mute channel, Binary)                                         |
|0x1015D|1    |Sample Location (Hi-byte of mem address for the sample directory table)|
|0x1015E|1    |Unknown                                                                |
|0x1015F|1    |Channel 6 Filter                                                       |
|0x10160|1    |Channel 7 Left Volume                                                  |
|0x10161|1    |Channel 7 Right Volume                                                 |
|0x10162|1    |Channel 7 Pitch Low                                                    |
|0x10163|1    |Channel 7 Pitch High                                                   |
|0x10164|1    |Channel 7 Instrument                                                   |
|0x10165|1    |Channel 7 ADSR 1                                                       |
|0x10166|1    |Channel 7 ADSR 2                                                       |
|0x10167|1    |Channel 7 Gain                                                         |
|0x10168|5    |Unknown                                                                |
|0x1016C|1    |Misc Voice Control (binary, ???)                                       |
|0x1016D|1    |Echo Location (same as 0x1015D?)                                       |
|0x1016E|1    |Unknown                                                                |
|0x1016F|1    |Channel 7 Filter                                                       |
|0x10170|1    |Channel 8 Left Volume                                                  |
|0x10171|1    |Channel 8 Right Volume                                                 |
|0x10172|1    |Channel 8 Pitch Low                                                    |
|0x10173|1    |Channel 8 Pitch High                                                   |
|0x10174|1    |Channel 8 Instrument                                                   |
|0x10175|1    |Channel 8 ADSR 1                                                       |
|0x10176|1    |Channel 8 ADSR 2                                                       |
|0x10177|1    |Channel 8 Gain                                                         |
|0x10178|5    |Unknown                                                                |
|0x1017D|1    |Echo Delay (Binary)                                                    |
|0x1017E|1    |Unknown                                                                |
|0x1017F|1    |Channel 8 Filter                                                       |
|0x10180|64   |Unused                                                                 |
|0x101C0|64   |SPC ExRAM (not sure what this is for)                                  |