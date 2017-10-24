# $D59589 - Enemy Configuration Table
Original Author: AnyoneEB

|Offset|Size| Meaning                                                          |
|------|----|------------------------------------------------------------------|
|00    |1   | "The" flag (1 = put "the" before enemy name)                     |
|01    |24  | Name of enemy in plain EB text                                   |
|1A    |1   | Gender (0x01 = Male; 0x02 = Female; 0x03 = Neutral)              |
|1B    |1   | Type (0x00 = Normal; 0x01 = Insect; 0x02 = Metal)                |
|1C    |2   | Battle sprite                                                    |
|1E    |2   | Out of battle sprite                                             |
|20    |1   | Run flag ("Fear higher leveled PCs") (0x00 = false; 0x01 = true) |
|21    |2   | HP                                                               |
|23    |2   | PP                                                               |
|25    |3   | Experience                                                       |
|29    |2   | Money                                                            |
|2B    |2   | Movement                                                         |
|2D    |3   | Start text pointer                                               |
|31    |3   | Death text pointer                                               |
|35    |1   | Palette of battle sprite                                         |
|36    |1   | Level                                                            |
|37    |1   | Music                                                            |
|38    |1   | Offense                                                          |
|39    |1   | *UNKNOWN* (K)                                                    |
|3A    |1   | Defense                                                          |
|3B    |1   | *UNKNOWN* (L)                                                    |
|3C    |1   | Speed                                                            |
|3D    |1   | Guts                                                             |
|3E    |1   | IQ                                                               |
|3F    |1   | Weakness to PSI Fire (See below for weaknesses)                  |
|40    |1   | Weakness to PSI Freeze                                           |
|41    |1   | Weakness to PSI Flash                                            |
|42    |1   | Weakness to Paralysis                                            |
|43    |1   | Weakness to Hypnosis/Brainshock                                  |
|44    |1   | Miss rate (/* TODO */)                                           |
|45    |1   | Order of action use (See below)                                  |
|46    |2   | Action #1                                                        |
|48    |2   | Action #2                                                        |
|4A    |2   | Action #3                                                        |
|4C    |2   | Action #4                                                        |
|4E    |2   | Final action (performed upon death)                              |
|50    |1   | Action #1 argument                                               |
|51    |1   | Action #2 argument                                               |
|52    |1   | Action #3 argument                                               |
|53    |1   | Action #4 argument                                               |
|54    |1   | Final action argument                                            |
|55    |1   | *UNKNOWN* (H)                                                    |
|56    |1   | Boss flag                                                        |
|57    |1   | Item drop frequency (0x00-0x07; P(item drop) = 2^(freq byte)/128)|
|58    |1   | Item dropped                                                     |
|59    |1   | Status (See below)                                               |
|5A    |1   | Death sound (0x00 = Normal; 0x01 = Boss)                         |
|5B    |1   | Row (0x00 = Front; 0x01 = Back)                                  |
|5C    |1   | Max call                                                         |
|5D    |1   | Percent chance of Poo's mirror to succeed                        |


## WEAKNESSES

### PSI Fire and PSI Freeze (damage PSI)

|Value|Effectiveness|
|-----|-------------|
|00   |200%         |
|01   |150%         |
|02   |100%         |
|03   |50%          |
|04   |1%           |

### PSI Flash, Paralysis, Hypnosis/Brainshock (status PSI)

|Value|Effectiveness|
|-----|-------------|
|00   |100%         |
|01   |75%          |
|02   |50%          |
|03   |25%          |
|04   |1%           |


## ORDER

|Value|Action order pattern                |
|-----|------------------------------------|
|00   |Random                              |
|01   |Weighted towards action 1           |
|02   |In Order                            |
|03   |Alternates between 1 or 2 and 3 or 4|


## STATUS

|Value|Starting status                |
|-----|-------------------------------|
|00   |Normal                         |
|01   |PSI Shield Alpha (Blocks PSI)  |
|02   |PSI Shield Beta (Reflects PSI) |
|03   |Shield Alpha (Blocks physical) |
|04   |Shield Beta (Reflects physical)|
|05   |Asleep                         |
|06   |Can't concentrate              |
|07   |Feeling strange                |