## In-place version

* Update the start pointer for [1F 19] ([1F 18] starts in the same place - no changes needed)

`C1/8486: A9 8A 65     LDA #$658A       <-- Formerly #$65AA`


* Entry points for the new [1F 18] and [1F 19]


    C1/6582: C2 31        REP #$31         <-- [1F 18] start point (Print Item Name with Indefinite Article)
    C1/6584: 0B           PHD
    C1/6585: F4 AD 65     PEA $65AD
    C1/6588: 80 06        BRA $6590
    C1/658A: C2 31        REP #$31         <-- [1F 19] start point (Print Item Name with Definite Article)
    C1/658C: 0B           PHD
    C1/658D: F4 BB 65     PEA $65BB


* Common assembly for both text codes: reserve direct page space, find what item we're talking about (handling the 00-arg case along the way), read a gender/quantity value from a new 256-byte table of our creation.


    C1/6590: 48           PHA
    C1/6591: 7B           TDC
    C1/6592: 69 EE FF     ADC #$FFEE       <-- Reserve some direct-page space
    C1/6595: 5B           TCD
    C1/6596: 68           PLA
    C1/6597: 8A           TXA
    C1/6598: F0 06        BEQ $65A0        <-- Handle the 00-argument case
    C1/659A: 85 06        STA $06
    C1/659C: 64 08        STZ $08
    C1/659E: 80 03        BRA $65A3
    C1/65A0: 20 DC 03     JSR $03DC
    C1/65A3: A6 06        LDX $06
    C1/65A5: BF -- -- --  LDA $------,X    <-- Address of a 256-byte table, one byte per item
    C1/65A9: 29 FF 00     AND #$00FF           Each byte has one of these 4 values: 00, 04, 08, 0C
    C1/65AC: AA           TAX                  (4 combinations of masculine/feminine and singular/plural)
    C1/65AD: 60           RTS              <-- Jump to the address we loaded with PEA earlier


* [1F 18]: Load an indefinite article string


    C1/65AE: BF -- -- --  LDA $------,X    <-- Address of a table of four 4-byte pointers
    C1/65B2: A5 0E        STA $0E                to the strings for indefinite articles
    C1/65B4: BF -- -- --  LDA $------,X    <-- That address plus two
    C1/65B8: A5 10        STA $10
    C1/65BA: 80 0C        BRA $65C8


* [1F 19]: Load a definite article string


    C1/65BC: BF -- -- --  LDA $------,X    <-- Address of a table of four 4-byte pointers
    C1/65C0: A5 0E        STA $0E                to the strings for definite articles
    C1/65C2: BF -- -- --  LDA $------,X    <-- That address plus two
    C1/65C6: A5 10        STA $10


* Common end assembly for both text codes


    C1/65C8: 22 B1 86 C1  JSR $C186B1      <-- Print the article string
    C1/65CC: A6 06        LDX $06          <-- Retrieve the item number from $06
    C1/65CE: 2B           PLD
    C1/65CF: 4C BF 46     JMP $46BF        <-- [1C 05 XX]


## Original attempt

    C2 31        REP #$31
    0B           PHD
    48           PHA
    7B           TDC
    69 EE FF     ADC #$FFEE       <-- Reserve some direct-page space.
    5B           TCD
    68           PLA
    8A           TXA
    F0 06        BEQ $0006        <-- Handle the 00-argument case.
    85 06        STA $06              Consequence: code must be in C1 bank (short JSR).
    64 08        STZ $08
    80 03        BRA $0003
    20 DC 03     JSR $03DC
    A6 06        LDX $06


There might not be enough space where [1F 18/19] was, so the following might need to be relocated to another bank and JSLed to...

    BF -- -- --  LDA $------,X    <-- Address of a 256-byte table, one byte per item.
    29 FF 00     AND #$00FF           Values 00-03, one for each combination of
    0A           ASL                  masculine/feminine and singular/plural.
    0A           ASL                  Becomes 00, 04, 08, 0C via shifting.
    AA           TAX
    BF -- -- --  LDA $------,X    <-- Address of a table of four 4-byte pointers.
    A5 0E        STA $0E              These four pointers point at the article strings.
    BF -- -- --  LDA $------,X    <-- Address from immediately above, plus two.
    A5 10        STA $10

Not sure if there's a display-text function that handles both in- and out-of-battle cases. I'll take the gamble that C1/86B1 can handle both.

`22 B1 86 C1  JSR $C186B1`

If the above code was JSLed to, return now.


    A6 06        LDX $06          <-- Retrieve the item number from $06.
    2B           PLD
    4C BF 46     JMP $46BF        <-- [1C 05 xx]
