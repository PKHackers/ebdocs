# CCF04D - PSI Animation Configuration Table

Original Author: Michael Cayer

This table controls various aspects of the PSI animations, such
as frame duration, palette duration, number of frames, and so on.

Durations are measured in frames (or 60ths of a second).

|GFX ptr|Frame duration|Palette duration|??        |??        |Frame count|Target     |Enemy colour delay|Enemy colour duration|Enemy colour|Animation       |
|-------|--------------|----------------|----------|----------|-----------|-----------|------------------|---------------------|------------|----------------|
|`27 DB`|05            |03              |01        |02        |2F         |02         |00                |00                   |`00 00`     |Counter-PSI Unit|
|`27 DB`|04            |03              |01        |03        |1D         |00         |32                |50                   |`D2 00`     |Brainshock α    |
|`27 DB`|04            |03              |01        |03        |2F         |02         |5A                |A0                   |`D0 00`     |Brainshock Ω    |
|`27 DB`|03            |03              |01        |03        |21         |00         |00                |00                   |`00 00`     |HP-Sucker       |
|`27 DB`|05            |03              |01        |03        |1C         |00         |28                |46                   |`00 28`     |Defense Down α  |
|`27 DB`|05            |03              |01        |03        |21         |02         |28                |46                   |`00 28`     |Defense Down Ω  |
|`13 B6`|05            |02              |01        |03        |0D         |01         |28                |46                   |`1F 00`     |PSI Fire α      |
|`13 B6`|05            |02              |01        |03        |10         |01         |32                |50                   |`1F 00`     |PSI Fire ϐ      |
|`13 B6`|05            |03              |01        |03        |11         |01         |32                |50                   |`1F 00`     |PSI Fire γ      |
|`13 B6`|05            |03              |01        |03        |1B         |01         |50                |82                   |`1F 00`     |PSI Fire Ω      |
|`25 AC`|05            |03              |01        |02        |06         |02         |0A                |28                   |`BD 77`     |PSI Flash α     |
|`25 AC`|05            |03              |01        |02        |0A         |02         |14                |32                   |`BD 77`     |PSI Flash ϐ     |
|`25 AC`|04            |02              |01        |03        |13         |02         |1E                |3C                   |`BD 77`     |PSI Flash γ     |
|`13 B6`|04            |02              |01        |03        |21         |02         |4C                |7D                   |`BD 77`     |PSI Flash Ω     |
|`25 AC`|04            |03              |01        |03        |17         |00         |32                |50                   |`00 7C`     |PSI Freeze α    |
|`25 AC`|04            |03              |01        |03        |16         |00         |32                |50                   |`00 7C`     |PSI Freeze ϐ    |
|`25 AC`|04            |03              |01        |03        |17         |00         |32                |50                   |`00 7C`     |PSI Freeze γ    |
|`25 AC`|04            |03              |01        |03        |23         |00         |3C                |87                   |`00 7C`     |PSI Freeze Ω    |
|`1D E3`|03            |02              |01        |03        |1F         |02         |3F                |5C                   |`DA 39`     |PSI Special α   |
|`1D E3`|03            |02              |01        |03        |30         |02         |72                |8F                   |`DA 39`     |PSI Special ϐ   |
|`1D E3`|03            |02              |01        |03        |36         |02         |84                |A1                   |`DA 39`     |PSI Special γ   |
|`1D E3`|04            |02              |01        |03        |40         |02         |8D                |FF                   |`DA 39`     |PSI Special Ω   |
|`27 DB`|05            |03              |01        |03        |1A         |00         |3C                |5A                   |`40 11`     |Paralysis α     |
|`27 DB`|05            |03              |01        |03        |1A         |02         |3C                |5A                   |`40 11`     |Paralysis Ω     |
|`27 DB`|03            |03              |01        |03        |22         |00         |2D                |4B                   |`52 4A`     |PSI Magnet α    |
|`27 DB`|03            |03              |01        |03        |1A         |02         |2D                |4B                   |`52 4A`     |PSI Magnet Ω    |
|`27 DB`|07            |04              |01        |02        |11         |02         |00                |00                   |`00 00`     |Shield Killer   |
|`25 AC`|05            |03              |01        |03        |0B         |00         |00                |00                   |`00 00`     |Hypnosis α      |
|`25 AC`|05            |03              |01        |03        |11         |02         |00                |00                   |`00 00`     |Hypnosis Ω      |
|`27 DB`|03            |03              |01        |03        |21         |02         |00                |00                   |`00 00`     |Hungry HP-Sucker|
|`25 AC`|04            |02              |01        |03        |21         |02         |50                |83                   |`52 7E`     |PSI Starstorm α |
|`25 AC`|04            |02              |01        |03        |2E         |02         |79                |B7                   |`52 7E`     |PSI Starstorm Ω |
|`25 AC`|04            |03              |01        |03        |0F         |03         |19                |36                   |`FF 2F`     |PSI Thunder α, ϐ|
|`25 AC`|04            |03              |01        |03        |13         |03         |31                |4B                   |`FF 2F`     |PSI Thunder γ, Ω|

## Target
- 00 = One Enemy
- 01 = Row of Enemies
- 02 = All Enemies
- 03 = Thunder Targetting