usb chip: ftdi ft245am

https://www.ftdichip.com/Support/Documents/DataSheets/ICs/ft245r09.pdf

everything always starts with ~ and ends with \r

# serial data format

```
'~1' is bottom row buttons
'~2' is top row buttons
'~3' is extra buttons
'~4' is fade
'~5', '~6', '~7' are A, B, C encoders
```

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

| button | value | binary |
| :-------:| :-----: | ------: |
| 1 | ~27F | 0111 1111 |
| 2 | ~2BF | 1011 1111 |
| 3 | ~2DF | 1101 1111 |
| 4 | ~2EF | 1110 1111 |
| 5 | ~2F7 | 1111 0111 |
| 6 | ~2FB | 1111 1011 |
| 7 | ~2FD | 1111 1101 |
| 8 | ~2FE | 1111 1110 |

all released: ~2FF

## bottom row:

| button | value | binary |
| :-------:| :-----: | ------: |
| 1 | ~17F | 0111 1111 |
| 2 | ~1BF | 1011 1111 |
| 3 | ~1DF | 1101 1111 |
| 4 | ~1EF | 1110 1111 |
| 5 | ~1F7 | 1111 0111 |
| 6 | ~1FB | 1111 1011 |
| 7 | ~1FD | 1111 1101 |
| 8 | ~1FE | 1111 1110 |

all released: ~1FF

## other buttons

| button | value | binary |
| :-------:| :-----: | ------: |
| fade dsk | ~37E | 0111 1110 |
| take dsk | ~3BE | 1011 1110 |
| ddr | ~3DE | 1101 1110 |
| alt | ~3EE | 1110 1110 |
| auto | ~3F6 | 1111 0110 |
| take | ~3F8 | 1111 1000 |

all released: ~3FE

## pots

pot a: ~500 - ~5FF
pot b: ~600 - ~6FF
pot c: ~700 - ~7FF

## fader

fade: ~400 - 4ff

### fader leds

TODO!

looks like the last two bits of `other buttons` is the top and bottom led for the slider

# disco

to see all the buttons flashing (auto and take does not have leds)

```bash
while true
do
  echo -ne "~1AA\r~255\r~35E\r" > /dev/ttyUSB0
  sleep 0.3
  echo -ne "~2AA\r~155\r~3AB\r" > /dev/ttyUSB0
  sleep 0.3
done
```