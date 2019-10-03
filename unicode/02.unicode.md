In the [first part]() of this 4 parts series, I went over a basic intro on the binary and hexadecimal numeral systems, the ASCII character set and a brief explanation of the motivations behind efforts to extend it.

It was becoming clear that having many incompatible character sets would cause problems on the long run. An attempt to solve this was to create a universal character set, a map of all possible symbols and characters. It would be called Unicode.

Unicode Code Points
-------------------

At the time of writing, Unicode includes over 1.1 million characters, each assigned a specific and unique number just like in ASCII and other legacy encodings. Those numbers are also called code points.

Lets forget about computers for a moment to simply concentrate on Unicode and its code points. If I wrote a sentence using only Unicode code points, you could transcribe it in letters and symbols, just by matching these code points to their assigned character.

Before we proceed, there is a sensible nuance that most articles discussing character sets and Unicode don't spend enough time emphasizing, or go the opposite way by adding so much details that it confuses the reader. As a result, the foundations necessary to understand the general idea remain brittle. But it's worth paying close attention so here it goes: 

Unicode code points are just numbers that map to characters.

In my opinion, one of the first things that confuse people the most about Unicode is that there's a premature connection made between Unicode and encodings. I believe the reason to be that code points are most often represented in hexadecimal. In reality, they don't need to be, that's just a convention. Again:

Unicode code points are just numbers that map to characters.

I didn't say Hexadecimal numbers, I said numbers. Also notice how I didn't mention encoding in that sentence? I haven't forgotten anything. As far as Unicode is concerned, there's no computer and no encoding. The map really is that straight forward. 

Code point -> character

Simple as that. No need to complicate this explanation with details about how characters will be represented in a computer and a whole other set of complexities. Think of Unicode as Morse Code, but with numbers instead of dots and dashes. You want to send a message in Unicode you grab the Unicode map, a pen and some paper and you just proceed to write down those numbers instead of characters. I want to decode your message, I grab my Unicode map and do the translation the other way around. That's it. Unicode code points know nothing about computers, or hex, or encodings.

To make the transition from ASCII to Unicode seamless, it was decided that ASCII characters would have the same code point value in Unicode. That is, `a` which is 97 in ASCII will also be 97 in Unicode, `1` will still be 49 and so forth. The Unicode folks even went as far as making their code points also compatible with the then popular ISO-8859-1 (Latin-1) extended code points (128 - 255). That is `é` which was 233 in Latin-1 will also be 233 in Unicode.

There's more to Unicode, but nothing that needs to be covered here to understand the big picture. By the end of this series you should be able to explore its darker corners with much less apprehension.

Lets recap on Unicode:

    - unicode code points -> characters
    - unicode code points 0 - 127 are the same as ASCII's
    - unicode code points 128 - 255 are the same as Latin-1

In [Part III] (part iii) we'll discuss some Unicode encoding attempts.
