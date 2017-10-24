# $D55000 - Item Configuration Table
Original Author: AnyoneEB

|Offset|Size|Meaning                                                       |
|------|----|--------------------------------------------------------------|
|00-18 |24  |Name of item in plain EB text.                                |
|19    |1   |Type of item. See [Item Types](#Item Types) below.            |
|1A    |2   |Cost of item. If zero, item may not be sold or dropped.       |
|1C    |1   |Misc [Flags](#Flags).                                         |
|1D    |2   |Effect.                                                       |
|1F    |1   |"Strength" different depending on type. See below.            |
|20    |1   |"Extra Power Increase" different depending on type. See below.|
|21    |1   |"Extra Power" different depending on type. See below.         |
|22    |1   |"Special" different depending on type. See below.             |
|23    |3   |Text address called when Goods-->Help menu item is selected.  |

## Flags

|Bit|Description                                   |
|---|----------------------------------------------|
|01 |Ness can use                                  |
|02 |Paula can use                                 |
|04 |Jeff can use                                  |
|08 |Poo can use                                   |
|10 |Item is source or target of item tranformation|
|20 |"Give" command disabled                       |
|40 |Unknown                                       |
|80 |Item disappears after use                     |

Known meanings of bytes 20-23 for various types. If the type or byte
is not mentioned in this list it is either unknown or unused.

## Item Types

### Type 4
STR = Game Sprite Character Table entry(?)

### Type 8 (broken item)
EPI: IQ required to fix item
EPW: Index of fixed item

### Food Items (types 32, 36, 40, and 44)
EPI: Regular HP recover
EPW: Poo HP recover

### Armor Items

#### Type 20
STR: Defence
EPW: Speed
SPE: Protection

#### Type 24
STR: Defence
EPW: Luck
SPE: Protection

#### Type 28
STR: Defence
EPW: Luck
SPE: Protection

### Weapon Items (types 16 and 17)
STR: Offense
EPI: Poo Offense (unsure for type 17)
EPW: Guts (unsure for type 17)
SPE: Miss Rate