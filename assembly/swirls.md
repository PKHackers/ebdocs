SWIRL-RELATED ASM
-----------------

NOTES:
origA = Top level swirl table (EDF41) entry number

WRAM ADDRESSES:
AEC3: Top level swirl table data 1
AEC4: Top level swirl table data 3
AEC5: Top level swirl table data 2
AEC6: State of bit 0x2 of origX
AEC7: State of bit 0x1 of origX
AEC8: State of bit 0x4 of origX, plus 0x1F

--------------------------------------------------------------------------------------------------------

    C4/A67E: C2 31        REP #$31                  Flags and direct page manipulation
    C4/A680: 0B           PHD
    C4/A681: 48           PHA
    C4/A682: 7B           TDC
    C4/A683: 69 EF FF     ADC #$FFEF
    C4/A686: 5B           TCD
    C4/A687: 68           PLA
    C4/A688: 86 02        STX $02                   $02 = origX
    C4/A68A: 85 04        STA $04                   $04 = origA
    C4/A68C: A5 02        LDA $02                   A = origX
    C4/A68E: 29 02 00     AND #$0002
    C4/A691: F0 09        BEQ $A69C
    C4/A693: E2 20        SEP #$20                  --- origX: bit 0x2 on ---
    C4/A695: A9 01        LDA #$01
    C4/A697: 8D C6 AE     STA $AEC6                 AEC6 = 0x1
    C4/A69A: 80 05        BRA $A6A1
    C4/A69C: E2 20        SEP #$20                  --- origX: bit 0x2 off ---
    C4/A69E: 9C C6 AE     STZ $AEC6                 AEC6 = 0x0
    C4/A6A1: C2 20        REP #$20                  --- Common code ---
    C4/A6A3: A5 02        LDA $02
    C4/A6A5: 29 01 00     AND #$0001
    C4/A6A8: F0 09        BEQ $A6B3
    C4/A6AA: E2 20        SEP #$20                  --- origX: bit 0x1 on ---
    C4/A6AC: A9 01        LDA #$01
    C4/A6AE: 8D C7 AE     STA $AEC7                 AEC7 = 0x1
    C4/A6B1: 80 05        BRA $A6B8
    C4/A6B3: E2 20        SEP #$20                  --- origX: bit 0x1 off ---
    C4/A6B5: 9C C7 AE     STZ $AEC7                 AEC7 = 0x0
    C4/A6B8: C2 20        REP #$20                  --- Common code ---
    C4/A6BA: A5 02        LDA $02
    C4/A6BC: 29 04 00     AND #$0004
    C4/A6BF: F0 09        BEQ $A6CA
    C4/A6C1: E2 20        SEP #$20                  --- origX: bit 0x4 on ---
    C4/A6C3: A9 20        LDA #$20
    C4/A6C5: 8D C8 AE     STA $AEC8                 AEC8 = 0x20
    C4/A6C8: 80 07        BRA $A6D1
    C4/A6CA: E2 20        SEP #$20                  --- origX: bit 0x1 off ---
    C4/A6CC: A9 1F        LDA #$1F
    C4/A6CE: 8D C8 AE     STA $AEC8                 AEC8 = 0x1F
    C4/A6D1: A9 01        LDA #$01                  --- Common code ---
    C4/A6D3: 8D C2 AE     STA $AEC2                 AEC2 = 0x1
    C4/A6D6: C2 20        REP #$20
    C4/A6D8: A9 41 DD     LDA #$DD41
    C4/A6DB: 85 06        STA $06
    C4/A6DD: A9 CE 00     LDA #$00CE
    C4/A6E0: 85 08        STA $08                   $06+08: Top level swirl table
    C4/A6E2: A5 04        LDA $04
    C4/A6E4: 0A           ASL
    C4/A6E5: 0A           ASL
    C4/A6E6: 85 0F        STA $0F                   A, $0F = origA * 4
    C4/A6E8: A6 06        LDX $06
    C4/A6EA: 86 0A        STX $0A
    C4/A6EC: A6 08        LDX $08
    C4/A6EE: 86 0C        STX $0C                   $0A+0C: $06+08
    C4/A6F0: 18           CLC
    C4/A6F1: 65 0A        ADC $0A
    C4/A6F3: 85 0A        STA $0A
    C4/A6F5: E2 20        SEP #$20
    C4/A6F7: A7 0A        LDA [$0A]
    C4/A6F9: 8D C3 AE     STA $AEC3                 AEC3: 1st byte of top level table entry
    C4/A6FC: A0 C4 AE     LDY #$AEC4
    C4/A6FF: C2 20        REP #$20
    C4/A701: A5 0F        LDA $0F
    C4/A703: 1A           INC
    C4/A704: 1A           INC
    C4/A705: A6 06        LDX $06
    C4/A707: 86 0A        STX $0A
    C4/A709: A6 08        LDX $08
    C4/A70B: 86 0C        STX $0C
    C4/A70D: 18           CLC
    C4/A70E: 65 0A        ADC $0A
    C4/A710: 85 0A        STA $0A
    C4/A712: E2 20        SEP #$20
    C4/A714: A7 0A        LDA [$0A]
    C4/A716: 99 00 00     STA $0000,Y               AEC4: 3rd byte of top level table entry
    C4/A719: A2 C5 AE     LDX #$AEC5
    C4/A71C: C2 20        REP #$20
    C4/A71E: A5 0F        LDA $0F
    C4/A720: 1A           INC
    C4/A721: 18           CLC
    C4/A722: 65 06        ADC $06
    C4/A724: 85 06        STA $06
    C4/A726: E2 20        SEP #$20
    C4/A728: A7 06        LDA [$06]
    C4/A72A: 85 0E        STA $0E
    C4/A72C: 9D 00 00     STA $0000,X               $0E, AEC5: 2nd byte of top level table entry
    C4/A72F: C2 20        REP #$20
    C4/A731: AD C7 AE     LDA $AEC7                 A = State of bit 0x1 of origX
    C4/A734: 29 FF 00     AND #$00FF
    C4/A737: F0 0F        BEQ $A748
    C4/A739: E2 20        SEP #$20                  --- origX: bit 0x1 on
    C4/A73B: B9 00 00     LDA $0000,Y               A = Top level swirl table data 3
    C4/A73E: 85 00        STA $00
    C4/A740: A5 0E        LDA $0E
    C4/A742: 18           CLC
    C4/A743: 65 00        ADC $00
    C4/A745: 9D 00 00     STA $0000,X               AEC5 += Top level swirl table data 3

