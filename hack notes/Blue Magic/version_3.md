# BLUE MAGIC SYSTEM, VERSION THREE

B7's addressing mode didn't work as intended, thus version three.
The three level bytes in each PSI entry will become a long pointer.
This pointer is to an eight-byte block that specifies which event flags toggle the PSI for each character.
This version does not allow Jeff to use PSI.

------------------------------------------------------------------------

    $C1/D790: F0 10        BEQ $D7A2           Ness    = $C1D7A2
    $C1/D792: C9 01 00     CMP #$0001
    $C1/D795: F0 6E        BEQ $D805           Paula   = $C1D805
    $C1/D797: C9 03 00     CMP #$0003
    $C1/D79A: D0 03        BNE $D79F
    $C1/D79C: 4C 67 D8     JMP $D867           Poo     = $C1D867
    $C1/D79F: 4C C7 D8     JMP $D8C7           Default = $C1D8C7

This is related to displaying the "Character learned PSI X!" message when levelling up. Since PSIs are based on event flags, this is no longer needed, so:

   `$C1/D790: 4C C7 D8     JMP $D8C7           Always use the default case`

------------------------------------------------------------------------

    $C1/C1E8: 98           TYA
    $C1/C1E9: F0 0C        BEQ $C1F7           Ness    = $C1C1F7
    $C1/C1EB: C9 01 00     CMP #$0001
    $C1/C1EE: F0 1C        BEQ $C20C           Paula   = $C1C20C
    $C1/C1F0: C9 03 00     CMP #$0003
    $C1/C1F3: F0 2C        BEQ $C221           Poo     = $C1C221
    $C1/C1F5: 80 3D        BRA $C234           Default = $C1C234

This is involved with the actual PSI selection window.
Replace it with:

    $C1/C1E8: 98           TYA
    $C1/C1E9: C9 04 00     CMP #$0004
    $C1/C1EC: B0 46        BCS $C234
    $C1/C1EE: 0A           ASL
    $C1/C1EF: 48           PHA
    $C1/C1F0: A5 0E        LDA $0E
    $C1/C1F2: 18           CLC
    $C1/C1F3: 65 06        ADC $06
    $C1/C1F5: 85 06        STA $06
    $C1/C1F7: A0 06 00     LDY #$0006
    $C1/C1FA: B7 06        LDA [$06],Y
    $C1/C1FC: 48           PHA
    $C1/C1FD: A0 08 00     LDY #$0008
    $C1/C200: B7 06        LDA [$06],Y
    $C1/C202: 29 FF 00     AND #$00FF
    $C1/C205: 85 08        STA $08
    $C1/C207: 68           PLA
    $C1/C208: 85 06        STA $06
    $C1/C20A: 7A           PLY
    $C1/C20B: B7 06        LDA [$06],Y         A = Event flag
    $C1/C20D: F0 08        BEQ $C217           0x0000 flag case.
    $C1/C20F: DA           PHX                 Normal flag case.
    $C1/C210: 5A           PHY
    $C1/C211: 22 28 16 C2  JSL $C21628
    $C1/C215: 7A           PLY
    $C1/C216: FA           PLX
    $C1/C217: E2 20        SEP #$20
    $C1/C219: 85 00        STA $00
    $C1/C21B: 85 10        STA $10
    $C1/C21D: 80 15        BRA $C234

------------------------------------------------------------------------

    $C1/C5D1: F0 0C        BEQ $C5DF           Ness    = $C1C5DF
    $C1/C5D3: C9 01 00     CMP #$0001
    $C1/C5D6: F0 14        BEQ $C5EC           Paula   = $C1C5EC
    $C1/C5D8: C9 03 00     CMP #$0003
    $C1/C5DB: F0 1C        BEQ $C5F9           Poo     = $C1C5F9
    $C1/C5DD: 80 25        BRA $C604           Default = $C1C604

