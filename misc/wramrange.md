# EarthBound RAM Map

              7E000D = Somehow like the $2100 register

              7E001E = Somehow like the $4200 register

    7E0024 to ------ = Random number generator

    7E0031 to 7E0032 = X position of visible screen's top-left corner, in absolute pixel coordinates (BG1)
    7E0033 to 7E0034 = Y position of visible screen's top-left corner, in absolute pixel coordinates (BG1)
    7E0035 to 7E0036 = X position of visible screen's top-left corner, in absolute pixel coordinates (BG2)
    7E0037 to 7E0038 = Y position of visible screen's top-left corner, in absolute pixel coordinates (BG2)

              7E0069 = Mimics the $4218 register
              7E006A = Mimics the $4219 register

              7E006D = Mimics the $4218 register
              7E006E = Mimics the $4219 register

    7E00A7 to 7E00AA = Frame counter?





    7E0200 to 7E023F = Current text palette
    7E0240 to 7E02FF = Current map palette

    7E03A0 to 7E03BF = Current party palette
    7E03C0 to 7E03DF = Current NPC palette (humans?)
    7E03E0 to 7E03FF = Current NPC palette (nonhumans?)





    There's a bunch of tables in WRAM with 30 two-byte entries.
    These tables (hereafter "30x2 tables") are related to onscreen sprites.
    The last six entries of each table are used by the player party.
    (i.e. The Chosen Four and two party NPCs)

    7E0A62 to 7E0A9D = *** UNKNOWN - 30x2 table ***
    7E0A9E to 7E0AD9 = *** UNKNOWN - 30x2 table ***
    7E0ADA to 7E0B15 = *** UNKNOWN - 30x2 table ***
    7E0B16 to 7E0B51 = 30x2 table - X position in screen pixel coordinates
    7E0B52 to 7E0B8D = 30x2 table - Y position in screen pixel coordinates
    7E0B8E to 7E0BC9 = 30x2 table - X position in absolute pixel coordinates
    7E0BCA to 7E0C05 = 30x2 table - Y position in absolute pixel coordinates
    7E0C06 to 7E0C41 = 30x2 table - Z position in absolute pixel coordinates
    7E0C42 to 7E0C7D = 30x2 table - X position fractional component
    7E0C7E to 7E0CB9 = 30x2 table - Y position fractional component
    7E0CBA to 7E0CF5 = 30x2 table - Z position fractional component
    7E0CF6 to 7E0D31 = 30x2 table - X delta in pixels
    7E0D32 to 7E0D6D = 30x2 table - Y delta in pixels
    7E0D6E to 7E0DA9 = 30x2 table - Z delta in pixels
    7E0DAA to 7E0DE5 = 30x2 table - X delta fractional component
    7E0DE6 to 7E0E21 = 30x2 table - Y delta fractional component
    7E0E22 to 7E0E5D = 30x2 table - Z delta fractional component
    7E0E5E to 7E0E99 = *** UNKNOWN - 30x2 table - C09AF9 entry 0 - Used by timed events like Escargo Express? ***
    7E0E9A to 7E0ED5 = *** UNKNOWN - 30x2 table - C09AF9 entry 1 ***
    7E0ED6 to 7E0F11 = *** UNKNOWN - 30x2 table - C09AF9 entry 2 ***
    7E0F12 to 7E0F4D = *** UNKNOWN - 30x2 table - C09AF9 entry 3 ***
    7E0F4E to 7E0F89 = *** UNKNOWN - 30x2 table - C09AF9 entry 4 ***
    7E0F8A to 7E0FC5 = *** UNKNOWN - 30x2 table - C09AF9 entry 5 ***
    7E0FC6 to 7E1001 = 30x2 table - C09AF9 entry 6 - X destination in absolute pixel coordinates
    7E1002 to 7E103D = 30x2 table - C09AF9 entry 7 - Y destination in absolute pixel coordinates
    7E103E to 7E1079 = *** UNKNOWN - 30x2 table - Sprite-drawing priority, lower numbers first? ***
    7E107A to 7E10B5 = *** UNKNOWN - 30x2 table - Low part of a long pointer? ***
    7E10B6 to 7E10F1 = *** UNKNOWN - 30x2 table - High part of a long pointer? ***
    7E10F2 to 7E112D = *** UNKNOWN - 30x2 table - Adjusted by movement codes: [26], [3C], [3D], [3E] ***
    7E112E to 7E1169 = *** UNKNOWN - 30x2 table - Low part of a long pointer ***
    7E116A to 7E11A5 = *** UNKNOWN - 30x2 table - High part of a long pointer ***
    7E11A6 to 7E11E1 = *** UNKNOWN - 30x2 table - Short assembly pointer (set by [23] - callback: recalc screen coords?) ***
    7E11E2 to 7E121D = *** UNKNOWN - 30x2 table - Short assembly pointer (set by [22] - see C0/A0F6?) ***
    7E121E to 7E1259 = *** UNKNOWN - 30x2 table - Short assembly pointer (set by [25] - callback: atomic motion?) ***



    These next few start 0x8C bytes apart from each other.
    I'm not sure if they're actually 30x2.

    7E1372 to 7E13AD = *** UNKNOWN - 30x2 table? - Wait timer ***

    7E13FE to 7E1439 = *** UNKNOWN - 30x2 table? ***

    7E148A to 7E14C5 = *** UNKNOWN - 30x2 table? ***

    7E1516 to 7E1551 = *** UNKNOWN - 30x2 table? - Movement "tempvar" <-- RENAME ***



              7E1A42 = Used a lot, whatever it is...
                       "Onscreen sprite that's being meddled with right now"?
                       (00-1D, index for the 30x2 tables)



    7E289E to 7E28D9 = *** UNKNOWN - 30x2 table ***
    7E28DA to 7E2915 = *** UNKNOWN - 30x2 table ***
    7E2916 to 7E2951 = *** UNKNOWN - 30x2 table ***
    7E2952 to 7E298D = *** UNKNOWN - 30x2 table ***
    7E298E to 7E29C9 = *** UNKNOWN - 30x2 table ***
    7E29CA to 7E2A05 = *** UNKNOWN - 30x2 table ***
    7E2A06 to 7E2A41 = *** UNKNOWN - 30x2 table ***
    7E2A42 to 7E2A7D = *** UNKNOWN - 30x2 table ***
    7E2A7E to 7E2AB9 = *** UNKNOWN - 30x2 table ***
    7E2ABA to 7E2AF5 = *** UNKNOWN - 30x2 table ***
    7E2AF6 to 7E2B31 = *** UNKNOWN - 30x2 table ***
    7E2B32 to 7E2B6D = *** UNKNOWN - 30x2 table ***
    7E2B6E to 7E2BA9 = *** UNKNOWN - 30x2 table ***
    7E2BAA to 7E2BE5 = *** UNKNOWN - 30x2 table ***
    7E2BE6 to 7E2C21 = *** UNKNOWN - 30x2 table ***
    7E2C22 to 7E2C5D = *** UNKNOWN - 30x2 table ***
    7E2C5E to 7E2C99 = *** UNKNOWN - 30x2 table ***
    7E2C9A to 7E2CD5 = 30x2 table - TPT entry (has another meaning for generated enemies) <-- LOOK INTO
    7E2CD6 to 7E2D11 = 30x2 table - Sprite (uses standard out-of-battle sprite listing)
    7E2D12 to 7E2D4D = *** UNKNOWN - 30x2 table ***
    7E2D4E to 7E2D89 = *** UNKNOWN - 30x2 table ***
    7E2D8A to 7E2DC5 = *** UNKNOWN - 30x2 table ***
    7E2DC6 to 7E2E01 = *** UNKNOWN - 30x2 table ***
    7E2E02 to 7E2E3D = *** UNKNOWN - 30x2 table ***



              7E436C = General debug toggle (Normally set to zero at C0/B9B4 and never changed)



    7E467E to 7E4A02 = Table with 5-byte entries, related to drawing of onscreen sprites?
                       X position, graphics source, binary toggles, Y position, visibility?
                         E8 00 3A F8 00 --> F8 00 BA F8 00
                         F8 02 3A F8 80 --> E8 02 BA F8 80
                         E8 00 7A F8 00 --> F8 00 FA F8 00
                         F8 02 7A F8 80 --> E8 02 FA F8 80
                       Makes Ness walk around upside down! :D
                       Binary toggles: 80 is vertical flip, 40 horizontal... remaining bits unknown.

              7E4A60 = Magic butterfly appearance toggle (00 = appear, 01 = do not appear)

              7E4A66 = People-type TPT appearance toggle (00 = appear, 01 = do not appear)

    7E4A8C to 7E4A8D = Current battle group (see [1F 23] for listing)



              7E4DC2 = Debug flag for battle engine (0x00 = Debug mode)
                       Possibly serves another purpose most of the time? Look into this...

    7E4DC8+2n = Pointers to Chosen Four data in WRAM (99CE + n*5F, n = 0-5)



    7E4F96 to ------ = Related to movement of the Chosen Four

    7E5D60 to 7E5D61 = Countdown to battle starting.
                       Set to 0x78 (120 = 2 seconds) when battle swirl starts.
                       Only used by randomly generated enemies.
    7E5D62 to 7E5D63 = Current TPT entry

    7E5D74 to ------ = Related to timers?

    7E5DA0 to 7E5DA1 = If nonzero, causes mushroomization walking-direction-jumbling.

    <Penguin> Michael1: $7E5DC4 seems to be stairs-related...
    <Penguin> it's a bunch of flags of some kind

    7E5DEA to 7E5E01 = *** UNKNOWN - Door-related table, four entries, six bytes each, last four are a long pointer ***
    7E5E02 to 7E5E03 = *** UNKNOWN - Door-related ***
    7E5E04 to 7E5E05 = *** UNKNOWN - Door-related ***



    7E8650 to 7E88DF = Window statistics table (see WindowStat Footnote)
    7E88E0 to 7E88E3 = *** UNKNOWN ***
    7E88E4 to 7E8957 = Window existence table (see WindowExt Footnote)
              7E8958 = Current window with focus (not a pointer)

              7E89CA = Main menu PSI window character number (Ness = 0x00, Poo = 0x03)

    7E89D4 to 7E9621 = Text slots for 70 strings of 45 bytes each?
              7E9622 = *** UNKNOWN ***
              7E9623 = *** UNKNOWN ***
              7E9624 = *** UNKNOWN ***

    7E9643 to 7E9644 = 0x00 outside of battle, 0x01 in battle
              7E9645 = Locks button input during text parsing if 01 (used by 1F 50 and 1F 51)

              7E964D = 0x00 for blinking triangle prompt, nonzero for absence of this prompt

              7E964F = Related to the sound of text printing?



    7E96AA + (0x1B * n) = Table involved with text subroutines (08 code) somehow.
       Ten entries of 0x1B (27) bytes each, so it runs up to 7E97B7?
       The current entry to use seems to be in 97B8...
       Text system stack frames?

    7E96C5 to 7E96C7 = Current text parser location (usually)

              7E97B8 = Which entry in the 96AA table to use?



    7E97BA to 7E97C9 = CC argument storage
              7E97CA = CC argument-gathering loop counter

    7E97DA - Storage for strings loaded with [19]? (How much space?)

    7E97F5 to 7E9800 = First 12 letters of player name with spaces (0x50) replaced with K (0x7B). Leftover from Mother 2.
    7E9801 to 7E9818 = Player name
    7E9819 to 7E981E = Pet's name
    7E981F to 7E9824 = Favourite food
    7E9825 to 7E9828 = "PSI "
    7E9829 to 7E982E = Favourite thing
    7E982F to 7E9830 = " " and 0x00 (Favourite thing string termination)
    7E9831 to 7E9834 = Money on hand
    7E9835 to 7E9838 = Money in ATM
              7E9839 = [1F 71] effects (see [1F 71] Footnote)
              7E983A = Party NPC #1 - Character number ([1C 02] listing)
              7E983B = Party NPC #2 - Character number ([1C 02] listing)
    7E983C to 7E983D = Party NPC #1 - Current HP
    7E983E to 7E983F = Party NPC #2 - Current HP
              7E9840 = Party status (00 = Normal, 01 = Burnt, 03 = Skip Sandwich)
    7E9841 to 7E984A = *** UNKNOWN ***
    7E984B to 7E986E = Escargo Express stored items
    7E986F to 7E9874 = Party members <-- How does this differ from 7E988B?
    7E9875 to 7E9876 = *** UNKNOWN ***
    7E9877 to 7E9878 = Party leader X coordinate
    7E9879 to 7E987A = Party leader Y coordinate
    7E987B to 7E987E = *** UNKNOWN ***
              7E987F = Party leader facing direction
    7E9880 to 7E9882 = *** UNKNOWN ***
              7E9883 = Current walking style (see 7E9883 Footnote)
    7E9884 to 7E988A = *** UNKNOWN ***
    7E988B to 7E9890 = Party members <-- How does this differ from 7E986F?
    7E9891 to 7E98A2 = *** UNKNOWN ***
              7E98A3 = Number of party members
              7E98A4 = Number of player controlled party members
    7E98A5 to 7E98B0 = *** UNKNOWN ***
              7E98B1 = Auto Fight toggle (00 = Off, 01 = On)
    7E98B2 to 7E98B5 = Exit Mouse coordinates, stored when [1F 68] used
              7E98B6 = Text speed (01 = Fast, 02 = Medium, 03 = Slow)
              7E98B7 = Sound setting (01 = Stereo, 02 = Mono)
    7E98B8 to 7E98C0 = *** UNKNOWN ***

    (98C1 to 98C8 is used by hotspots somehow - four bytes for each of the two "slots")
    (98C9 to 99C8 is photoman-related! Eight-byte entries, one for each photo.)

    7E99C9 to 7E99CC = Filled with $00A7 and $00A9 when saving - a save timestamp?
              7E99CD = Text flavor (01 = Plain, 02 = Mint, ... 05 = Peanut)
    7E99CE to 7E99D2 = Ness name
              7E99D3 = Ness level
    7E99D4 to 7E99D7 = Ness exp
    7E99D8 to 7E99D9 = Ness max HP
    7E99DA to 7E99DB = Ness max PP
    7E99DC to 7E99E2 = Ness status (see Status Footnote)
              7E99E3 = Ness postmath stats - Offense
              7E99E4 = Ness postmath stats - Defense
              7E99E5 = Ness postmath stats - Speed
              7E99E6 = Ness postmath stats - Guts
              7E99E7 = Ness postmath stats - Luck
              7E99E8 = Ness postmath stats - Vitality
              7E99E9 = Ness postmath stats - IQ
              7E99EA = Ness base stats - Offense
              7E99EB = Ness base stats - Defense
              7E99EC = Ness base stats - Speed
              7E99ED = Ness base stats - Guts
              7E99EE = Ness base stats - Luck
              7E99EF = Ness base stats - Vitality
              7E99F0 = Ness base stats - IQ
    7E99F1 to 7E99FE = Ness items
    7E99FF to 7E9A02 = Ness equipment (numbers = positions in pack, 01-0E, 00 if unequipped)
    7E9A03 to 7E9A12 = Ness *** UNKNOWN ***
    7E9A13 to 7E9A14 = Ness current HP
    7E9A15 to 7E9A16 = Ness rolling HP target
    7E9A17 to 7E9A18 = Ness *** UNKNOWN ***
    7E9A19 to 7E9A1A = Ness current PP
    7E9A1B to 7E9A1C = Ness rolling PP target
    7E9A1D to 7E9A1E = Ness HP/PP window mode (see HP/PP Footnote)
              7E9A1F = Ness miss rate (misses/16 attempts)
              7E9A20 = Ness weakness to PSI Fire
              7E9A21 = Ness weakness to PSI Freeze
              7E9A22 = Ness weakness to PSI Flash
              7E9A23 = Ness weakness to Paralysis
              7E9A24 = Ness weakness to Hypnosis/Brainshock (see Hypnosis/Brainshock Footnote)
              7E9A25 = Ness additional stat boost - Speed
              7E9A26 = Ness additional stat boost - Guts
              7E9A27 = Ness additional stat boost - Vitality
              7E9A28 = Ness additional stat boost - IQ
              7E9A29 = Ness additional stat boost - Luck
    7E9A2A to 7E9A2C = Ness *** UNKNOWN ***
    7E9A2D to 7E9A31 = Paula name
              7E9A32 = Paula level
    7E9A33 to 7E9A36 = Paula exp
    7E9A37 to 7E9A38 = Paula max HP
    7E9A39 to 7E9A3A = Paula max PP
    7E9A3B to 7E9A41 = Paula status (see Status Footnote)
              7E9A42 = Paula postmath stats - Offense
              7E9A43 = Paula postmath stats - Defense
              7E9A44 = Paula postmath stats - Speed
              7E9A45 = Paula postmath stats - Guts
              7E9A46 = Paula postmath stats - Luck
              7E9A47 = Paula postmath stats - Vitality
              7E9A48 = Paula postmath stats - IQ
              7E9A49 = Paula base stats - Offense
              7E9A4A = Paula base stats - Defense
              7E9A4B = Paula base stats - Speed
              7E9A4C = Paula base stats - Guts
              7E9A4D = Paula base stats - Luck
              7E9A4E = Paula base stats - Vitality
              7E9A4F = Paula base stats - IQ
    7E9A50 to 7E9A5D = Paula items
    7E9A5E to 7E9A61 = Paula equipment (numbers = positions in pack, 01-0E, 00 if unequipped)
    7E9A62 to 7E9A71 = Paula *** UNKNOWN ***
    7E9A72 to 7E9A73 = Paula current HP
    7E9A74 to 7E9A75 = Paula rolling HP target
    7E9A76 to 7E9A77 = Paula *** UNKNOWN ***
    7E9A78 to 7E9A79 = Paula current PP
    7E9A7A to 7E9A7B = Paula rolling PP target
    7E9A7C to 7E9A7D = Paula HP/PP window mode (see HP/PP Footnote)
              7E9A7E = Paula miss rate (misses/16 attempts)
              7E9A7F = Paula weakness to PSI Fire
              7E9A80 = Paula weakness to PSI Freeze
              7E9A81 = Paula weakness to PSI Flash
              7E9A82 = Paula weakness to Paralysis
              7E9A83 = Paula weakness to Hypnosis/Brainshock (see Hypnosis/Brainshock Footnote)
              7E9A84 = Paula additional stat boost - Speed
              7E9A85 = Paula additional stat boost - Guts
              7E9A86 = Paula additional stat boost - Vitality
              7E9A87 = Paula additional stat boost - IQ
              7E9A88 = Paula additional stat boost - Luck
    7E9A89 to 7E9A8B = Paula *** UNKNOWN ***
    7E9A8C to 7E9A90 = Jeff name
              7E9A91 = Jeff level
    7E9A92 to 7E9A95 = Jeff exp
    7E9A96 to 7E9A97 = Jeff max HP
    7E9A98 to 7E9A99 = Jeff max PP
    7E9A9A to 7E9AA0 = Jeff status (see Status Footnote)
              7E9AA1 = Jeff postmath stats - Offense
              7E9AA2 = Jeff postmath stats - Defense
              7E9AA3 = Jeff postmath stats - Speed
              7E9AA4 = Jeff postmath stats - Guts
              7E9AA5 = Jeff postmath stats - Luck
              7E9AA6 = Jeff postmath stats - Vitality
              7E9AA7 = Jeff postmath stats - IQ
              7E9AA8 = Jeff base stats - Offense
              7E9AA9 = Jeff base stats - Defense
              7E9AAA = Jeff base stats - Speed
              7E9AAB = Jeff base stats - Guts
              7E9AAC = Jeff base stats - Luck
              7E9AAD = Jeff base stats - Vitality
              7E9AAE = Jeff base stats - IQ
    7E9AAF to 7E9ABC = Jeff items
    7E9ABD to 7E9AC0 = Jeff equipment (numbers = positions in pack, 01-0E, 00 if unequipped)
    7E9AC1 to 7E9AD0 = Jeff *** UNKNOWN ***
    7E9AD1 to 7E9AD2 = Jeff current HP
    7E9AD3 to 7E9AD4 = Jeff rolling HP target
    7E9AD5 to 7E9AD6 = Jeff *** UNKNOWN ***
    7E9AD7 to 7E9AD8 = Jeff current PP
    7E9AD9 to 7E9ADA = Jeff rolling PP target
    7E9ADB to 7E9ADC = Jeff HP/PP window mode (see HP/PP Footnote)
              7E9ADD = Jeff miss rate (misses/16 attempts)
              7E9ADE = Jeff weakness to PSI Fire
              7E9ADF = Jeff weakness to PSI Freeze
              7E9AE0 = Jeff weakness to PSI Flash
              7E9AE1 = Jeff weakness to Paralysis
              7E9AE2 = Jeff weakness to Hypnosis/Brainshock (see Hypnosis/Brainshock Footnote)
              7E9AE3 = Jeff additional stat boost - Speed
              7E9AE4 = Jeff additional stat boost - Guts
              7E9AE5 = Jeff additional stat boost - Vitality
              7E9AE6 = Jeff additional stat boost - IQ
              7E9AE7 = Jeff additional stat boost - Luck
    7E9AE8 to 7E9AEA = Jeff *** UNKNOWN ***
    7E9AEB to 7E9AEF = Poo name
              7E9AF0 = Poo level
    7E9AF1 to 7E9AF4 = Poo exp
    7E9AF5 to 7E9AF6 = Poo max HP
    7E9AF7 to 7E9AF8 = Poo max PP
    7E9AF9 to 7E9AFF = Poo status (see Status Footnote)
              7E9B00 = Poo postmath stats - Offense
              7E9B01 = Poo postmath stats - Defense
              7E9B02 = Poo postmath stats - Speed
              7E9B03 = Poo postmath stats - Guts
              7E9B04 = Poo postmath stats - Luck
              7E9B05 = Poo postmath stats - Vitality
              7E9B06 = Poo postmath stats - IQ
              7E9B07 = Poo base stats - Offense
              7E9B08 = Poo base stats - Defense
              7E9B09 = Poo base stats - Speed
              7E9B0A = Poo base stats - Guts
              7E9B0B = Poo base stats - Luck
              7E9B0C = Poo base stats - Vitality
              7E9B0D = Poo base stats - IQ
    7E9B0E to 7E9B1B = Poo items
    7E9B1C to 7E9B1F = Poo equipment (numbers = positions in pack, 01-0E, 00 if unequipped)
    7E9B20 to 7E9B2F = Poo *** UNKNOWN ***
    7E9B30 to 7E9B31 = Poo current HP
    7E9B32 to 7E9B33 = Poo rolling HP target
    7E9B34 to 7E9B35 = Poo *** UNKNOWN ***
    7E9B36 to 7E9B37 = Poo current PP
    7E9B38 to 7E9B39 = Poo rolling PP target
    7E9B3A to 7E9B3B = Poo HP/PP window mode (see HP/PP Footnote)
              7E9B3C = Poo miss rate (misses/16 attempts)
              7E9B3D = Poo weakness to PSI Fire
              7E9B3E = Poo weakness to PSI Freeze
              7E9B3F = Poo weakness to PSI Flash
              7E9B40 = Poo weakness to Paralysis
              7E9B41 = Poo weakness to Hypnosis/Brainshock (see Hypnosis/Brainshock Footnote)
              7E9B42 = Poo additional stat boost - Speed
              7E9B43 = Poo additional stat boost - Guts
              7E9B44 = Poo additional stat boost - Vitality
              7E9B45 = Poo additional stat boost - IQ
              7E9B46 = Poo additional stat boost - Luck
    7E9B47 to 7E9B49 = Poo *** UNKNOWN ***
    7E9B4A to 7E9BA8 = Party NPC #1 information (same format as Chosen Four entries above)
    7E9BA9 to 7E9C07 = Party NPC #2 information (same format as Chosen Four entries above)
    7E9C08 to 7E9C87 = Event flags
    7E9C88 to 7E9C89 = Event flag of the TPT entry you're currently interacting with
    7E9C8A to 7E9C94 = *** UNKNOWN - Filled by C2/0A20 when called at C1/9DCB ***

    7E9E3C to 7E9E3D = Timer for Skip Sandwich run effect?

    7E9E54 to 7E9E55 = Timer for Dad's phone calls

              7E9F79 = Bit flags indicating which saves (if any) have been corrupted?

    7E9F8A to 7E9F8B = Number of enemies in current battle

    7E9FAC to 7EA96B = Battlers table (see 7E9FAC_0x4E_byte_structure.txt)
    7EA96C to 7EA96F = Battler bit flags
    7EA970 to 7EA971 = Current attacker (Pointer to a 7E9FAC battler entry)
    7EA972 to 7EA973 = Current target (Pointer to a 7E9FAC battler entry)
    7EA974 to 7EA977 = Scratch space for battle-end experience calculations
    7EA978 to 7EA979 = Scratch space for battle-end money calculations
    7EA97A to 7EA97B = Giygas battle stage counter
              7EA97C = *** UNKNOWN ***
              7EA97D = *** UNKNOWN ***
              7EA97E = *** UNKNOWN ***
    7EA97F to 7EA980 = *** UNKNOWN ***
              7EA981 = *** UNKNOWN ***
              7EA982 = *** UNKNOWN ***
    7EA983 to 7EA9-- = C2/3BCF scratch space for attacker name adjustments (eg. 0xAC --> "Ness" for the Nightmare)
    7EA99E to 7EA9-- = C2/3D05 scratch space for target name adjustments (eg. 0xAC --> "Ness" for the Nightmare)

              7EAA10 = Item to be received at the end of battle
              7EAA12 = Enemy number of the enemy Poo is currently mirroring

    7EAA8E to 7EAA8F = ??? (Used in C2/7EAF, but irrelevantly)
    7EAA90 to 7EAA91 = ??? (Used in C2/7EAF)

              7EAA94 = See C2/941D
              7EAA96 = See C2/941D



              7EAD9E = Used for the green SMAAAASH flash?
              7EADA0 = Used for the red SMAAAASH flash?

              7EAEC5 = Current frame of an E6B14 animation?

              7EB4A1 = Current selected save slot (01-03)

    7EB53B to 7EB53C = Current music (1F 02 listing)
    7EB53D to 7EB542 = Current 4F90A entry contents (ex. "05 08 13"), one word for each part

              7EB549 = See 7EB549 Footnote



