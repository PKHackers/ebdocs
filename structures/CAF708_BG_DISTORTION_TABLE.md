# $CAF708 - BG Distortion Table

$CAF708 to $CAFFFE (0008F7)
135 17-byte entries.


Setup:
327 x `[01 01 04 00 00 0F 00 00 00 00 00 00 00 01 00 00 00]` at [$CADCA1](CADCA1_BG_DATA_TABLE.md).
Edit $CAF719's entry.

The entries seem to have a paired structure...
- [3 bytes] (seven bytes) (seven bytes)
Where the (seven bytes) groups are paired.

`[AA BB CC DD DD EE EE FF GG HH II II JJ JJ KK LL MM]`



- AA - ???
- BB - ???

- CC
    - 01: Horizontal, smooth
    - 02: Horizontal, interlaced
    - 03: Vertical, smooth
    - 04: ???

- DD - Ripple frequency
- EE - Ripple amplitude
- FF - ???
- GG - ???
- HH - ???

- II - Increase in (ripple frequency) each iteration
- JJ - Increase in (ripple amplitude) each iteration
- KK - Speed
- LL - ???
- MM - ???


Working from the assembly reference...

   $04 = Entry number

$06+08 = AF908 table root pointer

$0A+0C = Pointer to specific entry

   $10 = 17 * entry number

What is $1B holding?
A pointer, it would seem, but to where?


- $1B,66 = First and second bytes
- $1B,68 = Third byte
- $1B,69 = Fourth and fifth bytes
- $1B,6B = Sixth and seventh bytes
- $1B,6D = Eighth byte
- $1B,6E = Ninth and tenth bytes
- $1B,70 = Eleventh and twelfth bytes
- $1B,72 = Thirteenth and fourteenth bytes
- $1B,74 = Fifteenth byte
- $1B,75 = Sixteenth and seventeenth bytes


    C2/CE05: A7 0A        LDA [$0A]
    C2/CE07: A0 66 00     LDY #$0066
    C2/CE0A: 91 1B        STA ($1B),Y
    C2/CE0C: A5 1B        LDA $1B
    C2/CE0E: 18           CLC
    C2/CE0F: 69 68 00     ADC #$0068
    C2/CE12: AA           TAX

What's going on here with the first two bytes of the entry?

    C2/CE13: A5 10        LDA $10
    C2/CE15: 1A           INC
    C2/CE16: 1A           INC
    C2/CE17: A4 06        LDY $06
    C2/CE19: 84 0A        STY $0A
    C2/CE1B: A4 08        LDY $08
    C2/CE1D: 84 0C        STY $0C
    C2/CE1F: 18           CLC
    C2/CE20: 65 0A        ADC $0A
    C2/CE22: 85 0A        STA $0A
    C2/CE24: E2 20        SEP #$20
    C2/CE26: A7 0A        LDA [$0A]
    C2/CE28: 9D 00 00     STA $0000,X
    C2/CE2B: C2 20        REP #$20

It's getting a location to store the third byte at, it seems.

    C2/CE2D: A5 10        LDA $10
    C2/CE2F: 1A           INC
    C2/CE30: 1A           INC
    C2/CE31: 1A           INC
    C2/CE32: A4 06        LDY $06
    C2/CE34: 84 0A        STY $0A
    C2/CE36: A4 08        LDY $08
    C2/CE38: 84 0C        STY $0C
    C2/CE3A: 18           CLC
    C2/CE3B: 65 0A        ADC $0A
    C2/CE3D: 85 0A        STA $0A
    C2/CE3F: A7 0A        LDA [$0A]
    C2/CE41: A0 69 00     LDY #$0069
    C2/CE44: 91 1B        STA ($1B),Y

The fourth and fifth bytes get enigmatically stored...

    C2/CE46: A5 10        LDA $10
    C2/CE48: 18           CLC
    C2/CE49: 69 05 00     ADC #$0005
    C2/CE4C: A4 06        LDY $06
    C2/CE4E: 84 0A        STY $0A
    C2/CE50: A4 08        LDY $08
    C2/CE52: 84 0C        STY $0C
    C2/CE54: 18           CLC
    C2/CE55: 65 0A        ADC $0A
    C2/CE57: 85 0A        STA $0A
    C2/CE59: A7 0A        LDA [$0A]
    C2/CE5B: A0 6B 00     LDY #$006B
    C2/CE5E: 91 1B        STA ($1B),Y

Likewise for the sixth and seventh...

    C2/CE60: A5 10        LDA $10
    C2/CE62: 18           CLC
    C2/CE63: 69 07 00     ADC #$0007
    C2/CE66: A4 06        LDY $06
    C2/CE68: 84 0A        STY $0A
    C2/CE6A: A4 08        LDY $08
    C2/CE6C: 84 0C        STY $0C
    C2/CE6E: 18           CLC
    C2/CE6F: 65 0A        ADC $0A
    C2/CE71: 85 0A        STA $0A
    C2/CE73: E2 20        SEP #$20
    C2/CE75: A7 0A        LDA [$0A]
    C2/CE77: A0 6D 00     LDY #$006D
    C2/CE7A: 91 1B        STA ($1B),Y
    C2/CE7C: C2 20        REP #$20

