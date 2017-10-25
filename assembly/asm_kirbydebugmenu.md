# KIRBY DEBUG MENU ASM

$B555 = Cursor position (0-6)
$B557 = Controller input
$B559 = After making a selection, which selection you've made (0-6)


    EF/E689: C2 31        REP #$31
    EF/E68B: A9 80 00     LDA #$0080
    EF/E68E: 8D 61 B5     STA $B561
    EF/E691: A9 70 00     LDA #$0070
    EF/E694: 8D 63 B5     STA $B563
    EF/E697: A9 94 00     LDA #$0094
    EF/E69A: 8D 65 B5     STA $B565
    EF/E69D: A9 FF FF     LDA #$FFFF
    EF/E6A0: 8D 54 9E     STA $9E54                 Max out the Dad timer (About 3 days)
    EF/E6A3: 22 7C 92 C0  JSR $C0927C
    EF/E6A7: 20 05 DA     JSR $DA05
    EF/E6AA: 20 21 DB     JSR $DB21                 Display the text for the menu options
    EF/E6AD: A2 01 00     LDX #$0001
    EF/E6B0: A9 04 00     LDA #$0004
    EF/E6B3: 22 6C 88 C0  JSR $C0886C

## Kirby menu loop

    EF/E6B7: 22 B1 88 C0  JSR $C088B1
    EF/E6BB: 20 78 E5     JSR $E578                  Handle cursor movement
    EF/E6BE: 20 D3 E5     JSR $E5D3                  Process command selections
    EF/E6C1: 22 66 94 C0  JSR $C09466
    EF/E6C5: 22 26 8B C0  JSR $C08B26
    EF/E6C9: 22 56 87 C0  JSR $C08756
    EF/E6CD: 80 E8        BRA $E6B7

## $EFE578 - Handle cursor movement

    EF/E578: C2 31        REP #$31
    EF/E57A: 0B           PHD
    EF/E57B: 7B           TDC
    EF/E57C: 69 F0 FF     ADC #$FFF0
    EF/E57F: 5B           TCD
    EF/E580: AD 69 00     LDA $0069
    EF/E583: 85 0E        STA $0E
    EF/E585: 29 00 08     AND #$0800
    EF/E588: F0 10        BEQ $E59A
    EF/E58A: AD 55 B5     LDA $B555
    EF/E58D: F0 05        BEQ $E594
    EF/E58F: CE 55 B5     DEC $B555
    EF/E592: 80 06        BRA $E59A
    EF/E594: A9 06 00     LDA #$0006
    EF/E597: 8D 55 B5     STA $B555
    EF/E59A: A5 0E        LDA $0E
    EF/E59C: 29 00 04     AND #$0400
    EF/E59F: F0 10        BEQ $E5B1
    EF/E5A1: AD 55 B5     LDA $B555
    EF/E5A4: C9 06 00     CMP #$0006
    EF/E5A7: F0 05        BEQ $E5AE
    EF/E5A9: EE 55 B5     INC $B555
    EF/E5AC: 80 03        BRA $E5B1
    EF/E5AE: 9C 55 B5     STZ $B555
    EF/E5B1: AD 53 B5     LDA $B553
    EF/E5B4: 0A           ASL
    EF/E5B5: AA           TAX
    EF/E5B6: AD 55 B5     LDA $B555
    EF/E5B9: 85 04        STA $04
    EF/E5BB: 0A           ASL
    EF/E5BC: 65 04        ADC $04
    EF/E5BE: 0A           ASL
    EF/E5BF: 0A           ASL
    EF/E5C0: 0A           ASL
    EF/E5C1: 18           CLC
    EF/E5C2: 69 34 00     ADC #$0034
    EF/E5C5: 9D CA 0B     STA $0BCA,X
    EF/E5C8: AD 6D 00     LDA $006D
    EF/E5CB: 29 A0 90     AND #$90A0
    EF/E5CE: 8D 57 B5     STA $B557
    EF/E5D1: 2B           PLD
    EF/E5D2: 60           RTS