Lengthy explanation: if origX's bit 0x1 was on, add the third byte of the top-level swirl data table (EDF41) to the second byte's store place. This resolves to the last frame of the animation. Perhaps bit 0x1 of origX causes the effect to play backwards?

    C4/A748: A0 CC AE     LDY #$AECC                --- origX: bit 0x1 off and common code
    C4/A74B: C2 20        REP #$20
    C4/A74D: A9 00 00     LDA #$0000
    C4/A750: 85 06        STA $06                   Clear $06
    C4/A752: A9 00 00     LDA #$0000
    C4/A755: 85 08        STA $08                   Clear $08
    C4/A757: A5 06        LDA $06
    C4/A759: 99 00 00     STA $0000,Y               Clear 0xAECC
    C4/A75C: A5 08        LDA $08
    C4/A75E: 99 02 00     STA $0002,Y               Clear 0xAECE
    C4/A761: A5 04        LDA $04                   A = origA (entry number)
    C4/A763: D0 14        BNE $A779
    C4/A765: A9 CE A5     LDA #$A5CE                --- Entry zero in top-level swirl table (normal battle swirl)
    C4/A768: 85 06        STA $06
    C4/A76A: A9 C4 00     LDA #$00C4
    C4/A76D: 85 08        STA $08                   $06+08 = C4A5CE
    C4/A76F: A5 06        LDA $06
    C4/A771: 99 00 00     STA $0000,Y
    C4/A774: A5 08        LDA $08
    C4/A776: 99 02 00     STA $0002,Y               AECC+CE = C4A5CE
    C4/A779: E2 20        SEP #$20                  --- Common code
    C4/A77B: 9C C9 AE     STZ $AEC9                 AEC9 = 0x0
    C4/A77E: 9C CA AE     STZ $AECA                 AECA = 0x0
    C4/A781: A9 01        LDA #$01
    C4/A783: 8D CB AE     STA $AECB                 AECB = 0x1
    C4/A786: C2 20        REP #$20
    C4/A788: A5 02        LDA $02                   A = origX
    C4/A78A: 29 80 00     AND #$0080
    C4/A78D: F0 16        BEQ $A7A5
    C4/A78F: A5 04        LDA $04                   --- origX: bit 0x80 on
    C4/A791: E2 20        SEP #$20
    C4/A793: 8D E4 AE     STA $AEE4                 AEE4 = origA
    C4/A796: A9 04        LDA #$04
    C4/A798: 8D C3 AE     STA $AEC3                 AEC3 = 0x4
    C4/A79B: 9C E5 AE     STZ $AEE5                 AEE5 = 0x0
    C4/A79E: A9 08        LDA #$08
    C4/A7A0: 8D E6 AE     STA $AEE6                 AEE6 = 0x8
    C4/A7A3: 80 05        BRA $A7AA
    C4/A7A5: E2 20        SEP #$20                  --- origX: bit 0x80 off
    C4/A7A7: 9C E4 AE     STZ $AEE4                 AEE4 = 0x0
    C4/A7AA: 22 AA B0 C0  JSR $C0B0AA               --- Common code

    C0/B0AA: C2 20        REP #$20
    C0/B0AC: A9 FF 00     LDA #$00FF
    C0/B0AF: 8F 26 21 00  STA $002126               $002126 = 0xFF; wh0 ("Window position designation")
    C0/B0B3: 8F 28 21 00  STA $002128               $002128 = 0xFF; wh2 ("Window position designation")
    C0/B0B7: 6B           RTL

    C4/A7AE: 2B           PLD
    C4/A7AF: 6B           RTL                       End

