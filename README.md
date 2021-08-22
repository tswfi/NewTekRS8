usb chip: ftdi ft245am

https://www.ftdichip.com/Support/Documents/DataSheets/ICs/ft245r09.pdf

everything always starts with ~ and ends with \r

# serial data format

'~1' is bottom row buttons
'~2' is top row buttons
'~3' is extra buttons
'~4' is fade
'~5', '~6', '~7' are A, B, C encoders

# leds
switching the leds with sending data back to the device:

light up top row 2
```
echo -ne "~2BF\r" > /dev/ttyUSB0
```

light up bottom row 3
```
echo -ne "~1DF\r" > /dev/ttyUSB0
```

# all lights off bottom row
```
echo -ne "~1FF\r" > /dev/ttyUSB0
```

# also this works, but is probably invalid:
```
echo -ne "~1AA\r" > /dev/ttyUSB0
echo -ne "~155\r" > /dev/ttyUSB0
```

pressing multiple buttons will give the combined value.

for example pressing 1 then 2 then 3 then releasing 1 then 2 and 3 will give

```
~27F
~23F
~21F
~29F
~2DF
~2FF
```

# button values

## top row

1: ~27F  0111 1111
2: ~2BF  1011 1111
3: ~2DF  1101 1111
4: ~2EF  1110 1111
5: ~2F7  1111 0111
6: ~2FB  1111 1011
7: ~2FD  1111 1101
8: ~2FE  1111 1110
all released: ~2FF

## bottom row:

1: ~17F
2: ~1BF
3: ~1DF
4: ~1EF
5: ~1F7
6: ~1FB
7: ~1FD
8: ~1FE
all released: ~1FF

## other buttons

auto: ~3F6 release ~3FE
take: ~3F8 release ~3FE
fade dsk: ~37E
take dsk: ~3BE
ddr: ~3DE
alt: ~3EE
all released: ~3FE

## pots

pot a: ~500 - ~5FF
pot b: ~600 - ~6FF
pot c: ~700 - ~7FF

## fader

fade: ~400 - 4ff