## WindowStat Footnote
* This table stores data about currently open windows, such as font, window size and variables/memory stuff.
* The first entry is the first window opened (the window with 0x0000 in the 7E88E4 table); the second entry, the second window (0x0001); etc.
* Strictly speaking, the end of this table is not known. However, assuming it takes up all the space it can (that is, all the space up until the next known region) and factoring in the length of entries (0x52), then 7E88DF provides eight 0x52-byte-entries. That being said, this also means all hell should (theoretically) break loose if you try to have more than eight windows open.
* For more details, refer to:
  http://pkhack.fobby.net/misc/txt/7E8650_0x52_byte_structure.txt
  http://pkhack.fobby.net/misc/1bnotes_current.txt



## WindowExt Footnote
* This table corresponds to the window table at 0x3E450.
* Say, for example, you open the Command Window (with Talk To, Check, etc.)
* This is window 0, so entry 0 of this table changes. (7E88E4)
* Since it's the first window to be opened, 0000 is placed there.
* Say you chose Talk To. The text window pops up ("Who are you talking to?").
* This is window 1 in the table, so entry 1 here changes. (7E88E6)
* Since it's the second window to be opened, 0001 is placed here.
* In short: "Position indicates which window, and value indicates order of opening."
* For info on the window table:
  http://www.datacrystal.org/wiki/EarthBound:Dialog_Window_Attributes_Table
  http://pkhack.fobby.net/misc/txt/windows.txt