This is also involved with the actual PSI selection window.
Replace it with:

    $C1/C5D1: C9 04 00     CMP #$0004
    $C1/C5D4: B0 2E        BCS $C604
    $C1/C5D6: 0A           ASL
    $C1/C5D7: 48           PHA
    $C1/C5D8: A0 06 00     LDY #$0006
    $C1/C5DB: B7 0A        LDA [$0A],Y
    $C1/C5DD: 48           PHA
    $C1/C5DE: A0 08 00     LDY #$0008
    $C1/C5E1: B7 0A        LDA [$0A],Y
    $C1/C5E3: 29 FF 00     AND #$00FF
    $C1/C5E6: 85 0C        STA $0C
    $C1/C5E8: 68           PLA
    $C1/C5E9: 85 0A        STA $0A
    $C1/C5EB: 7A           PLY
    $C1/C5EC: B7 0A        LDA [$0A],Y         A = Event flag
    $C1/C5EE: F0 08        BEQ $C5F8           0x0000 flag case.
    $C1/C5F0: DA           PHX                 Normal flag case.
    $C1/C5F1: 5A           PHY
    $C1/C5F2: 22 28 16 C2  JSL $C21628
    $C1/C5F6: 7A           PLY
    $C1/C5F7: FA           PLX
    $C1/C5F8: E2 20        SEP #$20
    $C1/C5FA: 85 00        STA $00
    $C1/C5FC: 85 18        STA $18
    $C1/C5FE: 80 04        BRA $C604

------------------------------------------------------------------------

<Nitrodon> Michael1: remember to change the routine at C4/5ECE-5F7A (determines whether character A knows PSI X; used for Auto Fight)

    $C4/5EDA: C9 01 00     CMP #$0001
    $C4/5EDD: F0 0C        BEQ $5EEB
    $C4/5EDF: C9 02 00     CMP #$0002
    $C4/5EE2: F0 24        BEQ $5F08
    $C4/5EE4: C9 04 00     CMP #$0004
    $C4/5EE7: F0 3C        BEQ $5F25
    $C4/5EE9: 80 55        BRA $5F40

Replace it with:

    $C4/5EDA: 3A           DEC
    $C4/5EDB: C9 04 00     CMP #$0004
    $C4/5EDE: B0 60        BCS $5F40
    $C4/5EE0: 0A           ASL
    $C4/5EE1: 5A           PHY                 Backup Y
    $C4/5EE2: A8           TAY                 Y = One of 0x00, 0x02, 0x04, 0x06 (character number)
    $C4/5EE3: 8A           TXA
    $C4/5EE4: 85 04        STA $04
    $C4/5EE6: 0A           ASL
    $C4/5EE7: 0A           ASL
    $C4/5EE8: 0A           ASL
    $C4/5EE9: 0A           ASL
    $C4/5EEA: 38           SEC
    $C4/5EEB: E5 04        SBC $04
    $C4/5EED: 18           CLC
    $C4/5EEE: 69 06 00     ADC #$0006
    $C4/5EF1: AA           TAX
    $C4/5EF2: BF 50 8A D5  LDA $D58A50,X
    $C4/5EF6: 85 06        STA $06
    $C4/5EF8: BF 52 8A D5  LDA $D58A52,X
    $C4/5EFC: 85 08        STA $08
    $C4/5EFE: B7 06        LDA [$06],Y         A = Event flag
    $C4/5F00: F0 06        BEQ $5F08           0x0000 flag case.
    $C4/5F02: DA           PHX                 Normal flag case.
    $C4/5F03: 22 28 16 C2  JSL $C21628
    $C4/5F07: FA           PLX
    $C4/5F08: 7A           PLY                 Restore Y
    $C4/5F09: E2 20        SEP #$20
    $C4/5F0B: 85 00        STA $00
    $C4/5F0D: 85 10        STA $10
    $C4/5F0F: 80 2F        BRA $5F40
