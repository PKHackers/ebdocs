# $C4617C - References $C400D4.
  origA: ???
  origX: Movement pointer to load.

    C4/617C: C2 31        REP #$31                  Flags and direct page manipulation.
    C4/617E: 0B           PHD
    C4/617F: 48           PHA
    C4/6180: 7B           TDC
    C4/6181: 69 EE FF     ADC #$FFEE
    C4/6184: 5B           TCD
    C4/6185: 68           PLA
    C4/6186: 9B           TXY
    C4/6187: 84 10        STY $10                   Y, $10 = origX
    C4/6189: AA           TAX                       X = origA
    C4/618A: 22 5A 60 C4  JSR $C4605A

    C4/605A: C2 31        REP #$31                  Flags and direct page manipulation.
    C4/605C: 0B           PHD
    C4/605D: 48           PHA
    C4/605E: 7B           TDC
    C4/605F: 69 EE FF     ADC #$FFEE
    C4/6062: 5B           TCD
    C4/6063: 68           PLA
    C4/6064: AA           TAX                       X, $10 = origA
    C4/6065: 86 10        STX $10
    C4/6067: A9 00 00     LDA #$0000                A, $0E = 0
    C4/606A: 85 0E        STA $0E
    C4/606C: 80 14        BRA $6082                 Loop
    C4/606E: 0A           ASL                       A *= 2
    C4/606F: 48           PHA                       Store A
    C4/6070: A6 10        LDX $10
    C4/6072: 8A           TXA                       A = origA
    C4/6073: FA           PLX                       X = previously stored A
    C4/6074: DD 9A 2C     CMP $2C9A,X               Compare origA to 7E2C9A + previously stored A
    C4/6077: D0 04        BNE $607D                 If not equal, increment $0E.
    C4/6079: A5 0E        LDA $0E
    C4/607B: 80 0D        BRA $608A
    C4/607D: A5 0E        LDA $0E
    C4/607F: 1A           INC
    C4/6080: 85 0E        STA $0E
    C4/6082: C9 1E 00     CMP #$001E                Thirty iterations max.
    C4/6085: 90 E7        BCC $606E
    C4/6087: A9 FF FF     LDA #$FFFF
    C4/608A: 2B           PLD
    C4/608B: 6B           RTL

