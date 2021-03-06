BLUE MAGIC SYSTEM
-----------------
Based on psiflags1.txt, for Hyperbound's hack.
The first character has PSI, the second ("ISAAC") is a blue mage.
There are no other characters.
Poo's PSI is broken by the changes listed below.




SCRATCH NOTES FOLLOW

Note for future reference:
  $C21628 changes the X register and doesn't change it back. (This led to horrible breakage in an earlier version.)

Tracing for Paula's required level for PSI Freeze Alpha (0x158CDE = $D58ADE)
    $C1/C5F1 B7 0A       LDA [$0A],y[$D5:8ADE]   A:0001 X:00D5 Y:0007 P:envMxdIzC
    $C1/C219 A7 06       LDA [$06]  [$D5:8ADE]   A:8ADE X:0009 Y:0001 P:eNvMxdIzc

Checks against Paula's level (7E9A32) when opening the PSI window
    $C0/8F27 B7 0E       LDA [$0E],y[$7E:9A32]   A:0091 X:00D0 Y:0005 P:envMXdIzc
    $C0/8F27 B7 0E       LDA [$0E],y[$7E:9A32]   A:0091 X:0010 Y:0005 P:envMXdIzc
    $C1/C637 DD D3 99    CMP $99D3,x[$7E:9A32]   A:0001 X:005F Y:005F P:envMxdIzC
    $C0/8F27 B7 0E       LDA [$0E],y[$7E:9A32]   A:0091 X:00D0 Y:0005 P:envMXdIzc
    $C0/8F27 B7 0E       LDA [$0E],y[$7E:9A32]   A:0091 X:0010 Y:0005 P:envMXdIzc

$C2/35E5 has the existing window-construction code.
$C1DDDA accumulator values:
- 01 = Bash/Shoot/Do Nothing
- 02 = Goods
- 03 = Auto Fight
- 04 = PSI/Spy
- 05 = Defend
- 06 = Run Away
- 07 = Pray/Mirror
Pray/Mirror are both 07 - look how it does the change based on character number, both of printed name and functionality. We want to apply that to PSI. Preferably without tinkering with the Spy option.

$C2/38E0 reads the accumulator values listed above.

What I want to do: for character two, don't read the "Paula" byte in each PSI data table entry. Instead, read the Paula and Poo bytes as an event flag, check it, and return 0 or 1. psiflags1.txt may be helpful here...

------------------------------------------------------------------------

    $C1/D790: F0 10        BEQ $D7A2           Ness    = $C1D7A2
    $C1/D792: C9 01 00     CMP #$0001
    $C1/D795: F0 6E        BEQ $D805           Paula   = $C1D805
    $C1/D797: C9 03 00     CMP #$0003
    $C1/D79A: D0 03        BNE $D79F
    $C1/D79C: 4C 67 D8     JMP $D867           Poo     = $C1D867
    $C1/D79F: 4C C7 D8     JMP $D8C7           Default = $C1D8C7

This is related to displaying the "Character learned PSI X!" message when levelling up. That's not relevant for ISAAC, and there are no characters after him, so:

`$C1/D792: 4C C7 D8     JMP $D8C7           Branch to "default" after checking character one.`

------------------------------------------------------------------------

    $C1/C1E9: F0 0C        BEQ $C1F7           Ness    = $C1C1F7
    $C1/C1EB: C9 01 00     CMP #$0001
    $C1/C1EE: F0 1C        BEQ $C20C           Paula   = $C1C20C
    $C1/C1F0: C9 03 00     CMP #$0003
    $C1/C1F3: F0 2C        BEQ $C221           Poo     = $C1C221
    $C1/C1F5: 80 3D        BRA $C234           Default = $C1C234

This is involved with the actual PSI selection window. We're done after ISAAC, so don't bother checking Poo:

`$C1/C1F0: 4C 34 C2     JMP $C234`

ISAAC's skills are built on event flags instead of experience levels, so we'll be hacking the Paula case:

    $C1/C20C: A5 0E        LDA $0E
    $C1/C20E: 18           CLC
    $C1/C20F: 69 07 00     ADC #$0007
    $C1/C212: 18           CLC
    $C1/C213: 65 06        ADC $06
    $C1/C215: 85 06        STA $06
    $C1/C217: A7 06        LDA [$06]           A = Event flag
    $C1/C219: F0 06        BEQ $C221           Flag 0x0000 (no flag) case
    $C1/C21B: DA           PHX
    $C1/C21C: 22 28 16 C2  JSL $C21628         Normal case: Check the flag w/ $C21628
    $C1/C220: FA           PLX
    $C1/C221: E2 20        SEP #$20
    $C1/C223: 85 00        STA $00
    $C1/C225: 85 10        STA $10
    $C1/C227: 4C 34 C2     JMP $C234

