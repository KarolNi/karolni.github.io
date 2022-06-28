# Notes from ECC memory testing

I was infected by ECC memory distortion from reading [r/zfs](https://www.reddit.com/r/zfs/).

I tested some low cost hardware for using as NAS for backup:

- HP ProLiant ML310e Gen8 with Intel Xeon E3-1220 v2 and UDIMM (PC3-12800E)
- Supermicro
- Dell Wyse Z90D7 thin client with AMD G-T56N APU
(Used, delivered with UDIMM memory as per photos in internet flea market).


# Why?

## actual error correction

## error reporting

# Memory types

## plain consumer grade memory (DIMM) (8 or 16 memory chips per stick)

## unbuffered ECC (UDIMM)

9 or 18 memory chips per stick.
Capacity is similar to plain memory stick (e.g. 1-8GB per stick for DDR3 generation)
I have an interesting observation with this kind of memory. As I was fooled (by myself) that Z90D7 is compatible with ECC memory - it worked correctly with ECC sticks even with memory controller incapable of using ECC. This got me thinking and testing. It turns out that UDIMM (unbuffered ECC memory) is compatible with any consumer platform, ECC part is just ignored (be aware of my limited test sample with just DDR3 generation).

## Registered ECC (RDIMM)

This is the kind you are looking for when obsessed with ZFS.
It has a multiple of 9 memory chips per stick and one additional big buffer chip in the centre of the stick.
It is supported only on top tier hardware for servers and workstations e.g. Intel Xeon E5. When you are interested in error reporting and verifiability of error correction it is your best bet.
