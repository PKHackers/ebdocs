# $7E9FAC - Battlers' Table

## Misc Notes

Starts at 7E9FAC in WRAM.
Entries are 0x4E bytes in length.

This table stores stats for everyone acting in the current battle.
Battlers can be:

  - The Chosen Four
     - Ness
     - Paula
     - Jeff
     - Poo
  - Party NPCs
    These are computer-controlled and have behaviour based on an entry in enemy data. They use the NPC slots in the out-of-battle statistics table (7E9B4A, 7E9BA9) when they are in your party. As such, they have persistent stats (eg. HP) between battles.
     - Pokey
     - Picky
     - King
     - Tony
     - Bubble Monkey
     - Brick Road
     - Flying Man (1)
     - Flying Man (2)
     - Flying Man (3)
     - Flying Man (4)
     - Flying Man (5)
     - Teddy Bear
     - Super Plush Bear
  - Buzz Buzz
  - Tiny Li'l Ghost
  - Ordinary enemies


Some of the entries are reserved for specific battlers:

    Entry 00    9FAC to 9FF9    Ness
    Entry 01    9FFA to A047    Paula
    Entry 02    A048 to A095    Jeff
    Entry 03    A096 to A0E3    Poo
    Entry 04    A0E4 to A131    Party NPC #1
    Entry 05    A132 to A17F    Party NPC #2
    Entry 06    A180 to A1CD    Buzz Buzz, Tiny Li'l Ghost
    Entry 07    A1CE to A21B    *** Unused? ***

The remaining entries (08-1F) are used by ordinary enemies.

In the event that both Buzz Buzz and the Tiny Li'l Ghost are in the party at the same time, which takes precedence?



The resistances ARE in a different order from how they were in the enemy data. No idea why, maybe an artifact of the order in which elemental attacks were implemented.

Relevant routine:
C2B6EB - Copies enemy data into 7EA21C for in-battle use?

Observation on shield health:
  Newly created shields can withstand three hits. Stacking the same type of shield will increase the number of hits the shield can take by 3, to a maximum of eight hits.

## Data Layout

- 00: Character number for the Chosen Four
     - Enemy data number for everything else
- 01: ???
- 02: Battle sprite
- 03: ???
- 04 - 05: Current battle action
- 06: Action Order State
    - For the in-order action order, valid values are 0-3.
    - For the staggered action order, valid values are 0-1.
- 07: ???
- 08: Current battle action's argument
- 0A: Current target?
- 0B: Article ("The") flag
- 0C: Living flag
    - 00 if incapacitated (Unconscious or Diamondized)
    - 01 if alive