And the eighth. (Just one byte.)

    C2/CE7E: A5 10        LDA $10
    C2/CE80: 18           CLC
    C2/CE81: 69 08 00     ADC #$0008
    C2/CE84: A4 06        LDY $06
    C2/CE86: 84 0A        STY $0A
    C2/CE88: A4 08        LDY $08
    C2/CE8A: 84 0C        STY $0C
    C2/CE8C: 18           CLC
    C2/CE8D: 65 0A        ADC $0A
    C2/CE8F: 85 0A        STA $0A
    C2/CE91: A7 0A        LDA [$0A]
    C2/CE93: A0 6E 00     LDY #$006E
    C2/CE96: 91 1B        STA ($1B),Y

Ninth and tenth... this is getting quite repetitive.

    C2/CE98: A5 10        LDA $10
    C2/CE9A: 18           CLC
    C2/CE9B: 69 0A 00     ADC #$000A
    C2/CE9E: A4 06        LDY $06
    C2/CEA0: 84 0A        STY $0A
    C2/CEA2: A4 08        LDY $08
    C2/CEA4: 84 0C        STY $0C
    C2/CEA6: 18           CLC
    C2/CEA7: 65 0A        ADC $0A
    C2/CEA9: 85 0A        STA $0A
    C2/CEAB: A7 0A        LDA [$0A]
    C2/CEAD: A0 70 00     LDY #$0070
    C2/CEB0: 91 1B        STA ($1B),Y

Eleventh and twelfth.

    C2/CEB2: A5 10        LDA $10
    C2/CEB4: 18           CLC
    C2/CEB5: 69 0C 00     ADC #$000C
    C2/CEB8: A4 06        LDY $06
    C2/CEBA: 84 0A        STY $0A
    C2/CEBC: A4 08        LDY $08
    C2/CEBE: 84 0C        STY $0C
    C2/CEC0: 18           CLC
    C2/CEC1: 65 0A        ADC $0A
    C2/CEC3: 85 0A        STA $0A
    C2/CEC5: A7 0A        LDA [$0A]
    C2/CEC7: A0 72 00     LDY #$0072
    C2/CECA: 91 1B        STA ($1B),Y

Thirteenth and fourteenth.

    C2/CECC: A5 10        LDA $10
    C2/CECE: 18           CLC
    C2/CECF: 69 0E 00     ADC #$000E
    C2/CED2: A4 06        LDY $06
    C2/CED4: 84 0A        STY $0A
    C2/CED6: A4 08        LDY $08
    C2/CED8: 84 0C        STY $0C
    C2/CEDA: 18           CLC
    C2/CEDB: 65 0A        ADC $0A
    C2/CEDD: 85 0A        STA $0A
    C2/CEDF: E2 20        SEP #$20
    C2/CEE1: A7 0A        LDA [$0A]
    C2/CEE3: A0 74 00     LDY #$0074
    C2/CEE6: 91 1B        STA ($1B),Y
    C2/CEE8: C2 20        REP #$20

Fifteenth.

    C2/CEEA: A5 10        LDA $10
    C2/CEEC: 18           CLC
    C2/CEED: 69 0F 00     ADC #$000F
    C2/CEF0: 18           CLC
    C2/CEF1: 65 06        ADC $06
    C2/CEF3: 85 06        STA $06
    C2/CEF5: A7 06        LDA [$06]
    C2/CEF7: A0 75 00     LDY #$0075
    C2/CEFA: 91 1B        STA ($1B),Y

Sixteenth and seventeenth.

    C2/CEFC: BD 00 00     LDA $0000,X
    C2/CEFF: 29 FF 00     AND #$00FF
    C2/CF02: C9 03 00     CMP #$0003
    C2/CF05: D0 14        BNE $CF1B
    C2/CF07: A4 1D        LDY $1D
    C2/CF09: A6 19        LDX $19
    C2/CF0B: E8           INX
    C2/CF0C: E8           INX
    C2/CF0D: E8           INX
    C2/CF0E: E8           INX
    C2/CF0F: A5 1D        LDA $1D
    C2/CF11: 18           CLC
    C2/CF12: 69 05 00     ADC #$0005
    C2/CF15: 22 B2 AD C0  JSR $C0ADB2
    C2/CF19: 80 0E        BRA $CF29
    C2/CF1B: A4 1D        LDY $1D
    C2/CF1D: A6 19        LDX $19
    C2/CF1F: A5 1D        LDA $1D
    C2/CF21: 18           CLC
    C2/CF22: 69 05 00     ADC #$0005
    C2/CF25: 22 B2 AD C0  JSR $C0ADB2

X hasn't been modified, so this gets the third byte.
Cases:
  Third byte == 3: Go to C2/CF07
  Third byte != 3: Go to C2/CF1B
