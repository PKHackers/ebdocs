# Earthbound Music Program

This is the EarthBound Music Program, located at 0x600 in SPCs and $E60042 in the actual rom. More accurately, this is an interpretation of the program with SPC700 opcodes and comments.

$0030-3F holds some channelset stuff (the pointers).

    0500: 20          CLRP                          Clear direct page flag (DP = 0x0 to 0xFF)
    0501: CD CF       MOV      X, #$CF              X = 0xCF
    0503: BD          MOV      SP, X                Stack pointer = 0xCF
    0504: E8 00       MOV      A, #$00              A = 0x0
    0506: 5D          MOV      X, A                 X = 0x0
    0507: AF          MOV      (X)+, A              X += 1?
    0508: C8 E0       CMP      X, #$E0              Loop until X is 0xE0.
    050A: D0 FB       BNE      $0507
    050C: 3F A5 16    CALL     $16A5                Call a subroutine (16A5).
      ???
    050F: E8 55       MOV      A, #$55              A = 0x55
    0511: C4 18       MOV      $18, A               $18 = 0x55
    0513: C4 19       MOV      $19, A               $19 = 0x55
    0515: E8 00       MOV      A, #$00              A = 0x0
    0517: BC          INC      A                    A = 0x1
    0518: 3F 2C 0B    CALL     $0B2C                Call a subroutine (0B2C).
      ???
    051B: A2 48       SET1     $48, 5               Set bit 1 of $48?
    051D: E8 70       MOV      A, #$70              A = 0x70
    051F: 8D 0C       MOV      Y, #$0C              Y = 0xC
    0521: 3F 49 07    CALL     $0749                Call a subroutine (0749) - Set Main Volume (L) to 0x70.
      0749: CC F2 00    MOV      $00F2, Y
      074C: C5 F3 00    MOV      $00F3, A
      074F: 6F          RET
    0524: 8D 1C       MOV      Y, #$1C              Y = 0x1C
    0526: 3F 49 07    CALL     $0749                Call a subroutine (0749) - Set Main Volume (R) to 0x70.
      0749: CC F2 00    MOV      $00F2, Y
      074C: C5 F3 00    MOV      $00F3, A
      074F: 6F          RET
    0529: E8 6C       MOV      A, #$6C              A = 0x6C
    052B: 8D 5D       MOV      Y, #$5D              Y = 0x5D
    052D: 3F 49 07    CALL     $0749                Call a subroutine (0749).
      0749: CC F2 00    MOV      $00F2, Y           Set Offset Address of Source Directory to 0x6C. (?)
      074C: C5 F3 00    MOV      $00F3, A
      074F: 6F          RET
    0530: E8 F0       MOV      A, #$F0              A = 0xF0
    0532: C5 F1 00    MOV      $00F1, A             Write 0xF1 to the Control register (?)
    0535: E8 10       MOV      A, #$10              A = 0x10
    0537: C5 FA 00    MOV      $00FA, A             Set Timer0 to 0x10.
    053A: C4 53       MOV      $53, A               $53 = 0x10.
    053C: E8 01       MOV      A, #$01              A = 0x1
    053E: C5 F1 00    MOV      $00F1, A             Start Timer0. Every 0x10 time units, increment $00FD (4 bits).
    0541: D0 03       BNE      $0546                Unsure what this comparison is based on... the timer?
    0543: 5F D8 05    JMP      $05D8                --- ??? zero case: go to 05D8
    0546: E4 1B       MOV      A, $1B               --- ??? nonzero case
    0548: D0 F9       BNE      $0543                If $1B is nonzero, go to 05D8
    054A: 8D 0A       MOV      Y, #$0A              Y = 0xA
    054C: AD 05       CMP      Y, #$05
    054E: F0 07       BEQ      $0557
    0550: B0 08       BCS      $055A
    0552: 69 4D 4C    CMP      $4D, $4C             Compare $4D and $4C
    0555: D0 11       BNE      $0568                Branch to 0568 if not equal
    0557: E3 4C 0E    BBS      B7 $4C, $0568        Branch to 0568 if bit 7 is set in $4C (negative?).
    055A: F6 A7 0E    MOV      A, $0EA7+Y           A = value at 0EA7+Y
    055D: C5 F2 00    MOV      $00F2, A             Store to $00F2; prepare to modify "Key Off".
    0560: F6 B1 0E    MOV      A, $0EB1+Y           A = value at 0EB1+Y
    0563: 5D          MOV      X, A                 X = value at 0EB1+Y
    0564: E6          MOV      A, (X)               A = value at (value at 0EB1+Y)?
    0565: C5 F3 00    MOV      $00F3, A             Modify "Key Off".
    0568: FE E2       DBNZ     Y, $054C             Decrement Y; go to 054C if not 0.
    056A: CB 45       MOV      $45, Y               $45 = 0
    056C: CB 46       MOV      $46, Y               $46 = 0
    056E: E4 18       MOV      A, $18               A = 0x55
    0570: 44 19       EOR      A, $19               A = 0x55 XOR 0x55 = 0x0
    0572: 5C          LSR      A
    0573: 5C          LSR      A
    0574: ED          NOTC                          Inverse carry flag
    0575: 6B 18       ROR      $18                  $18 = 0x2A
    0577: 6B 19       ROR      $19                  $19 = 0x2A
    0579: EC FD 00    MOV      Y, $00FD
    057C: F0 FB       BEQ      $0579                Wait until $00FD (timer-affected) becomes nonzero
    057E: 6D          PUSH     Y                    Stack = Y
    057F: E8 20       MOV      A, #$20              A = 0x20
    0581: CF          MUL      YA                   (YA) = 0x20 * Y. Yes, "YA". The registers combine for 16-bitage.
    0582: 60          CLRC                          Clear carry.
    0583: 84 43       ADC      A, $43
    0585: C4 43       MOV      $43, A               $43 += A
    0587: 90 44       BCC      $05CD                Go to 05CD if carry is clear.
    0589: 5F 96 05    JMP      $0596                Else, go to 0596.
    058C: E5 B1 04    MOV      A, $04B1
    058F: 68 AA       CMP      A, #$AA
    0591: D0 03       BNE      $0596
    0593: 3F FC 05    CALL     $05FC
    0596: 3F 09 16    CALL     $1609
    0599: 3F AB 2D    CALL     $2DAB
    059C: 3F F3 2D    CALL     $2DF3
    059F: 3F D5 2D    CALL     $2DD5
    05A2: 3F DC 11    CALL     $11DC
    05A5: CD 03       MOV      X, #$03
    05A7: 3F 35 06    CALL     $0635
    05AA: C5 B3 04    MOV      $04B3, A
    05AD: 3F 1D 2E    CALL     $2E1D
    05B0: 3F BA 11    CALL     $11BA
    05B3: CD 02       MOV      X, #$02
    05B5: 3F 35 06    CALL     $0635
    05B8: C5 B2 04    MOV      $04B2, A
    05BB: 3F 98 11    CALL     $1198
    05BE: CD 01       MOV      X, #$01
    05C0: 3F 35 06    CALL     $0635
    05C3: C5 B1 04    MOV      $04B1, A
    05C6: 69 4D 4C    CMP      $4D, $4C
    05C9: F0 02       BEQ      $05CD
    05CB: AB 4C       INC      $4C
    05CD: E4 53       MOV      A, $53
    05CF: EE          POP      Y
    05D0: CF          MUL      YA
    05D1: 60          CLRC
    05D2: 84 51       ADC      A, $51
    05D4: C4 51       MOV      $51, A
    05D6: 90 08       BCC      $05E0
    05D8: 3F F9 07    CALL     $07F9
    05DB: 3F 25 06    CALL     $0625
    05DE: 2F 19       BRA      $05F9
    05E0: E4 04       MOV      A, $04
    05E2: F0 12       BEQ      $05F6
    05E4: CD 00       MOV      X, #$00
    05E6: 8F 01 47    MOV      $01, #$47
    05E9: F4 31       MOV      A, $31+X
    05EB: F0 03       BEQ      $05F0
    05ED: 3F D0 0D    CALL     $0DD0
    05F0: 3D          INC      X
    05F1: 3D          INC      X
    05F2: 0B 47       ASL      $47
    05F4: D0 F3       BNE      $05E9
    05F6: 5F 46 05    JMP      $0546
    05F9: 5F 46 05    JMP      $0546
    05FC: E8 00       MOV      A, #$00
    05FE: 8D 2C       MOV      Y, #$2C
    0600: 3F 49 07    CALL     $0749
    0603: 8D 3C       MOV      Y, #$3C
    0605: 3F 49 07    CALL     $0749
    0608: E8 FF       MOV      A, #$FF
    060A: 8D 5C       MOV      Y, #$5C
    060C: 3F 49 07    CALL     $0749
    060F: E8 00       MOV      A, #$00
    0611: 8D 4D       MOV      Y, #$4D
    0613: 3F 49 07    CALL     $0749
    0616: E8 20       MOV      A, #$20
    0618: 8D 6C       MOV      Y, #$6C
    061A: 3F 49 07    CALL     $0749
    061D: E8 80       MOV      A, #$80
    061F: C5 F1 00    MOV      $00F1, A
    0622: 5F C0 FF    JMP      $FFC0
    0625: E4 04       MOV      A, $04
    0627: C5 F4 00    MOV      $00F4, A
    062A: E5 F4 00    MOV      A, $00F4
    062D: 65 F4 00    CMP      A, $00F4
    0630: D0 F8       BNE      $062A
    0632: C4 00       MOV      $00, A
    0634: 6F          RET
    0635: F5 A4 04    MOV      A, $04A4+X
    0638: C4 20       MOV      $20, A
    063A: F5 F4 00    MOV      A, $00F4+X
    063D: 75 F4 00    CMP      A, $00F4+X
    0640: D0 F8       BNE      $063A
    0642: D4 00       MOV      $00+X, A
    0644: D5 F4 00    MOV      $00F4+X, A
    0647: D5 A4 04    MOV      $04A4+X, A
    064A: 64 20       CMP      A, $20
    064C: D0 02       BNE      $0650
    064E: E8 00       MOV      A, #$00
    0650: D5 A0 04    MOV      $04A0+X, A
    0653: 6F          RET
    0654: AD CA       CMP      Y, #$CA              Note (well, 80-DF) parsing! :D
    0656: 90 05       BCC      $065D                065D: 80-C9 cases
    0658: 3F 5F 09    CALL     $095F                095F: CA-DF cases
    065B: 8D A4       MOV      Y, #$A4
    065D: AD C8       CMP      Y, #$C8
    065F: B0 F2       BCS      $0653                End if C8 or C9
    0661: E4 1A       MOV      A, $1A               80-C7 cases from here onward...
    0663: 24 47       AND      A, $47
    0665: D0 EC       BNE      $0653                End if an unknown condition is fulfilled
    0667: DD          MOV      A, Y                 A = Y = note
    0668: 28 7F       AND      A, #$7F              Remove the unneeded 0x80. (00-47, notes)
    066A: 60          CLRC
    066B: 84 50       ADC      A, $50
    066D: 60          CLRC
    066E: 95 F0 02    ADC      A, $02F0+X           Add unknown quantities ($0050, $02F0+X)...
    0671: D5 61 03    MOV      $0361+X, A           Modified note to $0361+X.
    0674: F5 81 03    MOV      A, $0381+X
    0677: D5 60 03    MOV      $0360+X, A           Unknown ($0381+X) to $0360+X
    067A: F5 B1 02    MOV      A, $02B1+X
    067D: 5C          LSR      A
    067E: E8 00       MOV      A, #$00
    0680: 7C          ROR      A
    0681: D5 A0 02    MOV      $02A0+X, A           Those last few lines were weird... O_o
    0684: E8 00       MOV      A, #$00
    0686: D4 B0       MOV      $B0+X, A             $B0+X = 0x0
    0688: D5 00 01    MOV      $0100+X, A           $100+X = 0x0
    068B: D5 D0 02    MOV      $02D0+X, A           $2D0+X = 0x0
    068E: D4 C0       MOV      $C0+X, A             $C0+X = 0x0
    0690: 09 47 5E    OR       $47, $5E
    0693: 09 47 45    OR       $47, $45             Current channel bit set for $005E and $0045
    0696: F5 80 02    MOV      A, $0280+X
    0699: D4 A0       MOV      $A0+X, A             $A0+X = $280+X
    069B: F0 1E       BEQ      $06BB                If some condition is not equal, enter the unknown segment.
    069D: F5 81 02    MOV      A, $0281+X           --- Unknown segment start ---
    06A0: D4 A1       MOV      $A1+X, A
    06A2: F5 90 02    MOV      A, $0290+X
    06A5: D0 0A       BNE      $06B1
    06A7: F5 61 03    MOV      A, $0361+X
    06AA: 80          SETC
    06AB: B5 91 02    SBC      A, $0291+X
    06AE: D5 61 03    MOV      $0361+X, A
    06B1: F5 91 02    MOV      A, $0291+X
    06B4: 60          CLRC
    06B5: 95 61 03    ADC      A, $0361+X
    06B8: 3F A4 0B    CALL     $0BA4                --- Unknown segment end ---
    06BB: 3F BC 0B    CALL     $0BBC
    06BE: 8D 00       MOV      Y, #$00
    06C0: E4 11       MOV      A, $11
    06C2: 80          SETC
    06C3: A8 34       SBC      A, #$34
    06C5: B0 09       BCS      $06D0
    06C7: E4 11       MOV      A, $11
    06C9: 80          SETC
    06CA: A8 13       SBC      A, #$13
    06CC: B0 06       BCS      $06D4
    06CE: DC          DEC      Y
    06CF: 1C          ASL      A
    06D0: 7A 10       ADDW     YA, $10
    06D2: DA 10       MOVW     $10, YA
    06D4: 4D          PUSH     X
    06D5: E4 11       MOV      A, $11
    06D7: 1C          ASL      A
    06D8: 8D 00       MOV      Y, #$00
    06DA: CD 18       MOV      X, #$18
    06DC: 9E          DIV      YA, X
    06DD: 5D          MOV      X, A
    06DE: F6 BD 0E    MOV      A, $0EBD+Y
    06E1: C4 15       MOV      $15, A
    06E3: F6 BC 0E    MOV      A, $0EBC+Y
    06E6: C4 14       MOV      $14, A
    06E8: F6 BF 0E    MOV      A, $0EBF+Y
    06EB: 2D          PUSH     A
    06EC: F6 BE 0E    MOV      A, $0EBE+Y
    06EF: EE          POP      Y
    06F0: 9A 14       SUBW     YA, $14
    06F2: EB 10       MOV      Y, $10
    06F4: CF          MUL      YA
    06F5: DD          MOV      A, Y
    06F6: 8D 00       MOV      Y, #$00
    06F8: 7A 14       ADDW     YA, $14
    06FA: CB 15       MOV      $15, Y
    06FC: 1C          ASL      A
    06FD: 2B 15       ROL      $15
    06FF: C4 14       MOV      $14, A
    0701: 2F 04       BRA      $0707
    0703: 4B 15       LSR      $15
    0705: 7C          ROR      A
    0706: 3D          INC      X
    0707: C8 06       MOV      X, #$06
    0709: D0 F8       BNE      $0703
    070B: C4 14       MOV      $14, A
    070D: CE          POP      X
    070E: F5 20 02    MOV      A, $0220+X
    0711: EB 15       MOV      Y, $15
    0713: CF          MUL      YA
    0714: DA 16       MOVW     $16, YA
    0716: F5 20 02    MOV      A, $0220+X
    0719: EB 14       MOV      Y, $14
    071B: CF          MUL      YA
    071C: 6D          PUSH     Y
    071D: F5 21 02    MOV      A, $0221+X
    0720: EB 14       MOV      Y, $14
    0722: CF          MUL      YA
    0723: 7A 16       ADDW     YA, $16
    0725: DA 16       MOVW     $16, YA
    0727: F5 21 02    MOV      A, $0221+X
    072A: EB 15       MOV      Y, $15
    072C: CF          MUL      YA
    072D: FD          MOV      Y, A
    072E: AE          POP      A
    072F: 7A 16       ADDW     YA, $16
    0731: DA 16       MOVW     $16, YA
    0733: 7D          MOV      A, X
    0734: 9F          XCN      A
    0735: 5C          LSR      A
    0736: 08 02       OR       A, #$02
    0738: FD          MOV      Y, A
    0739: E4 16       MOV      A, $16
    073B: 3F 41 07    CALL     $0741
    073E: FC          INC      Y
    073F: E4 17       MOV      A, $17
    0741: 2D          PUSH     A
    0742: E4 47       MOV      A, $47
    0744: 24 1A       AND      A, $1A
    0746: AE          POP      A
    0747: D0 06       BNE      $074F
    0749: CC F2 00    MOV      $00F2, Y
    074C: C5 F3 00    MOV      $00F3, A
    074F: 6F          RET
    0750: 8D 00       MOV      Y, #$00
    0752: F7 40       MOV      A, [$40]+Y
    0754: 3A 40       INCW     $40
    0756: 2D          PUSH     A
    0757: F7 40       MOV      A, [$40]+Y
    0759: 3A 40       INCW     $40
    075B: FD          MOV      Y, A
    075C: AE          POP      A
    075D: 6F          RET
    075E: E8 FF       MOV      A, #$FF
    0760: 8D 5C       MOV      Y, #$5C
    0762: 3F 49 07    CALL     $0749
    0765: 3F 1A 14    CALL     $141A
    0768: 3F 53 14    CALL     $1453
    076B: 3F E1 0E    CALL     $0EE1
    076E: E8 00       MOV      A, #$00
    0770: C4 08       MOV      $08, A
    0772: C4 04       MOV      $04, A
    0774: 60          CLRC
    0775: 88 00       ADC      A, #$00              Recalculate the carry flag... clever.
    0777: 30 21       BMI      $079A
    0779: 1C          ASL      A
    077A: 5D          MOV      X, A
    077B: F5 49 2E    MOV      A, $2E49+X
    077E: FD          MOV      Y, A
    077F: F5 48 2E    MOV      A, $2E48+X
    0782: DA 40       MOVW     $40, YA
    0784: 8F 02 0C    MOV      $02, #$0C
    0787: E8 00       MOV      A, #$00
    0789: C5 91 04    MOV      $0491, A
    078C: C5 B1 04    MOV      $04B1, A
    078F: C5 B5 04    MOV      $04B5, A
    0792: E4 1A       MOV      A, $1A
    0794: 48 FF       EOR      A, #$FF
    0796: 0E 46 00    TSET1    $0046
    0799: 6F          RET
    079A: 28 7F       AND      A, #$7F
    079C: BC          INC      A
    079D: 1C          ASL      A
    079E: 5D          MOV      X, A
    079F: F5 47 2F    MOV      A, $2F47+X
    07A2: FD          MOV      Y, A
    07A3: F5 46 2F    MOV      A, $2F46+X
    07A6: 5F 82 07    JMP      $0782
    07A9: CD 0E       MOV      X, #$0E
    07AB: 8F 80 47    MOV      $80, #$47
    07AE: E8 FF       MOV      A, #$FF
    07B0: D5 01 03    MOV      $0301+X, A
    07B3: E8 0A       MOV      A, #$0A
    07B5: 3F B8 09    CALL     $09B8
    07B8: D5 11 02    MOV      $0211+X, A
    07BB: D5 81 03    MOV      $0381+X, A
    07BE: D5 F0 02    MOV      $02F0+X, A
    07C1: D5 80 02    MOV      $0280+X, A
    07C4: D5 00 04    MOV      $0400+X, A
    07C7: D4 B1       MOV      $B1+X, A
    07C9: D4 C1       MOV      $C1+X, A
    07CB: 1D          DEC      X
    07CC: 1D          DEC      X
    07CD: 4B 47       LSR      $47
    07CF: D0 DD       BNE      $07AE
    07D1: C4 5A       MOV      $5A, A
    07D3: C4 68       MOV      $68, A
    07D5: C4 54       MOV      $54, A
    07D7: C4 50       MOV      $50, A
    07D9: C4 42       MOV      $42, A
    07DB: C4 5F       MOV      $5F, A
    07DD: 8F C0 59    MOV      $C0, #$59
    07E0: 8F 20 53    MOV      $20, #$53
    07E3: 6F          RET
    07E4: 5F 5E 07    JMP      $075E
    07E7: 5F 72 07    JMP      $0772
    07EA: E8 00       MOV      A, #$00
    07EC: C5 F4 00    MOV      $00F4, A
    07EF: E8 00       MOV      A, #$00
    07F1: 5F 72 07    JMP      $0772
    07F4: E4 08       MOV      A, $08
    07F6: 30 07       BMI      $07FF
    07F8: 6F          RET
    07F9: EB 08       MOV      Y, $08
    07FB: E4 00       MOV      A, $00
    07FD: C4 08       MOV      $08, A
    07FF: 68 F0       CMP      A, #$F0
    0801: F0 84       BEQ      $0787
    0803: 68 F1       CMP      A, #$F1
    0805: F0 08       BEQ      $080F
    0807: 68 FF       CMP      A, #$FF
    0809: F0 D9       BEQ      $07E4
    080B: 7E 00       CMP      Y, $00
    080D: D0 D8       BNE      $07E7
    080F: E4 04       MOV      A, $04
    0811: F0 D0       BEQ      $07E3
    0813: E4 0C       MOV      A, $0C
    0815: F0 5A       BEQ      $0871
    0817: 6E 0C 8F    DBNZ     $0C, $07A9
    081A: 3F 50 07    CALL     $0750                $0750 does stuff seemingly related to locations with
    081D: D0 22       BNE      $0841                 music data. Parsing, perhaps? A, then Y.
    081F: FD          MOV      Y, A
    0820: F0 C8       BEQ      $07EA
    0822: 68 80       CMP      A, #$80
    0824: F0 06       BEQ      $082C
    0826: 68 81       CMP      A, #$81
    0828: D0 06       BNE      $0830
    082A: E8 00       MOV      A, #$00
    082C: C4 1B       MOV      $1B, A
    082E: 2F EA       BRA      $081A
    0830: 8B 42       DEC      $42
    0832: 10 02       BPL      $0836
    0834: C4 42       MOV      $42, A
    0836: 3F 50 07    CALL     $0750
    0839: F8 42       MOV      X, $42
    083B: F0 DD       BEQ      $081A
    083D: DA 40       MOVW     $40, YA
    083F: 2F D9       BRA      $081A
    0841: DA 16       MOVW     $16, YA
    0843: 8D 0F       MOV      Y, #$0F
    0845: F7 16       MOV      A, [$16]+Y           Loop to move 0x10 bytes... I'm thinking a channel set.
    0847: D6 30 00    MOV      $0030+Y, A           UPDATE: it gets a channelset and drops it in $0030-F.
    084A: DC          DEC      Y
    084B: 10 F8       BPL      $0845
    084D: CD 00       MOV      X, #$00
    084F: 8F 01 47    MOV      $01, #$47            Spctool reports this the other way - 0x1 to $0047. Seems more likely.
    0852: F4 31       MOV      A, $31+X             Get high order of a channel pointer. If 0x0, skip onwards (null case)
    0854: F0 0A       BEQ      $0860
    0856: F5 11 02    MOV      A, $0211+X           Get... something unknown. If NOT zero, jump onward.
    0859: D0 05       BNE      $0860
    085B: E8 00       MOV      A, #$00
    085D: 3F 5F 09    CALL     $095F
    0860: E8 00       MOV      A, #$00
    0862: D4 80       MOV      $80+X, A
    0864: D4 90       MOV      $90+X, A
    0866: D4 91       MOV      $91+X, A
    0868: BC          INC      A
    0869: D4 70       MOV      $70+X, A
    086B: 3D          INC      X
    086C: 3D          INC      X
    086D: 0B 47       ASL      $47                  $0047 log base2 = current channel.
    086F: D0 E1       BNE      $0852
    0871: CD 00       MOV      X, #$00              And off we go, from the land of channelset setup.
    0873: D8 5E       MOV      $5E, X
    0875: 8F 01 47    MOV      $01, #$47            $0047 = 0x1 (No, not an error)
    0878: D8 44       MOV      $44, X
    087A: F4 31       MOV      A, $31+X
    087C: F0 72       BEQ      $08F0                If you're working with a pointer-in-channelset of 0000, go to 08F0.
    087E: 9B 70       DEC      $70+X                Duration.
    0880: D0 64       BNE      $08E6                If already crunching on a note of any duration, skip this.

    ; Space added for importance. This looks like the start of the parser. The REAL parser.

    0882: 3F 55 09    CALL     $0955                Increment the pointer. A and Y hold the value at the pre-inc address.
    0885: D0 17       BNE      $089E
    0887: F4 80       MOV      A, $80+X             --- 00 CASE: start ---
    0889: F0 8F       BEQ      $081A
    088B: 3F C4 0A    CALL     $0AC4
    088E: 9B 80       DEC      $80+X
    0890: D0 F0       BNE      $0882
    0892: F5 30 02    MOV      A, $0230+X
    0895: D4 30       MOV      $30+X, A
    0897: F5 31 02    MOV      A, $0231+X
    089A: D4 31       MOV      $31+X, A
    089C: 2F E4       BRA      $0882                --- 00 CASE: end ---
    089E: 30 20       BMI      $08C0
    08A0: D5 00 02    MOV      $0200+X, A           --- 01-7F CASE: start ---
    08A3: 3F 55 09    CALL     $0955                Codes in this range may have args... if the args are
    08A6: 30 18       BMI      $08C0                 greater than 0x80, go to "80-FF CASE: start".
    08A8: 2D          PUSH     A                    Otherwise, push the arg.
    08A9: 9F          XCN      A                    Swap bits 0-3 and 4-7. (0xXY -> 0xYX)
    08AA: 28 07       AND      A, #$07              Preserve the arg's original bits 4-6.
    08AC: FD          MOV      Y, A
    08AD: F6 80 6F    MOV      A, $6F80+Y           A = 6F80 + (arg's original bits 4-6); 6F80-7 is a table
    08B0: D5 01 02    MOV      $0201+X, A           0201+X = A.
    08B3: AE          POP      A
    08B4: 28 0F       AND      A, #$0F              Preserve the arg's original bits 0-3.
    08B6: FD          MOV      Y, A
    08B7: F6 88 6F    MOV      A, $6F88+Y           A = 6F88 + (arg's original bits 0-3); 6F88-97 is a table
    08BA: D5 10 02    MOV      $0210+X, A           0210+X = A
    08BD: 3F 55 09    CALL     $0955                --- 01-7F CASE: end ---
    08C0: 68 E0       CMP      A, #$E0              --- 80-FF CASE: start ---
    08C2: 90 05       BCC      $08C9
    08C4: 3F 43 09    CALL     $0943                --- E0-FF CASE: Go to 0943 ---
    08C7: 2F B9       BRA      $0882
    08C9: F5 00 04    MOV      A, $0400+X           --- 80-DF CASE ---
    08CC: 04 1B       OR       A, $1B
    08CE: D0 04       BNE      $08D4                No idea what this is... maybe skips $0654 for non-notes?
    08D0: DD          MOV      A, Y                 A = Y = current byte

    ; 0654 handles note parsing. The code is scary, though...

    08D1: 3F 54 06    CALL     $0654
    08D4: F5 00 02    MOV      A, $0200+X
    08D7: D4 70       MOV      $70+X, A             $70+X = $200+X = Duration
    08D9: FD          MOV      Y, A
    08DA: F5 01 02    MOV      A, $0201+X
    08DD: CF          MUL      YA                   YA = Duration * Style
    08DE: DD          MOV      A, Y                 A = Y = Duration * Style / 0x100
    08DF: D0 01       BNE      $08E2
    08E1: BC          INC      A                    If 0, add one
    08E2: D4 71       MOV      $71+X, A             And store to $0071+X
    08E4: 2F 07       BRA      $08ED
    08E6: E4 1B       MOV      A, $1B
    08E8: D0 06       BNE      $08F0
    08EA: 3F F7 0C    CALL     $0CF7
    08ED: 3F 84 0B    CALL     $0B84
    08F0: 3D          INC      X
    08F1: 3D          INC      X
    08F2: 0B 47       ASL      $47
    08F4: D0 82       BNE      $0878
    08F6: E4 54       MOV      A, $54
    08F8: F0 0B       BEQ      $0905
    08FA: BA 56       MOVW     YA, $56
    08FC: 7A 52       ADDW     YA, $52
    08FE: 6E 54 02    DBNZ     $54, $0903
    0901: BA 54       MOVW     YA, $54
    0903: DA 52       MOVW     $52, YA
    0905: E4 68       MOV      A, $68
    0907: F0 15       BEQ      $091E
    0909: BA 64       MOVW     YA, $64
    090B: 7A 60       ADDW     YA, $60
    090D: DA 60       MOVW     $60, YA
    090F: BA 66       MOVW     YA, $66
    0911: 7A 62       ADDW     YA, $62
    0913: 6E 68 06    DBNZ     $68, $091C
    0916: BA 68       MOVW     YA, $68
    0918: DA 60       MOVW     $60, YA
    091A: EB 6A       MOV      Y, $6A
    091C: DA 62       MOVW     $62, YA
    091E: E4 5A       MOV      A, $5A
    0920: F0 0E       BEQ      $0930
    0922: BA 5C       MOVW     YA, $5C
    0924: 7A 58       ADDW     YA, $58
    0926: 6E 5A 02    DBNZ     $5A, $092B
    0929: BA 5A       MOVW     YA, $5A
    092B: DA 58       MOVW     $58, YA
    092D: 8F FF 5E    MOV      $FF, #$5E
    0930: CD 00       MOV      X, #$00
    0932: 8F 01 47    MOV      $01, #$47
    0935: F4 31       MOV      A, $31+X
    0937: F0 03       BEQ      $093C
    0939: 3F 40 0C    CALL     $0C40
    093C: 3D          INC      X
    093D: 3D          INC      X
    093E: 0B 47       ASL      $47
    0940: D0 F3       BNE      $0935
    0942: 6F          RET
    0943: 1C          ASL      A                    --- E0-FF CASE ---
    0944: FD          MOV      Y, A
    0945: F6 24 0B    MOV      A, $0B24+Y
    0948: 2D          PUSH     A
    0949: F6 23 0B    MOV      A, $0B23+Y
    094C: 2D          PUSH     A
    094D: DD          MOV      A, Y
    094E: 5C          LSR      A
    094F: FD          MOV      Y, A
    0950: F6 C1 0B    MOV      A, $0BC1+Y
    0953: F0 08       BEQ      $095D
    0955: E7 30       MOV      A, [$30+X]           --- Start of "Increment a channel pointer." ---
    0957: BB 30       INC      $30+X                 This returns with the value at the pre-increment
    0959: D0 02       BNE      $095D                 pointer in both A AND Y.
    095B: BB 31       INC      $31+X
    095D: FD          MOV      Y, A
    095E: 6F          RET                           --- End of "Increment a channel pointer." ---
    095F: D5 11 02    MOV      $0211+X, A
    0962: FD          MOV      Y, A
    0963: 10 06       BPL      $096B
    0965: 80          SETC
    0966: A8 CA       SBC      A, #$CA
    0968: 60          CLRC
    0969: 84 5F       ADC      A, $5F
    096B: 8D 06       MOV      Y, #$06
    096D: CF          MUL      YA
    096E: DA 14       MOVW     $14, YA
    0970: 60          CLRC
    0971: 98 00 14    ADC      $00, #$14
    0974: 98 6E 15    ADC      $6E, #$15            This relates to 6E00 + n. Which has values that are eventually
    0977: E4 1A       MOV      A, $1A                dumped to $00F2/F3...
    0979: 24 47       AND      A, $47
    097B: D0 3A       BNE      $09B7
    097D: 4D          PUSH     X
    097E: 7D          MOV      A, X
    097F: 9F          XCN      A
    0980: 5C          LSR      A
    0981: 08 04       OR       A, #$04
    0983: 5D          MOV      X, A
    0984: 8D 00       MOV      Y, #$00
    0986: F7 14       MOV      A, [$14]+Y
    0988: 10 0E       BPL      $0998
    098A: 28 1F       AND      A, #$1F
    098C: 38 20 48    AND      $20, #$48
    098F: 0E 48 00    TSET1    $0048
    0992: 09 47 49    OR       $47, $49
    0995: DD          MOV      A, Y
    0996: 2F 07       BRA      $099F
    0998: E4 47       MOV      A, $47
    099A: 4E 49 00    TCLR1    $0049
    099D: F7 14       MOV      A, [$14]+Y
    099F: C9 F2 00    MOV      $00F2, X             If you null this opcode, and the next, you'll get no sound.
    09A2: C5 F3 00    MOV      $00F3, A             Update: this messes with SRCN, ADSR1+2, and GAIN for channels.
    09A5: 3D          INC      X                     It uses values from 6E00 + n for this task. Then it dumps a
    09A6: FC          INC      Y                     fifth value, and sixth, to 0221 and 0220...
    09A7: AD 04       CMP      Y, #$04
    09A9: D0 F2       BNE      $099D
    09AB: CE          POP      X
    09AC: F7 14       MOV      A, [$14]+Y
    09AE: D5 21 02    MOV      $0221+X, A
    09B1: FC          INC      Y
    09B2: F7 14       MOV      A, [$14]+Y
    09B4: D5 20 02    MOV      $0220+X, A
    09B7: 6F          RET
    09B8: C4 20       MOV      $20, A
    09BA: E5 31 04    MOV      A, $0431
    09BD: 68 00       CMP      A, #$00
    09BF: F0 05       BEQ      $09C6
    09C1: E8 0A       MOV      A, #$0A
    09C3: 5F C8 09    JMP      $09C8
    09C6: E4 20       MOV      A, $20
    09C8: D5 51 03    MOV      $0351+X, A
    09CB: 28 1F       AND      A, #$1F
    09CD: D5 31 03    MOV      $0331+X, A
    09D0: E8 00       MOV      A, #$00
    09D2: D5 30 03    MOV      $0330+X, A
    09D5: 6F          RET
    09D6: D4 91       MOV      $91+X, A
    09D8: 2D          PUSH     A
    09D9: 3F 55 09    CALL     $0955
    09DC: C4 20       MOV      $20, A
    09DE: E5 31 04    MOV      A, $0431
    09E1: 68 00       CMP      A, #$00
    09E3: F0 03       BEQ      $09E8
    09E5: 8F 0A 20    MOV      $0A, #$20
    09E8: E4 20       MOV      A, $20
    09EA: D5 50 03    MOV      $0350+X, A
    09ED: 80          SETC
    09EE: B5 31 03    SBC      A, $0331+X
    09F1: CE          POP      X
    09F2: 3F C7 0B    CALL     $0BC7
    09F5: D5 40 03    MOV      $0340+X, A
    09F8: DD          MOV      A, Y
    09F9: D5 41 03    MOV      $0341+X, A
    09FC: 6F          RET
    09FD: D5 B0 02    MOV      $02B0+X, A
    0A00: 3F 55 09    CALL     $0955
    0A03: D5 A1 02    MOV      $02A1+X, A
    0A06: 3F 55 09    CALL     $0955
    0A09: D4 B1       MOV      $B1+X, A
    0A0B: D5 C1 02    MOV      $02C1+X, A
    0A0E: E8 00       MOV      A, #$00
    0A10: D5 B1 02    MOV      $02B1+X, A
    0A13: 6F          RET
    0A14: D5 B1 02    MOV      $02B1+X, A
    0A17: 2D          PUSH     A
    0A18: 8D 00       MOV      Y, #$00
    0A1A: F4 B1       MOV      A, $B1+X
    0A1C: CE          POP      X
    0A1D: 9E          DIV      YA, X
    0A1E: F8 44       MOV      X, $44
    0A20: D5 C0 02    MOV      $02C0+X, A
    0A23: 6F          RET
    0A24: E8 00       MOV      A, #$00
    0A26: DA 58       MOVW     $58, YA
    0A28: 6F          RET
    0A29: C4 5A       MOV      $5A, A
    0A2B: 3F 55 09    CALL     $0955
    0A2E: C4 5B       MOV      $5B, A
    0A30: 80          SETC
    0A31: A4 59       SBC      A, $59
    0A33: F8 5A       MOV      X, $5A
    0A35: 3F C7 0B    CALL     $0BC7
    0A38: DA 5C       MOVW     $5C, YA
    0A3A: 6F          RET
    0A3B: E8 00       MOV      A, #$00
    0A3D: DA 52       MOVW     $52, YA
    0A3F: 6F          RET
    0A40: C4 54       MOV      $54, A
    0A42: 3F 55 09    CALL     $0955
    0A45: C4 55       MOV      $55, A
    0A47: 80          SETC
    0A48: A4 53       SBC      A, $53
    0A4A: F8 54       MOV      X, $54
    0A4C: 3F C7 0B    CALL     $0BC7
    0A4F: DA 56       MOVW     $56, YA
    0A51: 6F          RET
    0A52: C4 50       MOV      $50, A
    0A54: 6F          RET
    0A55: D5 F0 02    MOV      $02F0+X, A
    0A58: 6F          RET
    0A59: D5 E0 02    MOV      $02E0+X, A
    0A5C: 3F 55 09    CALL     $0955
    0A5F: D5 D1 02    MOV      $02D1+X, A
    0A62: 3F 55 09    CALL     $0955
    0A65: D4 C1       MOV      $C1+X, A
    0A67: 6F          RET
    0A68: E8 01       MOV      A, #$01
    0A6A: 2F 02       BRA      $0A6E
    0A6C: E8 00       MOV      A, #$00
    0A6E: D5 90 02    MOV      $0290+X, A
    0A71: DD          MOV      A, Y
    0A72: D5 81 02    MOV      $0281+X, A
    0A75: 3F 55 09    CALL     $0955
    0A78: D5 80 02    MOV      $0280+X, A
    0A7B: 3F 55 09    CALL     $0955
    0A7E: D5 91 02    MOV      $0291+X, A
    0A81: 6F          RET
    0A82: D5 80 02    MOV      $0280+X, A
    0A85: 6F          RET
    0A86: D5 01 03    MOV      $0301+X, A
    0A89: E8 00       MOV      A, #$00
    0A8B: D5 00 03    MOV      $0300+X, A
    0A8E: 6F          RET
    0A8F: D4 90       MOV      $90+X, A
    0A91: 2D          PUSH     A
    0A92: 3F 55 09    CALL     $0955
    0A95: D5 20 03    MOV      $0320+X, A
    0A98: 80          SETC
    0A99: B5 01 03    SBC      A, $0301+X
    0A9C: CE          POP      X
    0A9D: 3F C7 0B    CALL     $0BC7
    0AA0: D5 10 03    MOV      $0310+X, A
    0AA3: DD          MOV      A, Y
    0AA4: D5 11 03    MOV      $0311+X, A
    0AA7: 6F          RET
    0AA8: D5 81 03    MOV      $0381+X, A
    0AAB: 6F          RET
    0AAC: D5 40 02    MOV      $0240+X, A
    0AAF: 3F 55 09    CALL     $0955
    0AB2: D5 41 02    MOV      $0241+X, A
    0AB5: 3F 55 09    CALL     $0955
    0AB8: D4 80       MOV      $80+X, A
    0ABA: F4 30       MOV      A, $30+X
    0ABC: D5 30 02    MOV      $0230+X, A
    0ABF: F4 31       MOV      A, $31+X
    0AC1: D5 31 02    MOV      $0231+X, A
    0AC4: F5 40 02    MOV      A, $0240+X
    0AC7: D4 30       MOV      $30+X, A
    0AC9: F5 41 02    MOV      A, $0241+X
    0ACC: D4 31       MOV      $31+X, A
    0ACE: 6F          RET
    0ACF: C4 4A       MOV      $4A, A
    0AD1: 3F 55 09    CALL     $0955
    0AD4: E8 00       MOV      A, #$00
    0AD6: DA 60       MOVW     $60, YA
    0AD8: 3F 55 09    CALL     $0955
    0ADB: E8 00       MOV      A, #$00
    0ADD: DA 62       MOVW     $62, YA
    0ADF: B2 48       CLR1     $48, 5
    0AE1: 6F          RET
    0AE2: C4 68       MOV      $68, A
    0AE4: 3F 55 09    CALL     $0955
    0AE7: C4 69       MOV      $69, A
    0AE9: 80          SETC
    0AEA: A4 61       SBC      A, $61
    0AEC: F8 68       MOV      X, $68
    0AEE: 3F C7 0B    CALL     $0BC7
    0AF1: DA 64       MOVW     $64, YA
    0AF3: 3F 55 09    CALL     $0955
    0AF6: C4 6A       MOV      $6A, A
    0AF8: 80          SETC
    0AF9: A4 63       SBC      A, $63
    0AFB: F8 68       MOV      X, $68
    0AFD: 3F C7 0B    CALL     $0BC7
    0B00: DA 66       MOVW     $66, YA
    0B02: 6F          RET
    0B03: DA 60       MOVW     $60, YA
    0B05: DA 62       MOVW     $62, YA
    0B07: A2 48       SET1     $48, 5
    0B09: 6F          RET
    0B0A: 3F 2C 0B    CALL     $0B2C
    0B0D: 3F 55 09    CALL     $0955
    0B10: C4 4E       MOV      $4E, A
    0B12: 3F 55 09    CALL     $0955
    0B15: 8D 08       MOV      Y, #$08
    0B17: CF          MUL      YA
    0B18: 5D          MOV      X, A
    0B19: 8D 0F       MOV      Y, #$0F
    0B1B: F5 88 0E    MOV      A, $0E88+X
    0B1E: 3F 49 07    CALL     $0749
    0B21: 3D          INC      X
    0B22: DD          MOV      A, Y
    0B23: 60          CLRC
    0B24: 88 10       ADC      A, #$10
    0B26: FD          MOV      Y, A
    0B27: 10 F2       BPL      $0B1B
    0B29: F8 44       MOV      X, $44
    0B2B: 6F          RET
    0B2C: C4 4D       MOV      $4D, A
    0B2E: 8D 7D       MOV      Y, #$7D
    0B30: CC F2 00    MOV      $00F2, Y
    0B33: E5 F3 00    MOV      A, $00F3
    0B36: 64 4D       CMP      A, $4D
    0B38: F0 2B       BEQ      $0B65
    0B3A: 28 0F       AND      A, #$0F
    0B3C: 48 FF       EOR      A, #$FF
    0B3E: F3 4C 03    BBC      B7 $4C, $0B44
    0B41: 60          CLRC
    0B42: 84 4C       ADC      A, $4C
    0B44: C4 4C       MOV      $4C, A
    0B46: 8D 04       MOV      Y, #$04
    0B48: F6 A7 0E    MOV      A, $0EA7+Y
    0B4B: C5 F2 00    MOV      $00F2, A
    0B4E: E8 00       MOV      A, #$00
    0B50: C5 F3 00    MOV      $00F3, A
    0B53: FE F3       DBNZ     Y, $0B48
    0B55: E4 48       MOV      A, $48
    0B57: 08 20       OR       A, #$20
    0B59: 8D 6C       MOV      Y, #$6C
    0B5B: 3F 49 07    CALL     $0749
    0B5E: E4 4D       MOV      A, $4D
    0B60: 8D 7D       MOV      Y, #$7D
    0B62: 3F 49 07    CALL     $0749
    0B65: 1C          ASL      A
    0B66: 1C          ASL      A
    0B67: 1C          ASL      A
    0B68: 48 FF       EOR      A, #$FF
    0B6A: 80          SETC
    0B6B: 88 FF       ADC      A, #$FF
    0B6D: 8D 6D       MOV      Y, #$6D
    0B6F: 5F 49 07    JMP      $0749
    0B72: C4 5F       MOV      $5F, A
    0B74: 6F          RET
    0B75: 3F 57 09    CALL     $0957
    0B78: 6F          RET
    0B79: BC          INC      A
    0B7A: D5 00 04    MOV      $0400+X, A
    0B7D: 6F          RET
    0B7E: BC          INC      A
    0B7F: C4 1B       MOV      $1B, A
    0B81: 5F 87 07    JMP      $0787
    0B84: F4 A0       MOV      A, $A0+X
    0B86: D0 33       BNE      $0BBB
    0B88: E7 30       MOV      A, [$30+X]
    0B8A: 68 F9       CMP      A, #$F9
    0B8C: D0 2D       BNE      $0BBB
    0B8E: 3F 57 09    CALL     $0957
    0B91: 3F 55 09    CALL     $0955
    0B94: D4 A1       MOV      $A1+X, A
    0B96: 3F 55 09    CALL     $0955
    0B99: D4 A0       MOV      $A0+X, A
    0B9B: 3F 55 09    CALL     $0955
    0B9E: 60          CLRC
    0B9F: 84 50       ADC      A, $50
    0BA1: 95 F0 02    ADC      A, $02F0+X
    0BA4: 28 7F       AND      A, #$7F
    0BA6: D5 80 03    MOV      $0380+X, A
    0BA9: 80          SETC
    0BAA: B5 61 03    SBC      A, $0361+X
    0BAD: FB A0       MOV      Y, $A0+X
    0BAF: 6D          PUSH     Y
    0BB0: CE          POP      X
    0BB1: 3F C7 0B    CALL     $0BC7
    0BB4: D5 70 03    MOV      $0370+X, A
    0BB7: DD          MOV      A, Y
    0BB8: D5 71 03    MOV      $0371+X, A
    0BBB: 6F          RET
    0BBC: F5 61 03    MOV      A, $0361+X
    0BBF: C4 11       MOV      $11, A
    0BC1: F5 60 03    MOV      A, $0360+X
    0BC4: C4 10       MOV      $10, A
    0BC6: 6F          RET
    0BC7: ED          NOTC
    0BC8: 6B 12       ROR      $12
    0BCA: 10 03       BPL      $0BCF
    0BCC: 48 FF       EOR      A, #$FF
    0BCE: BC          INC      A
    0BCF: 8D 00       MOV      Y, #$00
    0BD1: 9E          DIV      YA, X
    0BD2: 2D          PUSH     A
    0BD3: E8 00       MOV      A, #$00
    0BD5: 9E          DIV      YA, X
    0BD6: EE          POP      Y
    0BD7: F8 44       MOV      X, $44
    0BD9: F3 12 06    BBC      B7 $12, $0BE2
    0BDC: DA 14       MOVW     $14, YA
    0BDE: BA 0E       MOVW     YA, $0E
    0BE0: 9A 14       SUBW     YA, $14
    0BE2: 6F          RET
    0BE3: 5F 09 B8    JMP      $B809
    0BE6: 09 D6 09    OR       $D6, $09
    0BE9: FD          MOV      Y, A
    0BEA: 09 09 0A    OR       $09, $0A
    0BED: 24 0A       AND      A, $0A
    0BEF: 29 0A 3B    AND      $0A, $3B
    0BF2: 0A 40 0A    OR1      C, $0A40
    0BF5: 52 0A       CLR1     $0A, 2
    0BF7: 55 0A 59    EOR      A, $590A+X
    0BFA: 0A 65 0A    OR1      C, $0A65
    0BFD: 86          ADC      A, (X)
    0BFE: 0A 8F 0A    OR1      C, $0A8F
    0C01: AC 0A 14    INC      $140A
    0C04: 0A 68 0A    OR1      C, $0A68
    0C07: 6C 0A 82    ROR      $820A
    0C0A: 0A A8 0A    OR1      C, $0AA8
    0C0D: CF          MUL      YA
    0C0E: 0A 03 0B    OR1      C, $0B03
    0C11: 0A 0B E2    OR1      C, $E20B
    0C14: 0A 94 0B    OR1      C, $0B94
    0C17: 72 0B       CLR1     $0B, 3
    0C19: 75 0B 79    CMP      A, $790B+X
    0C1C: 0B 7E       ASL      $7E
    0C1E: 0B 7F       ASL      $7F
    0C20: 0B 01       ASL      $01
    0C22: 01          TCALL    0
    0C23: 02 03       SET1     $03, 0
    0C25: 00          NOP
    0C26: 01          TCALL    0
    0C27: 02 01       SET1     $01, 0
    0C29: 02 01       SET1     $01, 0
    0C2B: 01          TCALL    0
    0C2C: 03 00 01    BBS      B0 $00, $0C30
    0C2F: 02 03       SET1     $03, 0
    0C31: 01          TCALL    0
    0C32: 03 03 00    BBS      B0 $03, $0C35
    0C35: 01          TCALL    0
    0C36: 03 00 03    BBS      B0 $00, $0C3C
    0C39: 03 03 01    BBS      B0 $03, $0C3D
    0C3C: 02 00       SET1     $00, 0
    0C3E: 00          NOP
    0C3F: 00          NOP
    0C40: F4 90       MOV      A, $90+X
    0C42: F0 09       BEQ      $0C4D
    0C44: E8 00       MOV      A, #$00
    0C46: 8D 03       MOV      Y, #$03
    0C48: 9B 90       DEC      $90+X
    0C4A: 3F D3 0C    CALL     $0CD3
    0C4D: FB C1       MOV      Y, $C1+X
    0C4F: F0 23       BEQ      $0C74
    0C51: F5 E0 02    MOV      A, $02E0+X
    0C54: DE C0 1B    CBNE     $C0+X, $0C72
    0C57: 09 47 5E    OR       $47, $5E
    0C5A: F5 D0 02    MOV      A, $02D0+X
    0C5D: 10 07       BPL      $0C66
    0C5F: FC          INC      Y
    0C60: D0 04       BNE      $0C66
    0C62: E8 80       MOV      A, #$80
    0C64: 2F 04       BRA      $0C6A
    0C66: 60          CLRC
    0C67: 95 D1 02    ADC      A, $02D1+X
    0C6A: D5 D0 02    MOV      $02D0+X, A
    0C6D: 3F 56 0E    CALL     $0E56
    0C70: 2F 07       BRA      $0C79
    0C72: BB C0       INC      $C0+X
    0C74: E8 FF       MOV      A, #$FF
    0C76: 3F 61 0E    CALL     $0E61
    0C79: F4 91       MOV      A, $91+X
    0C7B: F0 09       BEQ      $0C86
    0C7D: E8 30       MOV      A, #$30
    0C7F: 8D 03       MOV      Y, #$03
    0C81: 9B 91       DEC      $91+X
    0C83: 3F D3 0C    CALL     $0CD3
    0C86: E4 47       MOV      A, $47
    0C88: 24 5E       AND      A, $5E
    0C8A: F0 46       BEQ      $0CD2
    0C8C: F5 31 03    MOV      A, $0331+X
    0C8F: FD          MOV      Y, A
    0C90: F5 30 03    MOV      A, $0330+X
    0C93: DA 10       MOVW     $10, YA
    0C95: 7D          MOV      A, X
    0C96: 9F          XCN      A
    0C97: 5C          LSR      A
    0C98: C4 12       MOV      $12, A
    0C9A: EB 11       MOV      Y, $11
    0C9C: F6 74 0E    MOV      A, $0E74+Y
    0C9F: 80          SETC
    0CA0: B6 73 0E    SBC      A, $0E73+Y
    0CA3: EB 10       MOV      Y, $10
    0CA5: CF          MUL      YA
    0CA6: DD          MOV      A, Y
    0CA7: EB 11       MOV      Y, $11
    0CA9: 60          CLRC
    0CAA: 96 73 0E    ADC      A, $0E73+Y
    0CAD: FD          MOV      Y, A
    0CAE: F5 21 03    MOV      A, $0321+X
    0CB1: CF          MUL      YA
    0CB2: F5 51 03    MOV      A, $0351+X
    0CB5: 1C          ASL      A
    0CB6: 13 12 01    BBC      B0 $12, $0CBA
    0CB9: 1C          ASL      A
    0CBA: DD          MOV      A, Y
    0CBB: 90 03       BCC      $0CC0
    0CBD: 48 FF       EOR      A, #$FF
    0CBF: BC          INC      A
    0CC0: EB 12       MOV      Y, $12
    0CC2: 3F 41 07    CALL     $0741
    0CC5: 8D 14       MOV      Y, #$14
    0CC7: E8 00       MOV      A, #$00
    0CC9: 9A 10       SUBW     YA, $10
    0CCB: DA 10       MOVW     $10, YA
    0CCD: AB 12       INC      $12
    0CCF: 33 12 C8    BBC      B1 $12, $0C9A
    0CD2: 6F          RET
    0CD3: 09 47 5E    OR       $47, $5E
    0CD6: DA 14       MOVW     $14, YA
    0CD8: DA 16       MOVW     $16, YA
    0CDA: 4D          PUSH     X
    0CDB: EE          POP      Y
    0CDC: 60          CLRC
    0CDD: D0 0A       BNE      $0CE9
    0CDF: 98 1F 16    ADC      $1F, #$16
    0CE2: E8 00       MOV      A, #$00
    0CE4: D7 14       MOV      [$14]+Y, A
    0CE6: FC          INC      Y
    0CE7: 2F 09       BRA      $0CF2
    0CE9: 98 10 16    ADC      $10, #$16
    0CEC: 3F F0 0C    CALL     $0CF0
    0CEF: FC          INC      Y
    0CF0: F7 14       MOV      A, [$14]+Y
    0CF2: 97 16       ADC      A, [$16]+Y
    0CF4: D7 14       MOV      [$14]+Y, A
    0CF6: 6F          RET
    0CF7: F4 71       MOV      A, $71+X
    0CF9: F0 65       BEQ      $0D60
    0CFB: 9B 71       DEC      $71+X
    0CFD: F0 05       BEQ      $0D04
    0CFF: E8 02       MOV      A, #$02
    0D01: DE 70 5C    CBNE     $70+X, $0D60
    0D04: F4 80       MOV      A, $80+X
    0D06: C4 17       MOV      $17, A
    0D08: F4 30       MOV      A, $30+X
    0D0A: FB 31       MOV      Y, $31+X
    0D0C: DA 14       MOVW     $14, YA              Looks like a start point; watch for calls to here with pointers in YA
    0D0E: 8D 00       MOV      Y, #$00
    0D10: F7 14       MOV      A, [$14]+Y           Get a value. This seems like a test parser of some kind... it doesn't
    0D12: F0 1E       BEQ      $0D32                 DO much, excluding manage address incrementation...
    0D14: 30 07       BMI      $0D1D
    0D16: FC          INC      Y
    0D17: 30 40       BMI      $0D59
    0D19: F7 14       MOV      A, [$14]+Y
    0D1B: 10 F9       BPL      $0D16                0D16: 01-7F
    0D1D: 68 C8       CMP      A, #$C8              0D60: C8
    0D1F: F0 3F       BEQ      $0D60
    0D21: 68 EF       CMP      A, #$EF              0D4E: EF
    0D23: F0 29       BEQ      $0D4E
    0D25: 68 E0       CMP      A, #$E0              0D59: 80-C7, C9-DF
    0D27: 90 30       BCC      $0D59
    0D29: 6D          PUSH     Y                    0D29: E0-EE, F0-FF
    0D2A: FD          MOV      Y, A                 Y = command (E0-EE, F0-FF)
    0D2B: AE          POP      A                    A = distance from base address (stored in $14)
    0D2C: 96 41 0B    ADC      A, $0B41+Y           Add to distance from base address, so as to not trip over command args
    0D2F: FD          MOV      Y, A                 And move it back to Y.
    0D30: 2F DE       BRA      $0D10                *END* E0-EE, F0-FF
    0D32: E4 17       MOV      A, $17
    0D34: F0 23       BEQ      $0D59
    0D36: 8B 17       DEC      $17
    0D38: D0 0A       BNE      $0D44
    0D3A: F5 31 02    MOV      A, $0231+X
    0D3D: 2D          PUSH     A
    0D3E: F5 30 02    MOV      A, $0230+X
    0D41: EE          POP      Y
    0D42: 2F C8       BRA      $0D0C
    0D44: F5 41 02    MOV      A, $0241+X
    0D47: 2D          PUSH     A
    0D48: F5 40 02    MOV      A, $0240+X
    0D4B: EE          POP      Y
    0D4C: 2F BE       BRA      $0D0C
    0D4E: FC          INC      Y
    0D4F: F7 14       MOV      A, [$14]+Y
    0D51: 2D          PUSH     A
    0D52: FC          INC      Y
    0D53: F7 14       MOV      A, [$14]+Y
    0D55: FD          MOV      Y, A
    0D56: AE          POP      A
    0D57: 2F B3       BRA      $0D0C
    0D59: E4 47       MOV      A, $47
    0D5B: 8D 5C       MOV      Y, #$5C
    0D5D: 3F 41 07    CALL     $0741
    0D60: F2 13       CLR1     $13, 7
    0D62: F4 A0       MOV      A, $A0+X
    0D64: F0 13       BEQ      $0D79
    0D66: F4 A1       MOV      A, $A1+X
    0D68: F0 04       BEQ      $0D6E
    0D6A: 9B A1       DEC      $A1+X
    0D6C: 2F 0B       BRA      $0D79
    0D6E: E2 13       SET1     $13, 7
    0D70: E8 60       MOV      A, #$60
    0D72: 8D 03       MOV      Y, #$03
    0D74: 9B A0       DEC      $A0+X
    0D76: 3F D6 0C    CALL     $0CD6
    0D79: 3F BC 0B    CALL     $0BBC
    0D7C: F4 B1       MOV      A, $B1+X
    0D7E: F0 4C       BEQ      $0DCC
    0D80: F5 B0 02    MOV      A, $02B0+X
    0D83: DE B0 44    CBNE     $B0+X, $0DCA
    0D86: F5 00 01    MOV      A, $0100+X
    0D89: 75 B1 02    CMP      A, $02B1+X
    0D8C: D0 05       BNE      $0D93
    0D8E: F5 C1 02    MOV      A, $02C1+X
    0D91: 2F 0D       BRA      $0DA0
    0D93: 40          SETP
    0D94: BB 00       INC      $00+X
    0D96: 20          CLRP
    0D97: FD          MOV      Y, A
    0D98: F0 02       BEQ      $0D9C
    0D9A: F4 B1       MOV      A, $B1+X
    0D9C: 60          CLRC
    0D9D: 95 C0 02    ADC      A, $02C0+X
    0DA0: D4 B1       MOV      $B1+X, A
    0DA2: F5 A0 02    MOV      A, $02A0+X
    0DA5: 60          CLRC
    0DA6: 95 A1 02    ADC      A, $02A1+X
    0DA9: D5 A0 02    MOV      $02A0+X, A
    0DAC: C4 12       MOV      $12, A
    0DAE: 1C          ASL      A
    0DAF: 1C          ASL      A
    0DB0: 90 02       BCC      $0DB4
    0DB2: 48 FF       EOR      A, #$FF
    0DB4: FD          MOV      Y, A
    0DB5: F4 B1       MOV      A, $B1+X
    0DB7: 68 F1       CMP      A, #$F1
    0DB9: 90 05       BCC      $0DC0
    0DBB: 28 0F       AND      A, #$0F
    0DBD: CF          MUL      YA
    0DBE: 2F 04       BRA      $0DC4
    0DC0: CF          MUL      YA
    0DC1: DD          MOV      A, Y
    0DC2: 8D 00       MOV      Y, #$00
    0DC4: 3F 41 0E    CALL     $0E41
    0DC7: 5F BE 06    JMP      $06BE
    0DCA: BB B0       INC      $B0+X
    0DCC: E3 13 F8    BBS      B7 $13, $0DC7
    0DCF: 6F          RET
    0DD0: F2 13       CLR1     $13, 7
    0DD2: F4 C1       MOV      A, $C1+X
    0DD4: F0 09       BEQ      $0DDF
    0DD6: F5 E0 02    MOV      A, $02E0+X
    0DD9: DE C0 03    CBNE     $C0+X, $0DDF
    0DDC: 3F 49 0E    CALL     $0E49
    0DDF: F5 31 03    MOV      A, $0331+X
    0DE2: FD          MOV      Y, A
    0DE3: F5 30 03    MOV      A, $0330+X
    0DE6: DA 10       MOVW     $10, YA
    0DE8: F4 91       MOV      A, $91+X
    0DEA: F0 0A       BEQ      $0DF6
    0DEC: F5 41 03    MOV      A, $0341+X
    0DEF: FD          MOV      Y, A
    0DF0: F5 40 03    MOV      A, $0340+X
    0DF3: 3F 2B 0E    CALL     $0E2B
    0DF6: F3 13 03    BBC      B7 $13, $0DFC
    0DF9: 3F 95 0C    CALL     $0C95
    0DFC: F2 13       CLR1     $13, 7
    0DFE: 3F BC 0B    CALL     $0BBC
    0E01: F4 A0       MOV      A, $A0+X
    0E03: F0 0E       BEQ      $0E13
    0E05: F4 A1       MOV      A, $A1+X
    0E07: D0 0A       BNE      $0E13
    0E09: F5 71 03    MOV      A, $0371+X
    0E0C: FD          MOV      Y, A
    0E0D: F5 70 03    MOV      A, $0370+X
    0E10: 3F 2B 0E    CALL     $0E2B
    0E13: F4 B1       MOV      A, $B1+X
    0E15: F0 B5       BEQ      $0DCC
    0E17: F5 B0 02    MOV      A, $02B0+X
    0E1A: DE B0 AF    CBNE     $B0+X, $0DCC
    0E1D: EB 51       MOV      Y, $51
    0E1F: F5 A1 02    MOV      A, $02A1+X
    0E22: CF          MUL      YA
    0E23: DD          MOV      A, Y
    0E24: 60          CLRC
    0E25: 95 A0 02    ADC      A, $02A0+X
    0E28: 5F AC 0D    JMP      $0DAC
    0E2B: E2 13       SET1     $13, 7
    0E2D: CB 12       MOV      $12, Y
    0E2F: 3F D9 0B    CALL     $0BD9
    0E32: 6D          PUSH     Y
    0E33: EB 51       MOV      Y, $51
    0E35: CF          MUL      YA
    0E36: CB 14       MOV      $14, Y
    0E38: 8F 00 15    MOV      $00, #$15
    0E3B: EB 51       MOV      Y, $51
    0E3D: AE          POP      A
    0E3E: CF          MUL      YA
    0E3F: 7A 14       ADDW     YA, $14
    0E41: 3F D9 0B    CALL     $0BD9
    0E44: 7A 10       ADDW     YA, $10
    0E46: DA 10       MOVW     $10, YA
    0E48: 6F          RET
    0E49: E2 13       SET1     $13, 7
    0E4B: EB 51       MOV      Y, $51
    0E4D: F5 D1 02    MOV      A, $02D1+X
    0E50: CF          MUL      YA
    0E51: DD          MOV      A, Y
    0E52: 60          CLRC
    0E53: 95 D0 02    ADC      A, $02D0+X
    0E56: 1C          ASL      A
    0E57: 90 02       BCC      $0E5B
    0E59: 48 FF       EOR      A, #$FF
    0E5B: FB C1       MOV      Y, $C1+X
    0E5D: CF          MUL      YA
    0E5E: DD          MOV      A, Y
    0E5F: 48 FF       EOR      A, #$FF
    0E61: EB 59       MOV      Y, $59
    0E63: CF          MUL      YA
    0E64: F5 10 02    MOV      A, $0210+X
    0E67: CF          MUL      YA
    0E68: F5 01 03    MOV      A, $0301+X
    0E6B: CF          MUL      YA
    0E6C: DD          MOV      A, Y
    0E6D: CF          MUL      YA
    0E6E: DD          MOV      A, Y
    0E6F: D5 21 03    MOV      $0321+X, A
    0E72: 6F          RET
    0E73: 00          NOP
    0E74: 01          TCALL    0
    0E75: 03 07 0D    BBS      B0 $07, $0E85
    0E78: 15 1E 29    OR       A, $291E+X
    0E7B: 34 42       AND      A, $42+X
    0E7D: 51          TCALL    5
    0E7E: 5E 67 6E    CMP      Y, $6E67
    0E81: 73 77 7A    BBC      B3 $77, $0EFE
    0E84: 7C          ROR      A
    0E85: 7D          MOV      A, X
    0E86: 7E 7F       CMP      Y, $7F
    0E88: 7F          RETI
    0E89: 00          NOP
    0E8A: 00          NOP
    0E8B: 00          NOP
    0E8C: 00          NOP
    0E8D: 00          NOP
    0E8E: 00          NOP
    0E8F: 00          NOP
    0E90: 58 BF DB    EOR      $BF, #$DB
    0E93: F0 FE       BEQ      $0E93
    0E95: 07 0C       OR       A, [$0C+X]
    0E97: 0C 0C 21    ASL      $210C
    0E9A: 2B 2B       ROL      $2B
    0E9C: 13 FE F3    BBC      B0 $FE, $0E92
    0E9F: F9 34       MOV      X, $34+Y
    0EA1: 33 00 D9    BBC      B1 $00, $0E7D
    0EA4: E5 01 FC    MOV      A, $FC01
    0EA7: EB 2C       MOV      Y, $2C
    0EA9: 3C          ROL      A
    0EAA: 0D          PUSH     PSW
    0EAB: 4D          PUSH     X
    0EAC: 6C 4C 5C    ROR      $5C4C
    0EAF: 3D          INC      X
    0EB0: 2D          PUSH     A
    0EB1: 5C          LSR      A
    0EB2: 61          TCALL    6
    0EB3: 63 4E 4A    BBS      B3 $4E, $0F00
    0EB6: 48 45       EOR      A, #$45
    0EB8: 0E 49 4B    TSET1    $4B49
    0EBB: 46          EOR      A, (X)
    0EBC: 5F 08 DE    JMP      $DE08
    0EBF: 08 65       OR       A, #$65
    0EC1: 09 F4 09    OR       $F4, $09
    0EC4: 8C 0A 2C    DEC      $2C0A
    0EC7: 0B D6       ASL      $D6
    0EC9: 0B 8B       ASL      $8B
    0ECB: 0C 4A 0D    ASL      $0D4A
    0ECE: 14 0E       OR       A, $0E+X
    0ED0: EA 0E CD    NOT1     $CD0E
    0ED3: 0F          BRK
    0ED4: BE          DAS      A
    0ED5: 10 2A       BPL      $0F01
    0ED7: 56 65 72    EOR      A, $7265+Y
    0EDA: 20          CLRP
    0EDB: 53 31 2E    BBC      B2 $31, $0F0C
    0EDE: 32 30       CLR1     $30, 1
    0EE0: 2A E8 AA    OR1      /C, $AAE8
    0EE3: C5 F4 00    MOV      $00F4, A             Looks like byte exchanges involving the data transfer setup of the
    0EE6: E8 BB       MOV      A, #$BB               SPC700 (0xAABB, etc)...
    0EE8: C5 F5 00    MOV      $00F5, A
    0EEB: E5 F4 00    MOV      A, $00F4
    0EEE: 68 CC       CMP      A, #$CC
    0EF0: D0 F9       BNE      $0EEB
    0EF2: 2F 20       BRA      $0F14
    0EF4: EC F4 00    MOV      Y, $00F4
    0EF7: D0 FB       BNE      $0EF4
    0EF9: 5E F4 00    CMP      Y, $00F4
    0EFC: D0 0F       BNE      $0F0D
    0EFE: E5 F5 00    MOV      A, $00F5
    0F01: CC F4 00    MOV      $00F4, Y
    0F04: D7 14       MOV      [$14]+Y, A
    0F06: FC          INC      Y
    0F07: D0 F0       BNE      $0EF9
    0F09: AB 15       INC      $15
    0F0B: 2F EC       BRA      $0EF9
    0F0D: 10 EA       BPL      $0EF9
    0F0F: 5E F4 00    CMP      Y, $00F4
    0F12: 10 E5       BPL      $0EF9
    0F14: E5 F6 00    MOV      A, $00F6
    0F17: EC F7 00    MOV      Y, $00F7
    0F1A: DA 14       MOVW     $14, YA
    0F1C: EC F4 00    MOV      Y, $00F4
    0F1F: E5 F5 00    MOV      A, $00F5
    0F22: CC F4 00    MOV      $00F4, Y
    0F25: D0 CD       BNE      $0EF4
    0F27: CD 31       MOV      X, #$31
    0F29: C9 F1 00    MOV      $00F1, X
    0F2C: 6F          RET
    0F2D: DA 20       MOVW     $20, YA
    0F2F: 8D 00       MOV      Y, #$00
    0F31: F7 20       MOV      A, [$20]+Y
    0F33: D6 D8 04    MOV      $04D8+Y, A
    0F36: FC          INC      Y
    0F37: AD 07       CMP      Y, #$07
    0F39: D0 F6       BNE      $0F31
    0F3B: 6F          RET
    0F3C: DA 20       MOVW     $20, YA              Copy 0x3 bytes from an address held in YA to $04DA.
    0F3E: 8D 00       MOV      Y, #$00
    0F40: F7 20       MOV      A, [$20]+Y
    0F42: D6 DA 04    MOV      $04DA+Y, A
    0F45: FC          INC      Y
    0F46: AD 03       CMP      Y, #$03
    0F48: D0 F6       BNE      $0F40
    0F4A: 6F          RET
    0F4B: 3F 56 0F    CALL     $0F56
    0F4E: F8 2E       MOV      X, $2E
    0F50: E8 01       MOV      A, #$01
    0F52: 3F 94 13    CALL     $1394
    0F55: 6F          RET
    0F56: 3F 3C 0F    CALL     $0F3C
    0F59: F7 20       MOV      A, [$20]+Y
    0F5B: C5 DE 04    MOV      $04DE, A
    0F5E: F8 2F       MOV      X, $2F
    0F60: E4 20       MOV      A, $20
    0F62: D5 80 04    MOV      $0480+X, A
    0F65: E4 21       MOV      A, $21
    0F67: 3D          INC      X
    0F68: D5 80 04    MOV      $0480+X, A
    0F6B: F8 2E       MOV      X, $2E
    0F6D: E8 00       MOV      A, #$00
    0F6F: D5 C8 04    MOV      $04C8+X, A
    0F72: 8D 04       MOV      Y, #$04
    0F74: F7 20       MOV      A, [$20]+Y
    0F76: C4 22       MOV      $22, A
    0F78: FC          INC      Y
    0F79: F7 20       MOV      A, [$20]+Y
    0F7B: C5 DD 04    MOV      $04DD, A
    0F7E: 68 C9       CMP      A, #$C9              *grasps at potential parsing straws*
    0F80: D0 03       BNE      $0F85
    0F82: D5 C8 04    MOV      $04C8+X, A
    0F85: 3F A1 0F    CALL     $0FA1
    0F88: E8 D8       MOV      A, #$D8
    0F8A: 8D 04       MOV      Y, #$04
    0F8C: 5F FC 14    JMP      $14FC
    0F8F: E4 46       MOV      A, $46
    0F91: 05 D7 04    OR       A, $04D7
    0F94: C4 46       MOV      $46, A
    0F96: E8 04       MOV      A, #$04
    0F98: C5 D8 04    MOV      $04D8, A
    0F9B: E8 02       MOV      A, #$02
    0F9D: C5 D9 04    MOV      $04D9, A
    0FA0: 6F          RET
    0FA1: E4 22       MOV      A, $22
    0FA3: 68 7F       CMP      A, #$7F
    0FA5: F0 E8       BEQ      $0F8F
    0FA7: 9C          DEC      A
    0FA8: FD          MOV      Y, A
    0FA9: F6 68 10    MOV      A, $1068+Y
    0FAC: C5 D8 04    MOV      $04D8, A
    0FAF: FC          INC      Y
    0FB0: F6 68 10    MOV      A, $1068+Y
    0FB3: C5 D9 04    MOV      $04D9, A
    0FB6: 6F          RET
    0FB7: 3F A0 13    CALL     $13A0
    0FBA: D0 FA       BNE      $0FB6
    0FBC: E4 2A       MOV      A, $2A
    0FBE: BC          INC      A
    0FBF: BC          INC      A
    0FC0: EB 2B       MOV      Y, $2B
    0FC2: 3F 3C 0F    CALL     $0F3C
    0FC5: FC          INC      Y
    0FC6: F7 20       MOV      A, [$20]+Y
    0FC8: C5 DE 04    MOV      $04DE, A
    0FCB: F4 D0       MOV      A, $D0+X
    0FCD: 60          CLRC
    0FCE: 88 06       ADC      A, #$06
    0FD0: FD          MOV      Y, A
    0FD1: BB D0       INC      $D0+X
    0FD3: F8 2F       MOV      X, $2F
    0FD5: F5 80 04    MOV      A, $0480+X
    0FD8: C4 20       MOV      $20, A
    0FDA: 3D          INC      X
    0FDB: F5 80 04    MOV      A, $0480+X
    0FDE: C4 21       MOV      $21, A
    0FE0: F7 20       MOV      A, [$20]+Y
    0FE2: C4 22       MOV      $22, A
    0FE4: F0 25       BEQ      $100B
    0FE6: F8 2E       MOV      X, $2E
    0FE8: 28 80       AND      A, #$80
    0FEA: F0 22       BEQ      $100E                More parserish possibility...
    0FEC: E4 22       MOV      A, $22
    0FEE: 68 E0       CMP      A, #$E0
    0FF0: F0 2E       BEQ      $1020
    0FF2: 68 C9       CMP      A, #$C9
    0FF4: F0 6E       BEQ      $1064
    0FF6: 68 E1       CMP      A, #$E1
    0FF8: F0 32       BEQ      $102C
    0FFA: 68 ED       CMP      A, #$ED
    0FFC: F0 5A       BEQ      $1058
    0FFE: EB 22       MOV      Y, $22
    1000: E8 00       MOV      A, #$00
    1002: D5 C8 04    MOV      $04C8+X, A
    1005: E5 DE 04    MOV      A, $04DE
    1008: 5F E7 10    JMP      $10E7
    100B: 5F 91 14    JMP      $1491
    100E: 3F A1 0F    CALL     $0FA1
    1011: E5 D8 04    MOV      A, $04D8
    1014: D5 BC 04    MOV      $04BC+X, A
    1017: E5 D9 04    MOV      A, $04D9
    101A: D5 C0 04    MOV      $04C0+X, A
    101D: 5F CB 0F    JMP      $0FCB
    1020: FC          INC      Y
    1021: F7 20       MOV      A, [$20]+Y
    1023: 3F 30 11    CALL     $1130
    1026: F8 2E       MOV      X, $2E
    1028: BB D0       INC      $D0+X
    102A: D0 9F       BNE      $0FCB
    102C: FC          INC      Y
    102D: F7 20       MOV      A, [$20]+Y
    102F: C4 23       MOV      $23, A
    1031: E5 DB 04    MOV      A, $04DB
    1034: C4 22       MOV      $22, A
    1036: E5 DF 04    MOV      A, $04DF
    1039: 5D          MOV      X, A
    103A: 3F B6 10    CALL     $10B6
    103D: EB 2E       MOV      Y, $2E
    103F: E8 08       MOV      A, #$08
    1041: CF          MUL      YA
    1042: 5D          MOV      X, A
    1043: E4 23       MOV      A, $23
    1045: D5 64 04    MOV      $0464+X, A
    1048: C5 DC 04    MOV      $04DC, A
    104B: E4 22       MOV      A, $22
    104D: D5 63 04    MOV      $0463+X, A
    1050: C5 DB 04    MOV      $04DB, A
    1053: F8 2E       MOV      X, $2E
    1055: 5F 28 10    JMP      $1028
    1058: FC          INC      Y
    1059: F7 20       MOV      A, [$20]+Y
    105B: C4 22       MOV      $22, A
    105D: E5 DC 04    MOV      A, $04DC
    1060: C4 23       MOV      $23, A
    1062: D0 D2       BNE      $1036
    1064: D5 C8 04    MOV      $04C8+X, A
    1067: 6F          RET
    1068: 03 01 03    BBS      B0 $01, $106E
    106B: 02 06       SET1     $06, 0
    106D: 02 06       SET1     $06, 0
    106F: 05 0C 02    OR       A, $020C
    1072: 0C 0A 18    ASL      $180A
    1075: 02 18       SET1     $18, 0
    1077: 16 30 02    OR       A, $0230+Y
    107A: 30 2E       BMI      $10AA
    107C: 60          CLRC
    107D: 02 60       SET1     $60, 0
    107F: 5E 12 02    CMP      Y, $0212
    1082: 12 10       CLR1     $10, 0
    1084: 24 02       AND      A, $02
    1086: 24 22       AND      A, $22
    1088: 09 02 09    OR       $02, $09
    108B: 07 01       OR       A, [$01+X]
    108D: 02 02       SET1     $02, 0
    108F: 02 03       SET1     $03, 0
    1091: 02 04       SET1     $04, 0
    1093: 02 05       SET1     $05, 0
    1095: 02 06       SET1     $06, 0
    1097: 02 03       SET1     $03, 0
    1099: 03 06 03    BBS      B0 $06, $109F
    109C: 0C 06 18    ASL      $1806
    109F: 0C 30 18    ASL      $1830
    10A2: 60          CLRC
    10A3: 30 12       BMI      $10B7
    10A5: 09 24 12    OR       $24, $12
    10A8: 04 02       OR       A, $02
    10AA: 04 02       OR       A, $02
    10AC: 04 03       OR       A, $03
    10AE: 05 03 05    OR       A, $0503
    10B1: 02 05       SET1     $05, 0
    10B3: 04 02       OR       A, $02
    10B5: 01          TCALL    0
    10B6: E4 22       MOV      A, $22
    10B8: D5 21 03    MOV      $0321+X, A
    10BB: E4 23       MOV      A, $23
    10BD: 28 C0       AND      A, #$C0
    10BF: D5 51 03    MOV      $0351+X, A
    10C2: E4 23       MOV      A, $23
    10C4: 28 3F       AND      A, #$3F
    10C6: C5 33 04    MOV      $0433, A
    10C9: E5 31 04    MOV      A, $0431
    10CC: 68 00       CMP      A, #$00
    10CE: F0 05       BEQ      $10D5
    10D0: E8 0A       MOV      A, #$0A
    10D2: C5 33 04    MOV      $0433, A
    10D5: E5 33 04    MOV      A, $0433
    10D8: C4 11       MOV      $11, A
    10DA: 8F 00 10    MOV      $00, #$10
    10DD: E4 47       MOV      A, $47
    10DF: 25 D6 04    AND      A, $04D6
    10E2: C4 47       MOV      $47, A
    10E4: 5F 95 0C    JMP      $0C95
    10E7: 4D          PUSH     X
    10E8: 2D          PUSH     A
    10E9: E4 47       MOV      A, $47
    10EB: 25 D6 04    AND      A, $04D6
    10EE: C4 47       MOV      $47, A
    10F0: E5 DF 04    MOV      A, $04DF
    10F3: 5D          MOV      X, A
    10F4: AE          POP      A
    10F5: DA 10       MOVW     $10, YA
    10F7: 3F D4 06    CALL     $06D4
    10FA: CE          POP      X
    10FB: 6F          RET
    10FC: F8 2F       MOV      X, $2F
    10FE: D5 58 04    MOV      $0458+X, A
    1101: DD          MOV      A, Y
    1102: D5 59 04    MOV      $0459+X, A
    1105: F8 2E       MOV      X, $2E
    1107: E8 01       MOV      A, #$01
    1109: D5 54 04    MOV      $0454+X, A
    110C: 6F          RET
    110D: 6D          PUSH     Y
    110E: 2D          PUSH     A
    110F: E5 D5 04    MOV      A, $04D5
    1112: 60          CLRC
    1113: 88 05       ADC      A, #$05
    1115: FD          MOV      Y, A
    1116: C4 20       MOV      $20, A
    1118: AE          POP      A
    1119: 3F 49 07    CALL     $0749
    111C: EE          POP      Y
    111D: DD          MOV      A, Y
    111E: EB 20       MOV      Y, $20
    1120: FC          INC      Y
    1121: 5F 49 07    JMP      $0749
    1124: 2D          PUSH     A
    1125: E5 D5 04    MOV      A, $04D5
    1128: 60          CLRC
    1129: 88 07       ADC      A, #$07
    112B: FD          MOV      Y, A
    112C: AE          POP      A
    112D: 5F 49 07    JMP      $0749
    1130: 2D          PUSH     A
    1131: E4 49       MOV      A, $49
    1133: 25 D6 04    AND      A, $04D6
    1136: C4 49       MOV      $49, A
    1138: E5 DF 04    MOV      A, $04DF
    113B: 5D          MOV      X, A
    113C: AE          POP      A
    113D: 8F 00 47    MOV      $00, #$47
    1140: 3F 62 09    CALL     $0962
    1143: 6F          RET
    1144: 2D          PUSH     A
    1145: E5 D5 04    MOV      A, $04D5
    1148: FD          MOV      Y, A
    1149: AE          POP      A
    114A: 3F 49 07    CALL     $0749
    114D: FC          INC      Y
    114E: 3F 49 07    CALL     $0749
    1151: 6F          RET
    1152: CD 00       MOV      X, #$00
    1154: 1A 20       DECW     $20
    1156: 1A 20       DECW     $20
    1158: E7 20       MOV      A, [$20+X]
    115A: C5 C6 17    MOV      $17C6, A
    115D: 1A 20       DECW     $20
    115F: E7 20       MOV      A, [$20+X]
    1161: C5 C5 17    MOV      $17C5, A
    1164: E8 01       MOV      A, #$01
    1166: C5 B2 04    MOV      $04B2, A
    1169: 3A 20       INCW     $20
    116B: 3A 20       INCW     $20
    116D: 3A 20       INCW     $20
    116F: 6F          RET
    1170: 3F 52 11    CALL     $1152
    1173: 5F 78 12    JMP      $1278
    1176: 3F 52 11    CALL     $1152
    1179: 5F 70 12    JMP      $1270
    117C: 0E 00 02    TSET1    $0200
    117F: 04 06       OR       A, $06
    1181: 08 0A       OR       A, #$0A
    1183: 0C 8F 60    ASL      $608F
    1186: 2A 8F 04    OR1      /C, $048F
    1189: 2B CD       ROL      $CD
    118B: 00          NOP
    118C: E8 EF       MOV      A, #$EF
    118E: 8D 10       MOV      Y, #$10
    1190: 8F 08 20    MOV      $08, #$20
    1193: 8F 40 21    MOV      $40, #$21
    1196: D0 6F       BNE      $1207
    1198: E5 B1 04    MOV      A, $04B1
    119B: 28 7F       AND      A, #$7F
    119D: C5 B1 04    MOV      $04B1, A
    11A0: 8F D5 24    MOV      $D5, #$24
    11A3: 8F 17 25    MOV      $17, #$25
    11A6: 8F 68 2A    MOV      $68, #$2A
    11A9: 8F 04 2B    MOV      $04, #$2B
    11AC: CD 01       MOV      X, #$01
    11AE: E8 DF       MOV      A, #$DF
    11B0: 8D 20       MOV      Y, #$20
    11B2: 8F 0A 20    MOV      $0A, #$20
    11B5: 8F 50 21    MOV      $50, #$21
    11B8: D0 4D       BNE      $1207
    11BA: E5 B2 04    MOV      A, $04B2
    11BD: 28 7F       AND      A, #$7F
    11BF: C5 B2 04    MOV      $04B2, A
    11C2: 8F C5 24    MOV      $C5, #$24
    11C5: 8F 17 25    MOV      $17, #$25
    11C8: 8F 70 2A    MOV      $70, #$2A
    11CB: 8F 04 2B    MOV      $04, #$2B
    11CE: CD 02       MOV      X, #$02
    11D0: E8 BF       MOV      A, #$BF
    11D2: 8D 40       MOV      Y, #$40
    11D4: 8F 0C 20    MOV      $0C, #$20
    11D7: 8F 60 21    MOV      $60, #$21
    11DA: D0 2B       BNE      $1207
    11DC: E5 B3 04    MOV      A, $04B3
    11DF: 28 7F       AND      A, #$7F
    11E1: C5 B3 04    MOV      $04B3, A
    11E4: 8F C7 24    MOV      $C7, #$24
    11E7: 8F 16 25    MOV      $16, #$25
    11EA: E4 19       MOV      A, $19
    11EC: C5 EF 18    MOV      $18EF, A
    11EF: C5 F7 18    MOV      $18F7, A
    11F2: C5 FF 18    MOV      $18FF, A
    11F5: 8F 78 2A    MOV      $78, #$2A
    11F8: 8F 04 2B    MOV      $04, #$2B
    11FB: CD 03       MOV      X, #$03
    11FD: E8 7F       MOV      A, #$7F
    11FF: 8D 80       MOV      Y, #$80
    1201: 8F 0E 20    MOV      $0E, #$20
    1204: 8F 70 21    MOV      $70, #$21
    1207: C5 D6 04    MOV      $04D6, A
    120A: CC D7 04    MOV      $04D7, Y
    120D: E4 20       MOV      A, $20
    120F: C5 DF 04    MOV      $04DF, A
    1212: E4 21       MOV      A, $21
    1214: C5 D5 04    MOV      $04D5, A
    1217: 7D          MOV      A, X
    1218: C4 2E       MOV      $2E, A
    121A: 1C          ASL      A
    121B: C4 2F       MOV      $2F, A
    121D: F5 B0 04    MOV      A, $04B0+X
    1220: C4 20       MOV      $20, A
    1222: C4 2D       MOV      $2D, A
    1224: F0 47       BEQ      $126D
    1226: 75 D0 04    CMP      A, $04D0+X
    1229: B0 42       BCS      $126D
    122B: 28 07       AND      A, #$07
    122D: FD          MOV      Y, A
    122E: F6 7C 11    MOV      A, $117C+Y
    1231: 8B 20       DEC      $20
    1233: 38 F8 20    AND      $F8, #$20
    1236: 0B 20       ASL      $20
    1238: 04 20       OR       A, $20
    123A: 5D          MOV      X, A
    123B: E4 2E       MOV      A, $2E
    123D: 7D          MOV      A, X
    123E: FD          MOV      Y, A
    123F: F7 24       MOV      A, [$24]+Y
    1241: C4 20       MOV      $20, A
    1243: 3A 24       INCW     $24
    1245: F7 24       MOV      A, [$24]+Y
    1247: C4 21       MOV      $21, A
    1249: 4D          PUSH     X
    124A: 1A 20       DECW     $20
    124C: CD 00       MOV      X, #$00
    124E: E7 20       MOV      A, [$20+X]
    1250: F8 2E       MOV      X, $2E
    1252: D5 8C 04    MOV      $048C+X, A
    1255: 3A 20       INCW     $20
    1257: 5D          MOV      X, A
    1258: 1F 5B 12    JMP      [$125B+X]
    125B: 80          SETC
    125C: 12 80       CLR1     $80, 0
    125E: 12 70       CLR1     $70, 0
    1260: 12 78       CLR1     $78, 0
    1262: 12 C7       CLR1     $C7, 0
    1264: 12 76       CLR1     $76, 0
    1266: 11          TCALL    1
    1267: 70 11       BVS      $127A
    1269: 88 12       ADC      A, #$12
    126B: 88 12       ADC      A, #$12
    126D: 5F 1A 13    JMP      $131A
    1270: CE          POP      X
    1271: E4 20       MOV      A, $20
    1273: EB 21       MOV      Y, $21
    1275: 5F 56 0F    JMP      $0F56
    1278: CE          POP      X
    1279: E4 20       MOV      A, $20
    127B: EB 21       MOV      Y, $21
    127D: 5F 4B 0F    JMP      $0F4B
    1280: CE          POP      X
    1281: E4 20       MOV      A, $20
    1283: EB 21       MOV      Y, $21
    1285: 5F DF 14    JMP      $14DF
    1288: CE          POP      X
    1289: CD 00       MOV      X, #$00
    128B: 1A 20       DECW     $20
    128D: 1A 20       DECW     $20
    128F: E7 20       MOV      A, [$20+X]
    1291: C4 27       MOV      $27, A
    1293: 1A 20       DECW     $20
    1295: E7 20       MOV      A, [$20+X]
    1297: C4 26       MOV      $26, A
    1299: 1A 20       DECW     $20
    129B: E7 20       MOV      A, [$20+X]
    129D: C4 29       MOV      $29, A
    129F: 1A 20       DECW     $20
    12A1: E7 20       MOV      A, [$20+X]
    12A3: C4 28       MOV      $28, A
    12A5: 3A 20       INCW     $20
    12A7: 3A 20       INCW     $20
    12A9: 3A 20       INCW     $20
    12AB: 3A 20       INCW     $20
    12AD: 3A 20       INCW     $20
    12AF: 8D 05       MOV      Y, #$05
    12B1: F7 20       MOV      A, [$20]+Y
    12B3: 5D          MOV      X, A
    12B4: FC          INC      Y
    12B5: F7 20       MOV      A, [$20]+Y
    12B7: C4 2C       MOV      $2C, A
    12B9: E4 20       MOV      A, $20
    12BB: EB 21       MOV      Y, $21
    12BD: 3F EE 14    CALL     $14EE
    12C0: E8 00       MOV      A, #$00
    12C2: 8D 00       MOV      Y, #$00
    12C4: 5F FC 10    JMP      $10FC
    12C7: CD 00       MOV      X, #$00
    12C9: 1A 20       DECW     $20
    12CB: 1A 20       DECW     $20
    12CD: E7 20       MOV      A, [$20+X]
    12CF: C4 23       MOV      $23, A
    12D1: 1A 20       DECW     $20
    12D3: E7 20       MOV      A, [$20+X]
    12D5: C4 22       MOV      $22, A
    12D7: CE          POP      X
    12D8: E4 2E       MOV      A, $2E
    12DA: 68 03       CMP      A, #$03
    12DC: F0 15       BEQ      $12F3
    12DE: 68 02       CMP      A, #$02
    12E0: F0 1E       BEQ      $1300
    12E2: 68 01       CMP      A, #$01
    12E4: F0 27       BEQ      $130D
    12E6: E4 22       MOV      A, $22
    12E8: C5 40 04    MOV      $0440, A
    12EB: E4 23       MOV      A, $23
    12ED: C5 41 04    MOV      $0441, A
    12F0: 1F 0D 18    JMP      [$180D+X]
    12F3: E4 22       MOV      A, $22
    12F5: C5 46 04    MOV      $0446, A
    12F8: E4 23       MOV      A, $23
    12FA: C5 47 04    MOV      $0447, A
    12FD: 1F C7 16    JMP      [$16C7+X]
    1300: E4 22       MOV      A, $22
    1302: C5 44 04    MOV      $0444, A
    1305: E4 23       MOV      A, $23
    1307: C5 45 04    MOV      $0445, A
    130A: 1F C5 17    JMP      [$17C5+X]
    130D: E4 22       MOV      A, $22
    130F: C5 42 04    MOV      $0442, A
    1312: E4 23       MOV      A, $23
    1314: C5 43 04    MOV      $0443, A
    1317: 1F D5 17    JMP      [$17D5+X]
    131A: F5 B4 04    MOV      A, $04B4+X
    131D: C4 20       MOV      $20, A
    131F: F0 0C       BEQ      $132D
    1321: 75 D0 04    CMP      A, $04D0+X
    1324: B0 07       BCS      $132D
    1326: F5 8C 04    MOV      A, $048C+X
    1329: 5D          MOV      X, A
    132A: 1F 2E 13    JMP      [$132E+X]
    132D: 6F          RET
    132E: 48 13       EOR      A, #$13
    1330: 48 13       EOR      A, #$13
    1332: 4B 13       LSR      $13
    1334: 4B 13       LSR      $13
    1336: 43 13 4B    BBS      B2 $13, $1384
    1339: 13 4B 13    BBC      B0 $4B, $134F
    133C: 40          SETP
    133D: 13 40 13    BBC      B0 $40, $1353
    1340: 5F 4E 13    JMP      $134E
    1343: F8 2F       MOV      X, $2F
    1345: 1F 40 04    JMP      [$0440+X]
    1348: 5F 8B 14    JMP      $148B
    134B: 5F B7 0F    JMP      $0FB7
    134E: 3F A0 13    CALL     $13A0
    1351: F0 3E       BEQ      $1391
    1353: BB D4       INC      $D4+X
    1355: F4 D4       MOV      A, $D4+X
    1357: 64 2C       CMP      A, $2C
    1359: D0 29       BNE      $1384
    135B: E8 00       MOV      A, #$00
    135D: D4 D4       MOV      $D4+X, A
    135F: F4 D0       MOV      A, $D0+X
    1361: FD          MOV      Y, A
    1362: BB D0       INC      $D0+X
    1364: F7 26       MOV      A, [$26]+Y
    1366: 68 00       CMP      A, #$00
    1368: F0 27       BEQ      $1391
    136A: 68 7F       CMP      A, #$7F
    136C: F0 17       BEQ      $1385
    136E: 6D          PUSH     Y
    136F: 3F 24 11    CALL     $1124
    1372: EE          POP      Y
    1373: E4 29       MOV      A, $29
    1375: F0 0D       BEQ      $1384
    1377: F7 28       MOV      A, [$28]+Y
    1379: 28 1F       AND      A, #$1F
    137B: C4 48       MOV      $48, A
    137D: E4 49       MOV      A, $49
    137F: 05 D7 04    OR       A, $04D7
    1382: C4 49       MOV      $49, A
    1384: 6F          RET
    1385: E4 46       MOV      A, $46
    1387: 05 D7 04    OR       A, $04D7
    138A: C4 46       MOV      $46, A
    138C: E8 00       MOV      A, #$00
    138E: 5F 44 11    JMP      $1144
    1391: 5F 91 14    JMP      $1491
    1394: D5 9C 04    MOV      $049C+X, A
    1397: 6F          RET
    1398: D5 BC 04    MOV      $04BC+X, A
    139B: DD          MOV      A, Y
    139C: D5 C0 04    MOV      $04C0+X, A
    139F: 6F          RET
    13A0: F8 2E       MOV      X, $2E
    13A2: F5 B8 04    MOV      A, $04B8+X
    13A5: BC          INC      A
    13A6: D5 B8 04    MOV      $04B8+X, A
    13A9: 68 01       CMP      A, #$01
    13AB: F0 32       BEQ      $13DF
    13AD: 75 BC 04    CMP      A, $04BC+X
    13B0: F0 15       BEQ      $13C7
    13B2: 75 C0 04    CMP      A, $04C0+X
    13B5: D0 1D       BNE      $13D4
    13B7: F5 9C 04    MOV      A, $049C+X
    13BA: 28 01       AND      A, #$01
    13BC: D0 16       BNE      $13D4
    13BE: E4 46       MOV      A, $46
    13C0: 05 D7 04    OR       A, $04D7
    13C3: C4 46       MOV      $46, A
    13C5: D0 0D       BNE      $13D4
    13C7: 68 FF       CMP      A, #$FF
    13C9: D0 04       BNE      $13CF
    13CB: E8 FE       MOV      A, #$FE
    13CD: D0 02       BNE      $13D1
    13CF: E8 00       MOV      A, #$00
    13D1: D5 B8 04    MOV      $04B8+X, A
    13D4: F5 B8 04    MOV      A, $04B8+X
    13D7: 6F          RET
    13D8: E8 81       MOV      A, #$81
    13DA: D5 9C 04    MOV      $049C+X, A
    13DD: D0 0B       BNE      $13EA
    13DF: F5 9C 04    MOV      A, $049C+X
    13E2: 68 01       CMP      A, #$01
    13E4: F0 F2       BEQ      $13D8
    13E6: 68 81       CMP      A, #$81
    13E8: F0 EA       BEQ      $13D4
    13EA: F5 50 04    MOV      A, $0450+X
    13ED: D0 1A       BNE      $1409
    13EF: BC          INC      A
    13F0: D5 50 04    MOV      $0450+X, A
    13F3: 3F 57 15    CALL     $1557
    13F6: F8 2E       MOV      X, $2E
    13F8: F5 54 04    MOV      A, $0454+X
    13FB: F0 0C       BEQ      $1409
    13FD: F8 2F       MOV      X, $2F
    13FF: F5 59 04    MOV      A, $0459+X
    1402: FD          MOV      Y, A
    1403: F5 58 04    MOV      A, $0458+X
    1406: 3F 0D 11    CALL     $110D
    1409: F8 2E       MOV      X, $2E
    140B: F5 C8 04    MOV      A, $04C8+X
    140E: D0 C4       BNE      $13D4
    1410: E4 45       MOV      A, $45
    1412: 05 D7 04    OR       A, $04D7
    1415: C4 45       MOV      $45, A
    1417: 5F D4 13    JMP      $13D4
    141A: 3F 38 14    CALL     $1438
    141D: E8 00       MOV      A, #$00
    141F: C5 91 04    MOV      $0491, A
    1422: C5 38 04    MOV      $0438, A
    1425: C5 39 04    MOV      $0439, A
    1428: C5 3A 04    MOV      $043A, A
    142B: C5 C8 04    MOV      $04C8, A
    142E: C5 C9 04    MOV      $04C9, A
    1431: C5 CA 04    MOV      $04CA, A
    1434: C5 CB 04    MOV      $04CB, A
    1437: 6F          RET
    1438: E8 00       MOV      A, #$00
    143A: C5 B4 04    MOV      $04B4, A
    143D: C5 B5 04    MOV      $04B5, A
    1440: C5 B6 04    MOV      $04B6, A
    1443: C5 B7 04    MOV      $04B7, A
    1446: C5 B0 04    MOV      $04B0, A
    1449: C5 B1 04    MOV      $04B1, A
    144C: C5 B2 04    MOV      $04B2, A
    144F: C5 B3 04    MOV      $04B3, A
    1452: 6F          RET
    1453: 3F 5D 14    CALL     $145D
    1456: 3F 6B 14    CALL     $146B
    1459: 3F 79 14    CALL     $1479
    145C: 6F          RET
    145D: B2 1A       CLR1     $1A, 5
    145F: A2 46       SET1     $46, 5
    1461: A2 47       SET1     $47, 5
    1463: A2 5E       SET1     $5E, 5
    1465: E8 00       MOV      A, #$00
    1467: C5 B5 04    MOV      $04B5, A
    146A: 6F          RET
    146B: D2 1A       CLR1     $1A, 6
    146D: C2 46       SET1     $46, 6
    146F: C2 47       SET1     $47, 6
    1471: C2 5E       SET1     $5E, 6
    1473: E8 00       MOV      A, #$00
    1475: C5 B6 04    MOV      $04B6, A
    1478: 6F          RET
    1479: F2 1A       CLR1     $1A, 7
    147B: E2 46       SET1     $46, 7
    147D: E2 47       SET1     $47, 7
    147F: E2 5E       SET1     $5E, 7
    1481: E8 00       MOV      A, #$00
    1483: C5 B7 04    MOV      $04B7, A
    1486: 6F          RET
    1487: 8A 14 08    EOR1     C, $0814
    148A: 6F          RET
    148B: 3F A0 13    CALL     $13A0
    148E: F0 01       BEQ      $1491
    1490: 6F          RET
    1491: E5 DF 04    MOV      A, $04DF
    1494: 5D          MOV      X, A
    1495: E4 1A       MOV      A, $1A
    1497: 25 D6 04    AND      A, $04D6
    149A: C4 1A       MOV      $1A, A
    149C: E4 47       MOV      A, $47
    149E: 05 D7 04    OR       A, $04D7
    14A1: C4 47       MOV      $47, A
    14A3: E4 5E       MOV      A, $5E
    14A5: 05 D7 04    OR       A, $04D7
    14A8: C4 5E       MOV      $5E, A
    14AA: F5 E0 04    MOV      A, $04E0+X
    14AD: D5 21 03    MOV      $0321+X, A
    14B0: F5 F0 04    MOV      A, $04F0+X
    14B3: D5 51 03    MOV      $0351+X, A
    14B6: 3F 86 0C    CALL     $0C86
    14B9: E5 DF 04    MOV      A, $04DF
    14BC: 5D          MOV      X, A
    14BD: E4 1A       MOV      A, $1A
    14BF: 25 D6 04    AND      A, $04D6
    14C2: C4 1A       MOV      $1A, A
    14C4: F5 11 02    MOV      A, $0211+X
    14C7: 3F 62 09    CALL     $0962
    14CA: F8 2E       MOV      X, $2E
    14CC: E8 00       MOV      A, #$00
    14CE: D5 B4 04    MOV      $04B4+X, A
    14D1: D5 C8 04    MOV      $04C8+X, A
    14D4: D5 8C 04    MOV      $048C+X, A
    14D7: E4 46       MOV      A, $46
    14D9: 05 D7 04    OR       A, $04D7
    14DC: C4 46       MOV      $46, A
    14DE: 6F          RET
    14DF: DA 22       MOVW     $22, YA
    14E1: E8 00       MOV      A, #$00
    14E3: F8 2E       MOV      X, $2E
    14E5: D5 CC 04    MOV      $04CC+X, A
    14E8: D5 C8 04    MOV      $04C8+X, A
    14EB: 5F 05 15    JMP      $1505
    14EE: DA 22       MOVW     $22, YA
    14F0: D8 24       MOV      $24, X
    14F2: E4 48       MOV      A, $48
    14F4: 28 E0       AND      A, #$E0
    14F6: 04 24       OR       A, $24
    14F8: C4 48       MOV      $48, A
    14FA: D0 04       BNE      $1500
    14FC: DA 22       MOVW     $22, YA
    14FE: E8 00       MOV      A, #$00
    1500: F8 2E       MOV      X, $2E
    1502: D5 CC 04    MOV      $04CC+X, A
    1505: 3F 47 15    CALL     $1547
    1508: FA 2A 20    MOV      $2A, $20
    150B: FA 2B 21    MOV      $2B, $21
    150E: 8D 00       MOV      Y, #$00
    1510: F7 22       MOV      A, [$22]+Y
    1512: D7 20       MOV      [$20]+Y, A
    1514: FC          INC      Y
    1515: AD 07       CMP      Y, #$07
    1517: D0 F7       BNE      $1510
    1519: E4 1A       MOV      A, $1A
    151B: 05 D7 04    OR       A, $04D7
    151E: C4 1A       MOV      $1A, A
    1520: E4 46       MOV      A, $46
    1522: 05 D7 04    OR       A, $04D7
    1525: C4 46       MOV      $46, A
    1527: E5 D7 04    MOV      A, $04D7
    152A: E8 00       MOV      A, #$00
    152C: D5 B8 04    MOV      $04B8+X, A
    152F: D4 D0       MOV      $D0+X, A
    1531: D4 D4       MOV      $D4+X, A
    1533: D4 D8       MOV      $D8+X, A
    1535: D5 9C 04    MOV      $049C+X, A
    1538: D5 B0 04    MOV      $04B0+X, A
    153B: D5 50 04    MOV      $0450+X, A
    153E: D5 54 04    MOV      $0454+X, A
    1541: E4 2D       MOV      A, $2D
    1543: D5 B4 04    MOV      $04B4+X, A
    1546: 6F          RET
    1547: 8D 00       MOV      Y, #$00
    1549: F7 22       MOV      A, [$22]+Y
    154B: D5 BC 04    MOV      $04BC+X, A
    154E: FC          INC      Y
    154F: F7 22       MOV      A, [$22]+Y
    1551: C4 21       MOV      $21, A
    1553: D5 C0 04    MOV      $04C0+X, A
    1556: 6F          RET
    1557: E4 2A       MOV      A, $2A
    1559: BC          INC      A
    155A: BC          INC      A
    155B: EB 2B       MOV      Y, $2B
    155D: DA 22       MOVW     $22, YA
    155F: 8D 00       MOV      Y, #$00
    1561: F5 CC 04    MOV      A, $04CC+X
    1564: F0 09       BEQ      $156F
    1566: E4 49       MOV      A, $49
    1568: 05 D7 04    OR       A, $04D7
    156B: C4 49       MOV      $49, A
    156D: D0 07       BNE      $1576
    156F: E4 49       MOV      A, $49
    1571: 25 D6 04    AND      A, $04D6
    1574: C4 49       MOV      $49, A
    1576: E9 DF 04    MOV      X, $04DF
    1579: F5 21 03    MOV      A, $0321+X
    157C: D5 E0 04    MOV      $04E0+X, A
    157F: F5 51 03    MOV      A, $0351+X
    1582: D5 F0 04    MOV      $04F0+X, A
    1585: 8F 00 47    MOV      $00, #$47
    1588: F7 22       MOV      A, [$22]+Y
    158A: 6D          PUSH     Y
    158B: 3F 62 09    CALL     $0962
    158E: EE          POP      Y
    158F: FC          INC      Y
    1590: F7 22       MOV      A, [$22]+Y
    1592: FC          INC      Y
    1593: D5 21 03    MOV      $0321+X, A
    1596: F7 22       MOV      A, [$22]+Y
    1598: 28 C0       AND      A, #$C0
    159A: D5 51 03    MOV      $0351+X, A
    159D: F7 22       MOV      A, [$22]+Y
    159F: FC          INC      Y
    15A0: 28 3F       AND      A, #$3F
    15A2: C5 33 04    MOV      $0433, A
    15A5: E5 31 04    MOV      A, $0431
    15A8: 68 00       CMP      A, #$00
    15AA: F0 05       BEQ      $15B1
    15AC: E8 0A       MOV      A, #$0A
    15AE: C5 33 04    MOV      $0433, A
    15B1: E5 33 04    MOV      A, $0433
    15B4: C4 11       MOV      $11, A
    15B6: 8F 00 10    MOV      $00, #$10
    15B9: E4 47       MOV      A, $47
    15BB: 25 D6 04    AND      A, $04D6
    15BE: C4 47       MOV      $47, A
    15C0: 6D          PUSH     Y
    15C1: 3F 95 0C    CALL     $0C95
    15C4: EE          POP      Y
    15C5: F7 22       MOV      A, [$22]+Y
    15C7: FC          INC      Y
    15C8: C4 11       MOV      $11, A
    15CA: F7 22       MOV      A, [$22]+Y
    15CC: C4 10       MOV      $10, A
    15CE: E4 47       MOV      A, $47
    15D0: 25 D6 04    AND      A, $04D6
    15D3: C4 47       MOV      $47, A
    15D5: 5F D4 06    JMP      $06D4
    15D8: 8A 14 08    EOR1     C, $0814
    15DB: E8 00       MOV      A, #$00
    15DD: C5 B5 04    MOV      $04B5, A
    15E0: C5 91 04    MOV      $0491, A
    15E3: 6F          RET
    15E4: 8A 14 08    EOR1     C, $0814
    15E7: E8 02       MOV      A, #$02
    15E9: C5 92 04    MOV      $0492, A
    15EC: E8 70       MOV      A, #$70
    15EE: C5 91 04    MOV      $0491, A
    15F1: D0 0D       BNE      $1600
    15F3: 8A 14 08    EOR1     C, $0814
    15F6: E8 01       MOV      A, #$01
    15F8: C5 92 04    MOV      $0492, A
    15FB: E8 18       MOV      A, #$18
    15FD: C5 91 04    MOV      $0491, A
    1600: C4 5A       MOV      $5A, A
    1602: E8 10       MOV      A, #$10
    1604: C4 5B       MOV      $5B, A
    1606: 5F 30 0A    JMP      $0A30
    1609: E5 91 04    MOV      A, $0491
    160C: 68 00       CMP      A, #$00
    160E: F0 17       BEQ      $1627
    1610: 68 FF       CMP      A, #$FF
    1612: F0 14       BEQ      $1628
    1614: 9C          DEC      A
    1615: C5 91 04    MOV      $0491, A
    1618: 68 00       CMP      A, #$00
    161A: D0 0B       BNE      $1627
    161C: E8 31       MOV      A, #$31
    161E: C5 F1 00    MOV      $00F1, A
    1621: 3F 38 14    CALL     $1438
    1624: 3F 53 14    CALL     $1453
    1627: 6F          RET
    1628: E8 00       MOV      A, #$00
    162A: C5 91 04    MOV      $0491, A
    162D: 6F          RET
    162E: FD          MOV      Y, A
    162F: 5F 24 0A    JMP      $0A24
    1632: FD          MOV      Y, A
    1633: E4 53       MOV      A, $53
    1635: C5 93 04    MOV      $0493, A
    1638: 5F 3B 0A    JMP      $0A3B
    163B: E5 93 04    MOV      A, $0493
    163E: FD          MOV      Y, A
    163F: D0 F7       BNE      $1638
    1641: C4 54       MOV      $54, A
    1643: E4 20       MOV      A, $20
    1645: C4 55       MOV      $55, A
    1647: 5F 47 0A    JMP      $0A47
    164A: D5 B0 02    MOV      $02B0+X, A
    164D: E4 20       MOV      A, $20
    164F: D5 A1 02    MOV      $02A1+X, A
    1652: E4 21       MOV      A, $21
    1654: D4 B1       MOV      $B1+X, A
    1656: D5 C1 02    MOV      $02C1+X, A
    1659: E8 00       MOV      A, #$00
    165B: D5 B1 02    MOV      $02B1+X, A
    165E: 6F          RET
    165F: E8 00       MOV      A, #$00
    1661: F0 F1       BEQ      $1654
    1663: C4 50       MOV      $50, A
    1665: 6F          RET
    1666: D5 F0 02    MOV      $02F0+X, A
    1669: 6F          RET
    166A: 3F 86 0A    CALL     $0A86
    166D: 6F          RET
    166E: D8 44       MOV      $44, X
    1670: D4 90       MOV      $90+X, A
    1672: 2D          PUSH     A
    1673: E4 20       MOV      A, $20
    1675: 5F 95 0A    JMP      $0A95
    1678: 2D          PUSH     A
    1679: E8 01       MOV      A, #$01
    167B: 2F 03       BRA      $1680
    167D: 2D          PUSH     A
    167E: E8 00       MOV      A, #$00
    1680: D5 90 02    MOV      $0290+X, A
    1683: AE          POP      A
    1684: D5 81 02    MOV      $0281+X, A
    1687: E4 20       MOV      A, $20
    1689: D5 80 02    MOV      $0280+X, A
    168C: E4 21       MOV      A, $21
    168E: D5 91 02    MOV      $0291+X, A
    1691: 6F          RET
    1692: E8 00       MOV      A, #$00
    1694: D5 80 02    MOV      $0280+X, A
    1697: 6F          RET
    1698: 5F C8 09    JMP      $09C8
    169B: D8 44       MOV      $44, X
    169D: D4 91       MOV      $91+X, A
    169F: 2D          PUSH     A
    16A0: E4 20       MOV      A, $20
    16A2: 5F DC 09    JMP      $09DC
    16A5: CD 20       MOV      X, #$20
    16A7: D5 00 04    MOV      $0400+X, A
    16AA: 3D          INC      X
    16AB: C8 00       MOV      X, #$00
    16AD: D0 F8       BNE      $16A7
    16AF: FD          MOV      Y, A
    16B0: 8F 00 24    MOV      $00, #$24
    16B3: 8F E8 25    MOV      $E8, #$25
    16B6: D7 24       MOV      [$24]+Y, A
    16B8: FC          INC      Y
    16B9: AD 00       CMP      Y, #$00
    16BB: D0 F9       BNE      $16B6
    16BD: AB 25       INC      $25
    16BF: 78 FF 25    CMP      $FF, #$25
    16C2: D0 F2       BNE      $16B6
    16C4: 5F C8 2F    JMP      $2FC8
    16C7: 33 18 2B    BBC      B1 $18, $16F5
    16CA: 18 3B 18    OR       $3B, #$18
    16CD: 43 18 4C    BBS      B2 $18, $171C
    16D0: 18 58 18    OR       $58, #$18
    16D3: 62 18       SET1     $18, 3
    16D5: 6A 18 72    AND1     C, /$7218
    16D8: 18 7A 18    OR       $7A, #$18
    16DB: 82 18       SET1     $18, 4
    16DD: 90 18       BCC      $16F7
    16DF: 98 18 A0    ADC      $18, #$A0
    16E2: 18 B7 18    OR       $B7, #$18
    16E5: BF          MOV      A, (X)+
    16E6: 18 CA 18    OR       $CA, #$18
    16E9: DE 18 E9    CBNE     $18+X, $16D5
    16EC: 18 F1 18    OR       $F1, #$18
    16EF: F9 18       MOV      X, $18+Y
    16F1: 03 19 37    BBS      B0 $19, $172B
    16F4: 19          OR       X, Y
    16F5: 3F 19 49    CALL     $4919
    16F8: 19          OR       X, Y
    16F9: 54 19       EOR      A, $19+X
    16FB: 67 19       CMP      A, [$19+X]
    16FD: 7F          RETI
    16FE: 19          OR       X, Y
    16FF: B4 19       SBC      A, $19+X
    1701: FB 19       MOV      Y, $19+X
    1703: 32 1A       CLR1     $1A, 1
    1705: 81          TCALL    8
    1706: 1A CB       DECW     $CB
    1708: 1A E4       DECW     $E4
    170A: 1A EE       DECW     $EE
    170C: 1A FA       DECW     $FA
    170E: 1A 34       DECW     $34
    1710: 1B 86       ASL      $86+X
    1712: 1B DA       ASL      $DA+X
    1714: 1B 7C       ASL      $7C+X
    1716: 1C          ASL      A
    1717: C6          MOV      (X), A
    1718: 1C          ASL      A
    1719: 0E 1D 53    TSET1    $531D
    171C: 1D          DEC      X
    171D: E3 1D D6    BBS      B7 $1D, $16F6
    1720: 1E 32 1F    CMP      X, $1F32
    1723: 42 1F       SET1     $1F, 2
    1725: 64 1F       CMP      A, $1F
    1727: 90 1F       BCC      $1748
    1729: DA 1F       MOVW     $1F, YA
    172B: 17 20       OR       A, [$20]+Y
    172D: 4F 20       PCALL    20
    172F: 7E 20       CMP      Y, $20
    1731: CB 20       MOV      $20, Y
    1733: 05 21 4C    OR       A, $4C21
    1736: 21          TCALL    2
    1737: 7E 21       CMP      Y, $21
    1739: AE          POP      A
    173A: 21          TCALL    2
    173B: DC          DEC      Y
    173C: 21          TCALL    2
    173D: 48 22       EOR      A, #$22
    173F: 70 22       BVS      $1763
    1741: 9E          DIV      YA, X
    1742: 22 D2       SET1     $D2, 1
    1744: 22 0F       SET1     $0F, 1
    1746: 23 9D 23    BBS      B1 $9D, $176C
    1749: EF          SLEEP
    174A: 23 5F 24    BBS      B1 $5F, $1771
    174D: C5 24 D8    MOV      $D824, A
    1750: 24 E0       AND      A, $E0
    1752: 24 01       AND      A, $01
    1754: 25 12 25    AND      A, $2512
    1757: 24 25       AND      A, $25
    1759: 56 25 8A    EOR      A, $8A25+Y
    175C: 25 93 1D    AND      A, $1D93
    175F: B8 25 CC    SBC      $25, #$CC
    1762: 25 47 1E    AND      A, $1E47
    1765: 02 1F       SET1     $1F, 0
    1767: DC          DEC      Y
    1768: 25 02 26    AND      A, $2602
    176B: 54 26       EOR      A, $26+X
    176D: 7C          ROR      A
    176E: 26          AND      A, (X)
    176F: D3 26 DF    BBC      B6 $26, $1751
    1772: 26          AND      A, (X)
    1773: 09 27 19    OR       $27, $19
    1776: 27 38       AND      A, [$38+X]
    1778: 27 75       AND      A, [$75+X]
    177A: 27 C1       AND      A, [$C1+X]
    177C: 27 FC       AND      A, [$FC+X]
    177E: 27 06       AND      A, [$06+X]
    1780: 28 D2       AND      A, #$D2
    1782: 2B 3C       ROL      $3C
    1784: 28 8E       AND      A, #$8E
    1786: 28 F1       AND      A, #$F1
    1788: 28 B6       AND      A, #$B6
    178A: 28 FF       AND      A, #$FF
    178C: 28 3E       AND      A, #$3E
    178E: 29 00 2A    AND      $00, $2A
    1791: 72 29       CLR1     $29, 3
    1793: B4 29       SBC      A, $29+X
    1795: 0D          PUSH     PSW
    1796: 2A 1F 2A    OR1      /C, $2A1F
    1799: 35 2A 4B    AND      A, $4B2A+X
    179C: 2A 61 2A    OR1      /C, $2A61
    179F: 77 2A       CMP      A, [$2A]+Y
    17A1: 8D 2A       MOV      Y, #$2A
    17A3: C9 2A DD    MOV      $DD2A, X
    17A6: 2A E8 2A    OR1      /C, $2AE8
    17A9: 26          AND      A, (X)
    17AA: 2B 50       ROL      $50
    17AC: 2B 5B       ROL      $5B
    17AE: 2B 65       ROL      $65
    17B0: 2B 70       ROL      $70
    17B2: 2B 7A       ROL      $7A
    17B4: 2B 87       ROL      $87
    17B6: 2B A1       ROL      $A1
    17B8: 2B E6       ROL      $E6
    17BA: 2B F8       ROL      $F8
    17BC: 2B 0D       ROL      $0D
    17BE: 2C 26 2C    ROL      $2C26
    17C1: 4F 2C       PCALL    2C
    17C3: 8A 14 11    EOR1     C, $1114
    17C6: 27 23       AND      A, [$23+X]
    17C8: 18 8A 14    OR       $8A, #$14
    17CB: 8A 14 8A    EOR1     C, $8A14
    17CE: 14 8A       OR       A, $8A+X
    17D0: 14 8A       OR       A, $8A+X
    17D2: 14 8A       OR       A, $8A+X
    17D4: 14 DB       OR       A, $DB+X
    17D6: 15 F6 15    OR       A, $15F6+X
    17D9: E7 15       MOV      A, [$15+X]
    17DB: 8A 14 C6    EOR1     C, $C614
    17DE: 2D          PUSH     A
    17DF: 8A 14 ED    EOR1     C, $ED14
    17E2: 2D          PUSH     A
    17E3: 0B 2E       ASL      $2E
    17E5: DA 2C       MOVW     $2C, YA
    17E7: E6          MOV      A, (X)
    17E8: 2C F2 2C    ROL      $2CF2
    17EB: FE 2C       DBNZ     Y, $1819
    17ED: 0A 2D 16    OR1      C, $162D
    17F0: 2D          PUSH     A
    17F1: 22 2D       SET1     $2D, 1
    17F3: 2E 2D 8A    CBNE     $2D, $1880
    17F6: 14 8A       OR       A, $8A+X
    17F8: 14 8A       OR       A, $8A+X
    17FA: 14 8A       OR       A, $8A+X
    17FC: 14 A2       OR       A, $A2+X
    17FE: 2C AA 2C    ROL      $2CAA
    1801: 92 2C       CLR1     $2C, 4
    1803: 9A 2C       SUBW     YA, $2C
    1805: 8D 2D       MOV      Y, #$2D
    1807: A3 2D 82    BBS      B5 $2D, $178C
    180A: 2D          PUSH     A
    180B: 98 2D 8A    ADC      $2D, #$8A
    180E: 14 08       OR       A, $08+X
    1810: 5F 91 14    JMP      $1491
    1813: 02 16       SET1     $16, 0
    1815: 14 05       OR       A, $05+X
    1817: FF          STOP
    1818: C2 A6       SET1     $A6, 6
    181A: 00          NOP
    181B: E8 D8       MOV      A, #$D8
    181D: 8D 04       MOV      Y, #$04
    181F: 5F DF 14    JMP      $14DF
    1822: 02 F9       SET1     $F9, 0
    1824: F4 18       MOV      A, $18+X
    1826: DC          DEC      Y
    1827: CA B0 00    MOV1     $00B0, C
    182A: 02 04       SET1     $04, 0
    182C: 02 08       SET1     $08, 0
    182E: C8 CA       MOV      X, #$CA
    1830: BC          INC      A
    1831: 00          NOP
    1832: 02 04       SET1     $04, 0
    1834: 02 08       SET1     $08, 0
    1836: B4 CA       SBC      A, $CA+X
    1838: C3 00 02    BBS      B6 $00, $183D
    183B: 04 02       OR       A, $02
    183D: 13 BE CA    BBC      B0 $BE, $180A
    1840: AB 00       INC      $00
    1842: 04 13       OR       A, $13
    1844: C8 CA       MOV      X, #$CA
    1846: 00          NOP
    1847: 45 93 B0    EOR      A, $B093
    184A: 00          NOP
    184B: 04 0E       OR       A, $0E
    184D: E6          MOV      A, (X)
    184E: CA 00 07    MOV1     $0700, C
    1851: 98 03 C9    ADC      $03, #$C9
    1854: 0B 98       ASL      $98
    1856: 00          NOP
    1857: 04 1C       OR       A, $1C
    1859: 01          TCALL    0
    185A: CA 00 0B    MOV1     $0B00, C
    185D: 8C 8C 8C    DEC      $8C8C
    1860: 00          NOP
    1861: 02 04       SET1     $04, 0
    1863: 02 12       SET1     $12, 0
    1865: 4B CA       LSR      $CA
    1867: BB 00       INC      $00+X
    1869: 02 16       SET1     $16, 0
    186B: 14 1C       OR       A, $1C+X
    186D: 82 CA       SET1     $CA, 4
    186F: B9          SBC      X, Y
    1870: 00          NOP
    1871: 02 19       SET1     $19, 0
    1873: 16 1C DC    OR       A, $DC1C+Y
    1876: CA 9F 00    MOV1     $009F, C
    1879: 04 11       OR       A, $11
    187B: 64 CA       CMP      A, $CA
    187D: 00          NOP
    187E: 0F          BRK
    187F: B0 00       BCS      $1881
    1881: 04 0D       OR       A, $0D
    1883: 3C          ROL      A
    1884: CA 00 43    MOV1     $4300, C
    1887: A6          SBC      A, (X)
    1888: ED          NOTC
    1889: 28 B3       AND      A, #$B3
    188B: ED          NOTC
    188C: 0A 9B 00    OR1      C, $009B
    188F: 02 1F       SET1     $1F, 0
    1891: 1C          ASL      A
    1892: 1B D2       ASL      $D2+X
    1894: CA 98 00    MOV1     $0098, C
    1897: 02 22       SET1     $22, 0
    1899: 20          CLRP
    189A: 1A FA       DECW     $FA
    189C: CA A4 00    MOV1     $00A4, C
    189F: 06          OR       A, (X)
    18A0: 08 5A       OR       A, #$5A
    18A2: CA 00 29    MOV1     $2900, C
    18A5: A4 A6       SBC      A, $A6
    18A7: A8 AA       SBC      A, #$AA
    18A9: AB AC       INC      $AC
    18AB: AD AE       CMP      Y, #$AE
    18AD: AF          MOV      (X)+, A
    18AE: B0 B1       BCS      $1861
    18B0: B2 B3       CLR1     $B3, 5
    18B2: B4 7F       SBC      A, $7F+X
    18B4: C9 00 02    MOV      $0200, X
    18B7: 14 12       OR       A, $12+X
    18B9: 1A 32       DECW     $32
    18BB: CA 98 00    MOV1     $0098, C
    18BE: 04 12       OR       A, $12
    18C0: DC          DEC      Y
    18C1: CA 00 33    MOV1     $3300, C
    18C4: B7 ED       SBC      A, [$ED]+Y
    18C6: 8C C7 00    DEC      $00C7
    18C9: 06          OR       A, (X)
    18CA: 08 5A       OR       A, #$5A
    18CC: CA 00 27    MOV1     $2700, C
    18CF: BE          DAS      A
    18D0: BD          MOV      SP, X
    18D1: BC          INC      A
    18D2: BB BA       INC      $BA+X
    18D4: B9          SBC      X, Y
    18D5: B8 B7 B6    SBC      $B7, #$B6
    18D8: B5 B4 7F    SBC      A, $7FB4+X
    18DB: C9 00 04    MOV      $0400, X
    18DE: 1D          DEC      X
    18DF: DC          DEC      Y
    18E0: CA 00 0F    MOV1     $0F00, C
    18E3: 9F          XCN      A
    18E4: 9F          XCN      A
    18E5: 9F          XCN      A
    18E6: 9F          XCN      A
    18E7: 00          NOP
    18E8: 02 16       SET1     $16, 0
    18EA: 14 1A       OR       A, $1A+X
    18EC: 32 CA       CLR1     $CA, 1
    18EE: 9A 64       SUBW     YA, $64
    18F0: 02 18       SET1     $18, 0
    18F2: 16 1A 46    OR       A, $461A+Y
    18F5: CA 8E 64    MOV1     $648E, C
    18F8: 02 0C       SET1     $0C, 0
    18FA: 09 1A 28    OR       $1A, $28
    18FD: CA A4 64    MOV1     $64A4, C
    1900: 1C          ASL      A
    1901: 19          OR       X, Y
    1902: 0A 02 5A    OR1      C, $5A02
    1905: C8 00       MOV      X, #$00
    1907: 13 B4 B7    BBC      B0 $B4, $18C1
    190A: 1F B4 0B    JMP      [$0BB4+X]
    190D: B2 13       CLR1     $13, 5
    190F: B0 1F       BCS      $1930
    1911: B2 0B       CLR1     $0B, 5
    1913: B4 1F       SBC      A, $1F+X
    1915: B7 0B       SBC      A, [$0B]+Y
    1917: B4 17       SBC      A, $17+X
    1919: B2 00       CLR1     $00, 5
    191B: 04 02       OR       A, $02
    191D: 14 CE       OR       A, $CE+X
    191F: 14 1B       OR       A, $1B+X
    1921: C9 13 B4    MOV      $B413, X
    1924: B7 1F       SBC      A, [$1F]+Y
    1926: B4 0B       SBC      A, $0B+X
    1928: B2 13       CLR1     $13, 5
    192A: B0 1F       BCS      $194B
    192C: B2 0B       CLR1     $0B, 5
    192E: B4 1F       SBC      A, $1F+X
    1930: B7 0B       SBC      A, [$0B]+Y
    1932: B4 17       SBC      A, $17+X
    1934: B2 00       CLR1     $00, 5
    1936: 02 23       SET1     $23, 0
    1938: 20          CLRP
    1939: 1A DC       DECW     $DC
    193B: CA A4 00    MOV1     $00A4, C
    193E: 04 0D       OR       A, $0D
    1940: 82 CA       SET1     $CA, 4
    1942: 00          NOP
    1943: 47 B0       EOR      A, [$B0+X]
    1945: B2 B0       CLR1     $B0, 5
    1947: 00          NOP
    1948: 04 0E       OR       A, $0E
    194A: DC          DEC      Y
    194B: CA 00 47    MOV1     $4700, C
    194E: 96 97 9C    ADC      A, $9C97+Y
    1951: 9B 00       DEC      $00+X
    1953: 06          OR       A, (X)
    1954: 18 14 CA    OR       $14, #$CA
    1957: 00          NOP
    1958: 27 C7       AND      A, [$C7+X]
    195A: ED          NOTC
    195B: 28 C6       AND      A, #$C6
    195D: ED          NOTC
    195E: 3C          ROL      A
    195F: C5 ED 64    MOV      $64ED, A
    1962: C5 7F C9    MOV      $C97F, A
    1965: 00          NOP
    1966: 06          OR       A, (X)
    1967: 18 F0 CA    OR       $F0, #$CA
    196A: 00          NOP
    196B: 27 B0       AND      A, [$B0+X]
    196D: ED          NOTC
    196E: 46          EOR      A, (X)
    196F: AB A4       INC      $A4
    1971: ED          NOTC
    1972: 32 9F       CLR1     $9F, 1
    1974: 98 ED 28    ADC      $ED, #$28
    1977: 93 90 7F    BBC      B4 $90, $19F9
    197A: C9 00 96    MOV      $9600, X
    197D: 19          OR       X, Y
    197E: 0A 08 32    OR1      C, $3208
    1981: CC 00 4B    MOV      $4B00, Y
    1984: AD ED       CMP      Y, #$ED
    1986: 46          EOR      A, (X)
    1987: B2 ED       CLR1     $ED, 5
    1989: 5A B8       CMPW     YA, $B8
    198B: ED          NOTC
    198C: 78 BC ED    CMP      $BC, #$ED
    198F: 5A BC       CMPW     YA, $BC
    1991: ED          NOTC
    1992: 28 BC       AND      A, #$BC
    1994: 00          NOP
    1995: 04 08       OR       A, $08
    1997: 14 C8       OR       A, $C8+X
    1999: 00          NOP
    199A: 07 C9       OR       A, [$C9+X]
    199C: 4B AE       LSR      $AE
    199E: ED          NOTC
    199F: 32 B3       CLR1     $B3, 1
    19A1: ED          NOTC
    19A2: 3C          ROL      A
    19A3: B9          SBC      X, Y
    19A4: ED          NOTC
    19A5: 50 BD       BVC      $1964
    19A7: ED          NOTC
    19A8: 3C          ROL      A
    19A9: BD          MOV      SP, X
    19AA: ED          NOTC
    19AB: 28 BD       AND      A, #$BD
    19AD: ED          NOTC
    19AE: 14 BD       OR       A, $BD+X
    19B0: 00          NOP
    19B1: D7 19       MOV      [$19]+Y, A
    19B3: 0C 08 0A    ASL      $0A08
    19B6: C8 00       MOV      X, #$00
    19B8: 03 C4 C3    BBS      B0 $C4, $197E
    19BB: ED          NOTC
    19BC: 14 C4       OR       A, $C4+X
    19BE: C3 ED 1E    BBS      B6 $ED, $19DF
    19C1: C4 C3       MOV      $C3, A
    19C3: ED          NOTC
    19C4: 28 C4       AND      A, #$C4
    19C6: C3 ED 32    BBS      B6 $ED, $19FB
    19C9: C4 C3       MOV      $C3, A
    19CB: ED          NOTC
    19CC: 28 C4       AND      A, #$C4
    19CE: C3 ED 1E    BBS      B6 $ED, $19EF
    19D1: C4 C3       MOV      $C3, A
    19D3: 7F          RETI
    19D4: C9 00 06    MOV      $0600, X
    19D7: 08 28       OR       A, #$28
    19D9: CC F0 4B    MOV      $4BF0, Y
    19DC: BA ED       MOVW     YA, $ED
    19DE: 32 C6       CLR1     $C6, 1
    19E0: ED          NOTC
    19E1: 28 C6       AND      A, #$C6
    19E3: ED          NOTC
    19E4: 3C          ROL      A
    19E5: C6          MOV      (X), A
    19E6: ED          NOTC
    19E7: 46          EOR      A, (X)
    19E8: C6          MOV      (X), A
    19E9: ED          NOTC
    19EA: 50 C6       BVC      $19B2
    19EC: ED          NOTC
    19ED: 3C          ROL      A
    19EE: C6          MOV      (X), A
    19EF: ED          NOTC
    19F0: 28 C6       AND      A, #$C6
    19F2: ED          NOTC
    19F3: 14 C6       OR       A, $C6+X
    19F5: 7F          RETI
    19F6: C9 00 0E    MOV      $0E00, X
    19F9: 1A 0A       DECW     $0A
    19FB: 14 BE       OR       A, $BE+X
    19FD: CA 00 01    MOV1     $0100, C
    1A00: 81          TCALL    8
    1A01: 88 ED       ADC      A, #$ED
    1A03: B4 8A       SBC      A, $8A+X
    1A05: 93 ED 96    BBC      B4 $ED, $199E
    1A08: 90 98       BCC      $19A2
    1A0A: 7F          RETI
    1A0B: C9 00 06    MOV      $0600, X
    1A0E: 0E 8C CA    TSET1    $CA8C
    1A11: 00          NOP
    1A12: 27 8E       AND      A, [$8E+X]
    1A14: 8C ED 82    DEC      $82ED
    1A17: 9B ED       DEC      $ED+X
    1A19: 78 9C 9D    CMP      $9C, #$9D
    1A1C: 8C ED 6E    DEC      $6EED
    1A1F: 93 92 ED    BBC      B4 $92, $1A0F
    1A22: 5A 90       CMPW     YA, $90
    1A24: 8F ED 46    MOV      $ED, #$46
    1A27: 8E          POP      PSW
    1A28: 8D ED       MOV      Y, #$ED
    1A2A: 28 8C       AND      A, #$8C
    1A2C: 7F          RETI
    1A2D: C9 00 5E    MOV      $5E00, X
    1A30: 1A 0A       DECW     $0A
    1A32: 14 BE       OR       A, $BE+X
    1A34: CA 00 01    MOV1     $0100, C
    1A37: C0          DI
    1A38: 93 ED B4    BBC      B4 $ED, $19EF
    1A3B: A1          TCALL    A
    1A3C: A3 A4 ED    BBS      B5 $A4, $1A2C
    1A3F: A0          EI
    1A40: 93 90 8C    BBC      B4 $90, $19CF
    1A43: ED          NOTC
    1A44: 8C 98 A3    DEC      $A398
    1A47: A1          TCALL    A
    1A48: ED          NOTC
    1A49: 78 91 8E    CMP      $91, #$8E
    1A4C: 8C ED 64    DEC      $64ED
    1A4F: 98 97 ED    ADC      $97, #$ED
    1A52: 50 93       BVC      $19E7
    1A54: ED          NOTC
    1A55: 3C          ROL      A
    1A56: 90 ED       BCC      $1A45
    1A58: 32 8C       CLR1     $8C, 1
    1A5A: 98 A4 00    ADC      $A4, #$00
    1A5D: 06          OR       A, (X)
    1A5E: 0E 96 CA    TSET1    $CA96
    1A61: 00          NOP
    1A62: 27 98       AND      A, [$98+X]
    1A64: 97 98       ADC      A, [$98]+Y
    1A66: 9A 9C       SUBW     YA, $9C
    1A68: 9D          MOV      X, SP
    1A69: 93 91 8F    BBC      B4 $91, $19FB
    1A6C: 8D 8B       MOV      Y, #$8B
    1A6E: ED          NOTC
    1A6F: A0          EI
    1A70: 91          TCALL    9
    1A71: 90 8F       BCC      $1A02
    1A73: 8E          POP      PSW
    1A74: 8D ED       MOV      Y, #$ED
    1A76: 8C 8C 8B    DEC      $8B8C
    1A79: 89 87 7F    ADC      $87, $7F
    1A7C: C9 00 A9    MOV      $A900, X
    1A7F: 1A 0C       DECW     $0C
    1A81: 0E 82 CC    TSET1    $CC82
    1A84: 00          NOP
    1A85: 29 98 9C    AND      $98, $9C
    1A88: 9F          XCN      A
    1A89: A1          TCALL    A
    1A8A: 9F          XCN      A
    1A8B: 9D          MOV      X, SP
    1A8C: 9C          DEC      A
    1A8D: 9A ED       SUBW     YA, $ED
    1A8F: 64 97       CMP      A, $97
    1A91: 95 93 91    ADC      A, $9193+X
    1A94: ED          NOTC
    1A95: 50 90       BVC      $1A27
    1A97: 8E          POP      PSW
    1A98: 8C ED 3C    DEC      $3CED
    1A9B: 8B 89       DEC      $89
    1A9D: ED          NOTC
    1A9E: 28 87       AND      A, #$87
    1AA0: 85 ED 1E    ADC      A, $1EED
    1AA3: 84 82       ADC      A, $82
    1AA5: 7F          RETI
    1AA6: C9 00 06    MOV      $0600, X
    1AA9: 0E 8C CA    TSET1    $CA8C
    1AAC: 00          NOP
    1AAD: 27 8C       AND      A, [$8C+X]
    1AAF: 90 93       BCC      $1A44
    1AB1: 95 93 91    ADC      A, $9193+X
    1AB4: 90 8E       BCC      $1A44
    1AB6: ED          NOTC
    1AB7: 50 8B       BVC      $1A44
    1AB9: 89 87 85    ADC      $87, $85
    1ABC: 84 82       ADC      A, $82
    1ABE: 8C 8B 89    DEC      $898B
    1AC1: ED          NOTC
    1AC2: 32 87       CLR1     $87, 1
    1AC4: 85 84 82    ADC      A, $8284
    1AC7: 7F          RETI
    1AC8: C9 00 06    MOV      $0600, X
    1ACB: 18 32 CA    OR       $32, #$CA
    1ACE: 00          NOP
    1ACF: 4B 8E       LSR      $8E
    1AD1: 91          TCALL    9
    1AD2: ED          NOTC
    1AD3: 3C          ROL      A
    1AD4: 9F          XCN      A
    1AD5: A1          TCALL    A
    1AD6: ED          NOTC
    1AD7: 46          EOR      A, (X)
    1AD8: A3 A4 ED    BBS      B5 $A4, $1AC8
    1ADB: 3C          ROL      A
    1ADC: A5 ED 14    SBC      A, $14ED
    1ADF: A6          SBC      A, (X)
    1AE0: 7F          RETI
    1AE1: C9 00 04    MOV      $0400, X
    1AE4: 08 E6       OR       A, #$E6
    1AE6: CA 00 05    MOV1     $0500, C
    1AE9: BF          MOV      A, (X)+
    1AEA: 49 C5 00    EOR      $C5, $00
    1AED: 04 08       OR       A, $08
    1AEF: E6          MOV      A, (X)
    1AF0: CA 00 47    MOV1     $4700, C
    1AF3: BE          DAS      A
    1AF4: C2 BE       SET1     $BE, 6
    1AF6: 00          NOP
    1AF7: 15 1B 0A    OR       A, $0A1B+X
    1AFA: 08 DC       OR       A, #$DC
    1AFC: CA 00 4B    MOV1     $4B00, C
    1AFF: B0 B7       BCS      $1AB8
    1B01: ED          NOTC
    1B02: 96 B9 BB    ADC      A, $BBB9+Y
    1B05: ED          NOTC
    1B06: 5A B0       CMPW     YA, $B0
    1B08: B7 B9       SBC      A, [$B9]+Y
    1B0A: BB ED       INC      $ED+X
    1B0C: 46          EOR      A, (X)
    1B0D: B0 B7       BCS      $1AC6
    1B0F: ED          NOTC
    1B10: 1E B9 BB    CMP      X, $BBB9
    1B13: 00          NOP
    1B14: 04 08       OR       A, $08
    1B16: 1E D2 8C    CMP      X, $8CD2
    1B19: 23 C9 4B    BBS      B1 $C9, $1B67
    1B1C: B0 B7       BCS      $1AD5
    1B1E: ED          NOTC
    1B1F: 3C          ROL      A
    1B20: B9          SBC      X, Y
    1B21: BB ED       INC      $ED+X
    1B23: 28 B0       AND      A, #$B0
    1B25: B7 ED       SBC      A, [$ED]+Y
    1B27: 1E B9 BB    CMP      X, $BBB9
    1B2A: ED          NOTC
    1B2B: 0A B0 B7    OR1      C, $B7B0
    1B2E: B9          SBC      X, Y
    1B2F: BB 00       INC      $00+X
    1B31: 57 1B       EOR      A, [$1B]+Y
    1B33: 0A 08 50    OR1      C, $5008
    1B36: CB 00       MOV      $00, Y
    1B38: 03 B2 B7    BBS      B0 $B2, $1AF2
    1B3B: ED          NOTC
    1B3C: B4 BC       SBC      A, $BC+X
    1B3E: BE          DAS      A
    1B3F: ED          NOTC
    1B40: 46          EOR      A, (X)
    1B41: B9          SBC      X, Y
    1B42: C1          TCALL    C
    1B43: C3 C5 ED    BBS      B6 $C5, $1B33
    1B46: 1E BA C2    CMP      X, $C2BA
    1B49: C4 C6       MOV      $C6, A
    1B4B: ED          NOTC
    1B4C: 14 BB       OR       A, $BB+X
    1B4E: C3 C5 C7    BBS      B6 $C5, $1B18
    1B51: B4 B9       SBC      A, $B9+X
    1B53: BE          DAS      A
    1B54: C0          DI
    1B55: 00          NOP
    1B56: 04 08       OR       A, $08
    1B58: 1E C3 A0    CMP      X, $A0C3
    1B5B: 23 C9 03    BBS      B1 $C9, $1B61
    1B5E: B2 B7       CLR1     $B7, 5
    1B60: ED          NOTC
    1B61: 46          EOR      A, (X)
    1B62: BC          INC      A
    1B63: BE          DAS      A
    1B64: E1          TCALL    E
    1B65: 11          TCALL    1
    1B66: ED          NOTC
    1B67: 1E B5 BD    CMP      X, $BDB5
    1B6A: BF          MOV      A, (X)+
    1B6B: C1          TCALL    C
    1B6C: E1          TCALL    E
    1B6D: 03 ED 14    BBS      B0 $ED, $1B84
    1B70: B6 BE C0    SBC      A, $C0BE+Y
    1B73: C2 E1       SET1     $E1, 6
    1B75: 11          TCALL    1
    1B76: ED          NOTC
    1B77: 0A B7 BF    OR1      C, $BFB7
    1B7A: C1          TCALL    C
    1B7B: C3 E1 03    BBS      B6 $E1, $1B81
    1B7E: B8 C0 C2    SBC      $C0, #$C2
    1B81: C4 00       MOV      $00, A
    1B83: AA 1B 0A    MOV1     C, $0A1B
    1B86: 11          TCALL    1
    1B87: 64 CC       CMP      A, $CC
    1B89: 00          NOP
    1B8A: 07 A4       OR       A, [$A4+X]
    1B8C: ED          NOTC
    1B8D: 46          EOR      A, (X)
    1B8E: 45 B0 B2    EOR      A, $B2B0
    1B91: ED          NOTC
    1B92: 5A B4       CMPW     YA, $B4
    1B94: B5 ED 78    SBC      A, $78ED+X
    1B97: B7 B9       SBC      A, [$B9]+Y
    1B99: ED          NOTC
    1B9A: 8C BB C7    DEC      $C7BB
    1B9D: ED          NOTC
    1B9E: 46          EOR      A, (X)
    1B9F: 0B A4       ASL      $A4
    1BA1: A8 AB       SBC      A, #$AB
    1BA3: AF          MOV      (X)+, A
    1BA4: B0 B4       BCS      $1B5A
    1BA6: B7 BB       SBC      A, [$BB]+Y
    1BA8: 00          NOP
    1BA9: 04 11       OR       A, $11
    1BAB: 3C          ROL      A
    1BAC: C8 64       MOV      X, #$64
    1BAE: 1B C9       ASL      $C9+X
    1BB0: 07 A4       OR       A, [$A4+X]
    1BB2: 45 A4 A6    EOR      A, $A6A4
    1BB5: A8 A9       SBC      A, #$A9
    1BB7: AB AD       INC      $AD
    1BB9: AF          MOV      (X)+, A
    1BBA: BB ED       INC      $ED+X
    1BBC: 1E E1 12    CMP      X, $12E1
    1BBF: 0B A4       ASL      $A4
    1BC1: E1          TCALL    E
    1BC2: 02 A8       SET1     $A8, 0
    1BC4: E1          TCALL    E
    1BC5: 12 AB       CLR1     $AB, 0
    1BC7: E1          TCALL    E
    1BC8: 02 AF       SET1     $AF, 0
    1BCA: E1          TCALL    E
    1BCB: 12 B0       CLR1     $B0, 0
    1BCD: E1          TCALL    E
    1BCE: 02 B4       SET1     $B4, 0
    1BD0: E1          TCALL    E
    1BD1: 12 B7       CLR1     $B7, 0
    1BD3: E1          TCALL    E
    1BD4: 02 BB       SET1     $BB, 0
    1BD6: 00          NOP
    1BD7: 34 1C       AND      A, $1C+X
    1BD9: 0A 0F 5A    OR1      C, $5A0F
    1BDC: CF          MUL      YA
    1BDD: 00          NOP
    1BDE: 0B 98       ASL      $98
    1BE0: ED          NOTC
    1BE1: 28 03       AND      A, #$03
    1BE3: BC          INC      A
    1BE4: BB E1       INC      $E1+X
    1BE6: 05 ED 3C    OR       A, $3CED
    1BE9: C5 C3 E1    MOV      $E1C3, A
    1BEC: 0F          BRK
    1BED: ED          NOTC
    1BEE: 5A C1       CMPW     YA, $C1
    1BF0: C0          DI
    1BF1: BE          DAS      A
    1BF2: ED          NOTC
    1BF3: 78 E1 05    CMP      $E1, #$05
    1BF6: B0 BB       BCS      $1BB3
    1BF8: ED          NOTC
    1BF9: 96 E1 0F    ADC      A, $0FE1+Y
    1BFC: B9          SBC      X, Y
    1BFD: B7 E1       SBC      A, [$E1]+Y
    1BFF: 05 ED 50    OR       A, $50ED
    1C02: B5 B4 B2    SBC      A, $B2B4+X
    1C05: E1          TCALL    E
    1C06: 0A ED 96    OR1      C, $96ED
    1C09: E0          CLRV
    1C0A: 08 43       OR       A, #$43
    1C0C: B0 B4       BCS      $1BC2
    1C0E: ED          NOTC
    1C0F: 5A C3       CMPW     YA, $C3
    1C11: C7 BC       MOV      [$BC+X], A
    1C13: C0          DI
    1C14: C3 C7 ED    BBS      B6 $C7, $1C04
    1C17: 50 B0       BVC      $1BC9
    1C19: B4 B7       SBC      A, $B7+X
    1C1B: BB ED       INC      $ED+X
    1C1D: 32 BC       CLR1     $BC, 1
    1C1F: C0          DI
    1C20: C3 C7 ED    BBS      B6 $C7, $1C10
    1C23: 1E B0 B4    CMP      X, $B4B0
    1C26: B7 BB       SBC      A, [$BB]+Y
    1C28: ED          NOTC
    1C29: 14 BC       OR       A, $BC+X
    1C2B: C0          DI
    1C2C: C3 C7 BC    BBS      B6 $C7, $1BEB
    1C2F: C0          DI
    1C30: C3 C7 00    BBS      B6 $C7, $1C33
    1C33: 04 0F       OR       A, $0F
    1C35: 1E CA 64    CMP      X, $64CA
    1C38: 0B 9F       ASL      $9F
    1C3A: 1B C9       ASL      $C9+X
    1C3C: 03 BC BB    BBS      B0 $BC, $1BFA
    1C3F: B9          SBC      X, Y
    1C40: B7 B5       SBC      A, [$B5]+Y
    1C42: B4 A6       SBC      A, $A6+X
    1C44: B0 BB       BCS      $1C01
    1C46: B9          SBC      X, Y
    1C47: C3 C1 C0    BBS      B6 $C1, $1C0A
    1C4A: BE          DAS      A
    1C4B: ED          NOTC
    1C4C: 1E E1 02    CMP      X, $02E1
    1C4F: E0          CLRV
    1C50: 08 43       OR       A, #$43
    1C52: BC          INC      A
    1C53: C0          DI
    1C54: C3 C7 E1    BBS      B6 $C7, $1C38
    1C57: 12 B0       CLR1     $B0, 0
    1C59: B4 B7       SBC      A, $B7+X
    1C5B: BB E1       INC      $E1+X
    1C5D: 02 BC       SET1     $BC, 0
    1C5F: C0          DI
    1C60: C3 C7 E1    BBS      B6 $C7, $1C44
    1C63: 12 B0       CLR1     $B0, 0
    1C65: B4 B7       SBC      A, $B7+X
    1C67: BB E1       INC      $E1+X
    1C69: 02 BC       SET1     $BC, 0
    1C6B: C0          DI
    1C6C: C3 C7 E1    BBS      B6 $C7, $1C50
    1C6F: 12 B0       CLR1     $B0, 0
    1C71: B4 B7       SBC      A, $B7+X
    1C73: BB BC       INC      $BC+X
    1C75: C0          DI
    1C76: C3 C7 00    BBS      B6 $C7, $1C79
    1C79: 9F          XCN      A
    1C7A: 1C          ASL      A
    1C7B: 0A 0B E6    OR1      C, $E60B
    1C7E: CC 00 29    MOV      $2900, Y
    1C81: 85 87 8B    ADC      A, $8B87
    1C84: 8C ED 78    DEC      $78ED
    1C87: 93 95 99    BBC      B4 $95, $1C23
    1C8A: 9A ED       SUBW     YA, $ED
    1C8C: 5A A1       CMPW     YA, $A1
    1C8E: A3 A7 A8    BBS      B5 $A7, $1C39
    1C91: ED          NOTC
    1C92: 50 A3       BVC      $1C37
    1C94: A5 A9 AA    SBC      A, $AAA9
    1C97: ED          NOTC
    1C98: 28 B1       AND      A, #$B1
    1C9A: B3 B7 B8    BBC      B5 $B7, $1C55
    1C9D: 00          NOP
    1C9E: 04 0B       OR       A, $0B
    1CA0: 5A C8       CMPW     YA, $C8
    1CA2: 00          NOP
    1CA3: 29 C9 C9    AND      $C9, $C9
    1CA6: 85 87 8B    ADC      A, $8B87
    1CA9: 8C ED 3C    DEC      $3CED
    1CAC: 93 95 99    BBC      B4 $95, $1C48
    1CAF: 9A ED       SUBW     YA, $ED
    1CB1: 28 A1       AND      A, #$A1
    1CB3: A3 A7 A8    BBS      B5 $A7, $1C5E
    1CB6: ED          NOTC
    1CB7: 1E A3 A5    CMP      X, $A5A3
    1CBA: A9 AA ED    SBC      $AA, $ED
    1CBD: 14 B1       OR       A, $B1+X
    1CBF: B3 B7 B8    BBC      B5 $B7, $1C7A
    1CC2: 00          NOP
    1CC3: E8 1C       MOV      A, #$1C
    1CC5: 0A 0B 78    OR1      C, $780B
    1CC8: C8 00       MOV      X, #$00
    1CCA: 29 B5 BA    AND      $B5, $BA
    1CCD: B5 B0 ED    SBC      A, $EDB0+X
    1CD0: B4 A7       SBC      A, $A7+X
    1CD2: AC A7 A2    INC      $A2A7
    1CD5: ED          NOTC
    1CD6: 50 99       BVC      $1C71
    1CD8: 9E          DIV      YA, X
    1CD9: 99          ADC      X, Y
    1CDA: 94 ED       ADC      A, $ED+X
    1CDC: 46          EOR      A, (X)
    1CDD: 97 90       ADC      A, [$90]+Y
    1CDF: 97 86       ADC      A, [$86]+Y
    1CE1: ED          NOTC
    1CE2: 32 8B       CLR1     $8B, 1
    1CE4: 84 82       ADC      A, $82
    1CE6: 00          NOP
    1CE7: 04 0B       OR       A, $0B
    1CE9: 5A CE       CMPW     YA, $CE
    1CEB: 64 29       CMP      A, $29
    1CED: C9 C9 B5    MOV      $B5C9, X
    1CF0: BA B5       MOVW     YA, $B5
    1CF2: B0 ED       BCS      $1CE1
    1CF4: 3C          ROL      A
    1CF5: A7 AC       SBC      A, [$AC+X]
    1CF7: A7 A2       SBC      A, [$A2+X]
    1CF9: ED          NOTC
    1CFA: 28 99       AND      A, #$99
    1CFC: 9E          DIV      YA, X
    1CFD: 99          ADC      X, Y
    1CFE: 94 ED       ADC      A, $ED+X
    1D00: 1E 97 90    CMP      X, $9097
    1D03: 97 86       ADC      A, [$86]+Y
    1D05: ED          NOTC
    1D06: 14 8B       OR       A, $8B+X
    1D08: 84 82       ADC      A, $82
    1D0A: 00          NOP
    1D0B: 36 1D 0A    AND      A, $0A1D+Y
    1D0E: 0F          BRK
    1D0F: 50 CD       BVC      $1CDE
    1D11: 00          NOP
    1D12: 4B A4       LSR      $A4
    1D14: ED          NOTC
    1D15: 96 E1 07    ADC      A, $07E1+Y
    1D18: B0 ED       BCS      $1D07
    1D1A: 64 E1       CMP      A, $E1
    1D1C: 03 B4 ED    BBS      B0 $B4, $1D0C
    1D1F: 8C E1 0A    DEC      $0AE1
    1D22: B8 ED 1E    SBC      $ED, #$1E
    1D25: E1          TCALL    E
    1D26: 0F          BRK
    1D27: B0 E1       BCS      $1D0A
    1D29: 07 B4       OR       A, [$B4+X]
    1D2B: E1          TCALL    E
    1D2C: 0A B8 B0    OR1      C, $B0B8
    1D2F: B4 B8       SBC      A, $B8+X
    1D31: B0 B4       BCS      $1CE7
    1D33: B8 00 04    SBC      $00, #$04
    1D36: 0F          BRK
    1D37: 28 C8       AND      A, #$C8
    1D39: 82 1B       SET1     $1B, 4
    1D3B: C9 4B B0    MOV      $B04B, X
    1D3E: B4 B8       SBC      A, $B8+X
    1D40: E1          TCALL    E
    1D41: 0C ED 0A    ASL      $0AED
    1D44: B0 B4       BCS      $1CFA
    1D46: B8 E1 08    SBC      $E1, #$08
    1D49: B0 B4       BCS      $1CFF
    1D4B: B8 B0 B4    SBC      $B0, #$B4
    1D4E: B8 00 70    SBC      $00, #$70
    1D51: 1D          DEC      X
    1D52: 0A 0E 3C    OR1      C, $3C0E
    1D55: CC 00 13    MOV      $1300, Y
    1D58: C4 0F       MOV      $0F, A
    1D5A: C3 E0 0B    BBS      B6 $E0, $1D68
    1D5D: ED          NOTC
    1D5E: A0          EI
    1D5F: 07 9A       OR       A, [$9A+X]
    1D61: A6          SBC      A, (X)
    1D62: B2 BE       CLR1     $BE, 5
    1D64: ED          NOTC
    1D65: 3C          ROL      A
    1D66: 9A A6       SUBW     YA, $A6
    1D68: B2 BE       CLR1     $BE, 5
    1D6A: 9A A6       SUBW     YA, $A6
    1D6C: B2 BE       CLR1     $BE, 5
    1D6E: 00          NOP
    1D6F: 04 18       OR       A, $18
    1D71: 1E C8 64    CMP      X, $64C8
    1D74: 13 A3 0F    BBC      B0 $A3, $1D86
    1D77: A3 E0 0B    BBS      B5 $E0, $1D85
    1D7A: ED          NOTC
    1D7B: 5A 1B       CMPW     YA, $1B
    1D7D: C9 07 9A    MOV      $9A07, X
    1D80: A6          SBC      A, (X)
    1D81: ED          NOTC
    1D82: 1E B2 BE    CMP      X, $BEB2
    1D85: ED          NOTC
    1D86: 0A 9A A6    OR1      C, $A69A
    1D89: B2 BE       CLR1     $BE, 5
    1D8B: 9A A6       SUBW     YA, $A6
    1D8D: B2 BE       CLR1     $BE, 5
    1D8F: 00          NOP
    1D90: BA 1D       MOVW     YA, $1D
    1D92: 0A 0E 3C    OR1      C, $3C0E
    1D95: CC 00 13    MOV      $1300, Y
    1D98: C7 ED       MOV      [$ED+X], A
    1D9A: 1E 0F C7    CMP      X, $C70F
    1D9D: E0          CLRV
    1D9E: 0B ED       ASL      $ED
    1DA0: A0          EI
    1DA1: 07 9A       OR       A, [$9A+X]
    1DA3: A6          SBC      A, (X)
    1DA4: B2 BE       CLR1     $BE, 5
    1DA6: ED          NOTC
    1DA7: 28 9A       AND      A, #$9A
    1DA9: A6          SBC      A, (X)
    1DAA: B2 BE       CLR1     $BE, 5
    1DAC: ED          NOTC
    1DAD: 1E 9A A6    CMP      X, $A69A
    1DB0: B2 BE       CLR1     $BE, 5
    1DB2: ED          NOTC
    1DB3: 14 9A       OR       A, $9A+X
    1DB5: A6          SBC      A, (X)
    1DB6: B2 BE       CLR1     $BE, 5
    1DB8: 00          NOP
    1DB9: 04 18       OR       A, $18
    1DBB: 1E C8 64    CMP      X, $64C8
    1DBE: 13 C4 0F    BBC      B0 $C4, $1DD0
    1DC1: C4 E0       MOV      $E0, A
    1DC3: 0B ED       ASL      $ED
    1DC5: 5A 1B       CMPW     YA, $1B
    1DC7: C9 07 9A    MOV      $9A07, X
    1DCA: A6          SBC      A, (X)
    1DCB: ED          NOTC
    1DCC: 1E B2 BE    CMP      X, $BEB2
    1DCF: ED          NOTC
    1DD0: 14 9A       OR       A, $9A+X
    1DD2: A6          SBC      A, (X)
    1DD3: B2 BE       CLR1     $BE, 5
    1DD5: ED          NOTC
    1DD6: 0A 9A A6    OR1      C, $A69A
    1DD9: B2 BE       CLR1     $BE, 5
    1DDB: 9A A6       SUBW     YA, $A6
    1DDD: B2 BE       CLR1     $BE, 5
    1DDF: 00          NOP
    1DE0: 12 1E       CLR1     $1E, 0
    1DE2: 0A 08 A0    OR1      C, $A008
    1DE5: C8 00       MOV      X, #$00
    1DE7: 45 B3 B6    EOR      A, $B6B3
    1DEA: B5 B9 BD    SBC      A, $BDB9+X
    1DED: BF          MOV      A, (X)+
    1DEE: C2 BF       SET1     $BF, 6
    1DF0: BD          MOV      SP, X
    1DF1: B9          SBC      X, Y
    1DF2: B2 B4       CLR1     $B4, 5
    1DF4: B5 B9 BC    SBC      A, $BCB9+X
    1DF7: BD          MOV      SP, X
    1DF8: BE          DAS      A
    1DF9: BD          MOV      SP, X
    1DFA: BC          INC      A
    1DFB: B8 B7 B6    SBC      $B7, #$B6
    1DFE: E0          CLRV
    1DFF: 19          OR       X, Y
    1E00: 07 C9       OR       A, [$C9+X]
    1E02: C9 93 9F    MOV      $9F93, X
    1E05: A4 ED       SBC      A, $ED
    1E07: 46          EOR      A, (X)
    1E08: 93 9F A4    BBC      B4 $9F, $1DAF
    1E0B: ED          NOTC
    1E0C: 1E 93 9F    CMP      X, $9F93
    1E0F: A4 00       SBC      A, $00
    1E11: 04 08       OR       A, $08
    1E13: 3C          ROL      A
    1E14: CE          POP      X
    1E15: 1E 1B C9    CMP      X, $C91B
    1E18: 45 B3 B6    EOR      A, $B6B3
    1E1B: B5 B9 BD    SBC      A, $BDB9+X
    1E1E: BF          MOV      A, (X)+
    1E1F: C2 BF       SET1     $BF, 6
    1E21: BD          MOV      SP, X
    1E22: B9          SBC      X, Y
    1E23: B2 B4       CLR1     $B4, 5
    1E25: B5 B9 BC    SBC      A, $BCB9+X
    1E28: BD          MOV      SP, X
    1E29: BE          DAS      A
    1E2A: BD          MOV      SP, X
    1E2B: BC          INC      A
    1E2C: B8 B7 B6    SBC      $B7, #$B6
    1E2F: ED          NOTC
    1E30: 46          EOR      A, (X)
    1E31: E0          CLRV
    1E32: 19          OR       X, Y
    1E33: 07 C9       OR       A, [$C9+X]
    1E35: C9 93 9F    MOV      $9F93, X
    1E38: A4 ED       SBC      A, $ED
    1E3A: 1E 93 9F    CMP      X, $9F93
    1E3D: A4 ED       SBC      A, $ED
    1E3F: 14 93       OR       A, $93+X
    1E41: 9F          XCN      A
    1E42: A4 00       SBC      A, $00
    1E44: 8C 1E 0A    DEC      $0A1E
    1E47: 13 C8 C5    BBC      B0 $C8, $1E0F
    1E4A: 00          NOP
    1E4B: 45 B3 B6    EOR      A, $B6B3
    1E4E: B5 B9 BD    SBC      A, $BDB9+X
    1E51: BF          MOV      A, (X)+
    1E52: C2 BF       SET1     $BF, 6
    1E54: BD          MOV      SP, X
    1E55: B9          SBC      X, Y
    1E56: B2 B4       CLR1     $B4, 5
    1E58: B5 B9 BC    SBC      A, $BCB9+X
    1E5B: BD          MOV      SP, X
    1E5C: BE          DAS      A
    1E5D: BD          MOV      SP, X
    1E5E: BC          INC      A
    1E5F: B8 B7 B6    SBC      $B7, #$B6
    1E62: ED          NOTC
    1E63: 3C          ROL      A
    1E64: 45 B3 B6    EOR      A, $B6B3
    1E67: B5 B9 BD    SBC      A, $BDB9+X
    1E6A: BF          MOV      A, (X)+
    1E6B: C2 BF       SET1     $BF, 6
    1E6D: BD          MOV      SP, X
    1E6E: B9          SBC      X, Y
    1E6F: B2 B4       CLR1     $B4, 5
    1E71: B5 B9 BC    SBC      A, $BCB9+X
    1E74: BD          MOV      SP, X
    1E75: ED          NOTC
    1E76: 1E BE BD    CMP      X, $BDBE
    1E79: BC          INC      A
    1E7A: E0          CLRV
    1E7B: 19          OR       X, Y
    1E7C: 07 93       OR       A, [$93+X]
    1E7E: 9F          XCN      A
    1E7F: A4 ED       SBC      A, $ED
    1E81: 46          EOR      A, (X)
    1E82: 93 9F A4    BBC      B4 $9F, $1E29
    1E85: ED          NOTC
    1E86: 1E 93 9F    CMP      X, $9F93
    1E89: A4 00       SBC      A, $00
    1E8B: 04 13       OR       A, $13
    1E8D: 5A CF       CMPW     YA, $CF
    1E8F: 64 1B       CMP      A, $1B
    1E91: C9 45 B3    MOV      $B345, X
    1E94: B6 B5 B9    SBC      A, $B9B5+Y
    1E97: BD          MOV      SP, X
    1E98: BF          MOV      A, (X)+
    1E99: C2 BF       SET1     $BF, 6
    1E9B: BD          MOV      SP, X
    1E9C: B9          SBC      X, Y
    1E9D: B2 B4       CLR1     $B4, 5
    1E9F: B5 B9 BC    SBC      A, $BCB9+X
    1EA2: BD          MOV      SP, X
    1EA3: BE          DAS      A
    1EA4: BD          MOV      SP, X
    1EA5: BC          INC      A
    1EA6: B8 B7 B6    SBC      $B7, #$B6
    1EA9: ED          NOTC
    1EAA: 28 45       AND      A, #$45
    1EAC: B3 B6 B5    BBC      B5 $B6, $1E64
    1EAF: B9          SBC      X, Y
    1EB0: BD          MOV      SP, X
    1EB1: BF          MOV      A, (X)+
    1EB2: C2 BF       SET1     $BF, 6
    1EB4: BD          MOV      SP, X
    1EB5: B9          SBC      X, Y
    1EB6: B2 B4       CLR1     $B4, 5
    1EB8: B5 B9 BC    SBC      A, $BCB9+X
    1EBB: BD          MOV      SP, X
    1EBC: ED          NOTC
    1EBD: 14 BE       OR       A, $BE+X
    1EBF: BD          MOV      SP, X
    1EC0: BC          INC      A
    1EC1: E0          CLRV
    1EC2: 19          OR       X, Y
    1EC3: 07 93       OR       A, [$93+X]
    1EC5: 9F          XCN      A
    1EC6: A4 ED       SBC      A, $ED
    1EC8: 46          EOR      A, (X)
    1EC9: 93 9F A4    BBC      B4 $9F, $1E70
    1ECC: ED          NOTC
    1ECD: 1E 93 9F    CMP      X, $9F93
    1ED0: A4 00       SBC      A, $00
    1ED2: 00          NOP
    1ED3: E8 1E       MOV      A, #$1E
    1ED5: 0A 19 28    OR1      C, $2819
    1ED8: C8 00       MOV      X, #$00
    1EDA: 0F          BRK
    1EDB: BB B4       INC      $B4+X
    1EDD: BB B5       INC      $B5+X
    1EDF: E0          CLRV
    1EE0: 02 1B       SET1     $1B, 0
    1EE2: ED          NOTC
    1EE3: 8C B1 B3    DEC      $B3B1
    1EE6: 00          NOP
    1EE7: 04 19       OR       A, $19
    1EE9: 1E CC 00    CMP      X, $00CC
    1EEC: 1B C9       ASL      $C9+X
    1EEE: 0F          BRK
    1EEF: BB B4       INC      $B4+X
    1EF1: BB B5       INC      $B5+X
    1EF3: E0          CLRV
    1EF4: 02 1B       SET1     $1B, 0
    1EF6: ED          NOTC
    1EF7: 14 E1       OR       A, $E1+X
    1EF9: 03 B1 E1    BBS      B0 $B1, $1EDD
    1EFC: 11          TCALL    1
    1EFD: B3 00 17    BBC      B5 $00, $1F17
    1F00: 1F 0A 19    JMP      [$190A+X]
    1F03: 1E C8 00    CMP      X, $00C8
    1F06: 1F B0 ED    JMP      [$EDB0+X]
    1F09: 8C B4 9D    DEC      $9DB4
    1F0C: B1          TCALL    B
    1F0D: A5 E0 02    SBC      A, $02E0
    1F10: 1B ED       ASL      $ED+X
    1F12: AA B1 B3    MOV1     C, $B3B1
    1F15: 00          NOP
    1F16: 04 19       OR       A, $19
    1F18: 32 CC       CLR1     $CC, 1
    1F1A: 00          NOP
    1F1B: 1B C9       ASL      $C9+X
    1F1D: 1F 98 ED    JMP      [$ED98+X]
    1F20: 46          EOR      A, (X)
    1F21: B4 AB       SBC      A, $AB+X
    1F23: A5 B7 E0    SBC      A, $E0B7
    1F26: 02 1B       SET1     $1B, 0
    1F28: ED          NOTC
    1F29: 1E E1 03    CMP      X, $03E1
    1F2C: B1          TCALL    B
    1F2D: E1          TCALL    E
    1F2E: 11          TCALL    1
    1F2F: B3 00 06    BBC      B5 $00, $1F38
    1F32: 0D          PUSH     PSW
    1F33: 78 CA 00    CMP      $CA, #$00
    1F36: 27 90       AND      A, [$90+X]
    1F38: 8E          POP      PSW
    1F39: 8C ED 64    DEC      $64ED
    1F3C: 8A 88 7F    EOR1     C, $7F88
    1F3F: C9 00 06    MOV      $0600, X
    1F42: 0D          PUSH     PSW
    1F43: B4 CA       SBC      A, $CA+X
    1F45: 00          NOP
    1F46: 27 96       AND      A, [$96+X]
    1F48: 94 92       ADC      A, $92+X
    1F4A: 90 93       BCC      $1EDF
    1F4C: 91          TCALL    9
    1F4D: 8F 8D 8B    MOV      $8D, #$8B
    1F50: 89 ED A0    ADC      $ED, $A0
    1F53: 8B 89       DEC      $89
    1F55: 87 86       ADC      A, [$86+X]
    1F57: ED          NOTC
    1F58: 8C 88 84    DEC      $8488
    1F5B: 83 82 81    BBS      B4 $82, $1EDF
    1F5E: 7F          RETI
    1F5F: C9 00 7B    MOV      $7B00, X
    1F62: 1F 0A 19    JMP      [$190A+X]
    1F65: B4 CC       SBC      A, $CC+X
    1F67: 00          NOP
    1F68: 01          TCALL    0
    1F69: 9C          DEC      A
    1F6A: ED          NOTC
    1F6B: 32 AE       CLR1     $AE, 1
    1F6D: ED          NOTC
    1F6E: 46          EOR      A, (X)
    1F6F: B0 B2       BCS      $1F23
    1F71: ED          NOTC
    1F72: 32 B4       CLR1     $B4, 1
    1F74: B5 ED 14    SBC      A, $14ED+X
    1F77: C3 C5 00    BBS      B6 $C5, $1F7A
    1F7A: 04 19       OR       A, $19
    1F7C: 5A C8       CMPW     YA, $C8
    1F7E: 00          NOP
    1F7F: 4B C9       LSR      $C9
    1F81: 01          TCALL    0
    1F82: 9C          DEC      A
    1F83: ED          NOTC
    1F84: 0A AE B0    OR1      C, $B0AE
    1F87: B2 B4       CLR1     $B4, 5
    1F89: B5 C3 C5    SBC      A, $C5C3+X
    1F8C: 00          NOP
    1F8D: B9          SBC      X, Y
    1F8E: 1F 0A 0D    JMP      [$0D0A+X]
    1F91: 14 CC       OR       A, $CC+X
    1F93: 00          NOP
    1F94: 27 C3       AND      A, [$C3+X]
    1F96: C0          DI
    1F97: ED          NOTC
    1F98: 28 BF       AND      A, #$BF
    1F9A: BD          MOV      SP, X
    1F9B: ED          NOTC
    1F9C: 3C          ROL      A
    1F9D: BC          INC      A
    1F9E: B9          SBC      X, Y
    1F9F: ED          NOTC
    1FA0: 5A B7       CMPW     YA, $B7
    1FA2: B6 ED 50    SBC      A, $50ED+Y
    1FA5: C3 C0 ED    BBS      B6 $C0, $1F95
    1FA8: 32 BF       CLR1     $BF, 1
    1FAA: BD          MOV      SP, X
    1FAB: ED          NOTC
    1FAC: 1E BC B9    CMP      X, $B9BC
    1FAF: ED          NOTC
    1FB0: 14 B7       OR       A, $B7+X
    1FB2: B6 ED 0A    SBC      A, $0AED+Y
    1FB5: 7F          RETI
    1FB6: C9 00 04    MOV      $0400, X
    1FB9: 0D          PUSH     PSW
    1FBA: 0A C8 50    OR1      C, $50C8
    1FBD: 27 C9       AND      A, [$C9+X]
    1FBF: C9 C5 C2    MOV      $C2C5, X
    1FC2: C1          TCALL    C
    1FC3: BF          MOV      A, (X)+
    1FC4: ED          NOTC
    1FC5: 14 BE       OR       A, $BE+X
    1FC7: BB B9       INC      $B9+X
    1FC9: B8 ED 0A    SBC      $ED, #$0A
    1FCC: C5 C2 C1    MOV      $C1C2, A
    1FCF: BF          MOV      A, (X)+
    1FD0: BE          DAS      A
    1FD1: BB B9       INC      $B9+X
    1FD3: B8 7F C9    SBC      $7F, #$C9
    1FD6: 00          NOP
    1FD7: F7 1F       MOV      A, [$1F]+Y
    1FD9: 0A 08 C8    OR1      C, $C808
    1FDC: CB 00       MOV      $00, Y
    1FDE: 45 C7 C5    EOR      A, $C5C7
    1FE1: C3 C1 ED    BBS      B6 $C1, $1FD1
    1FE4: 28 C0       AND      A, #$C0
    1FE6: C1          TCALL    C
    1FE7: C3 C5 ED    BBS      B6 $C5, $1FD7
    1FEA: 82 C7       SET1     $C7, 4
    1FEC: C5 C3 C1    MOV      $C1C3, A
    1FEF: ED          NOTC
    1FF0: 28 C0       AND      A, #$C0
    1FF2: C1          TCALL    C
    1FF3: C3 C5 00    BBS      B6 $C5, $1FF6
    1FF6: 04 07       OR       A, $07
    1FF8: 82 C2       SET1     $C2, 4
    1FFA: 00          NOP
    1FFB: 45 B2 C9    EOR      A, $C9B2
    1FFE: C6          MOV      (X), A
    1FFF: E1          TCALL    E
    2000: 12 B3       CLR1     $B3, 0
    2002: C9 C5 E1    MOV      $E1C5, X
    2005: 02 B4       SET1     $B4, 0
    2007: C9 C4 E1    MOV      $E1C4, X
    200A: 12 B5       CLR1     $B5, 0
    200C: C9 C3 E1    MOV      $E1C3, X
    200F: 02 B6       SET1     $B6, 0
    2011: C9 C2 00    MOV      $00C2, X
    2014: 34 20       AND      A, $20+X
    2016: 0C 18 B4    ASL      $B418
    2019: CA 00 27    MOV1     $2700, C
    201C: 89 BB ED    ADC      $BB, $ED
    201F: 82 BA       SET1     $BA, 4
    2021: ED          NOTC
    2022: 5A B9       CMPW     YA, $B9
    2024: ED          NOTC
    2025: 50 B9       BVC      $1FE0
    2027: ED          NOTC
    2028: 3C          ROL      A
    2029: B7 ED       SBC      A, [$ED]+Y
    202B: 28 B5       AND      A, #$B5
    202D: ED          NOTC
    202E: 1E B4 7F    CMP      X, $7FB4
    2031: C9 00 06    MOV      $0600, X
    2034: 18 3C CA    OR       $3C, #$CA
    2037: 00          NOP
    2038: 29 A1 AF    AND      $A1, $AF
    203B: ED          NOTC
    203C: 28 AE       AND      A, #$AE
    203E: AD AD       CMP      Y, #$AD
    2040: AB A9       INC      $A9
    2042: A8 A8       SBC      A, #$A8
    2044: A8 A8       SBC      A, #$A8
    2046: A8 7F       SBC      A, #$7F
    2048: C9 00 00    MOV      $0000, X
    204B: 00          NOP
    204C: 56 20 10    EOR      A, $1020+Y
    204F: F0 E0       BEQ      $2031
    2051: 0F          BRK
    2052: 28 CA       AND      A, #$CA
    2054: 14 02       OR       A, $02+X
    2056: 0A 1E 28    OR1      C, $281E
    2059: 32 46       CLR1     $46, 1
    205B: 50 5A       BVC      $20B7
    205D: 64 78       CMP      A, $78
    205F: 82 8C       SET1     $8C, 4
    2061: AA B4 BE    MOV1     C, $BEB4
    2064: B4 96       SBC      A, $96+X
    2066: 78 82 8C    CMP      $82, #$8C
    2069: AA B4 BE    MOV1     C, $BEB4
    206C: B4 96       SBC      A, $96+X
    206E: 8C 82 78    DEC      $7882
    2071: 64 5A       CMP      A, $5A
    2073: 50 46       BVC      $20BB
    2075: 3C          ROL      A
    2076: 7F          RETI
    2077: 0A 00 A8    OR1      C, $A800
    207A: 20          CLRP
    207B: 85 20 10    ADC      A, $1020
    207E: F0 E0       BEQ      $2060
    2080: 0F          BRK
    2081: E6          MOV      A, (X)
    2082: CA 0F 02    MOV1     $020F, C
    2085: 01          TCALL    0
    2086: 0A 19 1E    OR1      C, $1E19
    2089: 31          TCALL    3
    208A: 32 3C       CLR1     $3C, 1
    208C: 5F 73 78    JMP      $7873
    208F: 7D          MOV      A, X
    2090: 78 64 5A    CMP      $64, #$5A
    2093: 50 46       BVC      $20DB
    2095: 73 78 7D    BBC      B3 $78, $2115
    2098: 78 64 5A    CMP      $64, #$5A
    209B: 50 46       BVC      $20E3
    209D: 46          EOR      A, (X)
    209E: 3C          ROL      A
    209F: 50 3C       BVC      $20DD
    20A1: 32 28       CLR1     $28, 1
    20A3: 1E 01 7F    CMP      X, $7F01
    20A6: 01          TCALL    0
    20A7: 00          NOP
    20A8: 0F          BRK
    20A9: 14 10       OR       A, $10+X
    20AB: 11          TCALL    1
    20AC: 14 12       OR       A, $12+X
    20AE: 11          TCALL    1
    20AF: 12 13       CLR1     $13, 0
    20B1: 12 11       CLR1     $11, 0
    20B3: 10 11       BPL      $20C6
    20B5: 12 11       CLR1     $11, 0
    20B7: 10 13       BPL      $20CC
    20B9: 12 11       CLR1     $11, 0
    20BB: 10 11       BPL      $20CE
    20BD: 12 11       CLR1     $11, 0
    20BF: 10 13       BPL      $20D4
    20C1: 13 12 13    BBC      B0 $12, $20D7
    20C4: 0E 0E 0F    TSET1    $0F0E
    20C7: 1F EA 20    JMP      [$20EA+X]
    20CA: 0A 18 3C    OR1      C, $3C18
    20CD: CA 00 0F    MOV1     $0F00, C
    20D0: 9D          MOV      X, SP
    20D1: 9F          XCN      A
    20D2: ED          NOTC
    20D3: 82 AF       SET1     $AF, 4
    20D5: A1          TCALL    A
    20D6: ED          NOTC
    20D7: 5A 9F       CMPW     YA, $9F
    20D9: 9D          MOV      X, SP
    20DA: ED          NOTC
    20DB: 28 9C       AND      A, #$9C
    20DD: 8E          POP      PSW
    20DE: ED          NOTC
    20DF: 3C          ROL      A
    20E0: 84 9A       ADC      A, $9A
    20E2: ED          NOTC
    20E3: 1E 9C ED    CMP      X, $ED9C
    20E6: 0A 9A 00    OR1      C, $009A
    20E9: 04 19       OR       A, $19
    20EB: 82 CA       SET1     $CA, 4
    20ED: 00          NOP
    20EE: 13 98 8C    BBC      B0 $98, $207D
    20F1: ED          NOTC
    20F2: 50 90       BVC      $2084
    20F4: 98 ED 28    ADC      $ED, #$28
    20F7: B0 8C       BCS      $2085
    20F9: ED          NOTC
    20FA: 14 A4       OR       A, $A4+X
    20FC: 8E          POP      PSW
    20FD: ED          NOTC
    20FE: 0A 85 98    OR1      C, $9885
    2101: 00          NOP
    2102: 28 21       AND      A, #$21
    2104: 0C 08 0A    ASL      $0A08
    2107: C8 00       MOV      X, #$00
    2109: 03 BF BE    BBS      B0 $BF, $20CA
    210C: ED          NOTC
    210D: 14 BF       OR       A, $BF+X
    210F: BE          DAS      A
    2110: ED          NOTC
    2111: 1E BF BE    CMP      X, $BEBF
    2114: ED          NOTC
    2115: 28 BF       AND      A, #$BF
    2117: BE          DAS      A
    2118: ED          NOTC
    2119: 32 BF       CLR1     $BF, 1
    211B: BE          DAS      A
    211C: ED          NOTC
    211D: 3C          ROL      A
    211E: BF          MOV      A, (X)+
    211F: BE          DAS      A
    2120: ED          NOTC
    2121: 28 BF       AND      A, #$BF
    2123: BE          DAS      A
    2124: 7F          RETI
    2125: C9 00 06    MOV      $0600, X
    2128: 08 14       OR       A, #$14
    212A: CC F0 4B    MOV      $4BF0, Y
    212D: B3 ED 1E    BBC      B5 $ED, $214E
    2130: BF          MOV      A, (X)+
    2131: ED          NOTC
    2132: 28 BF       AND      A, #$BF
    2134: ED          NOTC
    2135: 32 BF       CLR1     $BF, 1
    2137: ED          NOTC
    2138: 3C          ROL      A
    2139: BF          MOV      A, (X)+
    213A: ED          NOTC
    213B: 32 BF       CLR1     $BF, 1
    213D: ED          NOTC
    213E: 28 BF       AND      A, #$BF
    2140: ED          NOTC
    2141: 1E BF ED    CMP      X, $EDBF
    2144: 14 BF       OR       A, $BF+X
    2146: 7F          RETI
    2147: C9 00 65    MOV      $6500, X
    214A: 21          TCALL    2
    214B: 0A 08 D2    OR1      C, $D208
    214E: C8 00       MOV      X, #$00
    2150: 43 B5 B0    BBS      B2 $B5, $2103
    2153: ED          NOTC
    2154: 46          EOR      A, (X)
    2155: C7 BB       MOV      [$BB+X], A
    2157: ED          NOTC
    2158: 32 C7       CLR1     $C7, 1
    215A: BB ED       INC      $ED+X
    215C: 14 C7       OR       A, $C7+X
    215E: BB ED       INC      $ED+X
    2160: 0A C7 BB    OR1      C, $BBC7
    2163: 00          NOP
    2164: 04 08       OR       A, $08
    2166: BE          DAS      A
    2167: CC 00 0B    MOV      $0B00, Y
    216A: C9 43 B5    MOV      $B543, X
    216D: B0 ED       BCS      $215C
    216F: 28 C7       AND      A, #$C7
    2171: BB ED       INC      $ED+X
    2173: 0A C7 BB    OR1      C, $BBC7
    2176: C7 BB       MOV      [$BB+X], A
    2178: C7 BB       MOV      [$BB+X], A
    217A: 00          NOP
    217B: 95 21 0A    ADC      A, $0A21+X
    217E: 08 E6       OR       A, #$E6
    2180: C5 00 43    MOV      $4300, A
    2183: C7 C1       MOV      [$C1+X], A
    2185: ED          NOTC
    2186: 78 BC B0    CMP      $BC, #$B0
    2189: B5 BB ED    SBC      A, $EDBB+X
    218C: 14 C7       OR       A, $C7+X
    218E: C1          TCALL    C
    218F: BC          INC      A
    2190: B0 B5       BCS      $2147
    2192: BB 00       INC      $00+X
    2194: 04 08       OR       A, $08
    2196: 8C CF C8    DEC      $C8CF
    2199: 1B C9       ASL      $C9+X
    219B: 43 C7 C1    BBS      B2 $C7, $215F
    219E: BC          INC      A
    219F: B0 B5       BCS      $2156
    21A1: BB ED       INC      $ED+X
    21A3: 14 C7       OR       A, $C7+X
    21A5: C1          TCALL    C
    21A6: BC          INC      A
    21A7: B0 B5       BCS      $215E
    21A9: BB 00       INC      $00+X
    21AB: C3 21 0A    BBS      B6 $21, $21B8
    21AE: 19          OR       X, Y
    21AF: BE          DAS      A
    21B0: C8 00       MOV      X, #$00
    21B2: 07 82       OR       A, [$82+X]
    21B4: 03 91 9D    BBS      B0 $91, $2154
    21B7: A9 B5 C1    SBC      $B5, $C1
    21BA: ED          NOTC
    21BB: 1E 97 A3    CMP      X, $A397
    21BE: AF          MOV      (X)+, A
    21BF: BB C7       INC      $C7+X
    21C1: 00          NOP
    21C2: 04 19       OR       A, $19
    21C4: 5A CE       CMPW     YA, $CE
    21C6: 00          NOP
    21C7: 4B C9       LSR      $C9
    21C9: 07 82       OR       A, [$82+X]
    21CB: 03 91 9D    BBS      B0 $91, $216B
    21CE: A9 B5 C1    SBC      $B5, $C1
    21D1: ED          NOTC
    21D2: 14 97       OR       A, $97+X
    21D4: A3 AF BB    BBS      B5 $AF, $2192
    21D7: C7 00       MOV      [$00+X], A
    21D9: 11          TCALL    1
    21DA: 22 0A       SET1     $0A, 1
    21DC: 0D          PUSH     PSW
    21DD: BE          DAS      A
    21DE: C8 00       MOV      X, #$00
    21E0: 07 82       OR       A, [$82+X]
    21E2: 03 91 9D    BBS      B0 $91, $2182
    21E5: A9 B5 C1    SBC      $B5, $C1
    21E8: ED          NOTC
    21E9: 1E 97 A3    CMP      X, $A397
    21EC: AF          MOV      (X)+, A
    21ED: BB C7       INC      $C7+X
    21EF: E0          CLRV
    21F0: 08 ED       OR       A, #$ED
    21F2: 8C C6 C4    DEC      $C4C6
    21F5: C2 C0       SET1     $C0, 6
    21F7: C2 BF       SET1     $BF, 6
    21F9: BD          MOV      SP, X
    21FA: BC          INC      A
    21FB: ED          NOTC
    21FC: 3C          ROL      A
    21FD: C5 C3 C1    MOV      $C1C3, A
    2200: C0          DI
    2201: C1          TCALL    C
    2202: BE          DAS      A
    2203: BC          INC      A
    2204: BC          INC      A
    2205: ED          NOTC
    2206: 14 C6       OR       A, $C6+X
    2208: C4 C2       MOV      $C2, A
    220A: C0          DI
    220B: C2 BF       SET1     $BF, 6
    220D: BD          MOV      SP, X
    220E: BC          INC      A
    220F: 00          NOP
    2210: 04 0D       OR       A, $0D
    2212: 5A CE       CMPW     YA, $CE
    2214: 00          NOP
    2215: 4B C9       LSR      $C9
    2217: 07 82       OR       A, [$82+X]
    2219: 03 91 9D    BBS      B0 $91, $21B9
    221C: A9 B5 C1    SBC      $B5, $C1
    221F: ED          NOTC
    2220: 14 97       OR       A, $97+X
    2222: A3 AF BB    BBS      B5 $AF, $21E0
    2225: C7 E0       MOV      [$E0+X], A
    2227: 08 ED       OR       A, #$ED
    2229: 3C          ROL      A
    222A: C6          MOV      (X), A
    222B: C4 C2       MOV      $C2, A
    222D: C0          DI
    222E: C2 BF       SET1     $BF, 6
    2230: BD          MOV      SP, X
    2231: BC          INC      A
    2232: ED          NOTC
    2233: 1E C5 C3    CMP      X, $C3C5
    2236: C1          TCALL    C
    2237: C0          DI
    2238: C1          TCALL    C
    2239: BE          DAS      A
    223A: BC          INC      A
    223B: BC          INC      A
    223C: ED          NOTC
    223D: 14 C6       OR       A, $C6+X
    223F: C4 C2       MOV      $C2, A
    2241: C0          DI
    2242: C2 BF       SET1     $BF, 6
    2244: BD          MOV      SP, X
    2245: BC          INC      A
    2246: 00          NOP
    2247: 06          OR       A, (X)
    2248: 18 3C CA    OR       $3C, #$CA
    224B: 00          NOP
    224C: 01          TCALL    0
    224D: C7 C5       MOV      [$C5+X], A
    224F: C3 C1 C0    BBS      B6 $C1, $2212
    2252: BE          DAS      A
    2253: BC          INC      A
    2254: ED          NOTC
    2255: 32 BB       CLR1     $BB, 1
    2257: B9          SBC      X, Y
    2258: B7 B5       SBC      A, [$B5]+Y
    225A: B4 B2       SBC      A, $B2+X
    225C: B0 ED       BCS      $224B
    225E: 28 AF       AND      A, #$AF
    2260: AD ED       CMP      Y, #$ED
    2262: 1E AB ED    CMP      X, $EDAB
    2265: 14 A9       OR       A, $A9+X
    2267: A8 ED       SBC      A, #$ED
    2269: 0A A6 A4    OR1      C, $A4A6
    226C: 7F          RETI
    226D: C9 00 06    MOV      $0600, X
    2270: 18 78 CA    OR       $78, #$CA
    2273: 00          NOP
    2274: 01          TCALL    0
    2275: C7 C3       MOV      [$C3+X], A
    2277: C0          DI
    2278: BC          INC      A
    2279: BB B7       INC      $B7+X
    227B: B4 B0       SBC      A, $B0+X
    227D: AF          MOV      (X)+, A
    227E: AB A8       INC      $A8
    2280: A4 A3       SBC      A, $A3
    2282: 9F          XCN      A
    2283: 9C          DEC      A
    2284: 98 ED 3C    ADC      $ED, #$3C
    2287: 97 95       ADC      A, [$95]+Y
    2289: 93 91 90    BBC      B4 $91, $221C
    228C: 8E          POP      PSW
    228D: 8C ED 28    DEC      $28ED
    2290: 8B 89       DEC      $89
    2292: ED          NOTC
    2293: 14 87       OR       A, $87+X
    2295: 85 ED 0A    ADC      A, $0AED
    2298: 84 82       ADC      A, $82
    229A: 7F          RETI
    229B: C9 00 06    MOV      $0600, X
    229E: 18 BE CA    OR       $BE, #$CA
    22A1: 00          NOP
    22A2: 01          TCALL    0
    22A3: C4 C6       MOV      $C6, A
    22A5: C2 ED       SET1     $ED, 6
    22A7: 8C BE C0    DEC      $C0BE
    22AA: BC          INC      A
    22AB: B8 BA B6    SBC      $BA, #$B6
    22AE: ED          NOTC
    22AF: 50 B2       BVC      $2263
    22B1: B4 B0       SBC      A, $B0+X
    22B3: AC AE AA    INC      $AAAE
    22B6: A6          SBC      A, (X)
    22B7: A8 A4       SBC      A, #$A4
    22B9: A0          EI
    22BA: A2 9E       SET1     $9E, 5
    22BC: 9A 9C       SUBW     YA, $9C
    22BE: 98 ED 3C    ADC      $ED, #$3C
    22C1: 94 96       ADC      A, $96+X
    22C3: 92 ED       CLR1     $ED, 4
    22C5: 1E 8E 90    CMP      X, $908E
    22C8: 8C ED 0A    DEC      $0AED
    22CB: 82 84       SET1     $84, 4
    22CD: 82 7F       SET1     $7F, 4
    22CF: C9 00 06    MOV      $0600, X
    22D2: 18 F0 CA    OR       $F0, #$CA
    22D5: 00          NOP
    22D6: 27 AC       AND      A, [$AC+X]
    22D8: A4 A8       SBC      A, $A8
    22DA: AB A6       INC      $A6
    22DC: 92 A2       CLR1     $A2, 4
    22DE: 99          ADC      X, Y
    22DF: ED          NOTC
    22E0: D2 A3       CLR1     $A3, 6
    22E2: 9E          DIV      YA, X
    22E3: AD AB       CMP      Y, #$AB
    22E5: A1          TCALL    A
    22E6: A9 AC 99    SBC      $AC, $99
    22E9: 9F          XCN      A
    22EA: 9C          DEC      A
    22EB: AB A6       INC      $A6
    22ED: ED          NOTC
    22EE: 64 A0       CMP      A, $A0
    22F0: A4 A8       SBC      A, $A8
    22F2: 9F          XCN      A
    22F3: 9A 92       SUBW     YA, $92
    22F5: AE          POP      A
    22F6: A5 ED 50    SBC      A, $50ED
    22F9: A3 AA AD    BBS      B5 $AA, $22A9
    22FC: AB ED       INC      $ED
    22FE: 32 A1       CLR1     $A1, 1
    2300: 9D          MOV      X, SP
    2301: AC 99 ED    INC      $ED99
    2304: 1E 9F A8    CMP      X, $A89F
    2307: AB A6       INC      $A6
    2309: 7F          RETI
    230A: C9 00 54    MOV      $5400, X
    230D: 23 0A 0D    BBS      B1 $0A, $231D
    2310: 82 CC       SET1     $CC, 4
    2312: 00          NOP
    2313: 03 C4 C3    BBS      B0 $C4, $22D9
    2316: C1          TCALL    C
    2317: C0          DI
    2318: ED          NOTC
    2319: 5A BF       CMPW     YA, $BF
    231B: BE          DAS      A
    231C: BC          INC      A
    231D: BB B9       INC      $B9+X
    231F: ED          NOTC
    2320: B4 C6       SBC      A, $C6+X
    2322: C5 C3 C1    MOV      $C1C3, A
    2325: C0          DI
    2326: ED          NOTC
    2327: 32 C2       CLR1     $C2, 1
    2329: C1          TCALL    C
    232A: C0          DI
    232B: BE          DAS      A
    232C: ED          NOTC
    232D: 64 BD       CMP      A, $BD
    232F: BC          INC      A
    2330: BB B9       INC      $B9+X
    2332: B7 C4       SBC      A, [$C4]+Y
    2334: C3 C1 C0    BBS      B6 $C1, $22F7
    2337: ED          NOTC
    2338: 5A BF       CMPW     YA, $BF
    233A: BE          DAS      A
    233B: BC          INC      A
    233C: BB B9       INC      $B9+X
    233E: ED          NOTC
    233F: B4 C6       SBC      A, $C6+X
    2341: C5 C3 C1    MOV      $C1C3, A
    2344: C0          DI
    2345: ED          NOTC
    2346: 32 C2       CLR1     $C2, 1
    2348: C1          TCALL    C
    2349: C0          DI
    234A: BE          DAS      A
    234B: ED          NOTC
    234C: 64 BD       CMP      A, $BD
    234E: BC          INC      A
    234F: BB B9       INC      $B9+X
    2351: B7 00       SBC      A, [$00]+Y
    2353: 04 0D       OR       A, $0D
    2355: 82 C5       SET1     $C5, 4
    2357: C8 1B       MOV      X, #$1B
    2359: C9 03 C4    MOV      $C403, X
    235C: C3 C1 C0    BBS      B6 $C1, $231F
    235F: ED          NOTC
    2360: 5A BF       CMPW     YA, $BF
    2362: BE          DAS      A
    2363: BC          INC      A
    2364: BB B9       INC      $B9+X
    2366: ED          NOTC
    2367: B4 C6       SBC      A, $C6+X
    2369: C5 C3 C1    MOV      $C1C3, A
    236C: C0          DI
    236D: ED          NOTC
    236E: 32 C2       CLR1     $C2, 1
    2370: C1          TCALL    C
    2371: C0          DI
    2372: BE          DAS      A
    2373: ED          NOTC
    2374: 64 BD       CMP      A, $BD
    2376: BC          INC      A
    2377: BB B9       INC      $B9+X
    2379: B7 C4       SBC      A, [$C4]+Y
    237B: C3 C1 C0    BBS      B6 $C1, $233E
    237E: ED          NOTC
    237F: 5A BF       CMPW     YA, $BF
    2381: BE          DAS      A
    2382: BC          INC      A
    2383: BB B9       INC      $B9+X
    2385: ED          NOTC
    2386: B4 C6       SBC      A, $C6+X
    2388: C5 C3 C1    MOV      $C1C3, A
    238B: C0          DI
    238C: ED          NOTC
    238D: 32 C2       CLR1     $C2, 1
    238F: C1          TCALL    C
    2390: C0          DI
    2391: BE          DAS      A
    2392: ED          NOTC
    2393: 64 BD       CMP      A, $BD
    2395: BC          INC      A
    2396: BB B9       INC      $B9+X
    2398: B7 00       SBC      A, [$00]+Y
    239A: CB 23       MOV      $23, Y
    239C: 04 07       OR       A, $07
    239E: DC          DEC      Y
    239F: CA 00 4B    MOV1     $4B00, C
    23A2: BA E1       MOVW     YA, $E1
    23A4: 07 C6       OR       A, [$C6+X]
    23A6: E1          TCALL    E
    23A7: 0D          PUSH     PSW
    23A8: B9          SBC      X, Y
    23A9: ED          NOTC
    23AA: 32 E1       CLR1     $E1, 1
    23AC: 0A BA E1    OR1      C, $E1BA
    23AF: 0D          PUSH     PSW
    23B0: C6          MOV      (X), A
    23B1: E1          TCALL    E
    23B2: 07 B9       OR       A, [$B9+X]
    23B4: ED          NOTC
    23B5: 1E E1 0A    CMP      X, $0AE1
    23B8: BA E1       MOVW     YA, $E1
    23BA: 07 C6       OR       A, [$C6+X]
    23BC: B9          SBC      X, Y
    23BD: E1          TCALL    E
    23BE: 0A BA E1    OR1      C, $E1BA
    23C1: 0D          PUSH     PSW
    23C2: C6          MOV      (X), A
    23C3: B9          SBC      X, Y
    23C4: E1          TCALL    E
    23C5: 0A BA C6    OR1      C, $C6BA
    23C8: B9          SBC      X, Y
    23C9: 00          NOP
    23CA: 04 07       OR       A, $07
    23CC: 5A D2       CMPW     YA, $D2
    23CE: 00          NOP
    23CF: 4B C9       LSR      $C9
    23D1: C9 B6 C2    MOV      $C2B6, X
    23D4: B5 ED 14    SBC      A, $14ED+X
    23D7: E1          TCALL    E
    23D8: 02 B6       SET1     $B6, 0
    23DA: C2 B5       SET1     $B5, 6
    23DC: E1          TCALL    E
    23DD: 12 B6       CLR1     $B6, 0
    23DF: C2 B5       SET1     $B5, 6
    23E1: E1          TCALL    E
    23E2: 02 B6       SET1     $B6, 0
    23E4: C2 B5       SET1     $B5, 6
    23E6: E1          TCALL    E
    23E7: 12 B6       CLR1     $B6, 0
    23E9: C2 B5       SET1     $B5, 6
    23EB: 00          NOP
    23EC: 28 24       AND      A, #$24
    23EE: 0A 07 DC    OR1      C, $DC07
    23F1: D2 00       CLR1     $00, 6
    23F3: 4B 8D       LSR      $8D
    23F5: B3 C6 ED    BBC      B5 $C6, $23E5
    23F8: B4 C5       SBC      A, $C5+X
    23FA: AB E1       INC      $E1
    23FC: 0F          BRK
    23FD: ED          NOTC
    23FE: 50 B1       BVC      $23B1
    2400: B3 C6 C5    BBC      B5 $C6, $23C8
    2403: AB ED       INC      $ED
    2405: 3C          ROL      A
    2406: E1          TCALL    E
    2407: 0C B1 B3    ASL      $B3B1
    240A: C6          MOV      (X), A
    240B: C5 AB ED    MOV      $EDAB, A
    240E: 28 E1       AND      A, #$E1
    2410: 08 B1       OR       A, #$B1
    2412: B3 C6 C5    BBC      B5 $C6, $23DA
    2415: AB ED       INC      $ED
    2417: 1E E1 06    CMP      X, $06E1
    241A: B1          TCALL    B
    241B: B3 C6 C5    BBC      B5 $C6, $23E3
    241E: AB E1       INC      $E1
    2420: 03 B1 B3    BBS      B0 $B1, $23D6
    2423: C6          MOV      (X), A
    2424: C5 AB 00    MOV      $00AB, A
    2427: 04 07       OR       A, $07
    2429: BE          DAS      A
    242A: C2 78       SET1     $78, 6
    242C: 4B 8D       LSR      $8D
    242E: B3 C6 C5    BBC      B5 $C6, $23F6
    2431: AB E1       INC      $E1
    2433: 05 ED 1E    OR       A, $1EED
    2436: B1          TCALL    B
    2437: B3 C6 C5    BBC      B5 $C6, $23FF
    243A: AB ED       INC      $ED
    243C: 14 E1       OR       A, $E1+X
    243E: 08 B1       OR       A, #$B1
    2440: B3 C6 C5    BBC      B5 $C6, $2408
    2443: AB ED       INC      $ED
    2445: 0A E1 0C    OR1      C, $0CE1
    2448: B1          TCALL    B
    2449: B3 C6 C5    BBC      B5 $C6, $2411
    244C: AB E1       INC      $E1
    244E: 0E B1 B3    TSET1    $B3B1
    2451: C6          MOV      (X), A
    2452: C5 AB E1    MOV      $E1AB, A
    2455: 11          TCALL    1
    2456: B1          TCALL    B
    2457: B3 C6 C5    BBC      B5 $C6, $241F
    245A: AB 00       INC      $00
    245C: 93 24 0A    BBC      B4 $24, $2469
    245F: 07 BE       OR       A, [$BE+X]
    2461: CD 00       MOV      X, #$00
    2463: 45 B7 B9    EOR      A, $B9B7
    2466: BE          DAS      A
    2467: C0          DI
    2468: ED          NOTC
    2469: 28 B7       AND      A, #$B7
    246B: BB C0       INC      $C0+X
    246D: C2 C7       SET1     $C7, 6
    246F: 03 ED 28    BBS      B0 $ED, $249A
    2472: B7 B9       SBC      A, [$B9]+Y
    2474: BE          DAS      A
    2475: C0          DI
    2476: C3 BB C0    BBS      B6 $BB, $2439
    2479: C2 C7       SET1     $C7, 6
    247B: ED          NOTC
    247C: 1E B7 B9    CMP      X, $B9B7
    247F: BE          DAS      A
    2480: C0          DI
    2481: C3 BB C0    BBS      B6 $BB, $2444
    2484: C2 C7       SET1     $C7, 6
    2486: ED          NOTC
    2487: 14 B7       OR       A, $B7+X
    2489: B9          SBC      X, Y
    248A: BE          DAS      A
    248B: C0          DI
    248C: C3 BB C0    BBS      B6 $BB, $244F
    248F: C2 C7       SET1     $C7, 6
    2491: 00          NOP
    2492: 04 07       OR       A, $07
    2494: AA C7 F0    MOV1     C, $F0C7
    2497: 45 C9 C9    EOR      A, $C9C9
    249A: B7 B9       SBC      A, [$B9]+Y
    249C: BE          DAS      A
    249D: C0          DI
    249E: B7 BB       SBC      A, [$BB]+Y
    24A0: C0          DI
    24A1: C2 C7       SET1     $C7, 6
    24A3: 03 ED 14    BBS      B0 $ED, $24BA
    24A6: B7 C5       SBC      A, [$C5]+Y
    24A8: BE          DAS      A
    24A9: C0          DI
    24AA: B7 BB       SBC      A, [$BB]+Y
    24AC: C0          DI
    24AD: C2 C7       SET1     $C7, 6
    24AF: B7 C5       SBC      A, [$C5]+Y
    24B1: BE          DAS      A
    24B2: C0          DI
    24B3: B7 BB       SBC      A, [$BB]+Y
    24B5: C0          DI
    24B6: C2 C7       SET1     $C7, 6
    24B8: ED          NOTC
    24B9: 0A B7 C5    OR1      C, $C5B7
    24BC: BE          DAS      A
    24BD: C0          DI
    24BE: B7 BB       SBC      A, [$BB]+Y
    24C0: C0          DI
    24C1: C2 C7       SET1     $C7, 6
    24C3: 00          NOP
    24C4: 06          OR       A, (X)
    24C5: 08 E6       OR       A, #$E6
    24C7: CA 00 27    MOV1     $2700, C
    24CA: 98 97 95    ADC      $97, #$95
    24CD: 87 8B       ADC      A, [$8B+X]
    24CF: 90 93       BCC      $2464
    24D1: 97 9A       ADC      A, [$9A]+Y
    24D3: 9D          MOV      X, SP
    24D4: 7F          RETI
    24D5: C9 00 04    MOV      $0400, X
    24D8: 12 5A       CLR1     $5A, 0
    24DA: CA 00 0B    MOV1     $0B00, C
    24DD: A4 00       SBC      A, $00
    24DF: 06          OR       A, (X)
    24E0: 18 5A CA    OR       $5A, #$CA
    24E3: 00          NOP
    24E4: 27 8C       AND      A, [$8C+X]
    24E6: 9A 9C       SUBW     YA, $9C
    24E8: 9D          MOV      X, SP
    24E9: 9F          XCN      A
    24EA: ED          NOTC
    24EB: 3C          ROL      A
    24EC: A4 A8       SBC      A, $A8
    24EE: AB AF       INC      $AF
    24F0: B2 B5       CLR1     $B5, 5
    24F2: B7 B9       SBC      A, [$B9]+Y
    24F4: BB BC       INC      $BC+X
    24F6: BE          DAS      A
    24F7: C0          DI
    24F8: C1          TCALL    C
    24F9: C3 C5 7F    BBS      B6 $C5, $257B
    24FC: C9 00 8B    MOV      $8B00, X
    24FF: 14 08       OR       A, $08+X
    2501: E8 0A       MOV      A, #$0A
    2503: 8D 25       MOV      Y, #$25
    2505: CD 1F       MOV      X, #$1F
    2507: 5F EE 14    JMP      $14EE
    250A: 22 20       SET1     $20, 1
    250C: 0F          BRK
    250D: 46          EOR      A, (X)
    250E: CA B4 00    MOV1     $00B4, C
    2511: 04 08       OR       A, $08
    2513: 5A CA       CMPW     YA, $CA
    2515: 00          NOP
    2516: 4B B1       LSR      $B1
    2518: B3 AC ED    BBC      B5 $AC, $2508
    251B: 82 AE       SET1     $AE, 4
    251D: ED          NOTC
    251E: 3C          ROL      A
    251F: AA 00 39    MOV1     C, $3900
    2522: 25 0A 08    AND      A, $080A
    2525: E6          MOV      A, (X)
    2526: CC 00 07    MOV      $0700, Y
    2529: C7 C7       MOV      [$C7+X], A
    252B: ED          NOTC
    252C: 28 C7       AND      A, #$C7
    252E: C7 ED       MOV      [$ED+X], A
    2530: 14 C7       OR       A, $C7+X
    2532: C7 ED       MOV      [$ED+X], A
    2534: 0A C7 C7    OR1      C, $C7C7
    2537: 00          NOP
    2538: 04 08       OR       A, $08
    253A: 14 C5       OR       A, $C5+X
    253C: 14 01       OR       A, $01+X
    253E: C9 07 C7    MOV      $C707, X
    2541: C7 E1       MOV      [$E1+X], A
    2543: 0F          BRK
    2544: C7 C7       MOV      [$C7+X], A
    2546: E1          TCALL    E
    2547: 05 C7 C7    OR       A, $C7C7
    254A: E1          TCALL    E
    254B: 0F          BRK
    254C: C7 C7       MOV      [$C7+X], A
    254E: E1          TCALL    E
    254F: 05 C7 C7    OR       A, $C7C7
    2552: 00          NOP
    2553: 70 25       BVS      $257A
    2555: 0C 18 F0    ASL      $F018
    2558: CA 00 07    MOV1     $0700, C
    255B: 8C 2F ED    DEC      $ED2F
    255E: BE          DAS      A
    255F: A4 ED       SBC      A, $ED
    2561: 28 9F       AND      A, #$9F
    2563: ED          NOTC
    2564: 1E 98 ED    CMP      X, $ED98
    2567: 32 93       CLR1     $93, 1
    2569: ED          NOTC
    256A: 0A 8C 7F    OR1      C, $7F8C
    256D: C9 00 06    MOV      $0600, X
    2570: 18 F0 CA    OR       $F0, #$CA
    2573: 00          NOP
    2574: 12 89       CLR1     $89, 0
    2576: 2F ED       BRA      $2565
    2578: C8 93       MOV      X, #$93
    257A: ED          NOTC
    257B: 28 90       AND      A, #$90
    257D: ED          NOTC
    257E: 78 8B ED    CMP      $8B, #$ED
    2581: 14 87       OR       A, $87+X
    2583: ED          NOTC
    2584: 0A 97 7F    OR1      C, $7F97
    2587: C9 00 06    MOV      $0600, X
    258A: 18 5A CA    OR       $5A, #$CA
    258D: 00          NOP
    258E: 03 9F A0    BBS      B0 $9F, $2531
    2591: 9C          DEC      A
    2592: ED          NOTC
    2593: E6          MOV      A, (X)
    2594: 9F          XCN      A
    2595: A0          EI
    2596: 9C          DEC      A
    2597: 9F          XCN      A
    2598: A0          EI
    2599: 9C          DEC      A
    259A: 9F          XCN      A
    259B: A0          EI
    259C: 9C          DEC      A
    259D: 9F          XCN      A
    259E: A0          EI
    259F: 9C          DEC      A
    25A0: ED          NOTC
    25A1: 96 9F A0    ADC      A, $A09F+Y
    25A4: 9C          DEC      A
    25A5: ED          NOTC
    25A6: 64 9F       CMP      A, $9F
    25A8: A0          EI
    25A9: 9C          DEC      A
    25AA: ED          NOTC
    25AB: 1E 9F A0    CMP      X, $A09F
    25AE: 9C          DEC      A
    25AF: ED          NOTC
    25B0: 0A 9F A0    OR1      C, $A09F
    25B3: 9C          DEC      A
    25B4: 00          NOP
    25B5: C0          DI
    25B6: 25 0A 19    AND      A, $190A
    25B9: 82 CD       SET1     $CD, 4
    25BB: 00          NOP
    25BC: 17 B7       OR       A, [$B7]+Y
    25BE: 00          NOP
    25BF: 04 19       OR       A, $19
    25C1: 1E C5 64    CMP      X, $64C5
    25C4: 19          OR       X, Y
    25C5: C9 17 B7    MOV      $B717, X
    25C8: 00          NOP
    25C9: D4 25       MOV      $25+X, A
    25CB: 0A 1A DC    OR1      C, $DC1A
    25CE: C9 00 1F    MOV      $1F00, X
    25D1: A8 00       SBC      A, #$00
    25D3: 04 1A       OR       A, $1A
    25D5: DC          DEC      Y
    25D6: CB 14       MOV      $14, Y
    25D8: 1F A8 00    JMP      [$00A8+X]
    25DB: 06          OR       A, (X)
    25DC: 18 14 CA    OR       $14, #$CA
    25DF: 00          NOP
    25E0: 03 C7 ED    BBS      B0 $C7, $25D0
    25E3: 28 C7       AND      A, #$C7
    25E5: ED          NOTC
    25E6: 3C          ROL      A
    25E7: C7 ED       MOV      [$ED+X], A
    25E9: 50 C7       BVC      $25B2
    25EB: 0F          BRK
    25EC: ED          NOTC
    25ED: 5A C7       CMPW     YA, $C7
    25EF: 03 ED 32    BBS      B0 $ED, $2624
    25F2: C6          MOV      (X), A
    25F3: ED          NOTC
    25F4: 1E C5 ED    CMP      X, $EDC5
    25F7: 14 C4       OR       A, $C4+X
    25F9: ED          NOTC
    25FA: 0A C3 7F    OR1      C, $7FC3
    25FD: C9 00 2B    MOV      $2B00, X
    2600: 26          AND      A, (X)
    2601: 0A 08 E6    OR1      C, $E608
    2604: C8 00       MOV      X, #$00
    2606: 49 BD BE    EOR      $BD, $BE
    2609: C3 C7 ED    BBS      B6 $C7, $25F9
    260C: 50 BF       BVC      $25CD
    260E: C0          DI
    260F: C5 C6 ED    MOV      $EDC6, A
    2612: 46          EOR      A, (X)
    2613: BD          MOV      SP, X
    2614: BE          DAS      A
    2615: C3 C7 ED    BBS      B6 $C7, $2605
    2618: 28 BF       AND      A, #$BF
    261A: C0          DI
    261B: C5 C6 ED    MOV      $EDC6, A
    261E: 1E BD BE    CMP      X, $BEBD
    2621: C3 C7 ED    BBS      B6 $C7, $2611
    2624: 0A BF C0    OR1      C, $C0BF
    2627: C5 C6 00    MOV      $00C6, A
    262A: 04 08       OR       A, $08
    262C: 46          EOR      A, (X)
    262D: CC C8 1B    MOV      $1BC8, Y
    2630: C9 4B BD    MOV      $BD4B, X
    2633: BE          DAS      A
    2634: C3 ED 32    BBS      B6 $ED, $2669
    2637: C7 BF       MOV      [$BF+X], A
    2639: C0          DI
    263A: C5 C6 ED    MOV      $EDC6, A
    263D: 14 BD       OR       A, $BD+X
    263F: BE          DAS      A
    2640: C3 C7 BF    BBS      B6 $C7, $2602
    2643: C0          DI
    2644: C5 C6 ED    MOV      $EDC6, A
    2647: 0A BD BE    OR1      C, $BEBD
    264A: C3 C7 BF    BBS      B6 $C7, $260C
    264D: C0          DI
    264E: C5 C6 00    MOV      $00C6, A
    2651: 64 26       CMP      A, $26
    2653: 0A 0A 1E    OR1      C, $1E0A
    2656: CA 00 0B    MOV1     $0B00, C
    2659: B0 ED       BCS      $2648
    265B: 3C          ROL      A
    265C: B2 B3       CLR1     $B3, 5
    265E: ED          NOTC
    265F: 28 B4       AND      A, #$B4
    2661: B5 00 04    SBC      A, $0400+X
    2664: 0A 46 C8    OR1      C, $C846
    2667: 00          NOP
    2668: 4B C9       LSR      $C9
    266A: 0B AC       ASL      $AC
    266C: E1          TCALL    E
    266D: 0C AD E1    ASL      $E1AD
    2670: 08 AE       OR       A, #$AE
    2672: E1          TCALL    E
    2673: 0C AF E1    ASL      $E1AF
    2676: 08 B0       OR       A, #$B0
    2678: 00          NOP
    2679: A7 26       SBC      A, [$26+X]
    267B: 0A 16 8C    OR1      C, $8C16
    267E: CA 00 4B    MOV1     $4B00, C
    2681: 81          TCALL    8
    2682: E1          TCALL    E
    2683: 01          TCALL    0
    2684: 4D          PUSH     X
    2685: B0 E1       BCS      $2668
    2687: 0F          BRK
    2688: E0          CLRV
    2689: 09 94 E1    OR       $94, $E1
    268C: 03 E0 14    BBS      B0 $E0, $26A3
    268F: 86          ADC      A, (X)
    2690: E1          TCALL    E
    2691: 0A E0 05    OR1      C, $05E0
    2694: AA E1 13    MOV1     C, $13E1
    2697: 07 E0       OR       A, [$E0+X]
    2699: 16 A9 E1    OR       A, $E1A9+Y
    269C: 14 E0       OR       A, $E0+X
    269E: 11          TCALL    1
    269F: C7 E1       MOV      [$E1+X], A
    26A1: 0A E0 0E    OR1      C, $0EE0
    26A4: 82 00       SET1     $00, 4
    26A6: 04 16       OR       A, $16
    26A8: 3C          ROL      A
    26A9: C3 64 4B    BBS      B6 $64, $26F7
    26AC: C9 81 E1    MOV      $E181, X
    26AF: 14 4D       OR       A, $4D+X
    26B1: B0 E1       BCS      $2694
    26B3: 05 E0 09    OR       A, $09E0
    26B6: 94 E1       ADC      A, $E1+X
    26B8: 11          TCALL    1
    26B9: E0          CLRV
    26BA: 14 86       OR       A, $86+X
    26BC: E1          TCALL    E
    26BD: 0A E0 05    OR1      C, $05E0
    26C0: AA E1 02    MOV1     C, $02E1
    26C3: 07 E0       OR       A, [$E0+X]
    26C5: 16 A9 E1    OR       A, $E1A9+Y
    26C8: 01          TCALL    0
    26C9: E0          CLRV
    26CA: 11          TCALL    1
    26CB: C7 E1       MOV      [$E1+X], A
    26CD: 0A E0 0E    OR1      C, $0EE0
    26D0: 82 00       SET1     $00, 4
    26D2: 04 0F       OR       A, $0F
    26D4: 5A CA       CMPW     YA, $CA
    26D6: 00          NOP
    26D7: 23 82 92    BBS      B1 $82, $266C
    26DA: 89 00 F2    ADC      $00, $F2
    26DD: 26          AND      A, (X)
    26DE: 0C 18 F0    ASL      $F018
    26E1: CA 00 0B    MOV1     $0B00, C
    26E4: 8E          POP      PSW
    26E5: 8F 90 91    MOV      $90, #$91
    26E8: 92 91       CLR1     $91, 4
    26EA: 90 ED       BCC      $26D9
    26EC: 3C          ROL      A
    26ED: 8F 7F C9    MOV      $7F, #$C9
    26F0: 00          NOP
    26F1: 06          OR       A, (X)
    26F2: 18 A0 C5    OR       $A0, #$C5
    26F5: 00          NOP
    26F6: 0B AD       ASL      $AD
    26F8: AB A9       INC      $A9
    26FA: A8 A9       SBC      A, #$A9
    26FC: ED          NOTC
    26FD: 64 AC       CMP      A, $AC
    26FF: AD ED       CMP      Y, #$ED
    2701: 3C          ROL      A
    2702: B0 7F       BCS      $2783
    2704: C9 00 11    MOV      $1100, X
    2707: 27 0A       AND      A, [$0A+X]
    2709: 08 01       OR       A, #$01
    270B: CA 00 03    MOV1     $0300, C
    270E: 8E          POP      PSW
    270F: 00          NOP
    2710: 04 08       OR       A, $08
    2712: 01          TCALL    0
    2713: CA 00 03    MOV1     $0300, C
    2716: 8E          POP      PSW
    2717: 00          NOP
    2718: 06          OR       A, (X)
    2719: 18 78 CA    OR       $78, #$CA
    271C: 00          NOP
    271D: 03 B0 B5    BBS      B0 $B0, $26D5
    2720: B1          TCALL    B
    2721: B6 ED 3C    SBC      A, $3CED+Y
    2724: B2 B7       CLR1     $B7, 5
    2726: ED          NOTC
    2727: 32 B3       CLR1     $B3, 1
    2729: B8 ED 28    SBC      $ED, #$28
    272C: B4 B9       SBC      A, $B9+X
    272E: ED          NOTC
    272F: 1E B5 BA    CMP      X, $BAB5
    2732: 7F          RETI
    2733: C9 00 54    MOV      $5400, X
    2736: 27 0C       AND      A, [$0C+X]
    2738: 08 78       OR       A, #$78
    273A: CD 00       MOV      X, #$00
    273C: 07 B5       OR       A, [$B5+X]
    273E: B6 B7 B8    SBC      A, $B8B7+Y
    2741: B7 B6       SBC      A, [$B6]+Y
    2743: B5 ED 64    SBC      A, $64ED+X
    2746: B8 B9 BA    SBC      $B9, #$BA
    2749: BB ED       INC      $ED+X
    274B: 3C          ROL      A
    274C: BC          INC      A
    274D: BD          MOV      SP, X
    274E: ED          NOTC
    274F: 28 7F       AND      A, #$7F
    2751: C9 00 06    MOV      $0600, X
    2754: 08 3C       OR       A, #$3C
    2756: C7 8C       MOV      [$8C+X], A
    2758: 07 C9       OR       A, [$C9+X]
    275A: B5 B6 ED    SBC      A, $EDB6+X
    275D: 32 B7       CLR1     $B7, 1
    275F: B8 B7 B6    SBC      $B7, #$B6
    2762: B5 ED 28    SBC      A, $28ED+X
    2765: B8 B9 BA    SBC      $B9, #$BA
    2768: BB ED       INC      $ED+X
    276A: 1E BC BD    CMP      X, $BDBC
    276D: ED          NOTC
    276E: 0A 7F C9    OR1      C, $C97F
    2771: 00          NOP
    2772: 9A 27       SUBW     YA, $27
    2774: 0C 18 E6    ASL      $E618
    2777: CA 00 03    MOV1     $0300, C
    277A: A4 B4       SBC      A, $B4
    277C: 9F          XCN      A
    277D: BB A4       INC      $A4+X
    277F: B4 9F       SBC      A, $9F+X
    2781: BB A4       INC      $A4+X
    2783: B4 9F       SBC      A, $9F+X
    2785: BB 8C       INC      $8C+X
    2787: A8 98       SBC      A, #$98
    2789: B4 93       SBC      A, $93+X
    278B: AF          MOV      (X)+, A
    278C: 98 B4 93    ADC      $B4, #$93
    278F: AF          MOV      (X)+, A
    2790: 98 B4 93    ADC      $B4, #$93
    2793: AF          MOV      (X)+, A
    2794: 98 B4 7F    ADC      $B4, #$7F
    2797: C9 00 06    MOV      $0600, X
    279A: 18 BE C1    OR       $BE, #$C1
    279D: 00          NOP
    279E: 07 A6       OR       A, [$A6+X]
    27A0: B0 E1       BCS      $2783
    27A2: 14 A1       OR       A, $A1+X
    27A4: B7 E1       SBC      A, [$E1]+Y
    27A6: 02 A6       SET1     $A6, 0
    27A8: B0 E1       BCS      $278B
    27AA: 12 A1       CLR1     $A1, 0
    27AC: B7 E1       SBC      A, [$E1]+Y
    27AE: 04 A6       OR       A, $A6
    27B0: B0 E1       BCS      $2793
    27B2: 11          TCALL    1
    27B3: A1          TCALL    A
    27B4: B7 E1       SBC      A, [$E1]+Y
    27B6: 02 A6       SET1     $A6, 0
    27B8: B0 7F       BCS      $2839
    27BA: C9 00 E3    MOV      $E300, X
    27BD: 27 C8       AND      A, [$C8+X]
    27BF: 27 10       AND      A, [$10+X]
    27C1: F0 E0       BEQ      $27A3
    27C3: 0F          BRK
    27C4: E6          MOV      A, (X)
    27C5: CA 0F 02    MOV1     $020F, C
    27C8: 7D          MOV      A, X
    27C9: 5A 7D       CMPW     YA, $7D
    27CB: 78 45 78    CMP      $45, #$78
    27CE: 6E 7D 37    DBNZ     $7D, $2808
    27D1: 32 2D       CLR1     $2D, 1
    27D3: 28 23       AND      A, #$23
    27D5: 1E 19 14    CMP      X, $1419
    27D8: 14 12       OR       A, $12+X
    27DA: 0F          BRK
    27DB: 0C 0A 09    ASL      $090A
    27DE: 08 01       OR       A, #$01
    27E0: 7F          RETI
    27E1: 01          TCALL    0
    27E2: 00          NOP
    27E3: 0F          BRK
    27E4: 14 10       OR       A, $10+X
    27E6: 11          TCALL    1
    27E7: 14 12       OR       A, $12+X
    27E9: 11          TCALL    1
    27EA: 12 13       CLR1     $13, 0
    27EC: 12 11       CLR1     $11, 0
    27EE: 10 11       BPL      $2801
    27F0: 12 11       CLR1     $11, 0
    27F2: 10 13       BPL      $2807
    27F4: 13 12 13    BBC      B0 $12, $280A
    27F7: 0E 0E 0F    TSET1    $0F0E
    27FA: 1F 04 1A    JMP      [$1A04+X]
    27FD: BE          DAS      A
    27FE: C9 00 0F    MOV      $0F00, X
    2801: BB 00       INC      $00+X
    2803: 21          TCALL    2
    2804: 28 0A       AND      A, #$0A
    2806: 07 DC       OR       A, [$DC+X]
    2808: CC 00 45    MOV      $4500, Y
    280B: 81          TCALL    8
    280C: 9B AE       DEC      $AE+X
    280E: ED          NOTC
    280F: B4 C5       SBC      A, $C5+X
    2811: E1          TCALL    E
    2812: 0F          BRK
    2813: ED          NOTC
    2814: 1E 81 9B    CMP      X, $9B81
    2817: C6          MOV      (X), A
    2818: ED          NOTC
    2819: 1E E1 0C    CMP      X, $0CE1
    281C: 81          TCALL    8
    281D: 9B C6       DEC      $C6+X
    281F: 00          NOP
    2820: 04 19       OR       A, $19
    2822: F0 C8       BEQ      $27EC
    2824: 00          NOP
    2825: 01          TCALL    0
    2826: C0          DI
    2827: 93 ED 32    BBC      B4 $ED, $285C
    282A: 8E          POP      PSW
    282B: 93 97 9A    BBC      B4 $97, $27C8
    282E: 9F          XCN      A
    282F: A3 A6 AB    BBS      B5 $A6, $27DD
    2832: AF          MOV      (X)+, A
    2833: B2 ED       CLR1     $ED, 5
    2835: 1E B8 BB    CMP      X, $BBB8
    2838: 00          NOP
    2839: 63 28 0A    BBS      B3 $28, $2846
    283C: 0B 28       ASL      $28
    283E: CF          MUL      YA
    283F: 00          NOP
    2840: 07 93       OR       A, [$93+X]
    2842: 96 99 9C    ADC      A, $9C99+Y
    2845: 9F          XCN      A
    2846: A2 A5       SET1     $A5, 5
    2848: A8 AB       SBC      A, #$AB
    284A: AE          POP      A
    284B: B1          TCALL    B
    284C: B4 B7       SBC      A, $B7+X
    284E: BA BD       MOVW     YA, $BD
    2850: C0          DI
    2851: 95 98 9B    ADC      A, $9B98+X
    2854: 9E          DIV      YA, X
    2855: A1          TCALL    A
    2856: A4 A7       SBC      A, $A7
    2858: AA AD B0    MOV1     C, $B0AD
    285B: B3 B6 B9    BBC      B5 $B6, $2817
    285E: BC          INC      A
    285F: BF          MOV      A, (X)+
    2860: C2 00       SET1     $00, 6
    2862: 04 0B       OR       A, $0B
    2864: 1E C3 64    CMP      X, $64C3
    2867: 0B C9       ASL      $C9
    2869: 07 93       OR       A, [$93+X]
    286B: 96 99 9C    ADC      A, $9C99+Y
    286E: 9F          XCN      A
    286F: A2 A5       SET1     $A5, 5
    2871: A8 AB       SBC      A, #$AB
    2873: AE          POP      A
    2874: B1          TCALL    B
    2875: B4 B7       SBC      A, $B7+X
    2877: BA BD       MOVW     YA, $BD
    2879: C0          DI
    287A: 95 98 9B    ADC      A, $9B98+X
    287D: 9E          DIV      YA, X
    287E: A1          TCALL    A
    287F: A4 A7       SBC      A, $A7
    2881: AA AD B0    MOV1     C, $B0AD
    2884: B3 B6 B9    BBC      B5 $B6, $2840
    2887: BC          INC      A
    2888: BF          MOV      A, (X)+
    2889: C2 00       SET1     $00, 6
    288B: A0          EI
    288C: 28 0A       AND      A, #$0A
    288E: 0B 28       ASL      $28
    2890: CC 00 0F    MOV      $0F00, Y
    2893: 8E          POP      PSW
    2894: ED          NOTC
    2895: 3C          ROL      A
    2896: 94 ED       ADC      A, $ED+X
    2898: 82 93       SET1     $93, 4
    289A: ED          NOTC
    289B: 1E 1F 8D    CMP      X, $8D1F
    289E: 00          NOP
    289F: 04 0B       OR       A, $0B
    28A1: 1E C8 C8    CMP      X, $C8C8
    28A4: 0B C9       ASL      $C9
    28A6: 0F          BRK
    28A7: 8E          POP      PSW
    28A8: ED          NOTC
    28A9: 28 94       AND      A, #$94
    28AB: ED          NOTC
    28AC: 32 93       CLR1     $93, 1
    28AE: ED          NOTC
    28AF: 1E 1F 8D    CMP      X, $8D1F
    28B2: 00          NOP
    28B3: CC 28 0A    MOV      $0A28, Y
    28B6: 16 BE C8    OR       A, $C8BE+Y
    28B9: 00          NOP
    28BA: 07 98       OR       A, [$98+X]
    28BC: 98 93 87    ADC      $93, #$87
    28BF: 87 B0       ADC      A, [$B0+X]
    28C1: 98 C9 87    ADC      $C9, #$87
    28C4: B0 B0       BCS      $2876
    28C6: 98 C9 87    ADC      $C9, #$87
    28C9: C3 00 04    BBS      B6 $00, $28D0
    28CC: 16 BE C5    OR       A, $C5BE+Y
    28CF: 00          NOP
    28D0: 03 B4 B4    BBS      B0 $B4, $2887
    28D3: E1          TCALL    E
    28D4: 0F          BRK
    28D5: B4 B4       SBC      A, $B4+X
    28D7: E1          TCALL    E
    28D8: 02 9F       SET1     $9F, 0
    28DA: 9F          XCN      A
    28DB: E1          TCALL    E
    28DC: 12 9F       CLR1     $9F, 0
    28DE: 9F          XCN      A
    28DF: E1          TCALL    E
    28E0: 0A C3 C3    OR1      C, $C3C3
    28E3: E1          TCALL    E
    28E4: 0F          BRK
    28E5: B0 B0       BCS      $2897
    28E7: E1          TCALL    E
    28E8: 02 AB       SET1     $AB, 0
    28EA: AB E1       INC      $E1
    28EC: 12 9F       CLR1     $9F, 0
    28EE: 9F          XCN      A
    28EF: 00          NOP
    28F0: 04 1D       OR       A, $1D
    28F2: F0 CA       BEQ      $28BE
    28F4: 00          NOP
    28F5: 0B 9F       ASL      $9F
    28F7: 9F          XCN      A
    28F8: 9F          XCN      A
    28F9: 9F          XCN      A
    28FA: 9F          XCN      A
    28FB: 00          NOP
    28FC: 1C          ASL      A
    28FD: 29 0A 12    AND      $0A, $12
    2900: BE          DAS      A
    2901: CF          MUL      YA
    2902: 00          NOP
    2903: 07 9A       OR       A, [$9A+X]
    2905: 9C          DEC      A
    2906: A1          TCALL    A
    2907: A3 ED 8C    BBS      B5 $ED, $2896
    290A: A6          SBC      A, (X)
    290B: A8 AD       SBC      A, #$AD
    290D: AF          MOV      (X)+, A
    290E: ED          NOTC
    290F: 78 B2 B4    CMP      $B2, #$B4
    2912: B9          SBC      X, Y
    2913: BB ED       INC      $ED+X
    2915: 5A BE       CMPW     YA, $BE
    2917: C0          DI
    2918: C5 C7 00    MOV      $00C7, A
    291B: 04 12       OR       A, $12
    291D: 64 C5       CMP      A, $C5
    291F: 00          NOP
    2920: 1B C9       ASL      $C9+X
    2922: C9 07 9A    MOV      $9A07, X
    2925: 9C          DEC      A
    2926: A1          TCALL    A
    2927: A3 ED 50    BBS      B5 $ED, $297A
    292A: A6          SBC      A, (X)
    292B: A8 AD       SBC      A, #$AD
    292D: AF          MOV      (X)+, A
    292E: ED          NOTC
    292F: 32 B2       CLR1     $B2, 1
    2931: B4 B9       SBC      A, $B9+X
    2933: BB ED       INC      $ED+X
    2935: 1E BE C0    CMP      X, $C0BE
    2938: C5 C7 00    MOV      $00C7, A
    293B: 55 29 0A    EOR      A, $0A29+X
    293E: 12 96       CLR1     $96, 0
    2940: CC 00 33    MOV      $3300, Y
    2943: B4 B5       SBC      A, $B5+X
    2945: BC          INC      A
    2946: B9          SBC      X, Y
    2947: BA BF       MOVW     YA, $BF
    2949: ED          NOTC
    294A: 78 C7 C2    CMP      $C7, #$C2
    294D: BD          MOV      SP, X
    294E: ED          NOTC
    294F: 50 C6       BVC      $2917
    2951: BF          MOV      A, (X)+
    2952: BA 00       MOVW     YA, $00
    2954: 04 12       OR       A, $12
    2956: 64 C5       CMP      A, $C5
    2958: 00          NOP
    2959: 1B C9       ASL      $C9+X
    295B: 33 B4 B5    BBC      B1 $B4, $2913
    295E: BC          INC      A
    295F: ED          NOTC
    2960: 50 B9       BVC      $291B
    2962: BA BF       MOVW     YA, $BF
    2964: ED          NOTC
    2965: 3C          ROL      A
    2966: C7 C2       MOV      [$C2+X], A
    2968: BD          MOV      SP, X
    2969: ED          NOTC
    296A: 1E C6 BF    CMP      X, $BFC6
    296D: BA 00       MOVW     YA, $00
    296F: 92 29       CLR1     $29, 4
    2971: 0A 04 78    OR1      C, $7804
    2974: CB 00       MOV      $00, Y
    2976: 07 B4       OR       A, [$B4+X]
    2978: A4 B4       SBC      A, $B4
    297A: B5 B2 B5    SBC      A, $B5B2+X
    297D: 1B BC       ASL      $BC+X
    297F: 07 E1       OR       A, [$E1+X]
    2981: 02 ED       SET1     $ED, 0
    2983: 28 BC       AND      A, #$BC
    2985: ED          NOTC
    2986: 1E BC E1    CMP      X, $E1BC
    2989: 12 ED       CLR1     $ED, 0
    298B: 14 BC       OR       A, $BC+X
    298D: ED          NOTC
    298E: 0A BC 00    OR1      C, $00BC
    2991: 04 04       OR       A, $04
    2993: 78 C9 00    CMP      $C9, #$00
    2996: 07 B0       OR       A, [$B0+X]
    2998: AB B0       INC      $B0
    299A: B2 AF       CLR1     $AF, 5
    299C: B2 1B       CLR1     $1B, 5
    299E: B4 07       SBC      A, $07+X
    29A0: E1          TCALL    E
    29A1: 12 ED       CLR1     $ED, 0
    29A3: 28 B4       AND      A, #$B4
    29A5: ED          NOTC
    29A6: 1E B4 E1    CMP      X, $E1B4
    29A9: 02 ED       SET1     $ED, 0
    29AB: 14 B4       OR       A, $B4+X
    29AD: ED          NOTC
    29AE: 0A B4 00    OR1      C, $00B4
    29B1: DB 29       MOV      $29+X, Y
    29B3: 0A 05 78    OR1      C, $7805
    29B6: CB 00       MOV      $00, Y
    29B8: 4B B1       LSR      $B1
    29BA: B2 B4       CLR1     $B4, 5
    29BC: B8 C9 B6    SBC      $C9, #$B6
    29BF: B9          SBC      X, Y
    29C0: C9 ED 3C    MOV      $3CED, X
    29C3: B8 E1 02    SBC      $E1, #$02
    29C6: ED          NOTC
    29C7: 28 B9       AND      A, #$B9
    29C9: C9 ED 1E    MOV      $1EED, X
    29CC: B8 E1 12    SBC      $E1, #$12
    29CF: ED          NOTC
    29D0: 14 B9       OR       A, $B9+X
    29D2: C9 ED 0A    MOV      $0AED, X
    29D5: B8 B9 C9    SBC      $B9, #$C9
    29D8: B8 00 04    SBC      $00, #$04
    29DB: 05 78 C9    OR       A, $C978
    29DE: 00          NOP
    29DF: 4B A8       LSR      $A8
    29E1: AA AC AF    MOV1     C, $AFAC
    29E4: C9 AD B1    MOV      $B1AD, X
    29E7: C9 ED 32    MOV      $32ED, X
    29EA: AF          MOV      (X)+, A
    29EB: E1          TCALL    E
    29EC: 12 ED       CLR1     $ED, 0
    29EE: 1E B1 C9    CMP      X, $C9B1
    29F1: E1          TCALL    E
    29F2: 02 ED       SET1     $ED, 0
    29F4: 14 AF       OR       A, $AF+X
    29F6: ED          NOTC
    29F7: 0A B1 C9    OR1      C, $C9B1
    29FA: AF          MOV      (X)+, A
    29FB: B1          TCALL    B
    29FC: C9 AF 00    MOV      $00AF, X
    29FF: 04 10       OR       A, $10
    2A01: 46          EOR      A, (X)
    2A02: CC 00 1F    MOV      $1F00, Y
    2A05: A6          SBC      A, (X)
    2A06: E1          TCALL    E
    2A07: 08 ED       OR       A, #$ED
    2A09: 32 A7       CLR1     $A7, 1
    2A0B: 00          NOP
    2A0C: 04 0D       OR       A, $0D
    2A0E: 50 CA       BVC      $29DA
    2A10: 00          NOP
    2A11: 05 B0 ED    OR       A, $EDB0
    2A14: 3C          ROL      A
    2A15: 0B B7       ASL      $B7
    2A17: ED          NOTC
    2A18: 50 45       BVC      $2A5F
    2A1A: B0 00       BCS      $2A1C
    2A1C: 28 2A       AND      A, #$2A
    2A1E: 0A 07 6E    OR1      C, $6E07
    2A21: CC 00 0B    MOV      $0B00, Y
    2A24: A4 B0       SBC      A, $B0
    2A26: 00          NOP
    2A27: 04 07       OR       A, $07
    2A29: 1E C5 14    CMP      X, $14C5
    2A2C: 1B C9       ASL      $C9+X
    2A2E: 0B A4       ASL      $A4
    2A30: B0 00       BCS      $2A32
    2A32: 3E 2A       CMP      X, $2A
    2A34: 0A 07 6E    OR1      C, $6E07
    2A37: CC 00 0B    MOV      $0B00, Y
    2A3A: A6          SBC      A, (X)
    2A3B: B2 00       CLR1     $00, 5
    2A3D: 04 07       OR       A, $07
    2A3F: 1E C5 14    CMP      X, $14C5
    2A42: 1B C9       ASL      $C9+X
    2A44: 0B A6       ASL      $A6
    2A46: B2 00       CLR1     $00, 5
    2A48: 54 2A       EOR      A, $2A+X
    2A4A: 0A 07 6E    OR1      C, $6E07
    2A4D: CC 00 0B    MOV      $0B00, Y
    2A50: A8 B4       SBC      A, #$B4
    2A52: 00          NOP
    2A53: 04 07       OR       A, $07
    2A55: 1E C5 14    CMP      X, $14C5
    2A58: 1B C9       ASL      $C9+X
    2A5A: 0B A8       ASL      $A8
    2A5C: B4 00       SBC      A, $00+X
    2A5E: 6A 2A 0A    AND1     C, /$0A2A
    2A61: 07 6E       OR       A, [$6E+X]
    2A63: CC 00 0B    MOV      $0B00, Y
    2A66: A9 B5 00    SBC      $B5, $00
    2A69: 04 07       OR       A, $07
    2A6B: 1E C5 14    CMP      X, $14C5
    2A6E: 1B C9       ASL      $C9+X
    2A70: 0B A9       ASL      $A9
    2A72: B5 00 80    SBC      A, $8000+X
    2A75: 2A 0A 07    OR1      /C, $070A
    2A78: 6E CC 00    DBNZ     $CC, $2A7B
    2A7B: 0B AB       ASL      $AB
    2A7D: B7 00       SBC      A, [$00]+Y
    2A7F: 04 07       OR       A, $07
    2A81: 1E C5 14    CMP      X, $14C5
    2A84: 1B C9       ASL      $C9+X
    2A86: 0B AB       ASL      $AB
    2A88: B7 00       SBC      A, [$00]+Y
    2A8A: AA 2A 0A    MOV1     C, $0A2A
    2A8D: 07 6E       OR       A, [$6E+X]
    2A8F: CC 00 07    MOV      $0700, Y
    2A92: AB AC       INC      $AC
    2A94: AF          MOV      (X)+, A
    2A95: B0 AF       BCS      $2A46
    2A97: AC AB ED    INC      $EDAB
    2A9A: 32 AB       CLR1     $AB, 1
    2A9C: AC ED 1E    INC      $1EED
    2A9F: AF          MOV      (X)+, A
    2AA0: B0 ED       BCS      $2A8F
    2AA2: 14 AF       OR       A, $AF+X
    2AA4: AC ED 0A    INC      $0AED
    2AA7: AB 00       INC      $00
    2AA9: 04 07       OR       A, $07
    2AAB: 1E C5 14    CMP      X, $14C5
    2AAE: 1B C9       ASL      $C9+X
    2AB0: 07 B7       OR       A, [$B7+X]
    2AB2: B8 BB BC    SBC      $BB, #$BC
    2AB5: BB B8       INC      $B8+X
    2AB7: B7 E1       SBC      A, [$E1]+Y
    2AB9: 02 ED       SET1     $ED, 0
    2ABB: 14 AB       OR       A, $AB+X
    2ABD: AC AF B0    INC      $B0AF
    2AC0: ED          NOTC
    2AC1: 0A AF AC    OR1      C, $ACAF
    2AC4: AB 00       INC      $00
    2AC6: D3 2A 0A    BBC      B6 $2A, $2AD3
    2AC9: 15 DC C9    OR       A, $C9DC+X
    2ACC: 00          NOP
    2ACD: 07 B0       OR       A, [$B0+X]
    2ACF: B0 B0       BCS      $2A81
    2AD1: 00          NOP
    2AD2: 04 16       OR       A, $16
    2AD4: 64 C9       CMP      A, $C9
    2AD6: 00          NOP
    2AD7: 07 8C       OR       A, [$8C+X]
    2AD9: 8C 8C 00    DEC      $008C
    2ADC: 04 16       OR       A, $16
    2ADE: 0A C9 00    OR1      C, $00C9
    2AE1: 07 8C       OR       A, [$8C+X]
    2AE3: 00          NOP
    2AE4: 04 08       OR       A, $08
    2AE6: 2B 0A       ROL      $0A
    2AE8: 07 1E       OR       A, [$1E+X]
    2AEA: C9 00 0B    MOV      $0B00, X
    2AED: AD ED       CMP      Y, #$ED
    2AEF: 32 B2       CLR1     $B2, 1
    2AF1: ED          NOTC
    2AF2: 46          EOR      A, (X)
    2AF3: B3 ED 5A    BBC      B5 $ED, $2B50
    2AF6: B7 B8       SBC      A, [$B8]+Y
    2AF8: BE          DAS      A
    2AF9: BF          MOV      A, (X)+
    2AFA: ED          NOTC
    2AFB: 1E BF ED    CMP      X, $EDBF
    2AFE: 46          EOR      A, (X)
    2AFF: B9          SBC      X, Y
    2B00: ED          NOTC
    2B01: 28 ED       AND      A, #$ED
    2B03: 14 B9       OR       A, $B9+X
    2B05: B9          SBC      X, Y
    2B06: 00          NOP
    2B07: 04 07       OR       A, $07
    2B09: 14 CF       OR       A, $CF+X
    2B0B: 1E 1B C9    CMP      X, $C91B
    2B0E: 0B AD       ASL      $AD
    2B10: ED          NOTC
    2B11: 1E B2 ED    CMP      X, $EDB2
    2B14: 28 B3       AND      A, #$B3
    2B16: B7 ED       SBC      A, [$ED]+Y
    2B18: 32 B8       CLR1     $B8, 1
    2B1A: BE          DAS      A
    2B1B: BF          MOV      A, (X)+
    2B1C: ED          NOTC
    2B1D: 0A BF B9    OR1      C, $B9BF
    2B20: B9          SBC      X, Y
    2B21: B9          SBC      X, Y
    2B22: 00          NOP
    2B23: 3A 2B       INCW     $2B
    2B25: 0A 08 32    OR1      C, $3208
    2B28: C9 00 0F    MOV      $0F00, X
    2B2B: B9          SBC      X, Y
    2B2C: ED          NOTC
    2B2D: 3C          ROL      A
    2B2E: BE          DAS      A
    2B2F: ED          NOTC
    2B30: 64 BF       CMP      A, $BF
    2B32: ED          NOTC
    2B33: 46          EOR      A, (X)
    2B34: C3 ED 1E    BBS      B6 $ED, $2B55
    2B37: C2 00       SET1     $00, 6
    2B39: 04 08       OR       A, $08
    2B3B: 14 CC       OR       A, $CC+X
    2B3D: 00          NOP
    2B3E: 1B C9       ASL      $C9+X
    2B40: 0F          BRK
    2B41: B9          SBC      X, Y
    2B42: ED          NOTC
    2B43: 1E BE ED    CMP      X, $EDBE
    2B46: 28 BF       AND      A, #$BF
    2B48: ED          NOTC
    2B49: 14 C3       OR       A, $C3+X
    2B4B: ED          NOTC
    2B4C: 0A C2 00    OR1      C, $00C2
    2B4F: 04 0D       OR       A, $0D
    2B51: 8C CA 00    DEC      $00CA
    2B54: 33 AD B5    BBC      B1 $AD, $2B0C
    2B57: 05 B0 00    OR       A, $00B0
    2B5A: 04 12       OR       A, $12
    2B5C: BE          DAS      A
    2B5D: CA 00 33    MOV1     $3300, C
    2B60: BC          INC      A
    2B61: BE          DAS      A
    2B62: B4 00       SBC      A, $00+X
    2B64: 04 12       OR       A, $12
    2B66: C8 CA       MOV      X, #$CA
    2B68: 00          NOP
    2B69: 33 B7 ED    BBC      B1 $B7, $2B59
    2B6C: 8C C7 00    DEC      $00C7
    2B6F: 04 12       OR       A, $12
    2B71: B4 CA       SBC      A, $CA+X
    2B73: 00          NOP
    2B74: 07 B6       OR       A, [$B6+X]
    2B76: B8 B4 00    SBC      $B4, #$00
    2B79: 04 12       OR       A, $12
    2B7B: BE          DAS      A
    2B7C: CA 00 33    MOV1     $3300, C
    2B7F: B7 B8       SBC      A, [$B8]+Y
    2B81: B8 B9 00    SBC      $B9, #$00
    2B84: 93 2B 0A    BBC      B4 $2B, $2B91
    2B87: 0D          PUSH     PSW
    2B88: 64 C9       CMP      A, $C9
    2B8A: 00          NOP
    2B8B: 33 B4 35    BBC      B1 $B4, $2BC3
    2B8E: ED          NOTC
    2B8F: 50 C0       BVC      $2B51
    2B91: 00          NOP
    2B92: 04 0D       OR       A, $0D
    2B94: 0A CC 00    OR1      C, $00CC
    2B97: 03 C9 33    BBS      B0 $C9, $2BCD
    2B9A: B4 0B       SBC      A, $0B+X
    2B9C: C0          DI
    2B9D: 00          NOP
    2B9E: B8 2B 0A    SBC      $2B, #$0A
    2BA1: 08 BE       OR       A, #$BE
    2BA3: CA 00 33    MOV1     $3300, C
    2BA6: B0 B5       BCS      $2B5D
    2BA8: B9          SBC      X, Y
    2BA9: 03 C0 B7    BBS      B0 $C0, $2B63
    2BAC: C0          DI
    2BAD: B7 ED       SBC      A, [$ED]+Y
    2BAF: 32 C0       CLR1     $C0, 1
    2BB1: B7 ED       SBC      A, [$ED]+Y
    2BB3: 1E C0 B7    CMP      X, $B7C0
    2BB6: 00          NOP
    2BB7: 04 08       OR       A, $08
    2BB9: 3C          ROL      A
    2BBA: CA 00 33    MOV1     $3300, C
    2BBD: C9 B0 B5    MOV      $B5B0, X
    2BC0: B9          SBC      X, Y
    2BC1: 03 C0 B7    BBS      B0 $C0, $2B7B
    2BC4: C0          DI
    2BC5: B7 ED       SBC      A, [$ED]+Y
    2BC7: 1E C0 B7    CMP      X, $B7C0
    2BCA: ED          NOTC
    2BCB: 14 C0       OR       A, $C0+X
    2BCD: B7 00       SBC      A, [$00]+Y
    2BCF: DA 2B       MOVW     $2B, YA
    2BD1: 0A 10 BE    OR1      C, $BE10
    2BD4: C9 00 13    MOV      $1300, X
    2BD7: 9C          DEC      A
    2BD8: 00          NOP
    2BD9: 04 10       OR       A, $10
    2BDB: 14 CF       OR       A, $CF+X
    2BDD: 00          NOP
    2BDE: 4B C9       LSR      $C9
    2BE0: 13 9C 00    BBC      B0 $9C, $2BE3
    2BE3: EE          POP      Y
    2BE4: 2B 0A       ROL      $0A
    2BE6: 07 8C       OR       A, [$8C+X]
    2BE8: C9 00 13    MOV      $1300, X
    2BEB: 98 00 04    ADC      $00, #$04
    2BEE: 07 78       OR       A, [$78+X]
    2BF0: CB 00       MOV      $00, Y
    2BF2: 13 8C 00    BBC      B0 $8C, $2BF5
    2BF5: 00          NOP
    2BF6: 2C 0A 08    ROL      $080A
    2BF9: 78 CA 00    CMP      $CA, #$00
    2BFC: 45 C3 00    EOR      A, $00C3
    2BFF: 04 08       OR       A, $08
    2C01: 14 CF       OR       A, $CF+X
    2C03: 64 45       CMP      A, $45
    2C05: C3 E1 05    BBS      B6 $E1, $2C0D
    2C08: C3 00 17    BBS      B6 $00, $2C22
    2C0B: 2C 0A 05    ROL      $050A
    2C0E: 82 C8       SET1     $C8, 4
    2C10: 00          NOP
    2C11: 03 B9 13    BBS      B0 $B9, $2C27
    2C14: B0 00       BCS      $2C16
    2C16: 04 05       OR       A, $05
    2C18: 50 CF       BVC      $2BE9
    2C1A: 00          NOP
    2C1B: 03 C9 AD    BBS      B0 $C9, $2BCB
    2C1E: E1          TCALL    E
    2C1F: 05 13 A4    OR       A, $A413
    2C22: 00          NOP
    2C23: 38 2C 0A    AND      $2C, #$0A
    2C26: 05 78 C2    OR       A, $C278
    2C29: 00          NOP
    2C2A: 03 BC ED    BBS      B0 $BC, $2C1A
    2C2D: 28 E1       AND      A, #$E1
    2C2F: 12 BC       CLR1     $BC, 0
    2C31: ED          NOTC
    2C32: 1E E1 03    CMP      X, $03E1
    2C35: BC          INC      A
    2C36: 00          NOP
    2C37: 04 05       OR       A, $05
    2C39: 64 D2       CMP      A, $D2
    2C3B: 00          NOP
    2C3C: 03 C0 ED    BBS      B0 $C0, $2C2C
    2C3F: 82 E1       SET1     $E1, 4
    2C41: 05 C0 E1    OR       A, $E1C0
    2C44: 12 C0       CLR1     $C0, 0
    2C46: ED          NOTC
    2C47: 1E E1 03    CMP      X, $03E1
    2C4A: C0          DI
    2C4B: 00          NOP
    2C4C: 62 2C       SET1     $2C, 3
    2C4E: 0A 0D 78    OR1      C, $780D
    2C51: CA 00 0B    MOV1     $0B00, C
    2C54: B7 C3       SBC      A, [$C3]+Y
    2C56: E0          CLRV
    2C57: 0E 0F A3    TSET1    $A30F
    2C5A: E1          TCALL    E
    2C5B: 02 A4       SET1     $A4, 0
    2C5D: E1          TCALL    E
    2C5E: 10 AB       BPL      $2C0B
    2C60: 00          NOP
    2C61: 04 05       OR       A, $05
    2C63: 64 CA       CMP      A, $CA
    2C65: 00          NOP
    2C66: 03 BC C3    BBS      B0 $BC, $2C2C
    2C69: 49 B4 C3    EOR      $B4, $C3
    2C6C: C7 ED       MOV      [$ED+X], A
    2C6E: 78 E0 0E    CMP      $E0, #$0E
    2C71: 0B B7       ASL      $B7
    2C73: 9F          XCN      A
    2C74: AD 00       CMP      Y, #$00
    2C76: 04 0A       OR       A, $0A
    2C78: D2 CA       CLR1     $CA, 6
    2C7A: 00          NOP
    2C7B: 33 C0 BC    BBC      B1 $C0, $2C3A
    2C7E: 00          NOP
    2C7F: 02 04       SET1     $04, 0
    2C81: 02 08       SET1     $08, 0
    2C83: E6          MOV      A, (X)
    2C84: CA C5 00    MOV1     $00C5, C
    2C87: 02 04       SET1     $04, 0
    2C89: 02 08       SET1     $08, 0
    2C8B: E6          MOV      A, (X)
    2C8C: CA C5 F0    MOV1     $F0C5, C
    2C8F: 8A 14 08    EOR1     C, $0814
    2C92: E8 64       MOV      A, #$64
    2C94: 5F 2E 16    JMP      $162E
    2C97: 8A 14 08    EOR1     C, $0814
    2C9A: E8 F0       MOV      A, #$F0
    2C9C: 5F 2E 16    JMP      $162E
    2C9F: 8A 14 08    EOR1     C, $0814
    2CA2: E8 10       MOV      A, #$10
    2CA4: 5F 63 16    JMP      $1663
    2CA7: 8A 14 08    EOR1     C, $0814
    2CAA: E8 00       MOV      A, #$00
    2CAC: 5F 63 16    JMP      $1663
    2CAF: 00          NOP
    2CB0: 00          NOP
    2CB1: 00          NOP
    2CB2: 00          NOP
    2CB3: 00          NOP
    2CB4: 00          NOP
    2CB5: 00          NOP
    2CB6: 00          NOP
    2CB7: 00          NOP
    2CB8: 00          NOP
    2CB9: 00          NOP
    2CBA: 00          NOP
    2CBB: 00          NOP
    2CBC: 00          NOP
    2CBD: 78 64 00    CMP      $64, #$00
    2CC0: 00          NOP
    2CC1: 00          NOP
    2CC2: 78 64 64    CMP      $64, #$64
    2CC5: 00          NOP
    2CC6: 00          NOP
    2CC7: 78 64 64    CMP      $64, #$64
    2CCA: 00          NOP
    2CCB: 00          NOP
    2CCC: 78 64 64    CMP      $64, #$64
    2CCF: 64 00       CMP      A, $00
    2CD1: 78 64 64    CMP      $64, #$64
    2CD4: 64 78       CMP      A, $78
    2CD6: 78 8A 14    CMP      $8A, #$14
    2CD9: 08 8F       OR       A, #$8F
    2CDB: AF          MOV      (X)+, A
    2CDC: 22 8F       SET1     $8F, 1
    2CDE: 2C 23 5F    ROL      $5F23
    2CE1: 37 2D       AND      A, [$2D]+Y
    2CE3: 8A 14 08    EOR1     C, $0814
    2CE6: 8F B4 22    MOV      $B4, #$22
    2CE9: 8F 2C 23    MOV      $2C, #$23
    2CEC: 5F 37 2D    JMP      $2D37
    2CEF: 8A 14 08    EOR1     C, $0814
    2CF2: 8F B9 22    MOV      $B9, #$22
    2CF5: 8F 2C 23    MOV      $2C, #$23
    2CF8: 5F 37 2D    JMP      $2D37
    2CFB: 8A 14 08    EOR1     C, $0814
    2CFE: 8F BE 22    MOV      $BE, #$22
    2D01: 8F 2C 23    MOV      $2C, #$23
    2D04: 5F 37 2D    JMP      $2D37
    2D07: 8A 14 08    EOR1     C, $0814
    2D0A: 8F C3 22    MOV      $C3, #$22
    2D0D: 8F 2C 23    MOV      $2C, #$23
    2D10: 5F 37 2D    JMP      $2D37
    2D13: 8A 14 08    EOR1     C, $0814
    2D16: 8F C3 22    MOV      $C3, #$22
    2D19: 8F 2C 23    MOV      $2C, #$23
    2D1C: 5F 37 2D    JMP      $2D37
    2D1F: 8A 14 08    EOR1     C, $0814
    2D22: 8F CD 22    MOV      $CD, #$22
    2D25: 8F 2C 23    MOV      $2C, #$23
    2D28: 5F 37 2D    JMP      $2D37
    2D2B: 8A 14 08    EOR1     C, $0814
    2D2E: 8F D2 22    MOV      $D2, #$22
    2D31: 8F 2C 23    MOV      $2C, #$23
    2D34: 5F 37 2D    JMP      $2D37
    2D37: 8D 00       MOV      Y, #$00
    2D39: CD 02       MOV      X, #$02
    2D3B: F7 22       MOV      A, [$22]+Y
    2D3D: C4 20       MOV      $20, A
    2D3F: E8 14       MOV      A, #$14
    2D41: 6D          PUSH     Y
    2D42: 3F 6E 16    CALL     $166E
    2D45: EE          POP      Y
    2D46: FC          INC      Y
    2D47: CD 04       MOV      X, #$04
    2D49: F7 22       MOV      A, [$22]+Y
    2D4B: C4 20       MOV      $20, A
    2D4D: E8 14       MOV      A, #$14
    2D4F: 6D          PUSH     Y
    2D50: 3F 6E 16    CALL     $166E
    2D53: EE          POP      Y
    2D54: FC          INC      Y
    2D55: CD 06       MOV      X, #$06
    2D57: F7 22       MOV      A, [$22]+Y
    2D59: C4 20       MOV      $20, A
    2D5B: E8 14       MOV      A, #$14
    2D5D: 6D          PUSH     Y
    2D5E: 3F 6E 16    CALL     $166E
    2D61: EE          POP      Y
    2D62: FC          INC      Y
    2D63: CD 08       MOV      X, #$08
    2D65: F7 22       MOV      A, [$22]+Y
    2D67: C4 20       MOV      $20, A
    2D69: E8 14       MOV      A, #$14
    2D6B: 6D          PUSH     Y
    2D6C: 3F 6E 16    CALL     $166E
    2D6F: EE          POP      Y
    2D70: FC          INC      Y
    2D71: CD 0A       MOV      X, #$0A
    2D73: F7 22       MOV      A, [$22]+Y
    2D75: C4 20       MOV      $20, A
    2D77: E8 14       MOV      A, #$14
    2D79: 6D          PUSH     Y
    2D7A: 3F 6E 16    CALL     $166E
    2D7D: EE          POP      Y
    2D7E: 6F          RET
    2D7F: 8A 14 08    EOR1     C, $0814
    2D82: CD 0E       MOV      X, #$0E
    2D84: E8 5A       MOV      A, #$5A
    2D86: 3F 6A 16    CALL     $166A
    2D89: 6F          RET
    2D8A: 8A 14 08    EOR1     C, $0814
    2D8D: CD 0E       MOV      X, #$0E
    2D8F: E8 FA       MOV      A, #$FA
    2D91: 3F 6A 16    CALL     $166A
    2D94: 6F          RET
    2D95: 8A 14 08    EOR1     C, $0814
    2D98: CD 0E       MOV      X, #$0E
    2D9A: E8 28       MOV      A, #$28
    2D9C: 3F 6A 16    CALL     $166A
    2D9F: 6F          RET
    2DA0: 8A 14 08    EOR1     C, $0814
    2DA3: CD 0E       MOV      X, #$0E
    2DA5: E8 00       MOV      A, #$00
    2DA7: 3F 6A 16    CALL     $166A
    2DAA: 6F          RET
    2DAB: E5 38 04    MOV      A, $0438
    2DAE: 68 00       CMP      A, #$00
    2DB0: D0 01       BNE      $2DB3
    2DB2: 6F          RET
    2DB3: 9C          DEC      A
    2DB4: C5 38 04    MOV      $0438, A
    2DB7: 68 00       CMP      A, #$00
    2DB9: D0 F7       BNE      $2DB2
    2DBB: E8 00       MOV      A, #$00
    2DBD: 3F 63 16    CALL     $1663
    2DC0: 5F 3B 16    JMP      $163B
    2DC3: 8A 14 08    EOR1     C, $0814
    2DC6: E8 70       MOV      A, #$70
    2DC8: C5 38 04    MOV      $0438, A
    2DCB: E8 0E       MOV      A, #$0E
    2DCD: 3F 63 16    CALL     $1663
    2DD0: E8 C8       MOV      A, #$C8
    2DD2: 5F 32 16    JMP      $1632
    2DD5: E5 3A 04    MOV      A, $043A
    2DD8: 68 00       CMP      A, #$00
    2DDA: D0 01       BNE      $2DDD
    2DDC: 6F          RET
    2DDD: 9C          DEC      A
    2DDE: C5 3A 04    MOV      $043A, A
    2DE1: 68 00       CMP      A, #$00
    2DE3: D0 F7       BNE      $2DDC
    2DE5: E8 A0       MOV      A, #$A0
    2DE7: 5F 2E 16    JMP      $162E
    2DEA: 8A 14 08    EOR1     C, $0814
    2DED: E8 28       MOV      A, #$28
    2DEF: C5 3A 04    MOV      $043A, A
    2DF2: 6F          RET
    2DF3: E5 39 04    MOV      A, $0439
    2DF6: 68 00       CMP      A, #$00
    2DF8: D0 01       BNE      $2DFB
    2DFA: 6F          RET
    2DFB: 9C          DEC      A
    2DFC: C5 39 04    MOV      $0439, A
    2DFF: 68 00       CMP      A, #$00
    2E01: D0 F7       BNE      $2DFA
    2E03: E8 F0       MOV      A, #$F0
    2E05: 5F 2E 16    JMP      $162E
    2E08: 8A 14 08    EOR1     C, $0814
    2E0B: E8 69       MOV      A, #$69
    2E0D: C5 39 04    MOV      $0439, A
    2E10: 6F          RET
    2E11: E4 04       MOV      A, $04
    2E13: 68 1C       CMP      A, #$1C
    2E15: D0 05       BNE      $2E1C
    2E17: E8 02       MOV      A, #$02
    2E19: 3F 72 07    CALL     $0772
    2E1C: 6F          RET
    2E1D: 28 7F       AND      A, #$7F
    2E1F: C4 20       MOV      $20, A
    2E21: 68 08       CMP      A, #$08
    2E23: 90 13       BCC      $2E38
    2E25: E4 20       MOV      A, $20
    2E27: 68 07       CMP      A, #$07
    2E29: D0 0C       BNE      $2E37
    2E2B: E5 B7 04    MOV      A, $04B7
    2E2E: 68 07       CMP      A, #$07
    2E30: D0 05       BNE      $2E37
    2E32: E8 00       MOV      A, #$00
    2E34: C5 B3 04    MOV      $04B3, A
    2E37: 6F          RET
    2E38: E5 B7 04    MOV      A, $04B7
    2E3B: 68 00       CMP      A, #$00
    2E3D: F0 E6       BEQ      $2E25
    2E3F: 68 08       CMP      A, #$08
    2E41: 90 E2       BCC      $2E25
    2E43: E8 00       MOV      A, #$00
    2E45: C5 B3 04    MOV      $04B3, A
