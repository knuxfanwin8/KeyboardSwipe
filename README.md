# Keyboard Swipe Cipher

## Introduction

The Keyboard Swipe (KS) cipher is a cipher designed for use with computers and other devices with a keyboard. It only supports letters (unless otherwise specified) and is based off of the QWERTYUIOP keyboard layout. \
**IMPORTANT:** Do NOT use this cipher for storing sensitive data, such as passwords.

## General overview

Keyboard Swipe Cipher - KS

Usage: `KS[n][d][m] (--switches)`

**n** - number of keys to shift by (max. 6)\
**d** - direction to shift in (R/L/S)\
**m** - encoding/decoding mode, optional, defaults to D (D/F/B)

**--switches** - any switch (explained in Advanced techniques)

To decode, take the letter that you want to decode, and replace it with the letter that's `n` places away in the `d` direction. In case the row is over, use the mode to decide what to do next:

**D (default)** - jump back to the beggining of this row.\
**F (fall)** - go to the beggiing of the next row (if therre is no row below, go back to the beggining of the first row).\
**B (bounce)** -- reverse the direction (L to R, R to L).\

## Understanding Keyboard Swipe

### The naming system

Ciphers like ROT have naming systems that allow them to be expressed in a more understandable way. For example, ROT13 consists of two parts: the cipher name (ROT) and the amount of rotations (13). \
Keyboard Swipe works in a simmilar fashion. Let's say we found some text encoded in the Keyboard Swipe format, and we need to decode it. Typically, there's a hint on the top: a name. We have a document that looks like this:

```md
KS3RF
Lyccs tsuch!
```

From this, we know that the cipher is KS3RF. Let's break this down.

**KS** - Short for **K**eyboard **S**wipe. Appears on the beggining of each KS cipher hint. \
**3 [n]** - The number of keys to shift by. The reccomended maximum is 6, as it allows all rows to be encoded. \
**R [d]** - The direction for the swipe. Can be R (right), L (left) and S (switch, explained later). \
**F [m]** - The encoding mode. This can be left out, and defaults to D (default). The other options are F (fall) and B (bounce). We will explain modes more in-depth shortly.

With this information, we now know that we have to shift by 3 keys to the right, with the fall mode.

### Decoding and encoding (and modes)

The basic principle of KS is that a letter is moved `n` times in the `d` direction. For example, if we were to encode the letter "Q" in KS3R, we would have to do the following:

First, we assign the number 0 to the letter we want to encode. This means that in our case, `0` will be `Q`. Then, every time we move in the `d` direction, we add 1 to that number, and assign it to the letter we're on. In our case, we will end up with this:

```html
Q - 0 << WE BEGIN HERE >>
W - 1
E - 2
R - 3 << THIS IS OUR RESULT >>
```

Now let's encode the letter I, also in KS3R. We go through our usual process, but encounter a problem:

```html
I - 0 << WE BEGIN HERE >>
O - 1
P - 2
??? - 3 << HOUSTON, WE HAVE A PROBLEM >>
```

We are no longer in the same row of letters (mind you, KS is letter-only). This is where the **encoding/decoding modes** come in.

#### Encoding/decoding modes

In the D (default) mode, if we reach the end of a row, we jump back to the beggining of the row. Thus, if `P - 2`, then `Q - 3`, as we jumped back to the beggining.

In the F (fall) mode, we go to the beggining of the row below us. If there are no more rows below, we jump back to the first row (QWERTY...). Thus, if ``P - 2`` and the row below begins on A, then ``A - 3``.

In the B (bounce) mode, we bounce off of the edge, thus, we now go in the opposite direction. In this case, since we have reached the end (`P`), we now go to `O` (so ``O - 3``).

___

Going back to our encrypted sentence (which, in case you forgot, is 'Lyccs tsuch!', encoded in KS3RF), we can see that we will have to use the F (fall) mode while decoding.

```html
L - H
y - e
c - l
s - o
...and so on...
```

In the end, we get the sentence "Hello world!". How nice!

## Advanced techniques

This isn't all that KS can offer. There are some more advanced functions, but they are mostly experimental.

### Switches

Switches can be used to modify the ruleset. For now, there are only 3 switches:

**--numbers** or **-n** - applies the encoding to numbers.
**--special** or **-s** - applies the encoding to special characters (like ',', ':' etc.)
**--apply-all** or **-a** - combines the two above.

### S (switch) direction

This is a purely experimental direction and isn't guaranteed to work well. This direction switches the direction on every character (except for special charcters and spaces).