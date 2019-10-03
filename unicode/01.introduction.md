This series of articles aims at explaining Unicode, UTF-8 and other legacy encodings and how they relate to each other. I attempt the exercise by making little assumptions on what the reader's prior understanding of certain computing concepts are. That is I'll try to go from what I perceive to be good basics to introduce the topic[1]. I will sometimes therefore linger on some details that I believe to be key to have a solid foundation to better grasp the subject.

In this first lesson we brush over the decimal, binary and hexadecimal numeral systems, we talk about ASCII and its shortcomings. This will be a ramp for some and a reminder for others, but I feel that it could be a solid base upon which to develop further explanations.

1- The decimal, binary and hexadecimal numeral systems
---
It is speculated that humans have adopted ten symbols to represent numbers because they (usually) have ten fingers. Although we still use that system to develop the bulk of our understanding of maths, science and nature, we could have done it with another [numeral system](https://en.wikipedia.org/wiki/Numeral_system) using two, three, twenty, or hundreds or billions of symbols, each representing a different "quantity". But we settled for ten and it hasn't been too bad. Working with ten symbols that represent ten incrementing quantities starting from a zero value is called working in Base-10 (base-ten) or decimal.

To increment values in decimal, you start from 0, go all the way up to 9, you carry a value over to the left and you start over:

    0, 1, 2, ..., 8, 9, 10, 11, 12, ..., 19, 20, 21, ...

The common modern computer as currently designed can only understand electricity. Like most electronic devices they can really only detect that the power is On and the power is Off, which basically means that they can count from zero to one. Therefore electronic operations, or computations as they are often called, would need to be performed using a numeral system that best matches that limitation. The binary numeral system, also known as Base-2, is a complete system using only 2 symbols, 1 and 0. A perfect fit.

To increment quantities in binary, you start from 0 like in decimal, but you go up to 1 before carrying a value over to the left and start over:

    0, 1, 10, 11, 100, 101, ..., 111111, 1000000, 1000001, 1000010, ...

The quantity "twenty three" expressed in decimal would be: 23 and in binary 10111.

You can see that as useful as binary is for electronics, it can easily become rather tedious to work with for humans.

To make things easier, they needed a way to convert their decimal operations in binary and vice-versa. But that again isn't straight forward. It turns out, however, that it's relatively simple to convert between the binary and the base-16 numeral system (also known as hexadecimal). It also happens to be simple enough to go between base-10 and base-16. Thus, the hexadecimal was found to be an adequate compromise between base-10 and base-2, a sort of middle ground, and people who commonly need to perform binary math have learned to work in hex directly. It's not decimal, but it's easier on the eyes and the brains than binary.

Hexadecimal numbers are represented using sixteen symbols: 0-9 for the first ten, and past these, A-F for quantities ten to fifteen. In computing, hex numbers are generally represented with a leading 0 followed by an x. e.g. 0x21 tells me this isn't a decimal 21 (twenty-one) but rather hex 21 (two one), a completely different quantity.

    decimal |  binary | hexadecimal
    --------+---------+------------
       0    |      0  |     0x0
       1    |      1  |     0x1
       2    |     10  |     0x2
       3    |     11  |     0x3
       4    |    100  |     0x4
     ...    |    ...  |     ...
      10    |   1010  |     0xa
      11    |   1011  |     0xb
      12    |   1100  |     0xc
     ...    |   ...   |     ...
      15    |   1111  |     0xf
      16    |  10000  |    0x10
      17    |  10001  |    0x11
     ...    |    ...  |     ...
      29    |  11101  |    0x1d
      30    |  11110  |    0x1e
      31    |  11111  |    0x1f
      32    | 100000  |    0x20
     ...    |    ...  |     ...
     ...    |    ...  |     ...

Converting between decimal and hex is not going to be very relevant to our topic. Being able to convert between binary and hex on the other hand might be more helpful and is fortunately also rather easy. Keeping in mind that the hex numeral system spans 0 - F, wich is the equivalent in binary to 0 - 1111, to convert a binary number to its hex equivalent, simply gather the binary digits in groups of 4 starting from the right. Pad the leftmost remaining group with 0 to the left if you want, then simply convert each group to its hex equivalent:

    10011        =       0001 0011 = 0x13
    10111111     =       1011 1111 = 0xbf
    1010011010   =  0010 1001 1010 = 0x29a

To convert the other way around, you do the reverse, you replace each hex value by its binary equivalent.

    0xa8    = 1010 1000
    0xc1    = 1100 0001
    0x92d   = 1001 0010 1101


2- Bits and bytes 
---

Computers process data in 8 bit packets called Bytes:

                        1010 1111 = 0xaf     (1 byte)
            0001 0011   1010 1111 = 0x13af   (2 bytes)
0101 1010   1011 1010   1001 0010 = 0x5aba92 (3 bytes)


3- An oversimplified view of character sets
---

Computers don't actually understand characters. In reality, characters, whether they're in a document, on a command line, or a browser window, are represented as strings of bytes (i.e. batches of 8 bits packets). To display them on screen, a decoder processes the electronic stream and maps each number it identifies to yet another type of data, containing additional instructions relevant to a graphical device, whose purpose is to draw shapes on screen (in this case the characters). This is a big picture and an overly simplistic one, but I think for our purpose it'll do. In a nutshell, characters are stored as numbers that are mapped to instructions for graphical devices that draw shapes on screen for our benefit.

### ASCII

ASCII was an effort to create such a character map. 128 (one hundred and twenty eight) characters to be exact, numbered 0 to 127. The map includes upper and lower case english letters, numbers, punctuations, some symbols, as well as some white spaces, formatting characters (spaces, tabs, new lines, etc), as well as some unprintable characters. Some examples ASCII characters and their decimal, binary and hexadecimal representation.

    Char |   Dec  |   Binary      |   Hex
    -----+--------+---------------+-------------
    'A'  |   65   |   0100 0001   |   0x41
    'a'  |   97   |   0110 0001   |   0x61
    'b'  |   98   |   0110 0010   |   0x62
    '1'  |   49   |   0011 0001   |   0x31
    '!'  |   33   |   0010 0001   |   0x21
    '~'  |   126  |   0111 1110   |   0x7e

Note that if `~` is at position 126, it means that it's the 127th character (first character on the map is at position 0). Therefore the 128th and last ASCII character (an unprintable) is at 127, or 0111 1111 in binary, or 0x7f in hex. ASCII doesn't include characters with accents, nor other non-english letters and symbols.

### Extended Character Sets: Latin-1, etc.

As mentioned previously the last ASCII character is at binary position 0111 1111. You probably noticed the unused leading bit, yes? Well, so did a few folks who really wanted characters such as `ç`, `ñ`, `ß` and other such glyphs foreign to the english language. So, they proceeded to extend the ASCII set for their own purpose by switching that leading bit on. It then gave them an additional range of 128 new possible characters (1000 0000 to 1111 1111). Unfortunately, not everyone agreed as to what character should go where and so, each came with their own implementation. Thus began a plethora of different character sets that had everything in common from position 0 to 127 (ASCII) and then diverged from 128 to 255 (some would eventually overlap on some subsets). A couple of them, such ISO-8859-1 also known as Latin-1, are still very much in use today.

To recap:

    - ASCII was designed with 128 character positions (0 - 127)
    - the last position being at 0111 1111 (127).
    - ASCII requires only 1 byte as characters go from 0000 0000 to 0111 1111.
    - ASCII didn't include non-english characters.
    - non-english speakers needed them.
    - to extend ASCII the unused leading bit in the byte was turned on (1000 0000)
    - it provided 128 more positions for new characters (1000 0000 to 1111 1111).
    - though, not everyone agreed on which character should go where.
    - so ensued various incompatible implementations from position 128 onward.

[Part II]() of the series introduces Unicode.
