# STATUS ALTERING PSI NOTES

Original Author: michael_cayer

(P) indicates the last part of a pointer. Usually when one is used, the rest of the pointer is loaded nearby. These are rough notes and are not intended to be used as general documentation, so they may be a tad cryptic...

## HEALING α

    C2 31 0B 7B 69 EE FF 5B AD 72 A9 18 69 1D 00 AA
    BD 00 00 29 FF 00 C9 07 00 F0 07 C9 06 00 F0 1B
    80 32 E2 20 A9 00 9D 00 00 C2 20 A9 BC 6E 85 0E
    A9 EF 00 85 10 22 1C DC C1 80 53 E2 20 A9 00 9D
    00 00 C2 20 A9 38 6F 85 0E A9 EF 00 85 10 22 1C
    DC C1 80 3A AD 72 A9 18 69 1F 00 AA BD 00 00 29
    FF 00 C9 01 00 D0 19 E2 20 A9 00 9D 00 00 C2 20
    A9 54 6F 85 0E A9 EF 00 85 10 22 1C DC C1 80 0E
    A9 96 76 85 0E A9 EF 00 85 10 22 1C DC C1 2B 6B

## FLASH α

    C2 31 20 FD 7C C9 00 00 D0 1C 20 A1 98 C9 00 F0
    11 22 9A 8E C0 29 07 00 D0 05 20 DE 98 80 03 20
    50 99 20 CE 94 6B

* Standard flags-register stuff.

* Jump to 0x27EFD ($C27CFD). ASM here contains a pointer to "did not work on" text.
  This is believed to be a check for required PP, in case Magnet/PSI-disabling
  actions happened between the order and action, but this is just a hypothesis.

* Compare against NULL - jump to end if not NULL.

* Jump to 0x29AA1 ($C298A1). ASM here contains a pointer to "did not work on" text.

* Compare against NULL - if equal, go to 0x296CE ($C294CE) indirectly. ASM here
  contains a pointer to "shield disappeared" text.

* Jump to 0x909A ($C08E9A). ASM here contains no text pointers. No idea what this does.
  Update: this is a wild guess, but I suspect a RNG.

* Perform an AND operation. Effect: accumulator = (accumulator / 8) remainder.

* Branch if not equal to 0x29B50 ($C29950), indirectly. Combined with the above, I think
  it means "If result of 0x909A jump was NOT a multiple of 8, go to 0x29B50." The
  0x29B50 ASM has pointers to the "can't stop crying" and "did not work on" texts.

* Assuming the above did not happen (WAS a multiple of 8), jump to 0x29ADE ($C298DE).
  This ASM has pointers to the "felt a little strange" and "did not work on" texts.
  On return, branch to 0x296CE ($C294CE) indirectly.

* The above three comments indicate that if a "success" occurs, there is a 7/8 chance
  of crying and 1/8 feeling strange.

* END

## FLASH ϐ

    C2 31 20 FD 7C C9 00 00 D0 36 20 A1 98 C9 00 00
    00 F0 2B 22 9A 8E C0 29 07 00 F0 0C C9 01 00 F0
    10 C9 02 00 F0 10 80 13 AD 72 A9 22 50 75 C2 80
    0D 20 17 99 80 08 20 DE 98 80 03 20 50 99 20 CE
    94 6B

* Standard flags-register stuff.

* Jump to 0x27EFD ($C27CFD). ASM here contains a pointer to "did not work on" text.
  This is believed to be a check for required PP, in case Magnet/PSI-disabling
  actions happened between the order and action, but this is just a hypothesis.

* Compare against NULL - jump to end if not NULL.

* Jump to 0x29AA1 ($C298A1). ASM here contains a pointer to "did not work on" text.

* Compare against NULL - if equal, go to 0x296CE ($C294CE) indirectly. ASM here
  contains a pointer to "shield disappeared" text.

* Jump to 0x909A ($C08E9A). ASM here contains no text pointers. No idea what this does.
  Update: this is a wild guess, but I suspect a RNG.

* Perform an AND operation. Effect: accumulator = (accumulator / 8) remainder.

* Compare against 0x00 and jump to 0x29BD5 ($C299D5) if equal. From here you jump to
  0x27750 ($C27550). ASM here contains a text pointer to "got hurt and collapsed" text.
  This is absurdly complex and also contains references to enemy and battle effects
  data. Examine at your own risk.

* Compare against 0x01 and jump to 0x29B17 ($C29917) indirectly if equal. ASM here
  contains a text pointer to "body became numb" and "did not work on" texts.

* Compare against 0x02 and jump to 0x29ADE ($C298DE) indirectly if equal. ASM here
  contains a text pointer to "felt a little strange" and "did not work on" texts.

* If none of the above occured (0x03 through 0x07), jump to 0x29B50 ($C29950) indirectly.
  The 0x29B50 ASM has pointers to the "can't stop crying" and "did not work on" texts.

