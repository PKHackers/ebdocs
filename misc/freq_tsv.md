# FREQUENCY / TSV BLOCKS

Frequency/TSV blocks consist of:
- Bytes that represent note frequencies
- TSVs (Tempo / Style / Volume, a two-byte modifier of stuff)
- Unknowns

They are pointed to from $C4F947. Pointers in this table use an odd format: <high> <low> <middle>. For example, $C58000 would be represented as C5 00 80.

-------------------------------------------------------------------------

TSVs look like this:
XX YZ

    XX - Tempo: Cycles per note?
    Y - Style: ???
    Z - Volume.



## Entry Organization

The first two bytes are equal to distance from the third byte to the end, minus one.

Following this is a header of sorts; it appears to go on until an FF is reached, which marks the end of the header.

The data portion that follows is frequency bytes, TSVs and unknowns. Research is continuing.


## Notes

* How timing is handled is unknown.
* How the difference between freqs and TSVs is determined is unknown, though the 0x80 bit is suspected (one for freq, zero for TSV).