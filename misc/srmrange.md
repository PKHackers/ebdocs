# SRAM RANGE

|Range       |Description                                                                 |
|------------|----------------------------------------------------------------------------|
|0000 to 0013|Hal Labs signature                                                          |
|0014 to 0027|*** UNKNOWN *** (probably always null, since it isn't checksumed)           |
|0028 to 0029|Checksum (sum of all bytes from 0x002C to the end of this save)             |
|002A to 002B|Checksum complement (checks the checksum, format unknown, any ideas welcome)|
|002C to 004C|Player name                                                                 |
|0044 to 0049|Pet's name                                                                  |
|004A to 004F|Favourite food                                                              |
|0050 to 0053|"PSI "                                                                      |
|0054 to 0059|Favourite thing                                                             |
|005A to 0075|*** UNKNOWN ***                                                             |
|0076 to 0099|Escargo Express stored items                                                |
|009A to 00A1|*** UNKNOWN ***                                                             |
|00A2 to 00A8|Current location (XX XX 00 YY YY 00)                                        |
|00A9 to 00B5|*** UNKNOWN ***                                                             |
|00B6 to 00BC|Party members                                                               |
|00BD to 00C0|Player controlled party members                                             |
|        00C1|*** UNKNOWN (Null?) ***                                                     |
|00C2 to 00CD|*** UNKNOWN (X + 18 madness) ***                                            |
|        00CE|Number of party members                                                     |
|        00CF|Number of player controlled party members                                   |
|00D0 to 00DC|*** UNKNOWN ***                                                             |
|00DD to 00E0|Exit Mouse coordinates, stored when [1F 68] used                            |
|        00E1|Text speed (1 = Fast, 2 = Medium, 3 = Slow)                                 |
|        00E2|Sound setting (1 = Stereo, 2 = Mono)                                        |
|00E3 to 01F3|*** UNKNOWN ***                                                             |
|01F4 to 01F6|In-game timer (Presumed three bytes;  Measured in 1/60 second increments)   |
|        01F7|*** UNKNOWN ***                                                             |
|        01F8|Text palette                                                                |
|01F9 to 01FD|Ness name                                                                   |
|        01FE|Ness level                                                                  |
|01FF to 0202|Ness exp, may end at 0201                                                   |
|0203 to 0204|Ness max HP                                                                 |
|0205 to 0206|Ness max PP                                                                 |
|0207 to 020D|*** UNKNOWN ***                                                             |
|020E to 0214|Ness stats after item effects                                               |
|0215 to 021B|Ness stats before item effects                                              |
|021C to 0229|Ness items                                                                  |
|022A to 023D|*** UNKNOWN ***                                                             |
|023E to 023F|Ness current HP                                                             |
|0240 to 0241|Ness rolling HP target                                                      |
|0242 to 0243|*** UNKNOWN ***                                                             |
|0244 to 0245|Ness current PP                                                             |
|0246 to 0247|Ness rolling PP target                                                      |
|0248 to 0257|*** UNKNOWN ***                                                             |
|0258 to 025C|Paula name                                                                  |
|        025D|Paula level                                                                 |
|025E to 0261|Paula exp, may end at 0260                                                  |
|0262 to 0263|Paula max HP                                                                |
|0264 to 0265|Paula max PP                                                                |
|0266 to 026C|*** UNKNOWN ***                                                             |
|026D to 0273|Paula stats after item effects                                              |
|0274 to 027A|Paula stats before item effects                                             |
|027B to 0288|Paula items                                                                 |
|0289 to 029C|*** UNKNOWN ***                                                             |
|029D to 029E|Paula current HP                                                            |
|029F to 02A0|Paula rolling HP target                                                     |
|02A1 to 02A2|*** UNKNOWN ***                                                             |
|02A3 to 02A4|Paula current PP                                                            |
|02A5 to 02A6|Paula rolling PP target                                                     |
|02A7 to 02B6|*** UNKNOWN ***                                                             |
|02B7 to 02BB|Jeff name                                                                   |
|        02BC|Jeff level                                                                  |
|02BD to 02C0|Jeff exp, may end at 02BF                                                   |
|02C1 to 02C2|Jeff max HP                                                                 |
|02C3 to 02C4|Jeff max PP                                                                 |
|02C5 to 02CB|*** UNKNOWN ***                                                             |
|02CC to 02D2|Jeff stats after item effects                                               |
|02D3 to 02D9|Jeff stats before item effects                                              |
|02DA to 02E7|Jeff items                                                                  |
|02E8 to 02FB|*** UNKNOWN ***                                                             |
|02FC to 02FD|Jeff current HP                                                             |
|02FE to 02FF|Jeff rolling HP target                                                      |
|0300 to 0301|*** UNKNOWN ***                                                             |
|0302 to 0303|Jeff current PP                                                             |
|0304 to 0305|Jeff rolling PP target                                                      |
|0306 to 0315|*** UNKNOWN ***                                                             |
|0316 to 031A|Poo name                                                                    |
|        031B|Poo level                                                                   |
|031C to 031F|Poo exp, may end at 03                                                      |
|0320 to 0321|Poo max HP                                                                  |
|0322 to 0323|Poo max PP                                                                  |
|0324 to 032A|*** UNKNOWN ***                                                             |
|032B to 0331|Poo stats after item effects                                                |
|0332 to 0338|Poo stats before item effects                                               |
|0339 to 0346|Poo items                                                                   |
|0347 to 035A|*** UNKNOWN ***                                                             |
|035B to 035C|Poo current HP                                                              |
|035D to 035E|Poo rolling HP target                                                       |
|035F to 0360|*** UNKNOWN ***                                                             |
|0361 to 0362|Poo current PP                                                              |
|0363 to 0364|Poo rolling PP target                                                       |
|0365 to 0374|*** UNKNOWN ***                                                             |
|0375 to 0432|*** UNKNOWN ***                                                             |
|0433 to 04FF|Event flags (presumed to go up to 4FF; not confirmed)                       |