* The above four comments indicate that if a "success" occurs, there is a 1/8 chance
  of death, 1/8 chance of paralysis, 1/8 chance of feeling strange, and 5/8 chance of
  crying.

* END

## FLASH γ

    C2 31 20 FD 7C C9 00 00 D0 3B 20 A1 98 C9 00 00
    F0 30 22 9A 8E C0 29 07 00 F0 11 C9 01 00 F0 0C
    C9 02 00 F0 10 C9 03 00 F0 10 80 13 AD 72 A9 22
    50 75 C2 80 0D 20 17 99 80 08 20 DE 98 80 03 20
    50 99 20 CE 94 6B

* Standard flags-register stuff.

* Jump to 0x27EFD ($C27CFD). ASM here contains a pointer to "did not work on" text.
  This is believed to be a check for required PP, in case Magnet/PSI-disabling
  actions happened between the order and action, but this is just a hypothesis.

* Compare against NULL - jump to end if not NULL.

* Jump to 0x29AA1 ($C298A1). ASM here contains a pointer to "did not work on" text.

* Compare against NULL - if equal, go to 0x296CE ($C294CE) indirectly. ASM here
  contains a pointer to "shield disappeared" text.

* Jump to 0x909A ($C08E9A). ASM here contains no text pointers. No idea what this does.
  Update: this is a wild guess, but I suspect a RNG.

* Perform an AND operation. Effect: accumulator = (accumulator / 8) remainder.

* Compare against 0x00 and jump to 0x29C1B ($C29A1B) if equal. From here you jump to
  0x27750 ($C27550). ASM here contains a text pointer to "got hurt and collapsed" text.
  This is absurdly complex and also contains references to enemy and battle effects
  data. Examine at your own risk.

* Compare against 0x01 and jump to the same place as above if equal.

* Compare against 0x02 and jump to 0x29B17 ($C29917) indirectly if equal. ASM here
  contains a text pointer to "body became numb" and "did not work on" texts.

* Compare against 0x03 and jump to 0x29ADE ($C298DE) indirectly if equal. ASM here
  contains a text pointer to "felt a little strange" and "did not work on" texts.

* If none of the above occured (0x04 through 0x07), jump to 0x29B50 ($C29950) indirectly.
  The 0x29B50 ASM has pointers to the "can't stop crying" and "did not work on" texts.

* The above four comments indicate that if a "success" occurs, there is a 2/8 chance
  of death, 1/8 chance of paralysis, 1/8 chance of feeling strange, and 4/8 chance of
  crying.

* END

## FLASH Ω

    C2 31 20 FD 7C C9 00 00 D0 40 20 A1 98 C9 00 00
    F0 35 22 9A 8E C0 29 07 00 F0 16 C9 01 00 F0 11
    C9 02 00 F0 0C C9 03 00 F0 10 C9 04 00 F0 10 80
    13 AD 72 A9 22 50 75 C2 80 0D 20 17 99 80 08 20
    DE 98 80 03 20 50 99 20 CE 94 6B

* Standard flags-register stuff.

* Jump to 0x27EFD ($C27CFD). ASM here contains a pointer to "did not work on" text.
  This is believed to be a check for required PP, in case Magnet/PSI-disabling
  actions happened between the order and action, but this is just a hypothesis.

* Compare against NULL - jump to end if not NULL.

* Jump to 0x29AA1 ($C298A1). ASM here contains a pointer to "did not work on" text.

* Compare against NULL - if equal, go to 0x296CE ($C294CE) indirectly. ASM here
  contains a pointer to "shield disappeared" text.

* Jump to 0x909A ($C08E9A). ASM here contains no text pointers. No idea what this does.
  Update: this is a wild guess, but I suspect a RNG.

* Perform an AND operation. Effect: accumulator = (accumulator / 8) remainder.

* Compare against 0x00 and jump to 0x29C1B ($C29A1B) if equal. From here you jump to
  0x27750 ($C27550). ASM here contains a text pointer to "got hurt and collapsed" text.
  This is absurdly complex and also contains references to enemy and battle effects
  data. Examine at your own risk.

* Compare against 0x01 and jump to the same place as above if equal.

* Compare against 0x02 and jump to the same place as above if equal.

* Compare against 0x03 and jump to 0x29B17 ($C29917) indirectly if equal. ASM here
  contains a text pointer to "body became numb" and "did not work on" texts.

* Compare against 0x04 and jump to 0x29ADE ($C298DE) indirectly if equal. ASM here
  contains a text pointer to "felt a little strange" and "did not work on" texts.

* If none of the above occured (0x05 through 0x07), jump to 0x29B50 ($C29950) indirectly.
  The 0x29B50 ASM has pointers to the "can't stop crying" and "did not work on" texts.