------------------------------------------------------------------------

    $C1/C5D1: F0 0C        BEQ $C5DF           Ness    = $C1C5DF
    $C1/C5D3: C9 01 00     CMP #$0001
    $C1/C5D6: F0 14        BEQ $C5EC           Paula   = $C1C5EC
    $C1/C5D8: C9 03 00     CMP #$0003
    $C1/C5DB: F0 1C        BEQ $C5F9           Poo     = $C1C5F9
    $C1/C5DD: 80 25        BRA $C604           Default = $C1C604

This is also involved with the actual PSI selection window. We're done after ISAAC, so don't bother checking Poo:

    `$C1/C5D8: 4C 04 C6     JMP $C604`

And once again, we're hacking the Paula case:

    $C1/C5EC: A0 07 00     LDY #$0007
    $C1/C5EF: B7 0A        LDA [$0A],Y         A = Event flag
    $C1/C5F1: F0 06        BEQ $C5F9           Flag 0x0000 (no flag) case
    $C1/C5F3: DA           PHX
    $C1/C5F4: 22 28 16 C2  JSL $C21628         Normal case: Check the flag w/ $C21628
    $C1/C5F8: FA           PLX
    $C1/C5F9: E2 20        SEP #$20
    $C1/C5FB: 85 00        STA $00
    $C1/C5FD: 85 18        STA $18
    $C1/C5FF: 80 03        BRA $C604

------------------------------------------------------------------------

`<Nitrodon> Michael1: remember to change the routine at $C4/5ECE-5F7A (determines whether character A knows PSI X; used for Auto Fight)`

    $C4/5EDA: C9 01 00     CMP #$0001
    $C4/5EDD: F0 0C        BEQ $5EEB
    $C4/5EDF: C9 02 00     CMP #$0002
    $C4/5EE2: F0 24        BEQ $5F08
    $C4/5EE4: C9 04 00     CMP #$0004
    $C4/5EE7: F0 3C        BEQ $5F25
    $C4/5EE9: 80 55        BRA $5F40

Same basic changes - nothing after ISAAC, hack the Paula case.

    $C4/5EE4: 4C 40 5F     JMP $5F40
    $C4/5F08: 8A           TXA
    $C4/5F09: 85 04        STA $04
    $C4/5F0B: 0A           ASL
    $C4/5F0C: 0A           ASL
    $C4/5F0D: 0A           ASL
    $C4/5F0E: 0A           ASL
    $C4/5F0F: 38           SEC
    $C4/5F10: E5 04        SBC $04
    $C4/5F12: 18           CLC
    $C4/5F13: 69 07 00     ADC #$0007
    $C4/5F16: AA           TAX
    $C4/5F17: BF 50 8A D5  LDA $D58A50,X
    $C4/5F1B: F0 06        BEQ $5F23
    $C4/5F1D: DA           PHX
    $C4/5F1E: 22 28 16 C2  JSL $C21628
    $C4/5F22: FA           PLX
    $C4/5F23: E2 20        SEP #$20
    $C4/5F25: 85 00        STA $00
    $C4/5F27: 85 10        STA $10
    $C4/5F29: 4C 40 5F     JMP $5F40

------------------------------------------------------------------------

Additionally, Pray has been replaced with Mirror. Since they both use the same number and code case in $C2/38E0, this was really easy. :D

    $C2/3A76: A5 26        LDA $26
    $C2/3A78: C9 02 00     CMP #$0002
    $C2/3A7B: F0 0B        BEQ $3A88
    $C2/3A7D: C9 04 00     CMP #$0004
    $C2/3A80: D0 03        BNE $3A85
    $C2/3A82: 4C 19 3B     JMP $3B19
    $C2/3A85: 4C 4D 3B     JMP $3B4D

The instruction to replace:

    `$C2/3A7B: F0 05        BEQ $3A82`

------------------------------------------------------------------------