Loop exit conditions:
  1. Thirty failures to match origA to (7E2C9A + X). A = 0xFFFF in this case.
  2. First successful match between origA and (7E2C9A + X). A = number of failures. Number of failures also means "7E2C9A + 2X is equal to origA.

    C4/618E: AA           TAX                       A, X = number of failures or 0xFFFF
    C4/618F: E0 FF FF     CPX #$FFFF                If thirty failures occured, end.
    C4/6192: F0 36        BEQ $61CA
    C4/6194: A9 D4 00     LDA #$00D4                Otherwise, load up the root of movement pointers.
    C4/6197: 85 06        STA $06
    C4/6199: A9 C4 00     LDA #$00C4
    C4/619C: 85 08        STA $08
    C4/619E: A4 10        LDY $10
    C4/61A0: 98           TYA                       A, Y, $10 = origX
    C4/61A1: 85 04        STA $04
    C4/61A3: 0A           ASL
    C4/61A4: 65 04        ADC $04
    C4/61A6: 85 0E        STA $0E                   $0E = origX * 3
    C4/61A8: 1A           INC
    C4/61A9: 1A           INC
    C4/61AA: A4 06        LDY $06
    C4/61AC: 84 0A        STY $0A
    C4/61AE: A4 08        LDY $08
    C4/61B0: 84 0C        STY $0C                   Copy root of movement pointers from $06+08 to $0A+0C
    C4/61B2: 18           CLC
    C4/61B3: 65 0A        ADC $0A                   A = Root of movement pointers + (origX * 3) + 2
    C4/61B5: 85 0A        STA $0A
    C4/61B7: A7 0A        LDA [$0A]
    C4/61B9: 29 FF 00     AND #$00FF
    C4/61BC: A8           TAY                       Y = Highest order component of pointer.
    C4/61BD: A5 0E        LDA $0E
    C4/61BF: 18           CLC
    C4/61C0: 65 06        ADC $06
    C4/61C2: 85 06        STA $06
    C4/61C4: A7 06        LDA [$06]                 A = Lower order components of pointer.
    C4/61C6: 22 F9 93 C0  JSR $C093F9               YY/AAAA = Movement pointer.

    C0/93F9: 48           PHA                       Multiply X by two. X is number of failures - see the
    C0/93FA: 8A           TXA                         loop note above.
    C0/93FB: 0A           ASL
    C0/93FC: AA           TAX
    C0/93FD: 68           PLA
    C0/93FE: 22 03 94 C0  JSR $C09403               YY/AAAA = Movement pointer, X = 2 * number of failures (NOF).

    C0/9403: 5A           PHY
    C0/9404: 48           PHA                       Store Y, then A, on the stack.
    C0/9405: BD 62 0A     LDA $0A62,X
    C0/9408: 10 02        BPL $940C                 If 7E0A62 + (2 * NOF) is under 0x8000, continue.
    C0/940A: 80 FE        BRA $940A                 If 7E0A62 + (2 * NOF) is 0x8000 or over, start an empty infinite loop.
    C0/940C: 20 99 9C     JSR $9C99

    C0/9C99: BD DA 0A     LDA $0ADA,X               A = 7E0ADA + (2 * NOF)
    C0/9C9C: 30 16        BMI $9CB4                 If 0x8000 or greater, jump to the end right away.
    C0/9C9E: DA           PHX                       Else, push X.
    C0/9C9F: AD 54 0A     LDA $0A54                 A = 7E0A54.
    C0/9CA2: 48           PHA                       Push A.
    C0/9CA3: BD DA 0A     LDA $0ADA,X               A = 7E0ADA + (2 * NOF).
    C0/9CA6: 8D 54 0A     STA $0A54                 A, 7E0A54 = 7E0ADA + (2 * NOF)
    C0/9CA9: AA           TAX                       -- Loop -- X = 7E0ADA + (2 * NOF)
    C0/9CAA: BD 5A 12     LDA $125A,X               -- Loop -- A = 7E125A + (7E0ADA + (2 * NOF))
    C0/9CAD: 10 FA        BPL $9CA9                 -- Loop -- If less than 0x8000, loop back - messes with X.
    C0/9CAF: 68           PLA                       A = Old 7E0A54.
    C0/9CB0: 9D 5A 12     STA $125A,X               Store old 7E0A54 at 7E125A + X (unknown).
    C0/9CB3: FA           PLX                       X = (2 * NOF).
    C0/9CB4: 60           RTS

    C0/940F: 20 03 9D     JSR $9D03

    C0/9D03: AC 54 0A     LDY $0A54                 Y = 7E0A54
    C0/9D06: 10 02        BPL $9D0A                 If less than 0x8000, set the carry flag and end.
      C0/9D0A: B9 5A 12     LDA $125A,Y             Else, get (7E125A + Y) into A, store at 7E0A54, and clear carry.
      C0/9D0D: 8D 54 0A     STA $0A54
      C0/9D10: 18           CLC
      C0/9D11: 60           RTS
    C0/9D08: 38           SEC
    C0/9D09: 60           RTS

    C0/9412: 98           TYA                       A = (Old?) 7E0A54
    C0/9413: 9D DA 0A     STA $0ADA,X               Store (old?) 7E0A54 at 7E0ADA + (2 * NOF).
    C0/9416: A9 FF FF     LDA #$FFFF
    C0/9419: 99 5A 12     STA $125A,Y               Store 0xFFFF at 7E125A + (2 * NOF).
    C0/941C: 68           PLA                       Pull A and Y, then put them back.
    C0/941D: 7A           PLY
    C0/941E: 5A           PHY
    C0/941F: 48           PHA
    C0/9420: 20 A1 9D     JSR $9DA1

    C0/9DA1: A9 3B 94     LDA #$943B                Store 0x943B at 7E107A + (2 * NOF).
    C0/9DA4: 9D 7A 10     STA $107A,X
    C0/9DA7: A9 C0 00     LDA #$00C0                Store 0x00C0 at 7E10B6 + (2 * NOF).
    C0/9DAA: 9D B6 10     STA $10B6,X
    C0/9DAD: 60           RTS

    C0/9423: 9B           TXY                       Y = (2 * NOF).
    C0/9424: BE DA 0A     LDX $0ADA,Y               X = 7E0ADA + (2 * NOF).
    C0/9427: 68           PLA                       A = Low component of pointer.
    C0/9428: 9D FE 13     STA $13FE,X               Store at 7E13FE + (7E0ADA + (2 * NOF)).
    C0/942B: 68           PLA
    C0/942C: 29 FF 00     AND #$00FF
    C0/942F: 9D 8A 14     STA $148A,X               Store the high component at 7E148A + (7E0ADA + (2 * NOF)).
    C0/9432: 9E 72 13     STZ $1372,X               Store zero to 7E1372 + (7E0ADA + (2 * NOF)).
    C0/9435: 9E E6 12     STZ $12E6,X               Store zero to 7E12E6 + (7E0ADA + (2 * NOF)).
    C0/9438: 98           TYA                       A = (2 * NOF).
    C0/9439: 4A           LSR                       A = NOF.
    C0/943A: 18           CLC                       Clear carry.
    C0/943B: 6B           RTL

    C0/9402: 6B           RTL

    C4/61CA: 2B           PLD
    C4/61CB: 6B           RTL                       End.