* The above four comments indicate that if a "success" occurs, there is a 3/8 chance
  of death, 1/8 chance of paralysis, 1/8 chance of feeling strange, and 3/8 chance of
  crying.

* END

## BRAINSHOCK α

    C2 31 0B 7B 69 EE FF 5B 20 FD 7C C9 00 00 D0 3F
    AE 72 A9 E2 20 BD 3B 00 20 B8 6B C9 00 00 F0 21
    A0 01 00 A2 03 00 AD 72 A9 20 4A 72 C9 00 00 F0
    10 A9 3A 6C 85 0E A9 EF 00 85 10 22 1C DC C1 80
    0E A9 6E 76 85 0E A9 EF 00 85 10 22 1C DC C1 2B
    6B

## PARALYSIS α

    C2 31 0B 7B 69 EE FF 5B 20 FD 7C C9 00 00 D0 3F
    AE 72 A9 E2 20 BD 37 00 20 B8 6B C9 00 00 F0 21
    A0 03 00 A2 00 00 AD 72 A9 20 4A 72 C9 00 00 F0
    10 A9 E0 6A 85 0E A9 EF 00 85 10 22 1C DC C1 80
    0E A9 6E 76 85 0E A9 EF 00 85 10 22 1C DC C1 2B
    6B

## HYPNOSIS α

    C2 31 0B 7B 69 EE FF 5B 20 FD 7C C9 00 00 D0 3F
    AE 72 A9 E2 20 BD 3C 00 20 B8 6B C9 00 00 F0 21
    A0 01 00 A2 02 00 AD 72 A9 20 4A 72 C9 00 00 F0
    10 A9 55 6C 85 0E A9 EF 00 85 10 22 1C DC C1 80
    0E A9 6E 76 85 0E A9 EF 00 85 10 22 1C DC C1 2B
    6B

## BYTE DIFFERENCES α

    #23 = 3B 37 3C = ?
    #34 = 01 03 01 = Stat change
    #37 = 03 00 02 = Stat change
    #51 = 3A E0 55 = Text for stat change (P)
    #52 = 6C 6A 6C = Text for stat change (P)

#34 and #37 in Brainshock, Paralysis and Hypnosis control the stat change.
 * Take the numbers from the statchange CC. Subtract 1 from both numbers.
 * Switch the numbers.
 * Put the first number in #34 and the second in #37.
 * Save, load and test.
Some effects will not alter enemies, such as Possession and Homesickness. Effects on a PSI-Shielding enemy are unknown.

NOTE: The rest of the statchange text pointer is located at #56.

****************************************************************************************

## BRAINSHOCK Ω
`C2 31 22 56 A0 C2 6B`

## PARALYSIS Ω
`C2 31 22 FE 9F C2 6B`

## HYPNOSIS Ω
`C2 31 22 06 9F C2 6B`

## BYTE DIFFERENCES Ω

    #4 = 56 FE 06 = PSI to use on all enemies (P)
    #5 = A0 9F 9F = PSI to use on all enemies (P)

NOTE: The rest of the pointer is located at byte #6.

****************************************************************************************

## OFFENSE UP α

    C2 31 0B 7B 69 E8 FF 58 20 FD 7C C9 00 00 D0 35
    AE 72 A9 BC 26 00 84 16 AD 72 A9 20 28 7D A9 7D
    F7 85 0E A9 C8 00 85 10 A4 16 84 02 AE 72 A9 BD
    26 00 38 E5 02 85 06 64 08 A5 06 85 12 A5 08 85
    14 22 66 DC C1 2B 6B

## DEFENSE DOWN α

    C2 31 0B 7B 69 E8 FF 5B 20 FD 7C C9 00 00 D0 67
    20 96 7C C9 00 00 F0 51 AE 72 A9 BC 28 00 84 16
    AD 72 A9 20 33 7E AE 72 A9 A4 16 98 38 FD 28 00
    85 16 85 02 A9 00 00 18 E5 02 50 04 10 09 80 02
    30 05 A9 00 00 85 16 A9 A2 F8 85 0E A9 C8 00 85
    10 A5 16 85 06 64 08 10 02 C6 08 A5 06 85 12 A5
    08 85 14 22 66 DC C1 80 0E A9 6E 76 85 0E A9 EF
    00 85 10 22 1C DC C1 2B 6B

## SHIELD α

    C2 31 0B 7B 69 EE FF 5B A2 04 00 AD 72 A9 20 DC
    9C C9 00 00 F0 10 A9 BD 6F 85 0E A9 EF 00 85 10
    22 1C DC C1 80 0E A9 9A 6F 85 0E A9 EF 00 85 10
    22 1C DC C1 2B 6B