- 0D: ???
- 0E: Ally/Opponent flag
    - 00 for allies (Chosen Four, party NPCs, Buzz Buzz)
    - 01 for opponents (Tiny Li'l Ghost, ordinary enemies)
- 0F: Character number for NPCs
    - 00 for the Chosen Four and ordinary enemies
    - 05 for Pokey
    - 06 for Picky
    - 07 for King
    - 08 for Tony
    - 09 for Bubble Monkey
    - 0A for Brick Road
    - 0B for Flying Man (1)
    - 0C for Flying Man (2)
    - 0D for Flying Man (3)
    - 0E for Flying Man (4)
    - 0F for Flying Man (5)
    - 10 for Teddy Bear
    - 11 for Super Plush Bear
    - D5 for Tiny Li'l Ghost
    - D7 for Buzz Buzz
- 10: ???
    - 00 for Ness
    - 01 for Paula
    - 02 for Jeff
    - 03 for Poo
    - 00 for Party NPC #1
    - 01 for Party NPC #2
    Investigate this mysterious value!
- 11 - 12: HP (current)
- 13 - 14: HP (rolling target)
- 15 - 16: HP (max)
- 17 - 18: PP (current)
- 19 - 1A: PP (rolling target)
- 1B - 1C: PP (max)
- 1D: Status ailments
    - 01 = Unconscious
    - 02 = Diamondized
    - 03 = Paralyzed
    - 04 = Nauseous
    - 05 = Poisoned
    - 06 = Sunstroke
    - 07 = Sniffling
- 1E: Status ailments
    - 01 = Mushroomized
    - 02 = Possessed
- 1F: Status ailments
    - 01 = Asleep
    - 02 = Crying
    - 03 = Immobilized
    - 04 = Solidified
- 20: Status ailments
    - 01 = Feeling strange
- 21: Status ailments
    - "Can't concentrate" countdown
- 22: Status ailments
    - 01 = Homesick
- 23: Current type of shield
    - 00 = None
    - 01 = PSI reflecting      (PSI Shield Beta)
    - 02 = PSI cancelling      (PSI Shield Alpha)
    - 03 = Physical reflecting (Shield Beta)
    - 04 = Physical blocking   (Shield Alpha)
- 24: Guard state
    - 00 = Normal
    - 01 = On guard
- 25: Shield health
- 26 - 27: Offense
- 28 - 29: Defense
- 2A - 2B: Speed
- 2C - 2D: Guts
- 2E - 2F: Luck
- 30: Vitality (For enemies, 0x00)
- 31: IQ (For enemies, Enemy Data Unknown H)
- 32: Offense
- 33: Defense
- 34: Speed
- 35: Guts
- 36: Luck
- 37: Effectiveness of Paralysis    $C2B639 used on value before storing
- 38: Effectiveness of PSI Freeze   $C2B608 "
- 39: Effectiveness of PSI Flash    $C2B639 "
- 3A: Effectiveness of PSI Fire     $C2B608 "
- 3B: Effectiveness of Brainshock   $C2B639 "
- 3C: Effectiveness of Hypnosis     $C2B639 "

         C2/B608: 00 = 99% (255/256)
                  01 = 70% (179/256)
                  02 = 40% (102/256)
                  03 =  5% ( 13/256)

         C2/B639: 00 = 99% (255/256)
                  01 = 50% (128/256)
                  02 = 10% ( 26/256)
                  03 =  0% (  0/256)

         The "Effectiveness of Brainshock" is defined as:
            0x03 - (Effectiveness of Hypnosis/Brainshock in enemy data)
         The "Hypnosis/Brainshock" value in enemy data thus results in these effectiveness values:

  | ID |Hypnosis  | Brainshock |
  |----|----------|------------|
  | 00 |   99 %   |     0 %    |
  | 01 |   50 %   |    10 %    |
  | 02 |   10 %   |    50 %    |
  | 03 |    0 %   |    99 %    |

- 3D - 3E: Money
- 3F - 42: Experience
- 43: Battle sprite (out of a set of loaded-for-current-battle enemy sprites?)
- 44: Battle sprite X position
- 45: Battle sprite Y position
- 46 - 47: Initiative
- 48 - 4B: ???
- 4C: Enemy number (redundant?)
- 4D: ???

## Starting statuses

Starting status (enemy-root + 0x59)

    0x01 --> B8E5 --> X = 0x0002, A = $02, JSR 9CDC --> B92C           PSI Shield Alpha
    0x02 --> B8EF --> X = 0x0001, A = $02, JSR 9CDC --> B92C           PSI Shield Beta
    0x03 --> B8F9 --> X = 0x0004, A = $02, JSR 9CDC --> B92C           Shield Alpha
    0x04 --> B903 --> X = 0x0003, A = $02, JSR 9CDC --> B92C           Shield Beta
    0x05 --> B90D --> SEP 20, A = 0x01, X = $02, STA $001F,X --> B92C  Asleep
    0x06 --> B918 --> SEP 20, A = 0x04, X = $02, STA $0021,X --> B92C  Can't concentrate
    0x07 --> B923 --> SEP 20, A = 0x01, X = $02, STA $0020,X --> B92C  Feeling strange
    else --> B92C

## Weakness usage

"37: Weakness to Paralysis" is used in:
   - C2/8770 (Battle action: Spy)
   - C2/89CE (Battle action: Try to inflict diamondization)
   - C2/8A92 (Battle action: Try to inflict paralysis)
   - C2/8D5A (Battle action: Try to make the target unable to concentrate)
   - C2/9FFE (Battle action: Paralysis A)
   - C2/A902 (Battle action: Try to inflict solidification)
   - C2/A953 (Battle action: Try to inflict poison)

"38: Weakness to PSI Freeze" is used in:
   - C2/8770 (Battle action: Spy)
   - C2/8B6D (Battle action: Try to inflict sniffling)
   - C2/95CF (PSI Freeze (general))

"39: Weakness to PSI Flash" is used in:
   - C2/8770 (Battle action: Spy)
   - C2/8C69 (Battle action: Try to inflict crying)
   - C2/98A1 (Related to weakness to PSI Flash)

"3A: Weakness to PSI Fire" is used in:
   - C2/8770 (Battle action: Spy)
   - C2/900B (Battle action: 350 +- 25% fire damage)
   - C2/957A (PSI Fire (general))
   - C2/A99C (Battle action: 800 +- 25% fire damage)

"3B: Weakness to Brainshock" is used in:
   - C2/8770 (Battle action: Spy)
   - C2/A056 (Battle action: Brainshock A)

"3C: Weakness to Hypnosis" is used in:
   - C2/8770 (Battle action: Spy)
   - C2/9F06 (Battle action: Hypnosis A)
