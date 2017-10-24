# $C0AB06 - Music loader

|ADDRESS  |BYTES        |CODE         |COMMENTS                     |
|---------|-------------|-------------|-----------------------------|
|`$C0AB06`|`C2 30      `|`REP #$30   `|Index (16 bit) Accum (16 bit)|
|`$C0AB08`|`8D C6 00   `|`STA $00C6  `|                             |
|`$C0AB0B`|`8E C8 00   `|`STX $00C8  `|                             |
|`$C0AB0E`|`8B         `|`PHB        `|                             |
|`$C0AB0F`|`F4 00 00   `|`PEA #$0000 `|                             |
|`$C0AB12`|`AB         `|`PLB        `|                             |
|`$C0AB13`|`AB         `|`PLB        `|                             |
|`$C0AB14`|`0B         `|`PHD        `|                             |
|`$C0AB15`|`F4 00 00   `|`PEA #$0000 `|                             |
|`$C0AB18`|`2B         `|`PLD        `|                             |
|`$C0AB19`|`A0 00 00   `|`LDY #$0000 `|                             |
|`$C0AB1C`|`AD 43 21   `|`LDA $2143  `|APU I/O Port                 |
|`$C0AB1F`|`C9 AA BB   `|`CMP #$BBAA `|                             |
|`$C0AB22`|`F0 03      `|`BEQ $AB27  `|                             |
|`$C0AB24`|`20 A8 AB   `|`JSR $ABA8  `|                             |
|`$C0AB27`|`E2 20      `|`SEP #$20   `|Accum (8 bit)                |
|`$C0AB29`|`AD 1E 00   `|`LDA $001E  `|                             |
|`$C0AB2C`|`29 7F      `|`AND #$7F   `|                             |
|`$C0AB2E`|`8D 1E 00   `|`STA $001E  `|                             |
|`$C0AB31`|`8F 00 42 00`|`STA $004200`|                             |
|`$C0AB35`|`A9 CC      `|`LDA #$CC   `|                             |
|`$C0AB37`|`80 26      `|`BRA $AB5F  `|                             |
|`$C0AB39`|`B7 C6      `|`LDA [$C6],Y`|                             |
|`$C0AB3B`|`C8         `|`INY        `|                             |
|`$C0AB3C`|`EB         `|`XBA        `|                             |
|`$C0AB3D`|`A9 00      `|`LDA #$00   `|                             |
|`$C0AB3F`|`80 0B      `|`BRA $AB4C  `|                             |
|`$C0AB41`|`EB         `|`XBA        `|                             |
|`$C0AB42`|`B7 C6      `|`LDA [$C6],Y`|                             |
|`$C0AB44`|`C8         `|`INY        `|                             |
|`$C0AB45`|`EB         `|`XBA        `|                             |
|`$C0AB46`|`CD 40 21   `|`CMP $2140  `|APU I/O Port                 |
|`$C0AB49`|`D0 FB      `|`BNE $AB46  `|                             |
|`$C0AB4B`|`1A         `|`INC        `|                             |
|`$C0AB4C`|`C2 20      `|`REP #$20   `|Accum (16 bit)               |
|`$C0AB4E`|`8D 40 21   `|`STA $2140  `|APU I/O Port                 |
|`$C0AB51`|`E2 20      `|`SEP #$20   `|Accum (8 bit)                |
|`$C0AB53`|`CA         `|`DEX        `|                             |
|`$C0AB54`|`D0 EB      `|`BNE $AB41  `|                             |
|`$C0AB56`|`CD 40 21   `|`CMP $2140  `|APU I/O Port                 |
|`$C0AB59`|`D0 FB      `|`BNE $AB56  `|                             |
|`$C0AB5B`|`69 03      `|`ADC #$03   `|                             |
|`$C0AB5D`|`F0 FC      `|`BEQ $AB5B  `|                             |
|`$C0AB5F`|`48         `|`PHA        `|                             |
|`$C0AB60`|`C2 20      `|`REP #$20   `|Accum (16 bit)               |
|`$C0AB62`|`B7 C6      `|`LDA [$C6],Y`|                             |
|`$C0AB64`|`D0 06      `|`BNE $AB6C  `|                             |
|`$C0AB66`|`AA         `|`TAX        `|                             |
|`$C0AB67`|`A9 00 05   `|`LDA #$0500 `|                             |
|`$C0AB6A`|`80 07      `|`BRA $AB73  `|                             |
|`$C0AB6C`|`AA         `|`TAX        `|                             |
|`$C0AB6D`|`C8         `|`INY        `|                             |
|`$C0AB6E`|`C8         `|`INY        `|                             |
|`$C0AB6F`|`B7 C6      `|`LDA [$C6],Y`|                             |
|`$C0AB71`|`C8         `|`INY        `|                             |
|`$C0AB72`|`C8         `|`INY        `|                             |
|`$C0AB73`|`8D 42 21   `|`STA $2142  `|APU I/O Port                 |
|`$C0AB76`|`E2 20      `|`SEP #$20   `|Accum (8 bit)                |
|`$C0AB78`|`E0 01 00   `|`CPX #$0001 `|                             |
|`$C0AB7B`|`A9 00      `|`LDA #$00   `|                             |
|`$C0AB7D`|`2A         `|`ROL        `|                             |
|`$C0AB7E`|`8D 41 21   `|`STA $2141  `|APU I/O Port                 |
|`$C0AB81`|`69 7F      `|`ADC #$7F   `|                             |
|`$C0AB83`|`68         `|`PLA        `|                             |
|`$C0AB84`|`8D 40 21   `|`STA $2140  `|APU I/O Port                 |
|`$C0AB87`|`CD 40 21   `|`CMP $2140  `|APU I/O Port                 |
|`$C0AB8A`|`D0 FB      `|`BNE $AB87  `|                             |
|`$C0AB8C`|`70 AB      `|`BVS $AB39  `|                             |
|`$C0AB8E`|`C2 20      `|`REP #$20   `|Accum (16 bit)               |
|`$C0AB90`|`AD 40 21   `|`LDA $2140  `|APU I/O Port                 |
|`$C0AB93`|`D0 FB      `|`BNE $AB90  `|                             |
|`$C0AB95`|`E2 20      `|`SEP #$20   `|Accum (8 bit)                |
|`$C0AB97`|`AD 1E 00   `|`LDA $001E  `|                             |
|`$C0AB9A`|`09 80      `|`ORA #$80   `|                             |
|`$C0AB9C`|`8D 1E 00   `|`STA $001E  `|                             |
|`$C0AB9F`|`8F 00 42 00`|`STA $004200`|                             |
|`$C0ABA3`|`C2 20      `|`REP #$20   `|Accum (16 bit)               |
|`$C0ABA5`|`2B         `|`PLD        `|                             |
|`$C0ABA6`|`AB         `|`PLB        `|                             |
|`$C0ABA7`|`6B         `|`RTL        `|                             |