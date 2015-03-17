title: custom keyboard
date: 2015-02-10 17:13:56
tags:
---

INTRO

Specs

thanks to
https://github.com/showjean/ps2avrU
http://deskthority.net/workshop-f7/how-to-build-your-very-own-keyboard-firmware-t7177.html

progress

Keys to column/row
Using the bootmapper output from ps2avru

    (column,row)

    esc   1     2     3     4     5     6     7     8     9     0     -     =     \     `
    6,1   7,1   7,2   7,3   7,4   6,4   6,5   7,5   7,6   7,7   7,8   6,8   6,6   1,10  6,10 

    tab    q     w     e     r     t     y     u     i     o     p     [     ]     bksp
    1,1    0,1   0,2   0,3   0,4   1,4   1,5   0,5   0,6   0,7   0,8   1,8   1,6   2,10 

    caps     a     s     d     f     g     h     j     k     l     ;     '         ent
    1,2      2,1   2,2   2,3   2,4   3,4   3,5   2,5   2,6   2,7   2,8   3,8       4,10 

    shft      z     x     c     v     b     n     m     ,     .     /     lshft    fn
    1,9       4,1   4,2   4,3   4,4   5,4   5,5   4,5   4,6   4,7   5,8   4,9      5,10 

    ctrl    alt     win                space               win     alt     fn      ctrl
    0,9     0,10    3,9                3,10                5,9     6,9     5,7     2,9 

![circuit](https://raw.githubusercontent.com/showjean/ps2avrU/master/firmware/ps2avrU/Circuit/circuit_20131031_040055_001.png)

Matrix
------

PA0 ROW01
PA1 ROW02
PA2 ROW03
PA3 ROW04
PA4 ROW05
PA5 ROW06
PA6 ROW07
PA7 ROW08

PC7 ROW09
PC6 ROW10
PC5 ROW11
PC4 ROW12
PC3 ROW13
PC2 ROW14
PC1 ROW15
PC0 ROW16

PB0 COL01
PB1 COL02
PB2 COL03
PB3 COL04
PB4 COL05
PB5 COL06
PB6 COL07
PB7 COL08

PD0 LED_NL
PD1 LED_CL

subtract 1 from all cols and rows (0 indexed)


    row:   0   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15
    port: A0  A1  A2  A3  A4  A5  A6  A7  C7  C6  C5  C4  C3  C2  C1  C0

    col:   0   1   2   3   4   5   6   7   8
    port: B0  B1  B2  B3  B4  B5  B6  B7  B8


swap rows and pins maybe?

In keymap_common.h, set
#define KEYMAP(
// physical layout to col/row connection. Can be arbitrary numbers but I found this system to work well:
K(row)(col)
){
    // available hardware col/row
}
// TODO: draw picture with lines connecting top and bottom to demonstrate connection