# 42-CODES

The [42] code in movement CCs has a variable number of arguments, since
the subroutine called sometimes uses some. This file documents the number
of arguments used (last number is post-pointer number of arguments).
List sorted by pointer address.

------ NOTES ------
C09D86 = +1 (long)
C09D8D = +1 (short)
C09D94 = +2 (long)
C09D99 = +2 (short)


    42 F1 20 C0 = C020F1 to C0213F = 0


    42 AA 3D C0 = C03DAA to ------ = 0


    42 1E 3F C0 = C03F1E to C03FA8 = 0


    42 F0 4E C0 = C04EF0 to ------ = 0


    42 76 5E C0 = C05E76 to C05E81 = 4              I'm not sure about this...
    42 82 5E C0 = C05E82 to ------ = 0


    42 CE 5E C0 = C05ECE to ------ = 0


    42 78 64 C0 = C06478 to ------ = 0


    42 9A 8E C0 = C08E9A to ------ = 0


    42 51 94 C0 = C09451 to C09465 = 0


    42 71 9E C0 = C09E71 to C09E78 = 2


    42 43 9F C0 = C09F43 to C09F70 = 0
    42 71 9F C0 = C09F71 to C09F81 = 0
    42 82 9F C0 = C09F82 to C09FA7 = 1+2n           n is the first argument
    42 A8 9F C0 = C09FA8 to C09FAD = 0
    42 AE 9F C0 = C09FAE to C09FB7 = 2
    42 BB 9F C0 = C09FBB to C09FF0 = 2


    42 80 A4 C0 = C0A480 to C0A48D = 0


    42 A8 A4 C0 = C0A4A8 to C0A4B1 = 0
    42 B2 A4 C0 = C0A4B2 to C0A4BE = 0
    42 BF A4 C0 = C0A4BF to C0A56D = 0


    42 43 A6 C0 = C0A643 to C0A650 = 2
    42 51 A6 C0 = C0A651 to C0A65E = 1
    42 5F A6 C0 = C0A65F to C0A66C = 0


    42 73 A6 C0 = C0A673 to C0A678 = 0              A = $2AF6,*$88
    42 79 A6 C0 = C0A679 to C0A684 = 1              $2BAA,*$88 = [xx]
    42 85 A6 C0 = C0A685 to C0A690 = 2              $2B32,*$88 = [xx xx] <-- Set movement speed?
    42 8B A6 C0 = C0A68B to C0A690 = 0              $2B32,*$88 = A
    42 91 A6 C0 = C0A691 to C0A696 = 0              A = $2B32,*$88
    42 79 A6 C0 = C0A697 to C0A6A1 = 1              C0C83B with A = arg
    42 A2 A6 C0 = C0A6A2 to C0A6AC = 2              C0CA4E with A = args
    42 AD A6 C0 = C0A6AD to C0A6B7 = 2              C0CBD3 with A = args
    42 B8 A6 C0 = C0A6B8 to C0A6C4 = 0              A = 0x0000 if $289E,*$88 is negative, 0xFFFF if not



    42 D1 A6 C0 = C0A6D1 to C0A6D9 = 0              $289E,*$88 = 0x8000
    42 DA A6 C0 = C0A6DA to C0A6E2 = 0              $289E,*$88 = 0xFFFF
    42 E3 A6 C0 = C0A6E3 to C0A6FA = 0



    42 2F A8 C0 = C0A82F to C0A837 = 0              $289E,*$88 = 0x8000
    42 38 A8 C0 = C0A838 to C0A840 = 0              $289E,*$88 = 0xFFFF
    42 41 A8 C0 = C0A841 to C0A84B = 2              C0ABE0 with A = args
    42 4C A8 C0 = C0A84C to C0A856 = 2              C21628 with A = args
                                                       A = Flag [xx xx]
    42 57 A8 C0 = C0A857 to C0A863 = 2              C2165E with A = args, X = origA
                                                       Flag [xx xx] = origA (0x0000 to unset, 0x0001 to set)
    42 64 A8 C0 = C0A864 to C0A86E = 1              C46C9B with A = arg
    42 6F A8 C0 = C0A86F to C0A879 = 2              C46CC7 with A = args
    42 7A A8 C0 = C0A87A to C0A88C = 4              C46CF5 with A = args34, X = args12
    42 8D A8 C0 = C0A88D to C0A89F = 4              C46E4F with A = args34, X = args12
                                                       Display text (Argument pointer: High/Null/Low/Middle)
    42 A0 A8 C0 = C0A8A0 to C0A8B2 = 4              C466F0 with A = args34, X = args12
    42 B3 A8 C0 = C0A8B3 to C0A8C5 = 4              C46C5E with A = args34, X = args12
    42 C6 A8 C0 = C0A8C6 to C0A8D0 = 0              C47143 with A = 0x0000, X = 0x0000
    42 D1 A8 C0 = C0A8D1 to C0A8DB = 0              C47143 with A = 0x0001, X = 0x0000
    42 DC A8 C0 = C0A8DC to C0A8E6 = 0              C47143 with A = 0x0000, X = 0x0001
    42 E7 A8 C0 = C0A8E7 to C0A8EE = 0              C472A8 with A = 0x0000



    42 F7 A8 C0 = C0A8F7 to C0A8FE = 0              C46DAD with A = 0x0000
    42 FF A8 C0 = C0A8FF to C0A906 = 0              C46DAD with A = 0x0001
    42 07 A9 C0 = C0A907 to C0A911 = 1              C46DE5 with A = arg
                                                       [1F 21 xx] teleportation
    42 12 A9 C0 = C0A912 to C0A92C = 5              C46E37 with A = arg5, X = args12, Y = args34
    42 2D A9 C0 = C0A92D to C0A937 = 2              C46B8D with A = args
    42 38 A9 C0 = C0A938 to C0A942 = 2              C46BBB with A = args
    42 43 A9 C0 = C0A943 to C0A94D = 1              C46BE9 with A = arg
    42 4E A9 C0 = C0A94E to C0A958 = 2              C46984 with A = args
    42 59 A9 C0 = C0A959 to C0A963 = 2              C469F1 with A = args
    42 64 A9 C0 = C0A964 to C0A976 = 4              C47225 with A = args34, X = args12
    42 77 A9 C0 = C0A977 to C0A98A = 4              C47370 with A = args12, X = args34
    42 8B A9 C0 = C0A98B to C0A99E = 4              C46534 with A = args12, X = args34
    42 9F A9 C0 = C0A99F to C0A9B2 = 4              C4ECAD with A = args12, X = args34
    42 B3 A9 C0 = C0A9B3 to C0A9CE = 6
    42 CF A9 C0 = C0A9CF to C0A9EA = 6
    42 EB A9 C0 = C0A9EB to C0AA06 = 6
    42 07 AA C0 = C0AA07 to C0AA22 = 6
    42 23 AA C0 = C0AA23 to C0AA3E = 6
    42 3F AA C0 = C0AA3F to C0AA6D = 3
    42 6E AA C0 = C0AA6E to C0AA8F = 2              Used a LOT, whatever it is...



    42 AC AA C0 = C0AAAC to C0AAB4 = 0
    42 B5 AA C0 = C0AAB5 to C0AACC = 4
    42 CD AA C0 = C0AACD to C0AAD0 = 0


    42 D5 AA C0 = C0AAD5 to C0AAFC = 3
    42 FD AA C0 = C0AAFD to C0AB05 = 0


    42 9B C1 C0 = C0C19B to C0C250 = 0
    42 51 C2 C0 = C0C251 to C0C30B = 0


    42 53 C3 C0 = C0C353 to C0C35C = 0
    42 5D C3 C0 = C0C35D to C0C362 = 0              A = 7E9885


    42 8F C4 C0 = C0C48F to C0C4AE = 0
    42 AF C4 C0 = C0C4AF to C0C4CE = 0


    42 F7 C4 C0 = C0C4F7 to C0C523 = 0


    42 2B C6 C0 = C0C62B to C0C681 = 0
    42 82 C6 C0 = C0C682 to C0C69D = 0


    42 B6 C6 C0 = C0C6B6 to C0C710 = 0              A = 0x0000 if sprite near player's location, 0xFFFF otherwise?


    42 DB C7 C0 = C0C7DB to C0C807 = 0


    42 3B C8 C0 = C0C83B to C0CA4D = 0
    42 4E CA C0 = C0CA4E to C0CBD2 = 0


    42 11 CC C0 = C0CC11 to C0CCCB = 0
    42 CC CC C0 = C0CCCC to C0CD4F = 0
    42 50 CD C0 = C0CD50 to C0CEBD = 0
    42 BE CE C0 = C0CEBE to C0CF57 = 0


    42 D9 D0 C0 = C0D0D9 to C0D0E5 = 0
    42 E6 D0 C0 = C0D0E6 to C0D15B = 0
    42 5C D1 C0 = C0D15C to C0D194 = 0


    42 9B D5 C0 = C0D59B to C0D5AF = 0
    42 B0 D5 C0 = C0D5B0 to C0D77E = 0
    42 7F D7 C0 = C0D77F to C0D7B2 = 0
    42 B3 D7 C0 = C0D7B3 to C0D7C6 = 0
    42 C7 D7 C0 = C0D7C7 to C0D7DF = 0


    42 8F D9 C0 = C0D98F to C0DA30 = 0


    42 77 EC C0 = C0EC77 to C0ECB6 = 0              Related to the palettes for the title screen animation?
    42 B7 EC C0 = C0ECB7 to C0ED13 = 0              Related to the Title Screen copyright text palette?


    42 5C ED C0 = C0ED5C to C0EDD0 = 0              Related to the Title Screen copyright text palette?
    42 D1 ED C0 = C0EDD1 to C0EDD9 = 0
    42 DA ED C0 = C0EDDA to C0EE46 = 0


    42 B2 F3 C0 = C0F3B2 to C0F3E7 = 0              Related to the Gas Station alternate palette?
    42 E8 F3 C0 = C0F3E8 to C0F41D = 0              Related to the Gas Station palette?


    42 D3 FF C1 = C1FFD3 to C1FFDE = 0              Part of the anti-piracy checking?


    42 00 00 C2 = C20000 to C200B8 = 0


    42 4C 65 C2 = C2654C to C26633 = 0


    42 3F DB C2 = C2DB3F to C2DE0E = 0


    42 15 EA C2 = C2EA15 to C2EA73 = 0
    42 74 EA C2 = C2EA74 to C2EAA9 = 0


    42 CF EA C2 = C2EACF to C2EAE9 = 0


    42 9A FF C2 = C2FF9A to C2FFB6 = 0


    42 00 01 C3 = C30100 to C30141 = 0


    42 15 00 C4 = C40015 to C40022 = 0
    42 23 00 C4 = C40023 to C4002E = 0


    42 DC 23 C4 = C423DC to C42409 = 0
    42 0A 24 C4 = C4240A to C42438 = 0


    42 8A 24 C4 = C4248A to C42499 = 0


    42 D1 24 C4 = C424D1 to C42508 = 0


    42 7F 25 C4 = C4257F to C4258B = 0
    42 8C 25 C4 = C4258C to C425CB = 0


    42 F3 25 C4 = C425F3 to C425FC = 0


    42 24 26 C4 = C42624 to ------ = 0


    42 ED 26 C4 = C426ED to ------ = 0


    42 12 67 C4 = C46712 to ------ = 0


    42 B4 67 C4 = C467B4 to C467C1 = 0
    42 C2 67 C4 = C467C2 to C467E5 = 0


    42 A9 68 C4 = C468A9 to C468AE = 0


    42 B5 68 C4 = C468B5 to C468DB = 0
    42 DC 68 C4 = C468DC to C46902 = 0
    42 03 69 C4 = C46903 to C46913 = 0
    42 14 69 C4 = C46914 to C46956 = 0
    42 57 69 C4 = C46957 to C46983 = 0


    42 6E 6A C4 = C46A6E to C46A79 = 0


    42 9A 6A C4 = C46A9A to C46AA2 = 0


    42 6E 6A C4 = C46AAE to C46ADA = 0
    42 DB 6A C4 = C46ADB to C46B09 = 0
    42 0A 6B C4 = C46B0A to C46B2C = 0
    42 2D 6B C4 = C46B2D to C46B36 = 0


    42 51 6B C4 = C46B51 to C46B64 = 0
    42 65 6B C4 = C46B65 to ------ = 0


    42 45 6C C4 = C46C45 to C46C5D = 0


    42 87 6C C4 = C46C87 to ------ = 0


    42 23 6D C4 = C46D23 to ------ = 0


    42 4B 6D C4 = C46D4B to ------ = 0


    42 46 6E C4 = C46E46 to C46E4E = 0              Set 7E9641 to 0x1


    42 74 6E C4 = C46E74 to C46EF7 = 0
    42 F8 6E C4 = C46EF8 to ------ = 0


    42 44 70 C4 = C47044 to ------ = 0


    42 69 72 C4 = C47269 to C472A7 = 0


    42 0E 73 C4 = C4730E to C47332 = 0
    42 33 73 C4 = C47333 to C4733B = 0
    42 3C 73 C4 = C4733C to C4734B = 0
    42 4C 73 C4 = C4734C to C47368 = 0
    42 69 73 C4 = C47369 to C4736F = 0


    42 99 74 C4 = C47499 to C474A7 = 0
    42 A8 74 C4 = C474A8 to C474F5 = 0


    42 6B 7A C4 = C47A6B to ------ = 0
    42 9E 7A C4 = C47A9E to ------ = 0


    42 77 7B C4 = C47B77 to C47C3E = 0


    42 0B 80 C4 = C4800B to C48036 = 0


    42 0C 88 C4 = C4880C to ------ = 0


    42 6D 8A C4 = C48A6D to ------ = 0


    42 2C 8B C4 = C48B2C to C48B3A = 0
    42 3B 8B C4 = C48B3B to C48BD9 = 0


    42 40 97 C4 = C49740 to ------ = 0


    42 8E 97 C4 = C4978E to C497BF = 0


    42 1F 98 C4 = C4981F to C49840 = 0
    42 41 98 C4 = C49841 to C4984A = 0


    42 C4 9E C4 = C49EC4 to C49FE0 = 0


    42 B0 A7 C4 = C4A7B0 to ------ = 0


    42 4F CB C4 = C4CB4F to ------ = 0


    42 8F CB C4 = C4CB8F to ------ = 0


    42 E3 CB C4 = C4CBE3 to ------ = 0


    42 2C CC C4 = C4CC2C to C4CC2E = 0
    42 2F CC C4 = C4CC2F to C4CD43 = 0
    42 44 CD C4 = C4CD44 to C4CEAF = 0
    42 B0 CE C4 = C4CEB0 to C4CED7 = 0
    42 D8 CE C4 = C4CED8 to C4D00E = 0


    42 28 DD C4 = C4DD28 to ------ = 0


    42 D0 DD C4 = C4DDD0 to C4DE77 = 0


    42 D0 DE C4 = C4DED0 to ------ = 0


    42 D7 E2 C4 = C4E2D7 to ------ = 0


    42 DA E4 C4 = C4E4DA to ------ = 0


    42 F9 E4 C4 = C4E4F9 to ------ = 0


    42 E7 EC C4 = C4ECE7 to ------ = 0


    42 7D 02 EF = EF027D to EF02C3 = 0


    42 87 0C EF = EF0C87 to EF0C96 = 0
    42 97 0C EF = EF0C97 to EF0CA6 = 0


    42 23 0D EF = EF0D23 to EF0D45 = 0
    42 46 0D EF = EF0D46 to EF0D72 = 0
    42 73 0D EF = EF0D73 to EF0D8C = 0
    42 8D 0D EF = EF0D8D to EF0DF9 = 0
    42 FA 0D EF = EF0DFA to EF0E66 = 0
    42 67 0E EF = EF0E67 to EF0E89 = 0
    42 8A 0E EF = EF0E8A to EF0EAC = 0


    42 60 0F EF = EF0F60 to EF0FDA = 0
    42 DB 0F EF = EF0FDB to EF0FF5 = 0
    42 F6 0F EF = EF0FF6 to EF101A = 0


    42 56 E5 EF = EFE556 to EFE577 = 0