## $EFE5D3 - Process command selections

    EF/E5D3: C2 31        REP #$31
    EF/E5D5: AD 57 B5     LDA $B557
    EF/E5D8: D0 03        BNE $E5DD
    EF/E5DA: 4C 88 E6     JMP $E688
    EF/E5DD: AD 55 B5     LDA $B555
    EF/E5E0: F0 20        BEQ $E602                 00: E602 (Game)
    EF/E5E2: C9 01 00     CMP #$0001
    EF/E5E5: F0 2E        BEQ $E615                 01: E615 (View Map)
    EF/E5E7: C9 02 00     CMP #$0002
    EF/E5EA: F0 3A        BEQ $E626                 02: E626 (View Character)
    EF/E5EC: C9 03 00     CMP #$0003
    EF/E5EF: F0 4C        BEQ $E63D                 03: E63D (View Attribute)
    EF/E5F1: C9 04 00     CMP #$0004
    EF/E5F4: F0 52        BEQ $E648                 04: E648 (Show Battle)
    EF/E5F6: C9 05 00     CMP #$0005
    EF/E5F9: F0 59        BEQ $E654                 05: E654 (Check Position)
    EF/E5FB: C9 06 00     CMP #$0006
    EF/E5FE: F0 5F        BEQ $E65F                 06: E65F (Sound Mode)
    EF/E600: 80 6A        BRA $E66C

## $EFE602 - Game

    EF/E602: A0 00 00     LDY #$0000
    EF/E605: A2 01 00     LDX #$0001
    EF/E608: A9 04 00     LDA #$0004
    EF/E60B: 22 14 88 C0  JSR $C08814
    EF/E60F: 22 D8 B7 C0  JSR $C0B7D8
    EF/E613: 80 57        BRA $E66C

## $EFE615 - View Map

    EF/E615: A9 01 00     LDA #$0001
    EF/E618: 8D 59 B5     STA $B559
    EF/E61B: A9 FF FF     LDA #$FFFF
    EF/E61E: 8D 58 4A     STA $4A58
    EF/E621: 20 75 E1     JSR $E175
    EF/E624: 80 46        BRA $E66C

## $EFE626 - View Character

    EF/E626: A9 02 00     LDA #$0002
    EF/E629: 8D 59 B5     STA $B559
    EF/E62C: A9 0A 00     LDA #$000A
    EF/E62F: 8D 5E 4A     STA $4A5E
    EF/E632: A9 FF FF     LDA #$FFFF
    EF/E635: 8D 5A 4A     STA $4A5A
    EF/E638: 20 75 E1     JSR $E175
    EF/E63B: 80 2F        BRA $E66C

## $EFE63D - View Attribute

    EF/E63D: A9 03 00     LDA #$0003
    EF/E640: 8D 59 B5     STA $B559
    EF/E643: 20 75 E1     JSR $E175
    EF/E646: 80 24        BRA $E66C

## $EFE648 - Show Battle

    EF/E648: A9 04 00     LDA #$0004
    EF/E64B: 8D 59 B5     STA $B559
    EF/E64E: 22 21 48 C2  JSR $C24821
    EF/E652: 80 18        BRA $E66C

## $EFE654 - Check Position

    EF/E654: A9 05 00     LDA #$0005
    EF/E657: 8D 59 B5     STA $B559
    EF/E65A: 20 75 E1     JSR $E175
    EF/E65D: 80 0D        BRA $E66C

## $EFE65F - Sound Mode

    EF/E65F: A9 06 00     LDA #$0006
    EF/E662: 8D 59 B5     STA $B559
    EF/E665: AD 53 B5     LDA $B553
    EF/E668: 22 D4 D6 EF  JSR $EFD6D4

## $EFE66C - Common code after the options

    EF/E66C: 22 2A EB EF  JSR $EFEB2A
    EF/E670: 9C 57 B5     STZ $B557
    EF/E673: 9C 59 B5     STZ $B559
    EF/E676: 22 7C 92 C0  JSR $C0927C
    EF/E67A: 20 05 DA     JSR $DA05
    EF/E67D: 20 21 DB     JSR $DB21
    EF/E680: A2 01 00     LDX #$0001
    EF/E683: 8A           TXA
    EF/E684: 22 6C 88 C0  JSR $C0886C
    EF/E688: 60           RTS