Either way, it uses C0ADB2, which is involved with DMA stuff.
Common end code: C2/CF29. Let's look at that next.

    C2/CF29: A5 1B        LDA $1B
    C2/CF2B: 18           CLC
    C2/CF2C: 69 68 00     ADC #$0068
    C2/CF2F: 85 0E        STA $0E
    C2/CF31: AA           TAX
    C2/CF32: BD 00 00     LDA $0000,X
    C2/CF35: 29 FF 00     AND #$00FF
    C2/CF38: D0 03        BNE $CF3D
    C2/CF3A: 4C E3 CF     JMP $CFE3

$0E becomes a pointer to the third byte, which is then... loaded again?
What's going on here?
At any rate... if the third byte is 0, end. Otherwise, continue.

    C2/CF3D: A5 1B        LDA $1B
    C2/CF3F: 18           CLC
    C2/CF40: 69 69 00     ADC #$0069
    C2/CF43: AA           TAX
    C2/CF44: A0 70 00     LDY #$0070
    C2/CF47: BD 00 00     LDA $0000,X
    C2/CF4A: 18           CLC
    C2/CF4B: 71 1B        ADC ($1B),Y
    C2/CF4D: 9D 00 00     STA $0000,X

(Fourth and fifth bytes) += (Eleventh and twelfth bytes)

    C2/CF50: A5 1B        LDA $1B
    C2/CF52: 18           CLC
    C2/CF53: 69 6B 00     ADC #$006B
    C2/CF56: AA           TAX
    C2/CF57: A0 72 00     LDY #$0072
    C2/CF5A: BD 00 00     LDA $0000,X
    C2/CF5D: 18           CLC
    C2/CF5E: 71 1B        ADC ($1B),Y
    C2/CF60: 9D 00 00     STA $0000,X

(Sixth and seventh bytes) += (Thirteenth and fourteenth bytes)

    C2/CF63: A5 1B        LDA $1B
    C2/CF65: 18           CLC
    C2/CF66: 69 6D 00     ADC #$006D
    C2/CF69: AA           TAX
    C2/CF6A: A0 74 00     LDY #$0074
    C2/CF6D: E2 20        SEP #$20
    C2/CF6F: BD 00 00     LDA $0000,X
    C2/CF72: 18           CLC
    C2/CF73: 71 1B        ADC ($1B),Y
    C2/CF75: 9D 00 00     STA $0000,X
    C2/CF78: C2 20        REP #$20

(Eighth byte) += (Fifteenth byte)

    C2/CF7A: A5 1B        LDA $1B
    C2/CF7C: 18           CLC
    C2/CF7D: 69 6E 00     ADC #$006E
    C2/CF80: 85 02        STA $02
    C2/CF82: A0 75 00     LDY #$0075
    C2/CF85: A6 02        LDX $02
    C2/CF87: BD 00 00     LDA $0000,X
    C2/CF8A: 18           CLC
    C2/CF8B: 71 1B        ADC ($1B),Y
    C2/CF8D: A6 02        LDX $02
    C2/CF8F: 9D 00 00     STA $0000,X

$02 = Pointer to (Ninth and tenth bytes).
(Ninth and tenth bytes) += (Sixteenth and seventeenth bytes)

    C2/CF92: A4 1D        LDY $1D
    C2/CF94: A6 19        LDX $19
    C2/CF96: 86 14        STX $14
    C2/CF98: A5 0E        LDA $0E
    C2/CF9A: AA           TAX
    C2/CF9B: BD 00 00     LDA $0000,X
    C2/CF9E: 29 FF 00     AND #$00FF
    C2/CFA1: 3A           DEC
    C2/CFA2: A6 14        LDX $14

Y = $1D
X = $14 = $19
A = Third byte, less one

    C2/CFA4: 22 4C AE C0  JSR $C0AE4C
    C2/CFA8: A6 02        LDX $02
    C2/CFAA: BD 00 00     LDA $0000,X
    C2/CFAD: 22 56 AE C0  JSR $C0AE56
    C2/CFB1: AD 02 00     LDA $0002
    C2/CFB4: 29 FF 00     AND #$00FF
    C2/CFB7: 29 01 00     AND #$0001
    C2/CFBA: C5 1D        CMP $1D
    C2/CFBC: F0 05        BEQ $CFC3
    C2/CFBE: AD AC AD     LDA $ADAC
    C2/CFC1: D0 20        BNE $CFE3
    C2/CFC3: A0 6D 00     LDY #$006D
    C2/CFC6: B1 1B        LDA ($1B),Y
    C2/CFC8: 29 FF 00     AND #$00FF
    C2/CFCB: A8           TAY
    C2/CFCC: 84 10        STY $10
    C2/CFCE: A0 6B 00     LDY #$006B
    C2/CFD1: B1 1B        LDA ($1B),Y
    C2/CFD3: EB           XBA
    C2/CFD4: 29 FF 00     AND #$00FF
    C2/CFD7: AA           TAX
    C2/CFD8: A0 69 00     LDY #$0069
    C2/CFDB: B1 1B        LDA ($1B),Y
    C2/CFDD: A4 10        LDY $10
    C2/CFDF: 22 5A AE C0  JSR $C0AE5A
    C2/CFE3: 2B           PLD
    C2/CFE4: 6B           RTL

Haven't analyzed this last bit yet.