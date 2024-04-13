# Character Encodings.

A character set is a collection of characters used to represent text in a computer system. It includes letters, numbers, symbols, and other characters that a computer can display or process. Character sets map each character to a unique numeric code, allowing computers to store and manipulate textual data consistently.

Various character sets are available, with some designed to support specific languages or scripts, while others support multiple languages. Two of the most commonly used character sets are "UNICODE" and "ASCII".

Unicode defines a standard for representing and encoding characters in most of the writing systems worldwide. The unicode standard version 15.1 (released 12 September 2023) defines 149,813 encoded characters with unique identifying names mapped to immutable code points. For example, when using the unicode standard, the binary number equivalent to the decimal number "115" represents the lowercase character "s" inside the computer system.

ASCII defines a standard for representing and encoding characters in only the English language. It defines 128 characters: lowercase and uppercase a-z, numbers from 0-9, punctuations, and some control characters (like DEL). This standard is very commonly adopted, and if we work with something that does not understand a character outside of ASCII, such as a URL, we see a binary thing instead.

To display a complete list of ASCII character set, we can execute the following command on Unix-based operating systems, such as Linux and MacOS. This would return a list of ASCII character set in three numeric systems: octal, decimal, and hexadecimal. If we look closely at the hexadecimal list, it represents each of the 128 characters using a maximum of two digits. Therefore, we can evaluate that an ASCII character can take up a maximum of eight bits within a computer system.

```sh
man ascii
```

Note that ASCII is a subset of unicode. Therefore, characters within ASCII have the exact same numeric mapping in unicode by definition. It is also important to remember that character sets are a standard, not an application.

---

We often come across the following terms in the practical use of a computer: image encoders, image decoders, video encoders, video decoders, etc. What do we mean by encoders and decoders?

An encoder gets something meaningful for humans and turns it into something meaningful for computers (i.e. zeros and ones). For example, an image encoder takes an image, and converts it into some zeros and ones. Once completed, this procedure allows the computer to work with that image in a number of ways - storing, sending over the network, etc. A decoder, on the contrary, works in the exact opposite manner; it converts back the zeros and ones to something meaningful for human consumers.

---

Character encoding is the process of assigning a sequence of bytes (zeros and ones) to a character, allowing it to be stored, transmitted, and transformed using digital computers. It is something built into our operating system, and is not an abstract idea like the unicode character set. If an operating system does not have a built-in character encoding in place, it means that we cannot deal with text at all within that operating system, which is completely meaningless.

The most common character encoding that we have right now is the UTF-8 character encoding. It is defined by the Unicode Standard, therefore its characters have the same immutable code points as the unicode character set. This means that the lowercase letter "s" we saw earlier is converted to the binary equivalent of "115" when our operating system uses the UTF-8 character encoding.

We mentioned earlier that ASCII characters take up a maximum of eight bits or one byte. This holds true when the character encoding used is UTF-8, which is the case with MacOS and a ton of operating systems out there. When dealing with these 128 ASCII characters, the most significant bit is always zero and the other seven bits determine the value to be stored; the 128th character has the following value in binary numbers.

```
0111 1111
```

Characters outside of the ASCII subset can take up one to four bytes when using UTF-8 character encoding. Therefore, we often come across the following terms: one-byte encoding, two-bytes encoding, four-bytes encoding, etc.

One final thing to note here is that the ASCII characters can take up more than one byte to represent a character depending on the underlying character encoding. Therefore, we should always specify the encoding system where needed. If we encode something using UTF-16 and decode it using UTF-8, we'd get some weird output.
