# 42 XX XX XX Routine descriptions

## BRIEF

42 6E 6A C4: Fills the accumulator with 0005 if Ness is facing down/right, down, or down/left; otherwise fills it with 0001.

## DETAILED

42 6E 6A C4:

    C4/6A6E: C2 31        REP #%00011111  ;reset 5 rightmost system registers
    C4/6A6F: AD 7F 98     LDA $987F       ;load accumulator with value at page+$987F
    C4/6A70: 0A           ASL A           ;left-bitshift accumulator
    C4/6A71: AA           TAX             ;transfer accumulator to X
    C4/6A72: BF 5E 6A C4  LDA $C46A5E,X   ;load accumulator with value at $C46A5E+X
    C4/6A76: 6B           RTL             ;return

    Clear some status register bits.
    If Ness is facing:   Then $7E987F holds:   Which after a bitshift is:

    Up                   00 (00000000)         00 (00000000)
    Up-right             01 (00000001)         02 (00000010)
    Right                02 (00000010)         04 (00000100)
    Down-right           03 (00000011)         06 (00000110)
    Down                 04 (00000100)         08 (00001000)
    Down-left            05 (00000101)         0A (00001010)
    Left                 06 (00000110)         0C (00001100)
    Up-left              07 (00000111)         0E (00001110)

    Then, put this value in X, and go to $C46A5E + X.

    If X is:   Then the address is:      Value:
    00         C46A5E                    01 00
    02         C46A60                    01 00
    04         C46A62                    01 00
    06         C46A64                    05 00
    08         C46A66                    05 00
    0A         C46A68                    05 00
    0C         C46A6A                    05 00
    0E         C46A6C                    01 00

Final result: Stick 01 00 or 05 00 in the accumulator based on which way Ness is facing.
