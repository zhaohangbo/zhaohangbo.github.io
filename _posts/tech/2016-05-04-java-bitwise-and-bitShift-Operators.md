---
layout: post
title: Bitwise and Bit Shift Operators
category: 技术
tags: 技术
keywords: Java
---


### Bitwise and Bit Shift Operators

[Bitwise and Bit Shift Operators](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op3.html)

```

& 与
The bitwise & operator performs a bitwise AND operation.

^ 异或
The bitwise ^ operator performs a bitwise exclusive OR operation.

| 或
The bitwise | operator performs a bitwise inclusive OR operation.
```

### Bitwise shift operators. Signed and unsigned

[Bitwise shift operators. Signed and unsigned](https://stackoverflow.com/questions/2244387/bitwise-shift-operators-signed-and-unsigned)

```

">>" is signed because it keeps the sign.
It uses the most left digit in binary representation of a number as a filler.
For example:

  the value of sign is used as a filler
  (1)1011011
» (1)1101101

  (0)1010010
» (0)0101001


">>>" is unsigned version of this operator.
It always use ZERO as a filler:
For example:

    11011011
>>> 01101101

    01010010
>>> 00101001

```
