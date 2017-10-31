# Movement control code system

|Code              |Routine|Description                                                  |Argument XX                                   |Argument YY       |Argument ZZ|
|------------------|-------|-------------------------------------------------------------|----------------------------------------------|------------------|-----------|
|00                |$8095F2|End                                                          |                                              |                  |           |
|01 XX             |$809603|Loop start                                                   |Number of iterations                          |                  |           |
|02                |$809627|Loop end                                                     |                                              |                  |           |
|03 XX XX XX       |$80964D|Unconditional long jump to movement                          |Destination address                           |                  |           |
|04 XX XX XX       |$809685|Unconditional long call to movement                          |Destination address                           |                  |           |
|05                |$8096AA|Unconditional long return                                    |                                              |                  |           |
|06 XX             |$8096C3|Pause                                                        |Duration (frames)                             |                  |           |
|07 XX XX          |$8099DD|Conditional jump??? Unknown                                  |Short pointer?                                |                  |           |
|08 XX XX XX       |$809A1A|*** UNKNOWN ***                                              |Long pointer?                                 |                  |           |
|09                |$809A2E|Halt                                                         |                                              |                  |           |
|0A XX XX          |$80995D|Conditional (tempvar == 0) short jump to movement            |Destination address                           |                  |           |
|0B XX XX          |$80996B|Conditional (tempvar != 0) short jump to movement            |Destination address                           |                  |           |
|0C                |$8099C3|`[00]` if ?????                                              |                                              |                  |           |
|0D XX XX YY ZZ ZZ |$809A9F|Perform binary operation on WRAM                             |WRAM address                                  |Operation         |Operand    |
|0E XX YY YY       |$809AE2|Write a word to a C09AF9 table-entry                         |Index value for C09AF9                        |Word to be written|           |
|0F                |$809B09|Equivalent to `[08 3B 94 C0]`?                               |                                              |                  |           |
|10 XX (YY YY)     |$809979|Switch statement indexed by tempvar (short jump to movement?)|                                              |                  |           |
|11 XX (YY YY)     |$80999E|Switch statement indexed by tempvar (short call to movement?)|                                              |                  |           |
|12 XX XX YY       |$809B0F|Write a byte to WRAM                                         |WRAM address                                  |Byte to be written|           |
|13                |$809A0E|`[0C]` if ?????                                              |                                              |                  |           |
|14 XX YY ZZ ZZ    |$809A87|Perform binary operation on a C09AF9 table-entry             |Index value for C09AF9                        |Operation         |Operand    |
|15 XX XX YY YY    |$809B1F|Write a word to WRAM                                         |WRAM address                                  |Word to be written|           |
|16 XX XX          |$809B2C|Conditional (tempvar == 0) short jump w/ stack weirdness     |Destination address                           |                  |           |
|17 XX XX          |$809B44|Conditional (tempvar != 0) short jump w/ stack weirdness     |Destination address                           |                  |           |
|18 XX XX YY ZZ    |$809A5C|Perform binary operation on WRAM                             |WRAM address                                  |Operation         |Operand    |
|19 XX XX          |$809649|Unconditional short jump to movement                         |Destination address                           |                  |           |
|1A XX XX          |$809658|Unconditional short call to movement                         |Destination address                           |                  |           |
|1B                |$80966F|Unconditional short return                                   |                                              |                  |           |
|1C XX XX XX       |$809B4D|Writes argument to 7E112E (low), 7E116A (bank)?              |Long pointer?                                 |                  |           |
|1D XX XX          |$809B61|Write a word to tempvar                                      |Word to be written                            |                  |           |
|1E XX XX          |$809B6B|Write WRAM to tempvar                                        |WRAM address                                  |                  |           |
|1F XX             |$809B79|Write tempvar to a C09AF9 table-entry                        |Index value for C09AF9                        |                  |           |
|20 XX             |$809B91|Write a C09AF9 table-entry value to tempvar                  |Index value for C09AF9                        |                  |           |
|21 XX             |$809BB4|Write a C09AF9 table-entry value to the wait timer           |Index value for C09AF9                        |                  |           |
|22 XX XX          |$809BE4|Write argument to 7E11E2 (Short jump to assembly, C0 bank)   |Callback???                                   |                  |           |
|23 XX XX          |$809BEE|Write argument to 7E11A6 (Short call to assembly, C0 bank)   |Callback???                                   |                  |           |
|24                |$809620|Loop with tempvar iterations                                 |                                              |                  |           |
|25 XX XX          |$809BF8|Write argument to 7E121E (Short call to assembly, C0 bank)   |Callback???                                   |                  |           |
|26 XX             |$809BCC|Write a C09AF9 table-entry value to 7E10F2                   |???                                           |                  |           |
|27 XX YY YY       |$809A97|Perform binary operation on tempvar                          |Operation                                     |Operand           |           |
|28 XX XX          |$8096E3|Set X position (Absolute)                                    |X-coordinate, in pixels                       |                  |           |
|29 XX XX          |$8096F3|Set Y position (Absolute)                                    |Y-coordinate, in pixels                       |                  |           |
|2A XX XX          |$809703|Set Z position (Absolute)                                    |Z-coordinate, in pixels                       |                  |           |
|2B XX XX          |$8098A0|Set X position (Relative to current position)                |Change in X-coordinate, in pixels             |                  |           |
|2C XX XX          |$8098AE|Set Y position (Relative to current position)                |Change in Y-coordinate, in pixels             |                  |           |
|2D XX XX          |$8098BC|Set Z position (Relative to current position)                |Change in Z-coordinate, in pixels             |                  |           |
|2E XX XX          |$80976D|Set X velocity (Relative to current velocity)                |Change in X-velocity, in pixels per 256 frames|                  |           |
|2F XX XX          |$809792|Set Y velocity (Relative to current velocity)                |Change in Y-velocity, in pixels per 256 frames|                  |           |
|30 XX XX          |$8097B7|Set Z velocity (Relative to current velocity)                |Change in Z-velocity, in pixels per 256 frames|                  |           |
|31 XX YY YY       |$8097DC|*** UNKNOWN ***                                              |???                                           |???               |           |
|32 XX YY YY       |$8097EF|*** UNKNOWN ***                                              |???                                           |???               |           |
|33 XX YY YY       |$809802|*** UNKNOWN ***                                              |???                                           |???               |           |
|34 XX YY YY       |$809826|*** UNKNOWN ***                                              |???                                           |???               |           |
|35 XX YY YY       |$80984A|*** UNKNOWN ***                                              |???                                           |???               |           |
|36 XX YY YY       |$809875|*** UNKNOWN ***                                              |???                                           |???               |           |
|37 XX YY YY       |$8098CA|*** UNKNOWN ***                                              |???                                           |???               |           |
|38 XX YY YY       |$8098DE|*** UNKNOWN ***                                              |???                                           |???               |           |
|39                |$8098F2|Set all three velocities to zero                             |                                              |                  |           |
|3A XX             |$80991C|*** UNKNOWN ***                                              |???                                           |                  |           |
|3B XX             |$8096CF|7E10F2 entry = XX    (7E10F2 = 0xFFFF if XX is 0xFF)         |???                                           |                  |           |
|3C                |$809A38|7E10F2 entry += 1    (Never used?)                           |                                              |                  |           |
|3D                |$809A3E|7E10F2 entry -= 1    (Never used?)                           |                                              |                  |           |
|3E XX             |$809A44|7E10F2 entry += XX   (Never used?)                           |???                                           |                  |           |
|3F XX XX          |$809713|Set X velocity (Absolute)                                    |X-velocity, in pixels per 256 frames          |                  |           |
|40 XX XX          |$809731|Set Y velocity (Absolute)                                    |Y-velocity, in pixels per 256 frames          |                  |           |
|41 XX XX          |$80974F|Set Z velocity (Absolute)                                    |Z-velocity, in pixels per 256 frames          |                  |           |
|42 XX XX XX ...   |$80993D|Long call to assembly (see [42codes.md](42codes.md)          |Destination address                           |Varies            |           |
|43 XX             |$809931|*** UNKNOWN ***                                              |???                                           |                  |           |
|44                |$809BA9|Write tempvar to the wait timer                              |                                              |                  |           |
|45 XX             |$8096CF|*** COPY OF `[3B]` ***                                       |???                                           |                  |           |
|46                |$809A38|*** COPY OF `[3C]` ***                                       |                                              |                  |           |
|47                |$809A3E|*** COPY OF `[3D]` ***                                       |                                              |                  |           |
|48 XX             |$809A44|*** COPY OF `[3E]` ***                                       |???                                           |                  |           |
|49 XX XX          |$809713|*** COPY OF `[3F]` ***                                       |X-velocity, in pixels per 256 frames          |                  |           |
|4A XX XX          |$809731|*** COPY OF `[40]` ***                                       |Y-velocity, in pixels per 256 frames          |                  |           |
|4B XX XX          |$80974F|*** COPY OF `[41]` ***                                       |Z-velocity, in pixels per 256 frames          |                  |           |
|4C XX XX XX ...   |$80993D|*** COPY OF `[42]` ***                                       |Destination address                           |Varies            |           |

## Operations

Used by `[0D XX XX YY ZZ ZZ]` and `[18 XX XX YY ZZ]`

  00 = AND
  01 = OR
  02 = Addition
  03 = XOR
