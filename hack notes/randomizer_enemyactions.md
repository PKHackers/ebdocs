These battle actions can be randomly assigned safely.

If there is a byte after the action number, it indicates an argument that the action needs to take.



    04    - Bash
    05    - Shoot

    0A 01 - PSI Rockin Alpha
    0B 02 - PSI Rockin Beta
    0C 03 - PSI Rockin Gamma
    0D 04 - PSI Rockin Omega
    0E 05 - PSI Fire Alpha
    0F 06 - PSI Fire Beta
    10 07 - PSI Fire Gamma
    11 08 - PSI Fire Omega
    12 09 - PSI Freeze Alpha
    13 0A - PSI Freeze Beta
    14 0B - PSI Freeze Gamma
    15 0C - PSI Freeze Omega
    16 0D - PSI Thunder Alpha
    17 0E - PSI Thunder Beta
    18 0F - PSI Thunder Gamma
    19 10 - PSI Thunder Omega
    1A 11 - PSI Flash Alpha
    1B 12 - PSI Flash Beta
    1C 13 - PSI Flash Gamma
    1D 14 - PSI Flash Omega
    1E 15 - PSI Starstorm Alpha
    1F 16 - PSI Starstorm Omega
    20 17 - Lifeup Alpha
    21 18 - Lifeup Beta
    22 19 - Lifeup Gamma
    23 1A - Lifeup Omega
    24 1B - Healing Alpha
    25 1C - Healing Beta
    26 1D - Healing Gamma
    27 1E - Healing Omega
    28 1F - Shield Alpha     \
    29 21 - Shield Beta       \
    2A 20 - Shield Sigma       \
    2B 22 - Shield Omega        \ Action arguments aren't in numerical order here
    2C 23 - PSI Shield Alpha    /
    2D 25 - PSI Shield Beta    /
    2E 24 - PSI Shield Sigma  /
    2F 26 - PSI Shield Omega /
    30 27 - Offense up Alpha
    31 28 - Offense up Omega
    32 29 - Defense down Alpha
    33 2A - Defense down Omega
    34 2B - Hypnosis Alpha
    35 2C - Hypnosis Omega
    36 2D - PSI Magnet Alpha
    37 2E - PSI Magnet Omega
    38 2F - Paralysis Alpha
    39 30 - Paralysis Omega
    3A 31 - Brainshock Alpha
    3B 32 - Brainshock Omega

    40    - [USER] exploded into bits!
    41    - [USER] burst into flames!
    42    - [USER] stole a [ITEM] in the confusion of the battle!
    43    - [USER] froze you in time!
    44    - [USER] glared with [GENDER] eerie eyes!
    45    - [USER] generated a mysterious electric field!
    46    - [USER] stumbled, but fired a strange beam.
    47    - [USER] burped and blew his nauseating breath at you!
    48    - [USER] stung with its poison stinger!
    49    - [USER] gave the kiss of death!
    4A    - [USER] exhaled [GENDER] arctic-cold breath!
    4B    - [USER] scattered some spores!
    4C    - [USER] tried to possess you in a frightening manner!
    4D    - [USER] sprinkled around some wonderful-smelling powder.
    4E    - [USER] scattered some mold spores!
    4F    - [USER] employed a binding attack!
    50    - [USER] spit out a sticky mucus!
    51    - [USER] spewed "Fly Honey" out of his mouth!
    52    - [USER] shot spider silk out of its body!
    53    - [USER] said something really scary!
    54    - [USER] did something very mysterious!
    55    - [USER] disrupted your senses!
    56    - [USER] is sizing up the situation!
    57    - [USER] exhaled a blast of stinky breath!
    58    - [USER] summoned a storm!
    59    - [USER] spilled some scalding hot espresso!
    5A    - [USER] played a haunting melody!
    5B    - [USER] dispensed an extinguishing blast!
    5C    - [USER] used a Crashing Boom Bang attack!
    5D    - [USER] shot out a spray of fire!
    5E    - [USER] breathed fire!
    5F    - [USER] made something spin around!
    60    - [USER] lost his temper!
    61    - [USER] said something nasty!
    62    - [USER] used a vacuum attack!
    63    - [USER] replenished a fuel supply!
    64    - [USER] took a bite using its poisonous fangs!
    65    - [USER] fired a missile, making itself dizzy.
    66    - [USER] started a continuous attack!
    67    - [USER] is on guard.
    68    - [USER] spewed out a flaming fireball!
    69    - [USER] rushed in and inter-twined with you!
    6A    - [USER] attacked with a crushing chop!
    6B    - [USER] grappled and used his submission hold!
    6C    - [USER] revved and accelerated!
    6D    - [USER] brandished a knife!
    6E    - [USER] tore into you!
    6F    - [USER] used a biting attack!
    70    - [USER] clawed with [GENDER] sharp nails!
    71    - [USER] swung [GENDER] tail very hard!
    72    - [USER] growled and lunged forward!
    73    - [USER] wielded a shopping bag!
    74    - [USER] swung a club!
    75    - [USER] generated a tornado!
    76    - [USER] sprayed a gigantic blast of water!
    77    - [USER] flashed a menacing smile!
    78    - [USER] started laughing hysterically!
    79    - [USER] edged closer!
    7A    - [USER] whispered "3..."
    7B    - [USER] murmured "2..."
    7C    - [USER] muttered "1..."
    7D    - [USER] fell down!
    7E    - [USER] is being absentminded.
    7F    - [USER] generated a burst of steam!
    80    - [USER] is wobbly.
    81    - [USER] is reeling.
    82    - [USER] has a big grin on [GENDER] face.
    83    - [USER] is taking deep breaths for the next assault.
    84    - [USER] sends a greeting!
    85    - [USER] is making a loud, piercing howl.
    86    - [USER] is saying "tick-tock."

    95 7F - Wet towel       \  These four actions behave identically to the four
    96 80 - Refreshing herb  \ levels of PSI Healing. The "Horn of life" action
    97 81 - Secret herb      / can also take 0xFC as an argument to become the
    98 82 - Horn of life    /  same-behaviour "Cup of Lifenoodles" action.

    9F 83 - Item: Counter-PSI unit
    A0 84 - Item: Shield killer
    A1 87 - Item: HP-sucker
    A2 8E - Item: Mummy wrap
    A3 90 - Item: Bottle rocket
    A4 91 - Item: Big bottle rocket
    A5 92 - Item: Multi bottle rocket
    A6 9B - Item: Handbag strap
    A7 93 - Item: Bomb
    A8 94 - Item: Super bomb
    A9 8A - Item: Slime generator
    AA 8B - Item: Yogurt dispenser
    AB 8D - Item: Snake bag
    AC 98 - Item: Pair of dirty socks
    AD 99 - Item: Stag beetle
    AE 9A - Item: Toothbrush
    AF 9C - Item: Pharaoh's curse
    B0 88 - Item: Hungry HP-sucker
    B1 A0 - Item: Bag of Dragonite
    B2 95 - Item: Insecticide spray
    B3 89 - Item: Xterminator spray
    B4 96 - Item: Rust promoter
    B5 97 - Item: Rust promoter DX
    B6 9F - Item: Sudden guts pill
    B7 A1 - Item: Defense spray
    B8 9D - Item: Defense shower

    BA 8C - Item: Ruler
    BB 8F - Item: Protractor

    C9    - [USER] emitted a glorious light!
    CA    - [USER] used an electrical shock attack!
    CB    - [USER] scattered its pollen around!
    CC    - [USER] reached out with its icy hand.
    CD    - [USER] played a flute with [GENDER] poisonous breath!
    CE    - [USER] spewed exhaust fumes!
    CF    - [USER] started laughing maniacally!
    D0    - [USER] breathed in through [GENDER] flute!
    D1    - [USER] leaped forward and spread its wings!
    D2    - [USER] became friendly and affectionate!  You backed off.
    D3    - [USER] made a loud rumble!
    D4    - [USER] gave you a great big hug.
    D5    - [USER] let loose with a hacking cough.
    D6    - [USER] used [GENDER] misery attack!
    D7    - [USER] utilized a paint attack!
    D8    - [USER] came out swinging!
    D9    - [USER] scratched with its claws!
    DA    - [USER] pecked at your eyes!
    DB    - [USER] rammed and trampled you!
    DC    - [USER] threw a punch!
    DD    - [USER] spit its pumpkin seeds!
    DE    - [USER] fired a beam!
    DF    - [USER] jabbed with a spear!
    E0    - [USER] stomped with [GENDER] huge foot!
    E1    - [USER] swung his hula hoop.
    E2    - [USER] charged forward!
    E3    - [USER] shredded fiercely on a skateboard!
    E4    - [USER] bit you hard!
    E5    - [USER] grumbled about today's youth!
    E6    - [USER] started lecturing you!
    E7    - [USER] scowled sharply!
    E8    - [USER] vented a terrible odor!
    E9    - [USER] shouted in a loud voice!
    EA    - [USER] shrieked a war cry!
    EB    - [USER] knitted its brow!
    EC    - [CLUMSY ROBOT ANTICS]
    ED    - [USER] scattered some spores!
    EE    - [USER] used a biting attack!

    F1    - [USER] shot a beam that causes night-time stuffiness!
    F2    - [USER] coiled around you and attacked!

    F7 C3 - Item: Neutralizer
    F8    - [USER] emitted a pale green light!

    108    - [USER] is barking!
    109    - [USER] is chanting a magic spell!

    10B    - [USER] is scratching his head!
    10C C7 - Item: Snake
    10D C8 - Item: Viper

    111    - [USER] discharged a very stinky gas!
    112    - '.....[NESS]...'  You cannot grasp the true form of [USER]' attack!

    11B D1 - Item: Monkey's love

    12C    - '.....[NESS]...'  You cannot grasp the true form of [USER]' attack!
    12D    - '.....[NESS]...'  You cannot grasp the true form of [USER]' attack!
    12E    - [RANDOM GIYGAS QUOTE]  You cannot grasp the true form of [USER]' attack!
    12F    - [RANDOM GIYGAS QUOTE]  You cannot grasp the true form of [USER]' attack!
    130    - [RANDOM GIYGAS QUOTE]  You cannot grasp the true form of [USER]' attack!
    131    - [THREE RANDOM GIYGAS QUOTES]  You cannot grasp the true form of [USER]' attack!
    132    - [THREE RANDOM GIYGAS QUOTES]  You cannot grasp the true form of [USER]' attack!
    133    - [THREE RANDOM GIYGAS QUOTES]  You cannot grasp the true form of [USER]' attack!
    134    - [RANDOM GIYGAS QUOTE]
    135    - [THREE RANDOM GIYGAS QUOTES]
    136 85 - Item: Bazooka
    137 86 - Item: Heavy bazooka
    138    - .....[NESS]...
    139    - [USER] ate a bologne sandwich!  [USER]'s HP are maxed out!
    13A    - [USER] lost a gear and some bolts!
    13B    - [USER] re-applied a bandage!
    13C    - [USER] cleaned the area!
    13D    - [USER] wanted to go and get a battery!





