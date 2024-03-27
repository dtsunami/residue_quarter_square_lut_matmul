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

The CRT transform is completed for an integer $n$ by choosing several small coprime integers 
and calculating the modulo for $n$ with respect to each. So if $i_1,i_2,i_3$ are coprime integers then 
the CRT can be used to transform $n$ into residues and then calculations performed on the residues modulo 
the respective $i$ and the inverse transform can be used after to get the result in the original domain
as long as the $n < N = i_1 \cdot i_2 \cdot i_3$ then no information is lost. This is similar in practice to the use 
of FFT to perform convolutions where the surrogate field is the frequency domain rather than the residues.

### Dividers for CRT

The primary requirement for this choice is that the multiple of all the integers is larger than the range of your
original number format. For this design we will use 8 bit integers with max value of 255 and dividers $(5, 7, 8)$
with $5 \cdot 7 \cdot 8 = 280$. This choice is made without adequate exploration of design space so it is an
open question whether other sets may be more optimal. The interpolation formula for the inverse transform from
the residues to original domain is then 
$$f(r_5, r_7, r_8) = (56r_5 + 120r_7 + 105r_8) \bmod{280}$$
We can see that the divider choice will determine the multiplcations that need to be performed and we may be able 
to find sets of coprime integers that will result in simple to calculate multiplications. The design space that
should be explored is what is the ops per watt of each set of coprime integers whose product is more than 255 after 
applying all the tricks to do the constant divides and multiplies required by the transforms.

### Uarch for multiplication unit

The matrix multiply unit will take 2 32x32 matrices $\textbf{A}_d$ and $\textbf{A}_w$ of 8 bit intergers and 
produce the result
$$\textbf{C} =  \textbf{A}_w \cdot \textbf{A}_d$$
The matrix $\textbf{A}_w$ should be precalculated into the residue number system and supplied in a packed format
$a_5a_7a_8$ where $a_5$ is 2 bits and $a_7$ and $a_8$ are 3 bits each, its nice that there is no penalty in 
data storage or movement with this set of dividers. Each row should be shifted in while holding the io pin w_load low
and the io pin inc should be held low for 5 clock cycles to latch each one into the multiply units.
Similarly the matrix $\textbf{A}_d$ should be shifted in 1 column at a time in either residue or original format.

## How to test

TODO: finish this

## External hardware

dunno