## [1F 71] Footnote
* This works as a set of binary toggles.
* 01 = Ness, Teleport A
* 02 = Poo, Starstorm A
* 04 = Poo, Starstorm O
* 08 = Ness, Teleport B



## 7E9883 Footnote
* The value stuff here is based on tweaks of the value.
* 00 = Normal
* 03 = Bicycle movement style without sprite
* 04 = Party has ghost sprites but is alive
* 06 = Party moves slightly slower
* 07 = Ladder climbing
* 08 = Rope climbing
* 0A = Party moves VERY slowly (Deep-Darkness-style slow)
* 0C = Escalator sliding movement
* 0D = Stair-style movement



## HP/PP Footnote
* `[00 --] = 0x--00`  Normal
* `[0C --] = 0x--0C`  Thin border
* `[-- 04] = 0x04--`  Normal
* `[-- 0C] = 0x0C--`  Black
* `[-- 14] = 0x14--`  Flashing



## Hypnosis/Brainshock Footnote
* The actual value here is for Hypnosis.
* Brainshock is defined as: 0x03 - this value.
* 255/256, 128/256, 26/256 and 0/256 are the actual fractions.
* So these are the resulting effectiveness values:

  |Val|Hypnosis|Brainshock|
  |---|--------|----------|
  |00 |99 %    |0 %       |
  |01 |50 %    |10 %      |
  |02 |10 %    |50 %      |
  |03 |0 %     |99 %      |



## Status Footnote
* Add the first number to the start address of the status section of the chosen character.
* Place the second number at the address found in the previous step.

* `[00 00]` = Normal
* `[00 01]` = Unconscious
* `[00 02]` = Diamondized
* `[00 03]` = Paralyzed
* `[00 04]` = Nauseous
* `[00 05]` = Poisoned
* `[00 06]` = Sunstroke
* `[00 07]` = Sniffling
* `[01 01]` = Mushroomized
* `[01 02]` = Possessed
* `[02 01]` = Asleep
* `[02 02]` = Crying
* `[02 03]` = Immobilized
* `[02 04]` = Solidified
* `[03 01]` = Feeling strange
* `[04 --]` = Can't concentrate (countdown)
* `[05 01]` = Homesick
* `[06 01]` = PSI reflecting      (PSI Shield Beta)
* `[06 02]` = PSI blocking        (PSI Shield Alpha)
* `[06 03]` = Physical reflecting (Shield Beta)
* `[06 04]` = Physical blocking   (Shield Alpha)



## 7EB549 Footnote
* 00 = When crossing sector boundaries, the game does NOT check for music changes.
* 01 = When crossing sector boundaries, the game DOES check for music changes. (This is the default behaviour.)