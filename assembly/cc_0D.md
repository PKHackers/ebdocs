C145EF
------

    C1/45EF: C2 31        REP #$31
    C1/45F1: 0B           PHD
    C1/45F2: 48           PHA
    C1/45F3: 7B           TDC
    C1/45F4: 69 EE FF     ADC #$FFEE
    C1/45F7: 5B           TCD
    C1/45F8: 68           PLA
    C1/45F9: E0 00 00     CPX #$0000
    C1/45FC: F0 09        BEQ $4607
    C1/45FE: 20 00 04     JSR $0400
    C1/4601: 85 06        STA $06
    C1/4603: 64 08        STZ $08
    C1/4605: 80 03        BRA $460A
    C1/4607: 20 0A 04     JSR $040A
    C1/460A: A5 06        LDA $06
    C1/460C: 85 0E        STA $0E
    C1/460E: A5 08        LDA $08
    C1/4610: 85 10        STA $10
    C1/4612: 20 89 04     JSR $0489
    C1/4615: A9 00 00     LDA #$0000
    C1/4618: 2B           PLD
    C1/4619: 60           RTS

Direct page manipulation.
If X is not equal to 0000:
  JSR $0400.
  Store accumulator in $06 and null in $08.
If X is equal to 0000:
  JSR $040A.
Copy $06 to $0E and $08 to $10.
JSR $0489.
Load accumulator with 0.
Return.


C10400
------

    C1/0400: C2 31        REP #$31
    C1/0402: 20 01 03     JSR $0301
    C1/0405: AA           TAX
    C1/0406: BD 1F 00     LDA $001F,X
    C1/0409: 60           RTS

Jump to the C10301 subroutine. A = 0x8650 + (0x52 * unknown).
Transfer to accumulator to X.
Load the accumulator with $001F indexed by X.
Return.
Accumulator result: 0x866F + (0x52 * unknown).


C1040A
------

    C1/040A: C2 31        REP #$31
    C1/040C: 0B           PHD
    C1/040D: 7B           TDC
    C1/040E: 69 F2 FF     ADC #$FFF2
    C1/0411: 5B           TCD
    C1/0412: 20 01 03     JSR $0301
    C1/0415: 18           CLC
    C1/0416: 69 17 00     ADC #$0017
    C1/0419: A8           TAY
    C1/041A: B9 00 00     LDA $0000,Y
    C1/041D: 85 06        STA $06
    C1/041F: B9 02 00     LDA $0002,Y
    C1/0422: 85 08        STA $08
    C1/0424: A5 06        LDA $06
    C1/0426: 85 14        STA $14
    C1/0428: A5 08        LDA $08
    C1/042A: 85 16        STA $16
    C1/042C: 2B           PLD
    C1/042D: 60           RTS

Direct page manipulation.
Jump to the C10301 subroutine. A = 0x8650 + (0x52 * unknown).
Clear carry and add 0x0017.
Transfer A to Y. Y = 0x8667 + (0x52 * unknown)
Load the accumulator with the value at 0x8667 + (0x52 * unknown).
Store in $06.
Load the accumulator with the value at 0x8669 + (0x52 * unknown).
Store in $08.
Copy $06 to $14 and $08 to $16.
Return.
Accumulator result: 0x8669 + (0x52 * unknown).


C10301
------

    C1/0301: C2 31        REP #$31
    C1/0303: AD E0 88     LDA $88E0
    C1/0306: C9 FF FF     CMP #$FFFF
    C1/0309: D0 05        BNE $0310
    C1/030B: A9 FE 85     LDA #$85FE
    C1/030E: 80 13        BRA $0323
    C1/0310: AD 58 89     LDA $8958
    C1/0313: 0A           ASL
    C1/0314: AA           TAX
    C1/0315: BD E4 88     LDA $88E4,X
    C1/0318: A0 52 00     LDY #$0052
    C1/031B: 22 F7 8F C0  JSR $C08FF7
    C1/031F: 18           CLC
    C1/0320: 69 50 86     ADC #$8650
    C1/0323: 60           RTS

Accumulator becomes the value at $88E0.
If the accumulator is not equal to FFFF:
  Branch to $0310.
  Load the accumulator with the value at $8958.
  A shift left (multiply A by 2).
  Transfer A to X and use it to get a value from the C088E4 table (window related).
  Load Y with the constant value 0x52.
  JSR $C08FF7 - multiply A by 0x52.
  Clear the carry flag.
  Add 0x8650 to the accumulator.
  Return.
  Accumulator result: 0x8650 + (0x52 * unknown).

If the accumulator is equal to FFFF (error case?):
  Load the accumulator with 0x85FE.
  Branch to $0323.
  Return.