## PSI SHIELD α

    C2 31 0B 7B 69 EE FF 5B A2 02 00 AD 72 A9 20 DC
    9C C9 00 00 F0 10 A9 32 70 85 0E A9 EF 00 85 10
    22 1C DC C1 80 0E A9 0C 70 85 0E A9 EF 00 85 10
    22 1C DC C1 2B 6B

## BYTE DIFFERENCES α

    #10 = 04 02 = Effect; seems to be enemy status +1, untested
    #24 = BD 32 = Text when used on already-shielded target (P)
    #25 = 6F 70 = Text when used on already-shielded target (P)
    #40 = 9A 0C = Text for effect (P)
    #41 = 6F 70 = Text for effect (P)

NOTE: The rest of the already-shielded pointer is located at byte #29.
NOTE: The rest of the normal pointer is located at byte #45.
NOTE: Shield ϐ and PSI Shield ϐ have the same structure.

Enemy status notes:
http://pkhack.starmen.net/down/docs/enstatus.txt

## SHIELD ϐ

    C2 31 0B 7B 69 EE FF 5B A2 03 00 AD 72 A9 20 DC
    9C C9 00 00 F0 10 A9 F4 6F 85 0E A9 EF 00 85 10
    22 1C DC C1 80 0E A9 D3 6F 85 0E A9 EF 00 85 10
    22 1C DC C1 2B 6B

## PSI SHIELD ϐ

    C2 31 0B 7B 69 EE FF 5B A2 01 00 AD 72 A9 20 DC
    9C C9 00 00 F0 10 A9 7A 70 85 0E A9 EF 00 85 10
    22 1C DC C1 80 0E A9 50 70 85 0E A9 EF 00 85 10
    22 1C DC C1 2B 6B

## BYTE DIFFERENCES ϐ

    #10 = 03 01 = Effect; seems to be enemy status -1, untested
    #24 = F4 7A = Text when used on already-shielded target (P)
    #25 = 6F 70 = Text when used on already-shielded target (P)
    #40 = D3 50 = Text for effect (P)
    #41 = 6F 70 = Text for effect (P)

NOTE: The rest of the already-shielded pointer is located at byte #29.
NOTE: The rest of the normal pointer is located at byte #45.
NOTE: Shield A and PSI Shield A have the same structure.

Enemy status notes:
http://pkhack.starmen.net/down/docs/enstatus.txt

## SHIELD Σ
`C2 31 22 44 9D C2 6B`

## PSI SHIELD Σ
`C2 31 22 BE 9D C2 6B`

## BYTE DIFFERENCES Σ

`#4 = 44 BE = PSI to add reflection power to (P)`

NOTE: The rest of the pointer is located at byte #5-6.
NOTE: Shield O and PSI Shield O have the same structure.

## SHIELD Ω
`C2 31 22 81 9D C2 6B`

## PSI SHIELD Ω
`C2 31 22 FB 9D C2 6B`

## BYTE DIFFERENCES Ω

`#4 = 81 FB = PSI to add reflection power to (P)`

NOTE: The rest of the pointer is located at byte #5-6.
NOTE: Shield S and PSI Shield S have the same structure.

## PSI MAGNET α

    C2 31 0B 7B 69 E8 FF 5B AE 72 A9 BD 19 00 D0 10
    A9 05 FB 85 0E A9 C8 00 85 10 22 1C DC C1 80 61
    A9 04 00 20 2D 6A AA 86 16 A9 04 00 20 2D 6A 85
    02 A6 16 8A 18 65 02 85 02 E6 02 E6 02 AE 72 A9
    BD 19 00 C5 02 B0 02 85 02 A9 3F 77 85 0E A9 EF
    00 85 10 A5 02 85 06 64 08 10 02 C6 08 A5 06 85
    12 A5 08 85 14 22 66 DC C1 A6 02 AD 72 A9 20 1D
    72 AE 70 A9 A5 02 18 7D 19 00 AA AD 70 A9 20 91
    71 2B 6B

## HP SUCKER ASM

    C2 31 0B 7B 69 E8 FF 5B 20 9C 7C C9 00 00 D0 03
    4C F7 A4 AE 70 A9 BD 13 00 F0 71 AD 72 A9 CD 70
    A9 D0 10 A9 10 77 85 0E A9 EF 00 85 10 22 1C DC
    C1 80 67 AE 72 A9 BD 15 00 20 44 6A 4A 4A 4A A8
    84 16 BB AD 72 A9 20 F0 71 A9 29 77 85 0E A9 EF
    00 85 10 A4 16 98 85 06 64 08 10 02 C6 08 A5 06
    85 12 A5 08 85 14 22 66 DC C1 AE 70 A9 A4 16 98
    18 7D 11 00 AA AD 70 A9 20 26 71 AE 72 A9 BD 11
    00 D0 17 AD 72 A9 22 50 75 C2 80 0E A9 6E 76 85
    0E A9 EF 00 85 10 22 1C DC C1 2B 6B