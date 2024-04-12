# Binary Numbers.

In computer science, the term `binary` refers to any digital encoding/decoding system in which there are exactly two possible states: zero and one. A `binary digit`, or bit, is the smallest unit of data in digital information, and it can take on either of the two values, zero or one.

A `byte` is the smallest addressable unit of memory in many computer architectures, even though a `bit` is the smallest unit of data that can be represented on a computer. The size of the byte has historically been hardware-dependent and no definitive standards existed that mandated the size. Therefore, sizes from 1 to 48 bits have been used in early encoding systems. However, in modern computing, a byte is mandated to be eight adjacent binary digits (bits) long in most cases.

THe following code block shows an example of a byte. The value stored within this byte equates to a binary or a base-two number.

```
00001011
```

To convert a base-two number to a base-ten number, we can take each bit starting from the right, multiply it by two to the power of its index, and then add the result together.

<pre>
= 1 x 2<sup>0</sup> + 1 x 2<sup>1</sup> + 0 x 2<sup>2</sup> + 1 x 2<sup>3</sup> + 0 x 2<sup>4</sup> + 0 x 2<sup>5</sup> + 0 x 2<sup>6</sup> + 0 x 2<sup>7</sup> 
= 1 + 2 + 0 + 8 + 0 + 0 + 0 + 0 
= 11
</pre>

This shows that 11 items in the decimal number system can also be represented by 1011 items in the binary number system. The former system is not understood by computers, therefore they store this information in the binary number system; they also perform addition, comparison, and other operations in the binary number system.

Binary numbers are called base-two numbers because we multiply each bit by "two" to the power of its index when converting to a decimal number. Similarly, decimal numbers are called base-ten numbers because we multiply each character by "ten" to the power of its index when breaking it down to its constituents.

<pre>
319
= 9 x 10<sup>0</sup> + 1 x 10<sup>1</sup> + 3 x 10<sup>2</sup>
= 9 + 10 + 300
</pre>

We can build an infinite number of systems using this technique, such as the base-five number system, but in computer science, we work with three number systems for the most part: base-two, base-ten, and base-sixteen.
