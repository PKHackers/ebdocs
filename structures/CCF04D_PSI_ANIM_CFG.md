# $CCF04D - PSI Animation Configuration Table

Original Author: Michael Cayer

This table controls various aspects of the PSI animations, such
as frame duration, palette duration, number of frames, and so on.

Durations are measured in frames (or 60ths of a second).

|GFX ptr|Frame duration|Palette duration|??        |??        |Frame count|Target     |Enemy colour delay|Enemy colour duration|Enemy colour|Animation       |
|-------|--------------|----------------|----------|----------|-----------|-----------|------------------|---------------------|------------|----------------|
|`$DB27`|05            |03              |01        |02        |2F         |02         |00                |00                   |`0000`      |Counter-PSI Unit|
|`$DB27`|04            |03              |01        |03        |1D         |00         |32                |50                   |`00D2`      |Brainshock α    |
|`$DB27`|04            |03              |01        |03        |2F         |02         |5A                |A0                   |`00D0`      |Brainshock Ω    |
|`$DB27`|03            |03              |01        |03        |21         |00         |00                |00                   |`0000`      |HP-Sucker       |
|`$DB27`|05            |03              |01        |03        |1C         |00         |28                |46                   |`2800`      |Defense Down α  |
|`$DB27`|05            |03              |01        |03        |21         |02         |28                |46                   |`2800`      |Defense Down Ω  |
|`$B613`|05            |02              |01        |03        |0D         |01         |28                |46                   |`001F`      |PSI Fire α      |
|`$B613`|05            |02              |01        |03        |10         |01         |32                |50                   |`001F`      |PSI Fire ϐ      |
|`$B613`|05            |03              |01        |03        |11         |01         |32                |50                   |`001F`      |PSI Fire γ      |
|`$B613`|05            |03              |01        |03        |1B         |01         |50                |82                   |`001F`      |PSI Fire Ω      |
|`$AC25`|05            |03              |01        |02        |06         |02         |0A                |28                   |`77BD`      |PSI Flash α     |
|`$AC25`|05            |03              |01        |02        |0A         |02         |14                |32                   |`77BD`      |PSI Flash ϐ     |
|`$AC25`|04            |02              |01        |03        |13         |02         |1E                |3C                   |`77BD`      |PSI Flash γ     |
|`$B613`|04            |02              |01        |03        |21         |02         |4C                |7D                   |`77BD`      |PSI Flash Ω     |
|`$AC25`|04            |03              |01        |03        |17         |00         |32                |50                   |`7C00`      |PSI Freeze α    |
|`$AC25`|04            |03              |01        |03        |16         |00         |32                |50                   |`7C00`      |PSI Freeze ϐ    |
|`$AC25`|04            |03              |01        |03        |17         |00         |32                |50                   |`7C00`      |PSI Freeze γ    |
|`$AC25`|04            |03              |01        |03        |23         |00         |3C                |87                   |`7C00`      |PSI Freeze Ω    |
|`$E31D`|03            |02              |01        |03        |1F         |02         |3F                |5C                   |`39DA`      |PSI Special α   |
|`$E31D`|03            |02              |01        |03        |30         |02         |72                |8F                   |`39DA`      |PSI Special ϐ   |
|`$E31D`|03            |02              |01        |03        |36         |02         |84                |A1                   |`39DA`      |PSI Special γ   |
|`$E31D`|04            |02              |01        |03        |40         |02         |8D                |FF                   |`39DA`      |PSI Special Ω   |
|`$DB27`|05            |03              |01        |03        |1A         |00         |3C                |5A                   |`1140`      |Paralysis α     |
|`$DB27`|05            |03              |01        |03        |1A         |02         |3C                |5A                   |`1140`      |Paralysis Ω     |
|`$DB27`|03            |03              |01        |03        |22         |00         |2D                |4B                   |`4A52`      |PSI Magnet α    |
|`$DB27`|03            |03              |01        |03        |1A         |02         |2D                |4B                   |`4A52`      |PSI Magnet Ω    |
|`$DB27`|07            |04              |01        |02        |11         |02         |00                |00                   |`0000`      |Shield Killer   |
|`$AC25`|05            |03              |01        |03        |0B         |00         |00                |00                   |`0000`      |Hypnosis α      |
|`$AC25`|05            |03              |01        |03        |11         |02         |00                |00                   |`0000`      |Hypnosis Ω      |
|`$DB27`|03            |03              |01        |03        |21         |02         |00                |00                   |`0000`      |Hungry HP-Sucker|
|`$AC25`|04            |02              |01        |03        |21         |02         |50                |83                   |`7E52`      |PSI Starstorm α |
|`$AC25`|04            |02              |01        |03        |2E         |02         |79                |B7                   |`7E52`      |PSI Starstorm Ω |
|`$AC25`|04            |03              |01        |03        |0F         |03         |19                |36                   |`2FFF`      |PSI Thunder α, ϐ|
|`$AC25`|04            |03              |01        |03        |13         |03         |31                |4B                   |`2FFF`      |PSI Thunder γ, Ω|

## Target
- 00 = One Enemy
- 01 = Row of Enemies
- 02 = All Enemies
- 03 = Thunder Targetting