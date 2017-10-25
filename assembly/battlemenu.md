# BATTLE MENU ANALYSIS

The oft-referenced $C1DDDA is believed to be a final step (perhaps printing code/references to it). A, X
and Y seem to be written to right before each reference. Probably very important, INVESTIGATE!

JSR indicates go-and-return. JMP indicates go. These may not be the actual instructions used (branch stuff may be
used instead) - this is just what's done.

## $C235E5

Load accumulator with value at $20
Affects first entry in battle menu text
   0 = "Bash", JSR $C1DDDA, JMP $C23656
   1 = "Shoot", JSR $C1DDDA, JMP $C23656
   2 = "Do Nothing", JSR $C1DDDA, JMP $C23656
 DEF = JMP $C23656

## $C23656

Load accumulator with value at $20
   2 = JMP $C236E2
  !2 = JMP $C23660, "Bash", $C1DDDA, JMP $C236E2

## $C236E2

Load accumulator with value at $04
   0 = JMP $C236E9
  !0 = JMP $C23784

If zero at $C236E2, this code is processed.

## $C236E9

Load accumulator with value at $1A
   2 = LDX #$0010, STX $04
  !2 = LDX #$000B, STX $04
Load accumulator with value at $26
   2 = Increment $04 twice, "Bash"
   4 = Increment $04 twice, "Bash"
ELSE = "Bash"
Pointers above have 0x20 added ("Auto Fight") and values moved to $0E/10. JSR $C1DDDA. JMP $C23784
Pointers above have 0x80 added ("Run Away") and values moved to $0E/10. JSR $C1DDDA. JMP $C23784

## $C23784

Load accumulator with value at $26
   3 = "Spy", JSR $C1DDDA, JMP $C237D9
Load a variable with some indexed-by-Y mode; modulo 128
   0 = "PSI", JSR $C1DDDA, JMP $C237D9
  !0 = JMP $C237D9

## $C237D9

Load accumulator with value at $26
   2 = "Pray", JSR $C1DDDA, JMP $C23801
  !2 = JMP $C23801

## $C23801

Load accumulator with value at $26
   4 = "Mirror", JSR $C1DDDA, JMP $C23829
  !4 = JMP $C23829

## $C23829

Load X with value at $1A
Load accumulator with the value at (C4A1F2 + X); modulo 128
JSR $C1DB4D
Load accumulator with value at $24
   0 = JSR $C1DE25
  !0 = Increment $24, LDA #$0001, JSR $C1DE2B...

## $C1DDDA

    $C1/DDDA: C2 31        REP #$31
    $C1/DDDC: 0B           PHD
    $C1/DDDD: 48           PHA
    $C1/DDDE: 7B           TDC
    $C1/DDDF: 69 E4 FF     ADC #$FFE4
    $C1/DDE2: 5B           TCD
    $C1/DDE3: 68           PLA
    $C1/DDE4: 85 1A        STA $1A
    $C1/DDE6: A5 2E        LDA $2E
    $C1/DDE8: 85 06        STA $06
    $C1/DDEA: A5 30        LDA $30
    $C1/DDEC: 85 08        STA $08
    $C1/DDEE: A5 06        LDA $06
    $C1/DDF0: 85 16        STA $16
    $C1/DDF2: A5 08        LDA $08
    $C1/DDF4: 85 18        STA $18
    $C1/DDF6: A5 2A        LDA $2A
    $C1/DDF8: 85 0A        STA $0A
    $C1/DDFA: A5 2C        LDA $2C
    $C1/DDFC: 85 0C        STA $0C
    $C1/DDFE: A5 0A        LDA $0A
    $C1/DE00: 85 06        STA $06
    $C1/DE02: A5 0C        LDA $0C
    $C1/DE04: 85 08        STA $08
    $C1/DE06: A5 06        LDA $06
    $C1/DE08: 85 0E        STA $0E
    $C1/DE0A: A5 08        LDA $08
    $C1/DE0C: 85 10        STA $10
    $C1/DE0E: A5 16        LDA $16
    $C1/DE10: 85 06        STA $06
    $C1/DE12: A5 18        LDA $18
    $C1/DE14: 85 08        STA $08
    $C1/DE16: A5 06        LDA $06
    $C1/DE18: 85 12        STA $12
    $C1/DE1A: A5 08        LDA $08
    $C1/DE1C: 85 14        STA $14
    $C1/DE1E: A5 1A        LDA $1A
    $C1/DE20: 20 3B 15     JSR $153B
    $C1/DE23: 2B           PLD
    $C1/DE24: 6B           RTL