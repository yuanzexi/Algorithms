# Solution (Python)
```python
#!/bin/python

import sys


p,n,w = raw_input().strip().split(' ')
p,n,w = [int(p),int(n),int(w)]
a = map(int, raw_input().strip().split(' '))

def printArr(a,p):
    for i in range(len(a)):
        print a[i] % p,
def recursiveFFT(a, w, p):
    n = len(a)
    if n == 1:
        return a
    w0 = 1
    a0 = a[::2]
    a1 = a[1::2]
    y0 = recursiveFFT(a0, (w*w)%p, p)
    y1 = recursiveFFT(a1, (w*w)%p, p)
    y = a[:]
    for k in range(n/2):
        y[k] = (y0[k] + w0 * y1[k]) % p
        if w == 1:
            y[k+n/2] = (y0[k] + w0 * y1[k] + p) % p
        else:
            y[k+n/2] = (y0[k] - w0 * y1[k] + p) % p
        w0 = (w0 * w % p)
    return y
    
    
c = recursiveFFT(a,w,p)
printArr(c,p)
```

# Solution (java)
```java

import java.util.*;
public class Solution {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        long p = in.nextInt();
        int n = in.nextInt();
        long w = in.nextInt();
        long[] a = new long[n];
        for(int a_i=0; a_i < n; a_i++){
            a[a_i] = in.nextInt();
        }
        a = recursiveFFT(a,p,w);
        printArray(a,p);
    }
    private static void printArray(long[] a, long p){
        for (int i = 0 ; i < a.length; i++){
            System.out.print((a[i])+" ");
        }
    }
    private static long[] recursiveFFT(long[] a, long p,  long w){
        int n = a.length;
        if (n == 1) {
            return a;
        }
        long w0 = 1;
        long[] a0 = extractArray(a,0,2);
        long[] a1 = extractArray(a,1,2);
        long[] y0 = recursiveFFT(a0, p,((w * w)%p));
        long[] y1 = recursiveFFT(a1, p,((w * w)%p));
        long[] y = new long[n];
        for (int k = 0; k < n/2; k++){
            y[k] = (y0[k] + w0 * y1[k] % p) % p;
            if (w == 1){
                y[k+n/2] = (y0[k] + w0 * y1[k] % p + p) % p;
            }
            else {
                y[k+n/2] = (y0[k] - w0 * y1[k] % p+ p) % p;
            }
            w0 = ((w0 * w )% p);
        }
        return y;
    }
    private static long[] extractArray(long[] arr, int start, int space){
        long[] newArr = new long[arr.length / space];
        int k = 0;
        for (int i = start; i < arr.length; i += space){
            newArr[k++] = arr[i];
        }
        return newArr;
    }
}
```

# Problem Description (https://www.hackerrank.com/contests/cs526f17/challenges/fast-fourier-mod-p)
We are given an odd prime P, and N, a power of two that is less than P. We are also given W, an N-th root of one mod P. That is, W is an integer such that WN mod P (its remainder after dividing by P) equals one.

We are also given a polynomial A(x) with integer coefficients and degree less than N:

A(x) = a0 + a1 x + a2 x2 + ... + aN-1 xN-1
For each i≥0, define bi = A(Wi) mod P (an integer remainder in the range 0 to P-1). Our goal is to compute the N integers b0 b1 ... bN-1.

## Input Format

The input is two lines of space-separated integers. The first line has three integers:  P N W

The second line has the N coefficients of A(x):  a0 a1 ... aN-1

## Constraints

All inputs are non-negative integers.

P is an odd prime, 3 ≤ P < 230

N is a power of 2, 2 ≤ N < P, N ≤ 218

1 ≤ W < P, and WN mod P = 1

For each i, 0 ≤ ai < P

## Output Format

Print these N integers, separated by spaces:

b0 b1 ... bN-1

Each bi should be in the range 0 to P-1 (inclusive).

### Sample Input 0

5 4 2
1 2 3 4
### Sample Output 0

0 4 3 2
### Explanation 0

b0 = A(20) = A(1) = 1+2*1+3*1+4*1 = 0 (mod 5)

b1 = A(21) = A(2) = 1+2*2+3*4+4*8 = 4 (mod 5)

b2 = A(22) = A(4) = 1+2*4+3*16+4*64 = 3 (mod 5)

b3 = A(23) = A(8) = 1+2*8+3*64+4*512 = 2 (mod 5)

### Sample Input 1

5 4 3
0 4 3 2
### Sample Output 1

4 3 2 1
### Explanation 1

b0 = A(30) = A(1) = 0+4*1+3*1+2*1 = 4 (mod 5)

b1 = A(31) = A(3) = 0+4*3+3*9+2*27 = 3 (mod 5)

b2 = A(32) = A(9) = A(4) = 0+4*4+3*16+2*64 = 2 (mod 5)

b3 = A(33) = A(27) = A(2) = 0+4*2+3*4+2*8 = 1 (mod 5)

### Sample Input 2

17 8 2
0 1 0 1 0 1 0 1
### Sample Output 2

4 0 0 0 13 0 0 0
### Sample Input 3

17 8 9
4 0 0 0 13 0 0 0
### Sample Output 3

0 8 0 8 0 8 0 8
### Explanation 3

Note this transform is almost the inverse of the previous example (we still have to divide the result by N=8).

### Sample Input 4

13 8 1
0 1 0 1 0 1 0 1
### Sample Output 4

4 4 4 4 4 4 4 4
### Explanation 4

In this example, W=1 is an N-th root of one, but not a primitive N-th root (that is, N is not the smallest exponent D such that WD=1 (mod P)).

### Sample Input 5

97 32 19
3 9 59 80 81 70 59 32 44 37 15 51 93 22 2 6 71 17 51 14 71 83 38 24 14 96 27 76 40 57 10 71
### Sample Output 5

65 29 30 55 10 38 79 73 21 92 46 35 36 33 78 8 30 1 65 93 23 52 48 25 0 21 94 19 95 21 76 63
