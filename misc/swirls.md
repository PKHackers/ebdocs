# SWIRLS

## EDF41
[AA BB CC DD]

AA = Speed for effect. Corresponds roughly to seconds timewise (ie. 05 indicates about five seconds between the effect's beginning and end).
BB = EDE45 two-byte pointer table entry the effect starts on.
CC = Number of entries in EDE45 to pass through before ending.
DD = 00

## EDE45

Standard two-byte pointer table; base address is E0200.

## E6B14

See:
 [CE6914_SWIRL_DATA.md](../structures/CE6914_SWIRL_DATA.md)

Not everything is known yet, unfortunately.