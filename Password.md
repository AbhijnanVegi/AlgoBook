# How do passwords work ?

A **password**, sometime also called passcode is secret data, typically a string of characters usually used to confirm a user’s identity. Passwords are generally not stored in plain text, as they can be easily grabbed by malicious eyes. Instead we hash the password using a hash function and store its output. Every time the user inputs the password, we calculate its hash using the same function and compare the output with whatever was stored.

A cryptographic hash function must have the following properties

1. It is quick to compute the hash value of any given message
2. It is infeasible to generate a message that yields a given hash value
3. It is infeasible to find two different messages with the same hash value
4. A small change to a message should change the hash value so extensively that a new hash value appears uncorrelated with the old hash value.

Some common examples of cryptographic hash functions are MD5 and SHA-256.

We will now explore the algorithm behind the MD5 hash function.

## MD5

**MD5** processes a variable length message into a fixed length output of 128 bits.

1. The input message in broken up into chunks of 512 bit blocks.
2. The message is then padded so its length is divisible by 512.
   The message is padded as follows
   1. Append a single bit, 1 to the end of the message
   2. Add as many zeros a required to bring the length of the message up to 64 bits fewer than a multiple of 512.
   3. The remaining bits are filled up with 64 bits representing the length of the original message modulo $2^{64}$

The algorithm operates in a 128 bit state, divided into four 32 bit words, denoted A, B, C and D, which are initialized to certain fixed constants. It then uses each of the 512 bit message block in turn to modify the state. The processing of a message block consists of four similar stages, termed *rounds*; each round is composed of 16 similar operations based on a non-linear function *F*, modular addition, and left rotation. There are four possible functions; a different one is used in each round.

![img](Assets/300px-MD5_algorithm.svg.png)

Given below are the four possible functions, where $\bigoplus, \land,\lor,\lnot$ represent XOR, AND, OR and NOT respectively.

$$
F(B,C,D) = (B \land C)\lor(\lnot B \land D)\\
G(B,C,D) = (B \land D) \lor (C \land \not D)\\
H(B,C,D) = B \oplus C \oplus D\\
I(B,C,D) = C \oplus (B \land \lnot D)
$$
The psuedocode for MD5 algorithm is given below

```bash
# : All variables are unsigned 32 bit and wrap modulo 2^32 when calculating
var int s[64], K[64]
var int i
# s specifies the per-round shift amounts
s[ 0..15] := { 7, 12, 17, 22,  7, 12, 17, 22,  7, 12, 17, 22,  7, 12, 17, 22 }
s[16..31] := { 5,  9, 14, 20,  5,  9, 14, 20,  5,  9, 14, 20,  5,  9, 14, 20 }
s[32..47] := { 4, 11, 16, 23,  4, 11, 16, 23,  4, 11, 16, 23,  4, 11, 16, 23 }
s[48..63] := { 6, 10, 15, 21,  6, 10, 15, 21,  6, 10, 15, 21,  6, 10, 15, 21 }

# Use binary integer part of the sines of integers (Radians) as constants:
for i from 0 to 63 do
    K[i] := floor(232 × abs (sin(i + 1)))
end for

# Initialize variables:
var int a0 := 0x67452301   // A
var int b0 := 0xefcdab89   // B
var int c0 := 0x98badcfe   // C
var int d0 := 0x10325476   // D

# Pre-processing: adding a single 1 bit
append "1" bit to message    
 # Notice: the input bytes are considered as bits strings,
 #  where the first bit is the most significant bit of the byte.[51]
# Pre-processing: padding with zeros
append "0" bit until message length in bits ≡ 448 (mod 512)

# Notice: the two padding steps above are implemented in a simpler way
#  in implementations that only work with complete bytes: append 0x80
#  and pad with 0x00 bytes so that the message length in bytes ≡ 56 (mod 64).
append original length in bits mod 264 to message

# Process the message in successive 512-bit chunks:
for each 512-bit chunk of padded message do
    break chunk into sixteen 32-bit words M[j], 0 ≤ j ≤ 15
    # Initialize hash value for this chunk:
    var int A := a0
    var int B := b0
    var int C := c0
    var int D := d0
    # Main loop:
    for i from 0 to 63 do
        var int F, g
        if 0 ≤ i ≤ 15 then
            F := (B and C) or ((not B) and D)
            g := i
        else if 16 ≤ i ≤ 31 then
            F := (D and B) or ((not D) and C)
            g := (5×i + 1) mod 16
        else if 32 ≤ i ≤ 47 then
            F := B xor C xor D
            g := (3×i + 5) mod 16
        else if 48 ≤ i ≤ 63 then
            F := C xor (B or (not D))
            g := (7×i) mod 16
        // Be wary of the below definitions of a,b,c,d
        F := F + A + K[i] + M[g]  // M[g] must be a 32-bits block
        A := D
        D := C
        C := B
        B := B + leftrotate(F, s[i])
    end for
    # Add this chunk's hash to result so far:
    a0 := a0 + A
    b0 := b0 + B
    c0 := c0 + C
    d0 := d0 + D
end for
var char digest[16] := a0 append b0 append c0 append d0 # (Output is in little-endian)
```

MD5 is no longer a suggested to use in security applications since it has been proven to generate the same hash for different inputs. 