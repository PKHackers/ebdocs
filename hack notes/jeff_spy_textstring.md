# JEFF SPY-WITH-TEXT-STRING PROJECT

Objective: To have Jeff's PSI include a text message based on a table of text pointers (specific pointer chosen by enemy number) - in other words, to mimic the behaviour of EB0's Check.

Current Results: Complete.




At D5/7BB8, change: [70 87 C2] --> [00 03 F0]
This will cause Spy to use code at F0/0300 instead of the default - the new code.



At F0/0300, write this new code:

    22 70 87 C2 = JSL $C28770         Do the original Spy effect.
    22 B1 38 C4 = JSL $C438B1         Print a newline.
    C2 31       = REP #$31            Now for the new effect...
    0B          = PHD
    7B          = TDC
    69 EA FF    = ADC #$FFEA
    5B          = TCD
    A9 00 04    = LDA #$0400          F0/0400 = Root of the pointer table.
    85 06       = STA $06
    A9 F0 00    = LDA #$00F0
    85 08       = STA $08
    AE 72 A9    = LDX $A972
    BD 00 00    = LDA $0000,X         Retrieve the enemy number.
    0A          = ASL
    0A          = ASL
    65 06       = ADC $06
    85 06       = STA $06             $06+08: Text pointer location (ex. F00400, F00404...)
    A7 06       = LDA [$06]
    85 0E       = STA $0E             $0E = Low half of the actual text pointer.
    E6 06       = INC $06
    E6 06       = INC $06
    A7 06       = LDA [$06]
    85 10       = STA $10             $10 = High half of the actual text pointer.
    22 B1 86 C1 = JSR $C186B1         Display the text.
    2B          = PLD
    6B          = RTL



At F0/0400, supply four-byte text pointers to the desired texts for each enemy.
For a standard game of EarthBound, there will be 0xE7 (231) such pointers.