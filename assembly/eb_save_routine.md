Note on colon notation - the number before the colon refers to save slot, the number
after to value based on it.

[1:0]        = Value is 0 if using slot 1
[1:01]       = Value is 0 or 1 if using slot 1
[1:01, 2:23] = Value is 0 or 1 if using slot 1, and 2 or 3 if using slot 2

    EF/0A4D: C2 31        REP #$31                  Flags and direct page manipulation
    EF/0A4F: 0B           PHD
    EF/0A50: 48           PHA
    EF/0A51: 7B           TDC
    EF/0A52: 69 F0 FF     ADC #$FFF0
    EF/0A55: 5B           TCD
    EF/0A56: 68           PLA
    EF/0A57: 0A           ASL                       A shift left; 1:0, 2:2, 3:4.
    EF/0A58: AA           TAX                       X = 1:0, 2:2, 3:4.
    EF/0A59: 86 0E        STX $0E                   Stash in $0E.
    EF/0A5B: 8A           TXA
    EF/0A5C: 20 8F 08     JSR $088F                 Save function? (Any money it multiplies A by 0x500?)
    EF/0A5F: A6 0E        LDX $0E
    EF/0A61: 8A           TXA
    EF/0A62: 1A           INC                       A = 1:1, 2:3, 3:5.
    EF/0A63: 20 8F 08     JSR $088F                 Save function? (Any money it multiplies A by 0x500?)
    EF/0A66: 2B           PLD
    EF/0A67: 6B           RTL



    EF/088F: C2 31        REP #$31                  Flags and direct page manipulation.
    EF/0891: 0B           PHD
    EF/0892: 48           PHA
    EF/0893: 7B           TDC
    EF/0894: 69 DE FF     ADC #$FFDE
    EF/0897: 5B           TCD
    EF/0898: 68           PLA
    EF/0899: A8           TAY                       Y = [1:01, 2:23, 3:45]
    EF/089A: 84 20        STY $20                   $20 = [1:01, 2:23, 3:45]
    EF/089C: AD A7 00     LDA $00A7                 A7+A9 -> 06+08 and 99C9+99CB. <<< ERRORPOINT >>>
    EF/089F: 85 06        STA $06
    EF/08A1: AD A9 00     LDA $00A9
    EF/08A4: 85 08        STA $08
    EF/08A6: A5 06        LDA $06
    EF/08A8: 8D C9 99     STA $99C9
    EF/08AB: A5 08        LDA $08
    EF/08AD: 8D CB 99     STA $99CB
    EF/08B0: A4 20        LDY $20                   Y = [1:01, 2:23, 3:45]
    EF/08B2: 98           TYA                       A = [1:01, 2:23, 3:45]
    EF/08B3: A0 00 05     LDY #$0500                Y = 0x500
    EF/08B6: 22 32 90 C0  JSR $C09032               A = [0-5] * 0x500.
    EF/08BA: 85 0A        STA $0A                   Store in $0A and $06
    EF/08BC: 64 0C        STZ $0C                   Store null in $0C and $08
    EF/08BE: A5 0A        LDA $0A
    EF/08C0: 85 06        STA $06
    EF/08C2: A5 0C        LDA $0C
    EF/08C4: 85 08        STA $08
    EF/08C6: 18           CLC
    EF/08C7: A5 06        LDA $06                   Load [0-5] * 0x500.
    EF/08C9: 69 20 60     ADC #$6020                A = 0x6020 + ([0-5] * 0x500).
    EF/08CC: 85 06        STA $06                   $06 = 0x6020 + ([0-5] * 0x500).
    EF/08CE: A5 08        LDA $08
    EF/08D0: 69 30 00     ADC #$0030
    EF/08D3: 85 08        STA $08                   $08 = 0x30
    EF/08D5: A5 06        LDA $06
    EF/08D7: 85 1C        STA $1C                   $1C = 0x6020 + ([0-5] * 0x500).
    EF/08D9: A5 08        LDA $08
    EF/08DB: 85 1E        STA $1E                   $1E = 0x30
    EF/08DD: A9 F5 97     LDA #$97F5
    EF/08E0: 85 06        STA $06                   $06 = 0x97F5
    EF/08E2: 8B           PHB                       Push data bank register
    EF/08E3: E2 20        SEP #$20
    EF/08E5: 68           PLA                       Overwrite low order of A with data bank register (A = 0x9700 + <DB>)
    EF/08E6: 85 08        STA $08                   Store DB at $08, null at $09.
    EF/08E8: 64 09        STZ $09
    EF/08EA: C2 20        REP #$20
    EF/08EC: A5 06        LDA $06                   A = 0x97F5
    EF/08EE: 85 16        STA $16                   $16 = 0x97F5
    EF/08F0: A5 08        LDA $08
    EF/08F2: 85 18        STA $18                   $18 = DB
    EF/08F4: 7B           TDC
    EF/08F5: 18           CLC
    EF/08F6: 69 18 00     ADC #$0018                Add 0x18 to direct page, transfer to X, store in $1A.
    EF/08F9: AA           TAX
    EF/08FA: 86 1A        STX $1A
    EF/08FC: A9 7E 00     LDA #$007E
    EF/08FF: 9D 00 00     STA $0000,X               Store 0x7E at (0x18 + direct page: equal to $18).
    EF/0902: A5 1C        LDA $1C                   1C+1E -> 06+08 and 0E+10
    EF/0904: 85 06        STA $06
    EF/0906: A5 1E        LDA $1E
    EF/0908: 85 08        STA $08
    EF/090A: A5 06        LDA $06
    EF/090C: 85 0E        STA $0E
    EF/090E: A5 08        LDA $08
    EF/0910: 85 10        STA $10
    EF/0912: A5 16        LDA $16                   16+18 -> 06+08 and 12+14
    EF/0914: 85 06        STA $06
    EF/0916: A5 18        LDA $18
    EF/0918: 85 08        STA $08
    EF/091A: A5 06        LDA $06                   1C+1E = 0x306020 + ([0-5] * 0x500)
    EF/091C: 85 12        STA $12                   16+18 = 0x7E97F5
    EF/091E: A5 08        LDA $08
    EF/0920: 85 14        STA $14                   1C+1E -> 0E+10 / 16+18 -> 06+08 and 12+14
    EF/0922: A9 D9 01     LDA #$01D9                A = 0x1D9
    EF/0925: 22 ED 8E C0  JSR $C08EED

    C0/8EED: A8           TAY                       Y = 0x1D9
    C0/8EEE: E2 20        SEP #$20                  A = Eight bits
    C0/8EF0: 80 04        BRA $8EF6
    C0/8EF2: B7 12        LDA [$12],Y               Loop: LDA from (address composed of bytes at $12) indexed by Y
    C0/8EF4: 97 0E        STA [$0E],Y                     and STA to (address composed of bytes at $0E) indexed by Y
    C0/8EF6: 88           DEY                             until Y low order = 0 (0x1D8 cycles).
    C0/8EF7: 10 F9        BPL $8EF2                       So 0x1D8 bytes (7E97F5-7E99CD) are moved.
    C0/8EF9: C2 30        REP #$30                  A,X,Y = 16-bit
    C0/8EFB: 6B           RTL                       SRM: 0x20 to 0x1F8 accounted for.

    EF/0929: A9 D9 01     LDA #$01D9                A = 0x1D9
    EF/092C: A6 1C        LDX $1C                   1C+1E -> 06+08
    EF/092E: 86 06        STX $06
    EF/0930: A6 1E        LDX $1E
    EF/0932: 86 08        STX $08
    EF/0934: 18           CLC
    EF/0935: 65 06        ADC $06                   A = 0x61F9 + ([0-5] * 0x500)
    EF/0937: 85 06        STA $06
    EF/0939: 85 1C        STA $1C                   $06 and $1C = 0x61F9 + ([0-5] * 0x500)
    EF/093B: A5 08        LDA $08                   $08 and $1E = 0x30
    EF/093D: 85 1E        STA $1E
    EF/093F: A9 CE 99     LDA #$99CE
    EF/0942: 85 06        STA $06                   $06 = 0x99CE
    EF/0944: 8B           PHB                       Push data bank register
    EF/0945: E2 20        SEP #$20
    EF/0947: 68           PLA                       Overwrite low order of A with data bank register (A = 0x9900 + <DB>)
    EF/0948: 85 08        STA $08                   Store DB at $08, null at $09.
    EF/094A: 64 09        STZ $09
    EF/094C: C2 20        REP #$20
    EF/094E: A5 06        LDA $06                   A = 0x99CE
    EF/0950: 85 16        STA $16                   $16 = 0x99CE
    EF/0952: A5 08        LDA $08
    EF/0954: 85 18        STA $18                   $18 = DB
    EF/0956: A9 7E 00     LDA #$007E
    EF/0959: A6 1A        LDX $1A
    EF/095B: 9D 00 00     STA $0000,X               Store 0x7E at (0x18 + direct page: equal to $18).
    EF/095E: A5 1C        LDA $1C                   1C+1E -> 06+08 and 0E+10
    EF/0960: 85 06        STA $06
    EF/0962: A5 1E        LDA $1E
    EF/0964: 85 08        STA $08
    EF/0966: A5 06        LDA $06
    EF/0968: 85 0E        STA $0E
    EF/096A: A5 08        LDA $08
    EF/096C: 85 10        STA $10
    EF/096E: A5 16        LDA $16                   16+18 -> 06+08 and 12+14
    EF/0970: 85 06        STA $06
    EF/0972: A5 18        LDA $18
    EF/0974: 85 08        STA $08
    EF/0976: A5 06        LDA $06                   1C+1E = 0x3061F9 + ([0-5] * 0x500)
    EF/0978: 85 12        STA $12                   16+18 = 0x7E99CE
    EF/097A: A5 08        LDA $08
    EF/097C: 85 14        STA $14                   1C+1E -> 0E+10 / 16+18 -> 06+08 and 12+14
    EF/097E: A9 3A 02     LDA #$023A                A = 0x23A
    EF/0981: 22 ED 8E C0  JSR $C08EED               Loop as above 0x239 times: 7E99CE-7E9C07, 0x1F9 to 0x432 accounted for.
    EF/0985: A9 3A 02     LDA #$023A                A = 0x23A
    EF/0988: A6 1C        LDX $1C                   1C+1E -> 06+08
    EF/098A: 86 06        STX $06
    EF/098C: A6 1E        LDX $1E
    EF/098E: 86 08        STX $08
    EF/0990: 18           CLC
    EF/0991: 65 06        ADC $06                   A = 0x623A + ([0-5] * 0x500)
    EF/0993: 85 06        STA $06
    EF/0995: 85 1C        STA $1C                   $06 and $1C = 0x623A + ([0-5] * 0x500)
    EF/0997: A5 08        LDA $08                   $08 and $1E = 0x30
    EF/0999: 85 1E        STA $1E
    EF/099B: A9 08 9C     LDA #$9C08
    EF/099E: 85 06        STA $06                   $06 = 0x9C08
    EF/09A0: 8B           PHB                       Push data bank register
    EF/09A1: E2 20        SEP #$20
    EF/09A3: 68           PLA                       Overwrite low order of A with data bank register (A = 0x9C00 + <DB>)
    EF/09A4: 85 08        STA $08                   Store DB at $08, null at $09.
    EF/09A6: 64 09        STZ $09
    EF/09A8: C2 20        REP #$20
    EF/09AA: A5 06        LDA $06                   A = 0x9C08
    EF/09AC: 85 16        STA $16                   $16 = 0x9C08
    EF/09AE: A5 08        LDA $08
    EF/09B0: 85 18        STA $18                   $18 = DB
    EF/09B2: A9 7E 00     LDA #$007E
    EF/09B5: A6 1A        LDX $1A
    EF/09B7: 9D 00 00     STA $0000,X               Store 0x7E at (0x18 + direct page: equal to $18).
    EF/09BA: A5 1C        LDA $1C                   1C+1E -> 06+08 and 0E+10
    EF/09BC: 85 06        STA $06
    EF/09BE: A5 1E        LDA $1E
    EF/09C0: 85 08        STA $08
    EF/09C2: A5 06        LDA $06
    EF/09C4: 85 0E        STA $0E
    EF/09C6: A5 08        LDA $08
    EF/09C8: 85 10        STA $10
    EF/09CA: A5 16        LDA $16                   16+18 -> 06+08 and 12+14
    EF/09CC: 85 06        STA $06
    EF/09CE: A5 18        LDA $18
    EF/09D0: 85 08        STA $08
    EF/09D2: A5 06        LDA $06                   1C+1E = 0x30623A + ([0-5] * 0x500)
    EF/09D4: 85 12        STA $12                   16+18 = 0x7E9C08
    EF/09D6: A5 08        LDA $08
    EF/09D8: 85 14        STA $14                   1C+1E -> 0E+10 / 16+18 -> 06+08 and 12+14
    EF/09DA: A9 80 00     LDA #$0080                A = 0x80
    EF/09DD: 22 ED 8E C0  JSR $C08EED               Loop as above 0x7F times: 7E9C08-7E9C87, 0x433 to 0x4B2 accounted for.
    EF/09E1: A5 0A        LDA $0A                   0A+0C -> 06+08
    EF/09E3: 85 06        STA $06
    EF/09E5: A5 0C        LDA $0C
    EF/09E7: 85 08        STA $08
    EF/09E9: 18           CLC
    EF/09EA: A5 06        LDA $06
    EF/09EC: 69 1C 60     ADC #$601C                A = 0x601C + ([0-5] * 0x500)
    EF/09EF: 85 06        STA $06
    EF/09F1: A5 08        LDA $08
    EF/09F3: 69 30 00     ADC #$0030
    EF/09F6: 85 08        STA $08                   06+08 = 0x30601C
    EF/09F8: A4 20        LDY $20
    EF/09FA: 98           TYA                       A = [1:01, 2:23, 3:45]
    EF/09FB: 20 34 07     JSR $0734

    EF/0734: C2 31        REP #$31                  Flags and direct page manipulation
    EF/0736: 0B           PHD
    EF/0737: 48           PHA
    EF/0738: 7B           TDC
    EF/0739: 69 F0 FF     ADC #$FFF0
    EF/073C: 5B           TCD
    EF/073D: 68           PLA
    EF/073E: A0 00 05     LDY #$0500
    EF/0741: 22 32 90 C0  JSR $C09032               A = [0-5] * 0x500.
    EF/0745: 85 06        STA $06                   $06 = [0-5] * 0x500.
    EF/0747: 64 08        STZ $08                   $08 = 0x00
    EF/0749: 18           CLC
    EF/074A: A5 06        LDA $06
    EF/074C: 69 20 60     ADC #$6020                A = 0x6020 + ([0-5] * 0x500).
    EF/074F: 85 06        STA $06                   $06 = 0x6020 + ([0-5] * 0x500).
    EF/0751: A5 08        LDA $08
    EF/0753: 69 30 00     ADC #$0030                $08 = 0x30
    EF/0756: 85 08        STA $08
    EF/0758: A2 00 00     LDX #$0000
    EF/075B: 8A           TXA
    EF/075C: 85 0E        STA $0E                   X, A, $0E = 0x00
    EF/075E: 80 13        BRA $0773                 Loop: load from (address composed of bytes at $06) - in other
    EF/0760: A7 06        LDA [$06]                       words, 0x306020 + n. Ignore the high order, store in $02,
    EF/0762: 29 FF 00     AND #$00FF                      move X to A and add A (current byte). Move A back to X.
    EF/0765: 85 02        STA $02                         Increment $06 and $0E; when $0E is equal to 0x4E0, end
    EF/0767: 8A           TXA                             the loop.
    EF/0768: 18           CLC                             X holds the running total of bytes from 0x20 to 0x4E0.
    EF/0769: 65 02        ADC $02
    EF/076B: AA           TAX
    EF/076C: E6 06        INC $06
    EF/076E: A5 0E        LDA $0E
    EF/0770: 1A           INC
    EF/0771: 85 0E        STA $0E
    EF/0773: C9 E0 04     CMP #$04E0
    EF/0776: 90 E8        BCC $0760
    EF/0778: 8A           TXA                       A = Sum of bytes from 0x20 to 0x4E0.
    EF/0779: 2B           PLD
    EF/077A: 60           RTS

    EF/09FE: AA           TAX
    EF/09FF: 86 1A        STX $1A
    EF/0A01: 8A           TXA                       A, X, $1A = Sum of bytes from 0x20 to 0x4E0.
    EF/0A02: 87 06        STA [$06]                 0x306500 + ([0-5] * 0x500) = Sum of bytes from 0x20 to 0x4E0.
    EF/0A04: A4 20        LDY $20
    EF/0A06: 98           TYA                       A = [1:01, 2:23, 3:45]
    EF/0A07: 20 34 07     JSR $0734                 Repeat the subroutine above.
    EF/0A0A: 85 02        STA $02                   $02 = Sum of bytes from 0x20 to 0x4E0.
    EF/0A0C: A6 1A        LDX $1A
    EF/0A0E: 8A           TXA                       A = Sum of bytes from 0x20 to 0x4E0.
    EF/0A0F: C5 02        CMP $02                   Compare $02 to A.
    EF/0A11: F0 03        BEQ $0A16
    EF/0A13: 4C 9C 08     JMP $089C                 If ($02 != A), an error has occured; go to <<< ERRORPOINT >>>
    EF/0A16: A5 0A        LDA $0A                   If ($02 == A), do the following.
    EF/0A18: 85 06        STA $06                   $06 = [0-5] * 0x500.
    EF/0A1A: A5 0C        LDA $0C
    EF/0A1C: 85 08        STA $08                   $08 = 0x00
    EF/0A1E: 18           CLC
    EF/0A1F: A5 06        LDA $06
    EF/0A21: 69 1E 60     ADC #$601E
    EF/0A24: 85 06        STA $06
    EF/0A26: A5 08        LDA $08
    EF/0A28: 69 30 00     ADC #$0030
    EF/0A2B: 85 08        STA $08                   06+08 = 0x30601E
    EF/0A2D: A4 20        LDY $20
    EF/0A2F: 98           TYA                       A = [1:01, 2:23, 3:45]
    EF/0A30: 20 7B 07     JSR $077B

    EF/077B: C2 31        REP #$31                  Flags and direct page manipulation.
    EF/077D: 0B           PHD
    EF/077E: 48           PHA
    EF/077F: 7B           TDC
    EF/0780: 69 F0 FF     ADC #$FFF0
    EF/0783: 5B           TCD
    EF/0784: 68           PLA
    EF/0785: A0 00 05     LDY #$0500
    EF/0788: 22 32 90 C0  JSR $C09032
    EF/078C: 85 06        STA $06
    EF/078E: 64 08        STZ $08
    EF/0790: 18           CLC
    EF/0791: A5 06        LDA $06
    EF/0793: 69 20 60     ADC #$6020
    EF/0796: 85 06        STA $06                   $06 = 0x6020 + ([0-5] * 0x500).
    EF/0798: A5 08        LDA $08
    EF/079A: 69 30 00     ADC #$0030
    EF/079D: 85 08        STA $08                   $08 = 0x30
    EF/079F: A2 00 00     LDX #$0000
    EF/07A2: 8A           TXA
    EF/07A3: 85 0E        STA $0E                   X, A, $0E = 0x00
    EF/07A5: 80 11        BRA $07B8                 Loop: load from (address composed of bytes at $06) - in other
    EF/07A7: A7 06        LDA [$06]                       words, 0x306020 + n. Store in $02, transfer X to A, XOR
    EF/07A9: 85 02        STA $02                         A against $02, and transfer A back to X. Increment $06
    EF/07AB: 8A           TXA                             twice and $0E once; when $0E is equal to 0x270 (half of
    EF/07AC: 45 02        EOR $02                         0x4E0 - it's going two bytes at a time instead of one
    EF/07AE: AA           TAX                             so it makes sense), end the loop.
    EF/07AF: E6 06        INC $06                         X holds the result of the XOR looping madness.
    EF/07B1: E6 06        INC $06
    EF/07B3: A5 0E        LDA $0E
    EF/07B5: 1A           INC
    EF/07B6: 85 0E        STA $0E
    EF/07B8: C9 70 02     CMP #$0270
    EF/07BB: 90 EA        BCC $07A7
    EF/07BD: 8A           TXA                       A = XOR looping madness result.
    EF/07BE: 2B           PLD
    EF/07BF: 60           RTS

    EF/0A33: AA           TAX
    EF/0A34: 86 1A        STX $1A
    EF/0A36: 8A           TXA                       A, X, $1A = XOR looping madness result.
    EF/0A37: 87 06        STA [$06]                 0x306500 + ([0-5] * 0x500) = XOR looping madness result.
    EF/0A39: A4 20        LDY $20
    EF/0A3B: 98           TYA                       A = [1:01, 2:23, 3:45]
    EF/0A3C: 20 7B 07     JSR $077B                 Repeat the subroutine above.
    EF/0A3F: 85 02        STA $02                   $02 = XOR looping madness result.
    EF/0A41: A6 1A        LDX $1A
    EF/0A43: 8A           TXA                       A = XOR looping madness result.
    EF/0A44: C5 02        CMP $02                   Compare $02 to A.
    EF/0A46: F0 03        BEQ $0A4B
    EF/0A48: 4C 9C 08     JMP $089C                 If ($02 != A), an error has occured; go to <<< ERRORPOINT >>>
    EF/0A4B: 2B           PLD                       If ($02 == A), the saving process completes.
    EF/0A4C: 60           RTS                       The end! The glorious, glorious end!