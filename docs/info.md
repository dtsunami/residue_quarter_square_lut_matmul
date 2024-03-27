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

### Quarter-Squares lookup table multiply

$$ ab = \frac{1}{4}\left( a + b \right)^2 - \frac{1}{4}\left( a - b \right)^2 $$

If $a$ and $b$ are integers, in the case that $a + b$ is even the square will be a multiple of 4 
as well as the square of $a - b$ so that both $\frac{1}{4}(a + b)^2$ and $\frac{1}{4}(a - b)^2$ are integers. 
In the case that $a + b$ is odd then $a - b$ will be odd also and each square will have a factor of $\frac{1}{4}$ 
which will cancel in the difference so that $\frac{1}{4}( a + b )^2 - \frac{1}{4}(a - b)^2$ so this means that
the lookup table for the quarter square terms can be implemented with integers only. So the strategy is to 
precalculate the factors $int(n^2/4)$ for the domain of small integers(3-5 bits). To perform the lookup we
simply must calculate one addition and one subtraction, lookup the quarter squares factors and finally do another 
subtraction to get the multiplication result. Main idea is that this can be done in a cdyn optimized way to
get a compelling ops per watt, possibly at the expense of some area for the lookup tables.

### CRT Transform

The CRT transform is completed for a integer $n$ by choosing several small coprime integers 
and calculating the modulo for $n$ with respect to each. So if $i_1,i_2,i_3$ are coprime integers then 
the CRT can be used to transform $n$ into residues and then calculations performed on the residues modulo 
the respective $i$ and the inverse transform can be used after to get the result in the original domain
as long as the $n < N = i_1 \cdot i_2 \cdot i_3$ then no information is lost. This is similar in practice to the use 
of FFT to perform convolutions where the surrogate field is the frequency domain rather than the residues.


## How to test

TODO: finish this

## External hardware

dunno
