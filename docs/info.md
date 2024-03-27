<!---

This file is used to generate your project datasheet. Please fill in the information below and delete any unused
sections.

You can also include images in this folder and reference them in the markdown. Each image must be less than
512 kb in size, and the combined size of all images must be less than 1 MB.
-->

## Chinese Remainder Theorem(CRT) Matrix Multiply

Number theoretic transforms can be used to transform numbers into a surrogate field where it may be easier 
to perform computations. The CRT can be used to transfer large integers into a residue number system of several
smaller integers, calculations can then be performed on the residues and then they can be combined back into the
original domain to get the final result.

The goal of this project is to test creating a 32x32 asynchronous matrix multiply unit that performs the
multiplication using a quarter-square based lookup table. The quarter square formula allows the collapse
of a 2-dimensional multiplication lookup table into a single-dimensional table of quarter squares which
hopefully can be implemented in a area and gate efficient way:-P

$$ ab = \frac{1}{4}\left( a + b \right)^2 - \frac{1}{4}\left( a - b \right)^2 $$

If $a$ and $b$ are integer, in the case that $a + b$ is even the square will be a multiple of 4 
as well as the square of $a - b$ so that both $\frac{1}{4}(a + b)^2$ and $\frac{1}{4}(a - b)^2$ are integers. 
In the case that $a + b$ is odd then $a - b$ will be odd and each square will have a factor of $\frac{1}{4}$ 
which will cancel in the difference so that $ ab = \frac{1}{4}\left( a + b \right)^2 - \frac{1}{4}\left( a - b \right)^2 $
is an integer.

## How to test

TODO: finish this

## External hardware

dunno