--------------------------------------------------------------------------------------------------------

    C4/A7B0: C2 31        REP #$31                  Flags and direct page manipulation.
    C4/A7B2: 0B           PHD
    C4/A7B3: 7B           TDC
    C4/A7B4: 69 E9 FF     ADC #$FFE9
    C4/A7B7: 5B           TCD
    C4/A7B8: AD C2 AE     LDA $AEC2
    C4/A7BB: 29 FF 00     AND #$00FF
    C4/A7BE: D0 03        BNE $A7C3
    C4/A7C0: 4C 53 AC     JMP $AC53                 If AEC2 is 0, end.
    C4/A7C3: A0 CC AE     LDY #$AECC                Y = 0xAECC
    C4/A7C6: A9 00 00     LDA #$0000
    C4/A7C9: 85 06        STA $06
    C4/A7CB: A9 00 00     LDA #$0000
    C4/A7CE: 85 08        STA $08                   $06+08 = null
    C4/A7D0: B9 00 00     LDA $0000,Y
    C4/A7D3: 85 0A        STA $0A
    C4/A7D5: B9 02 00     LDA $0002,Y
    C4/A7D8: 85 0C        STA $0C                   $0A+0C = AECC+CE
    C4/A7DA: C5 08        CMP $08                   Check if $0C is null
    C4/A7DC: D0 04        BNE $A7E2
    C4/A7DE: A5 0A        LDA $0A
    C4/A7E0: C5 06        CMP $06
    C4/A7E2: D0 03        BNE $A7E7
    C4/A7E4: 4C 1A AA     JMP $AA1A                 Go to AA1A is $06+08 = $0A+0C (AECC+CE was null)
    C4/A7E7: A2 C2 AE     LDX #$AEC2                However, if there WAS a pointer...
    C4/A7EA: E2 20        SEP #$20
    C4/A7EC: BD 00 00     LDA $0000,X
    C4/A7EF: 3A           DEC
    C4/A7F0: 8D C2 AE     STA $AEC2                 Decrement AEC2
    C4/A7F3: C2 20        REP #$20
    C4/A7F5: 29 FF 00     AND #$00FF
    C4/A7F8: F0 03        BEQ $A7FD
    C4/A7FA: 4C 15 A9     JMP $A915                 If AEC2 is nonzero after the decrement, go to A915.
    C4/A7FD: B9 00 00     LDA $0000,Y
    C4/A800: 85 0A        STA $0A
    C4/A802: B9 02 00     LDA $0002,Y
    C4/A805: 85 0C        STA $0C                   If AEC2 is zero, $0A+0C = AACC+CE.
    C4/A807: E2 20        SEP #$20
    C4/A809: A7 0A        LDA [$0A]
    C4/A80B: 8D C2 AE     STA $AEC2                 Get a byte from the address located in $0A+0C and store to AEC2.
    C4/A80E: C2 20        REP #$20
    C4/A810: 29 FF 00     AND #$00FF
    C4/A813: D0 0D        BNE $A822
    C4/A815: A5 06        LDA $06                   --- If that loaded byte was 0...
    C4/A817: 99 00 00     STA $0000,Y
    C4/A81A: A5 08        LDA $08
    C4/A81C: 99 02 00     STA $0002,Y               $06+08 = AACC+CE
    C4/A81F: 4C 53 AC     JMP $AC53                 And end.
    C4/A822: B9 00 00     LDA $0000,Y               --- If that loaded byte was nonzero...
    C4/A825: 85 06        STA $06
    C4/A827: B9 02 00     LDA $0002,Y
    C4/A82A: 85 08        STA $08                   $06+08 = AACC+CE
    C4/A82C: A0 02 00     LDY #$0002
    C4/A82F: B7 06        LDA [$06],Y               Get the value at ((value at AACC+CE) + 0x2)
    C4/A831: C9 00 80     CMP #$8000
    C4/A834: F0 03        BEQ $A839
    C4/A836: 8D D0 AE     STA $AED0                 If that value is not 0x8000, store to AED0.
    C4/A839: AD CC AE     LDA $AECC
    C4/A83C: 85 06        STA $06
    C4/A83E: AD CE AE     LDA $AECE
    C4/A841: 85 08        STA $08                   Copy AACC+CE to $06+08.
    C4/A843: A0 04 00     LDY #$0004
    C4/A846: B7 06        LDA [$06],Y               Get the value at ((value at AACC+CE) + 0x4)
    C4/A848: C9 00 80     CMP #$8000
    C4/A84B: F0 03        BEQ $A850
    C4/A84D: 8D D2 AE     STA $AED2                 If that value is not 0x8000, store to AED2.
    C4/A850: AD CC AE     LDA $AECC
    C4/A853: 85 06        STA $06
    C4/A855: AD CE AE     LDA $AECE
    C4/A858: 85 08        STA $08
    C4/A85A: A0 06 00     LDY #$0006
    C4/A85D: B7 06        LDA [$06],Y               Get the value at ((value at AACC+CE) + 0x6)
    C4/A85F: C9 00 80     CMP #$8000
    C4/A862: F0 03        BEQ $A867
    C4/A864: 8D D4 AE     STA $AED4                 If that value is not 0x8000, store to AED4.
    C4/A867: AD CC AE     LDA $AECC
    C4/A86A: 85 06        STA $06
    C4/A86C: AD CE AE     LDA $AECE
    C4/A86F: 85 08        STA $08
    C4/A871: A0 08 00     LDY #$0008
    C4/A874: B7 06        LDA [$06],Y               Get the value at ((value at AACC+CE) + 0x8)
    C4/A876: C9 00 80     CMP #$8000
    C4/A879: F0 03        BEQ $A87E
    C4/A87B: 8D D6 AE     STA $AED6                 If that value is not 0x8000, store to AED6.
    C4/A87E: A0 CC AE     LDY #$AECC                Y = 0xAECC
    C4/A881: 84 15        STY $15                   $15 = 0xAECC
    C4/A883: B9 00 00     LDA $0000,Y
    C4/A886: 85 06        STA $06
    C4/A888: B9 02 00     LDA $0002,Y
    C4/A88B: 85 08        STA $08                   Copy AECC+CE to $06+08
    C4/A88D: A0 0A 00     LDY #$000A
    C4/A890: B7 06        LDA [$06],Y               Get the value at ((value at AACC+CE) + 0xA)
    C4/A892: 8D D8 AE     STA $AED8                 Store to AED8.
    C4/A895: A4 15        LDY $15
    C4/A897: B9 00 00     LDA $0000,Y               Copy AECC+CE to $06+08
    C4/A89A: 85 06        STA $06
    C4/A89C: B9 02 00     LDA $0002,Y
    C4/A89F: 85 08        STA $08
    C4/A8A1: A0 0C 00     LDY #$000C
    C4/A8A4: B7 06        LDA [$06],Y               Get the value at ((value at AACC+CE) + 0xC)
    C4/A8A6: 8D DA AE     STA $AEDA                 Store to AEDA.
    C4/A8A9: A4 15        LDY $15
    C4/A8AB: B9 00 00     LDA $0000,Y
    C4/A8AE: 85 06        STA $06
    C4/A8B0: B9 02 00     LDA $0002,Y
    C4/A8B3: 85 08        STA $08
    C4/A8B5: A0 0E 00     LDY #$000E
    C4/A8B8: B7 06        LDA [$06],Y               Get the value at ((value at AACC+CE) + 0xE)
    C4/A8BA: 8D DC AE     STA $AEDC                 Store to AEDC.
    C4/A8BD: A4 15        LDY $15
    C4/A8BF: B9 00 00     LDA $0000,Y
    C4/A8C2: 85 06        STA $06
    C4/A8C4: B9 02 00     LDA $0002,Y
    C4/A8C7: 85 08        STA $08
    C4/A8C9: A0 10 00     LDY #$0010
    C4/A8CC: B7 06        LDA [$06],Y               Get the value at ((value at AACC+CE) + 0x10)
    C4/A8CE: 8D DE AE     STA $AEDE                 Store to AEDE.
    C4/A8D1: A4 15        LDY $15
    C4/A8D3: B9 00 00     LDA $0000,Y
    C4/A8D6: 85 06        STA $06
    C4/A8D8: B9 02 00     LDA $0002,Y
    C4/A8DB: 85 08        STA $08
    C4/A8DD: A0 12 00     LDY #$0012
    C4/A8E0: B7 06        LDA [$06],Y               Get the value at ((value at AACC+CE) + 0x12)
    C4/A8E2: 8D E0 AE     STA $AEE0                 Store to AEE0.
    C4/A8E5: A4 15        LDY $15
    C4/A8E7: B9 00 00     LDA $0000,Y
    C4/A8EA: 85 06        STA $06
    C4/A8EC: B9 02 00     LDA $0002,Y
    C4/A8EF: 85 08        STA $08
    C4/A8F1: A0 14 00     LDY #$0014
    C4/A8F4: B7 06        LDA [$06],Y               Get the value at ((value at AACC+CE) + 0x14)
    C4/A8F6: 8D E2 AE     STA $AEE2                 Store to AEE2.
    C4/A8F9: A4 15        LDY $15
    C4/A8FB: B9 00 00     LDA $0000,Y
    C4/A8FE: 85 06        STA $06
    C4/A900: B9 02 00     LDA $0002,Y
    C4/A903: 85 08        STA $08                   Copy AECC+CE to $06+08
    C4/A905: A9 16 00     LDA #$0016
    C4/A908: 18           CLC
    C4/A909: 65 06        ADC $06
    C4/A90B: 85 06        STA $06
    C4/A90D: 99 00 00     STA $0000,Y
    C4/A910: A5 08        LDA $08
    C4/A912: 99 02 00     STA $0002,Y               AECC+CE += 0x16.
    C4/A915: A2 D0 AE     LDX #$AED0                X = 0xAED0
    C4/A918: BD 00 00     LDA $0000,X
    C4/A91B: 18           CLC
    C4/A91C: 6D D8 AE     ADC $AED8
    C4/A91F: 9D 00 00     STA $0000,X               AED0 += AED8
    C4/A922: A2 D2 AE     LDX #$AED2
    C4/A925: BD 00 00     LDA $0000,X
    C4/A928: 18           CLC
    C4/A929: 6D DA AE     ADC $AEDA
    C4/A92C: 9D 00 00     STA $0000,X               AED2 += AEDA
    C4/A92F: A2 DC AE     LDX #$AEDC
    C4/A932: BD 00 00     LDA $0000,X
    C4/A935: 18           CLC
    C4/A936: 6D E0 AE     ADC $AEE0
    C4/A939: 9D 00 00     STA $0000,X               AEDC += AEE0
    C4/A93C: A0 DE AE     LDY #$AEDE
    C4/A93F: B9 00 00     LDA $0000,Y
    C4/A942: 18           CLC
    C4/A943: 6D E2 AE     ADC $AEE2
    C4/A946: 99 00 00     STA $0000,Y               AEDE += AEE2
    C4/A949: BD 00 00     LDA $0000,X
    C4/A94C: 85 15        STA $15
    C4/A94E: 85 02        STA $02                   $02, $15 = AEDC
    C4/A950: A9 00 00     LDA #$0000
    C4/A953: 18           CLC
    C4/A954: E5 02        SBC $02
    C4/A956: 50 04        BVC $A95C                 Enigmatic branch block.
    C4/A958: 10 1E        BPL $A978
    C4/A95A: 80 02        BRA $A95E
    C4/A95C: 30 1A        BMI $A978
    C4/A95E: A2 D4 AE     LDX #$AED4                --- Branch block case one
    C4/A961: A5 15        LDA $15                   X = 0xAED4
    C4/A963: 49 FF FF     EOR #$FFFF
    C4/A966: 1A           INC
    C4/A967: 85 02        STA $02                   $02 = 0x10000 - $15 (two's complement?)
    C4/A969: BD 00 00     LDA $0000,X
    C4/A96C: C5 02        CMP $02                   Compare AED4 to $02
    C4/A96E: B0 08        BCS $A978
    C4/A970: A9 00 00     LDA #$0000                AED4 lesser case: AED4 = 0x0, go to A985
    C4/A973: 9D 00 00     STA $0000,X
    C4/A976: 80 0D        BRA $A985
    C4/A978: A2 D4 AE     LDX #$AED4                --- Branch block case two / AED4 greater or equal case
    C4/A97B: BD 00 00     LDA $0000,X
    C4/A97E: 18           CLC
    C4/A97F: 6D DC AE     ADC $AEDC
    C4/A982: 9D 00 00     STA $0000,X               AED4 += AEDC
    C4/A985: AD DE AE     LDA $AEDE
    C4/A988: 85 15        STA $15
    C4/A98A: 85 02        STA $02                   $02, $15 = AEDE
    C4/A98C: A9 00 00     LDA #$0000
    C4/A98F: 18           CLC
    C4/A990: E5 02        SBC $02
    C4/A992: 50 04        BVC $A998                 Enigmatic branch block.
    C4/A994: 10 1E        BPL $A9B4
    C4/A996: 80 02        BRA $A99A
    C4/A998: 30 1A        BMI $A9B4
    C4/A99A: A2 D6 AE     LDX #$AED6                --- Branch block case one
    C4/A99D: A5 15        LDA $15                   X = 0xAED6
    C4/A99F: 49 FF FF     EOR #$FFFF
    C4/A9A2: 1A           INC
    C4/A9A3: 85 02        STA $02                   $02 = 0x10000 - $15 (two's complement?)
    C4/A9A5: BD 00 00     LDA $0000,X
    C4/A9A8: C5 02        CMP $02                   Compare AED6 to $02
    C4/A9AA: B0 08        BCS $A9B4
    C4/A9AC: A9 00 00     LDA #$0000                AED4 lesser case: AED6 = 0x0, go to A9C1
    C4/A9AF: 9D 00 00     STA $0000,X
    C4/A9B2: 80 0D        BRA $A9C1
    C4/A9B4: A2 D6 AE     LDX #$AED6                --- Branch block case two / AED6 greater or equal case
    C4/A9B7: BD 00 00     LDA $0000,X
    C4/A9BA: 18           CLC
    C4/A9BB: 6D DE AE     ADC $AEDE
    C4/A9BE: 9D 00 00     STA $0000,X               AED6 += AEDE
    C4/A9C1: AD D4 AE     LDA $AED4
    C4/A9C4: D0 1B        BNE $A9E1
    C4/A9C6: AD D6 AE     LDA $AED6
    C4/A9C9: D0 16        BNE $A9E1
    C4/A9CB: E2 20        SEP #$20                  --- AED4+D6 = null case
    C4/A9CD: 9C C2 AE     STZ $AEC2                 AEC2 = 0
    C4/A9D0: C2 20        REP #$20
    C4/A9D2: A9 00 00     LDA #$0000
    C4/A9D5: 8D CC AE     STA $AECC
    C4/A9D8: A9 00 00     LDA #$0000
    C4/A9DB: 8D CE AE     STA $AECE                 AECC+CE = null
    C4/A9DE: 4C 53 AC     JMP $AC53                 And end.
    C4/A9E1: AD D6 AE     LDA $AED6                 --- AED4+DC != null case
    C4/A9E4: EB           XBA                       <<< CONTINUE DOCUMENTING FROM HERE >>>
    C4/A9E5: 29 FF 00     AND #$00FF
    C4/A9E8: 85 0E        STA $0E
    C4/A9EA: AD D4 AE     LDA $AED4
    C4/A9ED: EB           XBA
    C4/A9EE: 29 FF 00     AND #$00FF
    C4/A9F1: A8           TAY
    C4/A9F2: AE D2 AE     LDX $AED2
    C4/A9F5: AD D0 AE     LDA $AED0
    C4/A9F8: 22 49 B1 C0  JSR $C0B149
    C4/A9FC: A2 41 00     LDX #$0041
    C4/A9FF: A9 03 00     LDA #$0003
    C4/AA02: 22 EF B0 C0  JSR $C0B0EF
    C4/AA06: AD C6 AE     LDA $AEC6
    C4/AA09: 29 FF 00     AND #$00FF
    C4/AA0C: AA           TAX
    C4/AA0D: AD C8 AE     LDA $AEC8
    C4/AA10: 29 FF 00     AND #$00FF
    C4/AA13: 22 47 B0 C0  JSR $C0B047
    C4/AA17: 4C 53 AC     JMP $AC53
    C4/AA1A: A2 C2 AE     LDX #$AEC2
    C4/AA1D: E2 20        SEP #$20
    C4/AA1F: BD 00 00     LDA $0000,X
    C4/AA22: 3A           DEC
    C4/AA23: 8D C2 AE     STA $AEC2
    C4/AA26: C2 20        REP #$20
    C4/AA28: 29 FF 00     AND #$00FF
    C4/AA2B: F0 03        BEQ $AA30
    C4/AA2D: 4C 53 AC     JMP $AC53
    C4/AA30: A2 C4 AE     LDX #$AEC4
    C4/AA33: 86 15        STX $15
    C4/AA35: C2 20        REP #$20
    C4/AA37: BD 00 00     LDA $0000,X
    C4/AA3A: 29 FF 00     AND #$00FF
    C4/AA3D: D0 03        BNE $AA42
    C4/AA3F: 4C 20 AB     JMP $AB20
    C4/AA42: E2 20        SEP #$20
    C4/AA44: AD C3 AE     LDA $AEC3
    C4/AA47: 8D C2 AE     STA $AEC2
    C4/AA4A: A0 C9 AE     LDY #$AEC9
    C4/AA4D: 84 13        STY $13
    C4/AA4F: C2 20        REP #$20
    C4/AA51: B9 00 00     LDA $0000,Y
    C4/AA54: 29 FF 00     AND #$00FF
    C4/AA57: 1A           INC
    C4/AA58: 1A           INC
    C4/AA59: 1A           INC
    C4/AA5A: 22 34 AE C0  JSR $C0AE34
    C4/AA5E: A4 13        LDY $13
    C4/AA60: E2 20        SEP #$20
    C4/AA62: B9 00 00     LDA $0000,Y
    C4/AA65: 1A           INC
    C4/AA66: 99 00 00     STA $0000,Y
    C4/AA69: 29 01        AND #$01
    C4/AA6B: 99 00 00     STA $0000,Y
    C4/AA6E: C2 20        REP #$20
    C4/AA70: AD C7 AE     LDA $AEC7
    C4/AA73: 29 FF 00     AND #$00FF
    C4/AA76: D0 4B        BNE $AAC3
    C4/AA78: A2 C5 AE     LDX #$AEC5
    C4/AA7B: 86 15        STX $15
    C4/AA7D: E2 20        SEP #$20
    C4/AA7F: BD 00 00     LDA $0000,X
    C4/AA82: 85 12        STA $12
    C4/AA84: C2 20        REP #$20
    C4/AA86: A9 00 00     LDA #$0000
    C4/AA89: 85 06        STA $06
    C4/AA8B: A9 CE 00     LDA #$00CE
    C4/AA8E: 85 08        STA $08
    C4/AA90: A5 12        LDA $12
    C4/AA92: 29 FF 00     AND #$00FF
    C4/AA95: 0A           ASL
    C4/AA96: AA           TAX
    C4/AA97: BF 45 DC CE  LDA $CEDC45,X
    C4/AA9B: 18           CLC
    C4/AA9C: 65 06        ADC $06
    C4/AA9E: 85 06        STA $06
    C4/AAA0: E2 20        SEP #$20
    C4/AAA2: A5 12        LDA $12
    C4/AAA4: 1A           INC
    C4/AAA5: A6 15        LDX $15
    C4/AAA7: 9D 00 00     STA $0000,X
    C4/AAAA: C2 20        REP #$20
    C4/AAAC: A5 06        LDA $06
    C4/AAAE: 85 0E        STA $0E
    C4/AAB0: A5 08        LDA $08
    C4/AAB2: 85 10        STA $10
    C4/AAB4: B9 00 00     LDA $0000,Y
    C4/AAB7: 29 FF 00     AND #$00FF
    C4/AABA: 1A           INC
    C4/AABB: 1A           INC
    C4/AABC: 1A           INC
    C4/AABD: 22 B8 B0 C0  JSR $C0B0B8
    C4/AAC1: 80 3D        BRA $AB00
    C4/AAC3: A2 C5 AE     LDX #$AEC5
    C4/AAC6: E2 20        SEP #$20
    C4/AAC8: BD 00 00     LDA $0000,X
    C4/AACB: 3A           DEC
    C4/AACC: 85 12        STA $12
    C4/AACE: 9D 00 00     STA $0000,X
    C4/AAD1: C2 20        REP #$20
    C4/AAD3: A9 00 00     LDA #$0000
    C4/AAD6: 85 06        STA $06
    C4/AAD8: A9 CE 00     LDA #$00CE
    C4/AADB: 85 08        STA $08
    C4/AADD: A5 12        LDA $12
    C4/AADF: 29 FF 00     AND #$00FF
    C4/AAE2: 0A           ASL
    C4/AAE3: AA           TAX
    C4/AAE4: BF 45 DC CE  LDA $CEDC45,X
    C4/AAE8: 18           CLC
    C4/AAE9: 65 06        ADC $06
    C4/AAEB: 85 06        STA $06
    C4/AAED: 85 0E        STA $0E
    C4/AAEF: A5 08        LDA $08
    C4/AAF1: 85 10        STA $10
    C4/AAF3: B9 00 00     LDA $0000,Y
    C4/AAF6: 29 FF 00     AND #$00FF
    C4/AAF9: 1A           INC
    C4/AAFA: 1A           INC
    C4/AAFB: 1A           INC
    C4/AAFC: 22 B8 B0 C0  JSR $C0B0B8
    C4/AB00: AD C6 AE     LDA $AEC6
    C4/AB03: 29 FF 00     AND #$00FF
    C4/AB06: AA           TAX
    C4/AB07: AD C8 AE     LDA $AEC8
    C4/AB0A: 29 FF 00     AND #$00FF
    C4/AB0D: 22 47 B0 C0  JSR $C0B047
    C4/AB11: A2 C4 AE     LDX #$AEC4
    C4/AB14: E2 20        SEP #$20
    C4/AB16: BD 00 00     LDA $0000,X
    C4/AB19: 3A           DEC
    C4/AB1A: 9D 00 00     STA $0000,X
    C4/AB1D: 4C 53 AC     JMP $AC53
    C4/AB20: A9 E4 AE     LDA #$AEE4
    C4/AB23: 85 02        STA $02
    C4/AB25: A6 02        LDX $02
    C4/AB27: BD 00 00     LDA $0000,X
    C4/AB2A: 29 FF 00     AND #$00FF
    C4/AB2D: D0 03        BNE $AB32
    C4/AB2F: 4C 07 AC     JMP $AC07
    C4/AB32: A0 E6 AE     LDY #$AEE6
    C4/AB35: E2 20        SEP #$20
    C4/AB37: B9 00 00     LDA $0000,Y
    C4/AB3A: 3A           DEC
    C4/AB3B: 99 00 00     STA $0000,Y
    C4/AB3E: C2 20        REP #$20
    C4/AB40: 29 FF 00     AND #$00FF
    C4/AB43: F0 6B        BEQ $ABB0
    C4/AB45: A9 41 DD     LDA #$DD41
    C4/AB48: 85 06        STA $06
    C4/AB4A: A9 CE 00     LDA #$00CE
    C4/AB4D: 85 08        STA $08
    C4/AB4F: A6 02        LDX $02
    C4/AB51: BD 00 00     LDA $0000,X
    C4/AB54: 29 FF 00     AND #$00FF
    C4/AB57: 0A           ASL
    C4/AB58: 0A           ASL
    C4/AB59: 1A           INC
    C4/AB5A: 1A           INC
    C4/AB5B: A6 06        LDX $06
    C4/AB5D: 86 0A        STX $0A
    C4/AB5F: A6 08        LDX $08
    C4/AB61: 86 0C        STX $0C
    C4/AB63: 18           CLC
    C4/AB64: 65 0A        ADC $0A
    C4/AB66: 85 0A        STA $0A
    C4/AB68: E2 20        SEP #$20
    C4/AB6A: A7 0A        LDA [$0A]
    C4/AB6C: A6 15        LDX $15
    C4/AB6E: 9D 00 00     STA $0000,X
    C4/AB71: A0 C5 AE     LDY #$AEC5
    C4/AB74: A6 02        LDX $02
    C4/AB76: C2 20        REP #$20
    C4/AB78: BD 00 00     LDA $0000,X
    C4/AB7B: 29 FF 00     AND #$00FF
    C4/AB7E: 0A           ASL
    C4/AB7F: 0A           ASL
    C4/AB80: 1A           INC
    C4/AB81: 18           CLC
    C4/AB82: 65 06        ADC $06
    C4/AB84: 85 06        STA $06
    C4/AB86: E2 20        SEP #$20
    C4/AB88: A7 06        LDA [$06]
    C4/AB8A: 85 12        STA $12
    C4/AB8C: 99 00 00     STA $0000,Y
    C4/AB8F: C2 20        REP #$20
    C4/AB91: AD C7 AE     LDA $AEC7
    C4/AB94: 29 FF 00     AND #$00FF
    C4/AB97: D0 03        BNE $AB9C
    C4/AB99: 4C 30 AA     JMP $AA30
    C4/AB9C: A6 15        LDX $15
    C4/AB9E: E2 20        SEP #$20
    C4/ABA0: BD 00 00     LDA $0000,X
    C4/ABA3: 85 00        STA $00
    C4/ABA5: A5 12        LDA $12
    C4/ABA7: 18           CLC
    C4/ABA8: 65 00        ADC $00
    C4/ABAA: 99 00 00     STA $0000,Y
    C4/ABAD: 4C 30 AA     JMP $AA30
    C4/ABB0: A2 E5 AE     LDX #$AEE5
    C4/ABB3: E2 20        SEP #$20
    C4/ABB5: BD 00 00     LDA $0000,X
    C4/ABB8: 1A           INC
    C4/ABB9: 9D 00 00     STA $0000,X
    C4/ABBC: C2 20        REP #$20
    C4/ABBE: 29 FF 00     AND #$00FF
    C4/ABC1: C9 01 00     CMP #$0001
    C4/ABC4: F0 0C        BEQ $ABD2
    C4/ABC6: C9 02 00     CMP #$0002
    C4/ABC9: F0 15        BEQ $ABE0
    C4/ABCB: C9 03 00     CMP #$0003
    C4/ABCE: F0 1E        BEQ $ABEE
    C4/ABD0: 80 28        BRA $ABFA
    C4/ABD2: E2 20        SEP #$20
    C4/ABD4: A9 04        LDA #$04
    C4/ABD6: 99 00 00     STA $0000,Y
    C4/ABD9: A9 03        LDA #$03
    C4/ABDB: 8D C3 AE     STA $AEC3
    C4/ABDE: 80 1A        BRA $ABFA
    C4/ABE0: E2 20        SEP #$20
    C4/ABE2: A9 06        LDA #$06
    C4/ABE4: 99 00 00     STA $0000,Y
    C4/ABE7: A9 02        LDA #$02
    C4/ABE9: 8D C3 AE     STA $AEC3
    C4/ABEC: 80 0C        BRA $ABFA
    C4/ABEE: E2 20        SEP #$20
    C4/ABF0: A9 0C        LDA #$0C
    C4/ABF2: 99 00 00     STA $0000,Y
    C4/ABF5: A9 01        LDA #$01
    C4/ABF8: 8D C3 AE     STA $AEC3
    C4/ABFA: C2 20        REP #$20
    C4/ABFC: AD E6 AE     LDA $AEE6
    C4/ABFF: 29 FF 00     AND #$00FF
    C4/AC02: F0 03        BEQ $AC07
    C4/AC04: 4C 30 AA     JMP $AA30
    C4/AC07: A2 CA AE     LDX #$AECA
    C4/AC0A: BD 00 00     LDA $0000,X
    C4/AC0D: 29 FF 00     AND #$00FF
    C4/AC10: F0 10        BEQ $AC22
    C4/AC12: E2 20        SEP #$20
    C4/AC14: A9 01        LDA #$01
    C4/AC17: 8D C2 AE     STA $AEC2
    C4/AC19: BD 00 00     LDA $0000,X
    C4/AC1C: 3A           DEC
    C4/AC1D: 9D 00 00     STA $0000,X
    C4/AC20: 80 31        BRA $AC53
    C4/AC22: AD CB AE     LDA $AECB
    C4/AC25: 29 FF 00     AND #$00FF
    C4/AC28: F0 29        BEQ $AC53
    C4/AC2A: AD C9 AE     LDA $AEC9
    C4/AC2D: 29 FF 00     AND #$00FF
    C4/AC30: 1A           INC
    C4/AC31: 1A           INC
    C4/AC32: 1A           INC
    C4/AC33: 22 34 AE C0  JSR $C0AE34
    C4/AC37: A2 00 00     LDX #$0000
    C4/AC3A: 8A           TXA
    C4/AC3B: 22 47 B0 C0  JSR $C0B047
    C4/AC3F: 22 96 DE C2  JSR $C2DE96
    C4/AC43: A0 00 00     LDY #$0000
    C4/AC46: BB           TYX
    C4/AC47: 98           TYA
    C4/AC48: 22 1A B0 C0  JSR $C0B01A
    C4/AC4C: AD 8A AD     LDA $AD8A
    C4/AC4F: 22 CD AF C0  JSR $C0AFCD
    C4/AC53: C2 20        REP #$20
    C4/AC55: 2B           PLD
    C4/AC56: 6B           RTL

--------------------------------------------------------------------------------------------------------