Don't randomly assign these actions to enemies.
If an enemy already has one of these as an action, leave that action alone.

    0    - Null effect
    1 ## - [USER] used the [ITEM]! But nothing happened. (In-battle)
    2 ## - [USER] used the [ITEM]! But nothing happened. (Out-of-battle)
    3 ## - [USER] tried to use the [ITEM]. But [USER] could not use the item very well.

    6    - Spy
    7    - Pray
    8    - [USER] is on guard.
    9    - N/A

    3C 33 - Teleport Alpha
    3D 34 - Teleport Beta
    3E ## - [USER] called for help!
    3F ## - [USER] sowed some seeds around itself!

    87    - Food effect (recover HP)
    88    - Food effect (recover HP)
    89    - Food effect (recover HP)
    8A    - Food effect (recover HP)
    8B    - Food effect (recover HP)
    8C    - Food effect (recover HP)
    8D    - Food effect (recover HP)
    8E    - Food effect (recover PP)
    8F    - Food effect (recover PP)
    90    - Food effect (recover HP)
    91    - Food effect (increase one randomly-selected stat by 1d4)
    92    - Food effect (recover HP)
    93    - Food effect (recover HP)
    94    - Food effect (recover HP)

    99    - Food effect (cure poisoning)
    9A    - Food effect (increase IQ by 1d4)
    9B    - Food effect (increase Guts by 1d4)
    9C    - Food effect (increase Speed by 1d4)
    9D    - Food effect (increase Vitality by 1d4)
    9E    - Food effect (increase Luck by 1d4)

    B9    - Teleport box

    BC 68 - Item: Pak of bubble gum
    BD 69 - Item: Jar of Fly Honey
    BE A2 - Item: Piggy nose
    BF A3 - Item: For Sale sign
    C0 A4 - Item: Shyness book
    C1 A5 - Item: Picture postcard
    C2    - (If you get this message, it means that something is wrong.)
    C3 A8 - Item: Chick
    C4 A9 - Item: Chicken
    C5 B1 - Item: ATM card
    C6 AE - Item: Zombie paper
    C7 AF - Item: Hawk eye
    C8 B0 - Item: Bicycle

    EF    - Change weapons in battle (Damage halved by defending/shields?)
    F0    - Change armor in battle

    F3    - Clumsy Robot death action
    F4    - Master Barf death action
    F5 ## - Enemy extender
    F6    - Change weapons in battle (Damage unaffected by defending/shields?)

    F9    - Food effect (general use, one target)
    FA    - Food effect (general use, entire party)
    FB    - [AFFLICTION TEXT FOR HOMESICKNESS]
    FC    - [AFFLICTION TEXT FOR PARALYSIS]
    FD    - [AFFLICTION TEXT FOR SLEEP]
    FE    - [AFFLICTION TEXT FOR SOLIDIFICATION]
    FF    - [RECOVERY TEXT FOR SOLIDIFICATION]
    100    - [AFFLICTION TEXT FOR INABILITY TO CONCENTRATE]
    101    - [USER] lost his mind by wolfing down "Fly Honey"!
    102 C4 - Item: Sound Stone
    103 C5 - Item: Exit mouse
    104    - [POKEY ANTICS]
    105    - Pokey played dead!
    106    - Pokey pretended to cry!
    107    - Pokey apologized profusely!

    10A    - N/A (Used by [A3] Tony)

    10E B9 - Item: Hieroglyph copy
    10F CA - Item: Town map
    110    - N/A

    113    - Pokey's first speech
    114    - N/A (Used by [D9] Heavily Armed Pokey)
    115    - N/A (Used by [DB] Giygas)
    116    - Pokey's second speech
    117    - [RUN AWAY]
    118    - Mirror
    119 CC - Item: Suporma
    11A CE - Item: Insignificant item

    11C D3 - Item: Tendakraut
    11D A6 - Item: King banana
    11E B5 - Item: Receiver phone
    11F 9E - Item: Letter from mom
    120 A7 - Item: Letter from Tony
    121 B3 - Item: Letter from kids
    122 53 - All of a sudden, [USER] gave off a rainbow of colors!
    123    - Giygas prayer #1 (Saturn Valley)
    124    - Giygas prayer #2 (Runaway Five)
    125    - Giygas prayer #3 (Polestar Preschool)
    126    - Giygas prayer #4 (Snow Wood)
    127    - Giygas prayer #5 (Dalaam)
    128    - Giygas prayer #6 (Frank Fly)
    129    - Giygas prayer #7 (Ness' house)
    12A    - Giygas prayer #8 (Absorbed by the darkness)
    12B    - Giygas prayer #9 (Player)

.