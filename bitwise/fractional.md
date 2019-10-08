# How to write fractional numbers in binary

## Fractionals in base-10

In base-10 the first fractional power of 10 is 1/10 which is the decimal 0.1.

We can further break down that value by subdividing it into different powers of 10. 

Dividing 1 into progressive powers of 10 gives us different fractional "levels" or magnitude.

The subsequent fractional powers of 10 follow:

    1/10        0.1         1/10
    0.1/10      0.01        1/10²
    0.01/10     0.001       1/10³
    0.001/10    0.0001      1/10⁴
    0.0001/10   0.00001     1/10⁵

Because we're in base-10 each larger level represents ten of the smaller one directly underneath it. That is 1/10 is ten of 1/10², which is in turn ten of 1/10³.

To be able to express a quantity accurately we need a way to express intermediary quantities between a level and the one directly underneath it. In base-10 there are ten symbols we can use to express primitive quantities (0, 1, 2, 3, 4, 5, 6, 7, 8, and 9). So, we can combine them along with the above fractionals to accurately describe a fractional quantity. For example

    0.783029    =>  7 x 1/10¹
                =>  8 x 1/10²
                =>  3 x 1/10³
                =>  0 x 1/10⁴
                =>  2 x 1/10⁵
                =>  9 x 1/10⁶

## Fractionals in base-2
Simarly, in base-2 the first fractional power of 2 is 1/2, which is the decimal value 0.5 and the binary 0.1.

We can further create more base-2 fractional levels by subdividing 1 into different powers of 2.

the subsequent fractional powers of 2 follow, with their decimal equivalent:

    bin         dec      |  bin         dec     |   bin         dec     
    ---------------------|----------------------|-------------------
    1/10        1/2      |  0.1         .5      |   1/10        1/2     
    0.1/10      0.1/2    |  0.01        .25     |   1/100       1/2²    
    0.01/10     0.01/2   |  0.001       .125    |   1/1000      1/2³    
    0.001/10    0.001/2  |  0.0001      .0625   |   1/10000     1/2⁴    
    0.0001/10   0.0001/2 |  0.00001     .03125  |   1/100000    1/2⁵    

Since we're in base-2 each larger level represents two of the smaller one directly underneath it. That is 1/2 is two of 1/2², which is in turn two of 1/2³.

And since in base-2 we have only 2 symbols (0 and 1), to express a fractional value, we need to find the right combination of binary fractionals whose sum corresponds to the quantity we want to describe. That binary representaion can express the same quantity as its decimal counterpart, but can be considerably longer, which may impede to its practical accuracy in the real world (i.e. with computer). For instance, just contrast being able to express the decimal value 0.84 with just 3 digits versus its binary equivalent

    decimal to  binary
    -------     -------
    0.84        =>  1 x 1/10¹
                =>  1 x 1/10²
                =>  0 x 1/10³
                =>  1 x 1/10⁴
                =>  0 x 1/10⁵
                =>  1 x 1/10⁶
                =>  1 x 1/10⁷
                =>  1 x 1/10⁸
                =>  0 x 1/10⁹

                ...and on and on...
                

### A few useful observations from the above:

When multiplying a base-10 number by ten, you shift all its digits to the left, including the ones to the right of the period (which are theoretical zeros for a whole number). When dividing a base-10 number you do the opposite, you shift all digits to the right.

Similarly, multiplying a base-2 number by two results in shifting all its digits to the left and dividing by two shifts all digits to the right

The above implies that if I have decimal value `0.725` which is the binary equivalent `0.11`, by twice doubling the latter I'm shifting it twice to the left to become binary `11`. It's easy enough to verify that `0.725 * 2 * 2 = 3`, is indeed `11` in binary.

Dividing `1` by `2` gives us `0.5` in base-10, but in base-2 that's `0.1` (remember, in base-2 to divide by two we can simply shift to the right). This implies that any value larger than decimal `0.5` and less than `1` is also larger than binary `0.1` and less than `1`.  

i.e. the following are equivalent:

    base-10:  0 [.......] 0.5 [.......] 1 
    base-2 :  0 [.......] 0.1 [.......] 1 

A side-effect of the above is that any value between decimal `0.5` and `1` in its binary representation has `1` bit set right after the period. That is, `0.5293`, `0.73882`, `0.99382`, `0.6783` when expressed in binary all begin like `0.1...`  whereas `0.4923`, `0.2893`, `0.10293` when expressed in binary will all begin like `0.0...`.

This gives us the basis of a scheme to write any base-10 value containing fractional parts in its binary form.   

- First thing is to check whether the value is larger than `1`. If it is, we should isolate its integer and fractional parts and process them separately.

- now that we have the isolated fractional part, we can process it.

Processing fractional parts (approach through doubling)
---
1-) first thing is to double the fractional value and check if the new doubled value is larger or equal to 1. Remember that the first fractional power of 2 is the decimal 0.5 or binary 0.1. As we've seen, doubling that fractional binary number means shifting its bit to the left by 1.

2.a-) If the new (doubled) value is less than 1, it means that the pre-doubled value was less than decimal .5, thus could not have started with a leading 1 bit after the period. We thus record a 0 for that position instead. 
    
    e.g. decimal .483
    .483 * 2 = .966
    .966 < 1 => in binary, first digit after period cannot be 1
    we record .0

2.b-) If on the other hand the doubled value is above 1, then the pre-doubled value must have been larger or equal to .5 and in its binary form would be represented with a leading 1 after the period

    e.g. decimal .672
    .672 * 2 = 1.344
    1.344 > 1 => in binary, first digit after period is 1
    we record .1

Also, now that we've recorded a set bit, we remove 1 from the value and continue processing the remainder fractional part.

    1.344 - 1 = .344

3.a-) If remaining value is larger than zero go back to step (1) for continued processing.
3.b-) Else if remaining value is zero, output representation.

Examples:

    value = .343

    (1)     value *=2   # .686
    (2.a)   value < 1
            output <- 0 # [0]
    (3.a)   .686 > 0 -> goto (1)

    (1)     .686 *=2    # 1.372
    (2.b)   value > 1
            output <- 1 # [0,1]
            value -= 1  # .372
    (3.a)   .372 > 0 -> goto (1)

    (1)     .372 *=2    # .744
    (2.b)   value < 1
            output <- 1 # [0,1,0]
    (3.a)   .744 > 0 -> goto (1)

    (1)     value *= 2  # 1.488
    (2.b)   value > 1
            output <- 1 # [0,1,0,1]
            value -= 1  # .488
    (3.a)   .488 > 0 -> goto (1)

    (1)     value *=2   # .976
    (2.b)   value < 1
            output <- 0 # [0,1,0,1,0] 
    (3.a)   .976 > 0 -> goto (1)

    (1)     value *=2   # 1.952
    (2.b)   value > 1
            output <- 1 # [0,1,0,1,0,1] 
            value -= 1  # .952
    (3.a)   .952 > 0 -> goto (1)
    ...

