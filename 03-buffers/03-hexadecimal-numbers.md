# Hexadecimal Numbers.

Hexadecimal numeric system is the base-sixteen numeric system, so we take each character and multiply it by sixteen to the power of its index when converting to a decimal number.

Hexadecimal numbers are important because they can be used to represent large numbers with fewer digits; any set of four adjacent bits is equal to one and only one hexadecimal number when converted, so we can use one hexadecimal number to represent the value of four bits. For example, to view a file with 4000 bits, we can use 1000 hexadecimal numbers for convenience. Therefore, over the course of a software engineering career, hexadecimal numbers continue to serve an extremely important purpose.

By convention, we display a hexadecimcal number by adding "0x" before the number to differentiate it from those with different base values. The "0x" prefix has nothing to do with the number itself, it is simply an indicator.

```
0x456
```

Again, we can take each character starting from the right, multiply it by sixteen to the power of its index, and then add the result together to convert it to a decimal number.

<pre>
0x456
= 6 x 16<sup>0</sup> + 5 x 16<sup>1</sup> + 4 x 16<sup>2</sup>
= 6 + 80 + 1024
= 1110
</pre>

Now, to display hexadecimal numbers, we have a total of sixteen characters, and we do not care if the last six characters are uppercase or lowercase when writing out the complete number.

|  0  |  1  |  2  |  3  |  4  |  5  |  6  |  7  |  8  |  9  |  A  |  B  |  C  |  D  |  E  |  F  |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
|  0  |  1  |  2  |  3  |  4  |  5  |  6  |  7  |  8  |  9  | 10  | 11  | 12  | 13  | 14  | 15  |

To convert a number involving the last six characters, we use their numeric form in the multiplication part of the formula. The following example confirms what that means.

<pre>
0xfa3c
= 12 x 16<sup>0</sup> + 3 x 16<sup>1</sup> + 10 x 16<sup>2</sup> + 15 x 16<sup>3</sup>
= 12 + 48 + 2560 + 61440
= 64060
</pre>

Hexadecimal numbers make our life much easier when dealing with IP addresses, memory locations and a ton of other things in computing. For comparison, it is much easier to read the hexadecimal number with six characters than a binary number with twenty-four characters.

| Hexadecimal  |   Decimal    |            Binary             |
| :----------: | :----------: | :---------------------------: |
|   0xFFFFFF   |   16777215   | 1111 1111 1111 1111 1111 1111 |
| 6 characters | 8 characters |         24 characters         |

Conversion from hexadecimal to binary and vice-versa is extremely simple because each hexadecimal character is exactly four bits long. Therefore, we can use a table to convert between the two numeric systems.

```
0 -> 0000
1 -> 0001
2 -> 0010
3 -> 0011
4 -> 0100
5 -> 0101
6 -> 0110
7 -> 0111
8 -> 1000
9 -> 1001
A -> 1010
B -> 1011
C -> 1100
D -> 1101
E -> 1110
F -> 1111
```

The following code-block shows an example of a binary to hexadecimal conversion using the table above.

```
0101 0101 0111 1101 0101 1111 0000 0001
= 557D 5F01
```
