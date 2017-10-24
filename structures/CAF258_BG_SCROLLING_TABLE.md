# $CAF258 - BG Scrolling Table

Battle background movement of the "full screen" style (as opposed to mode-7-style shifting, which is controlled by [$CAF708](CAF708_BG_DISTORTION_TABLE.md).) Ten-byte entries.

[AAAA BBBB CCCC DDDD EEEE]


AA - Duration (measured in 1/60ths of a second?)
BB - Horizontal movement.
CC - Vertical movement.
DD - Horizontal acceleration.
EE - Vertical acceleration.


Horizontal:
- Positive left
- Negative right

Vertical:
- Positive up
- Negative down