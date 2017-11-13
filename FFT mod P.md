# Solution (to be continued)
```java
package FFT;
import java.util.*;
public class Solution {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int p = in.nextInt();
        int n = in.nextInt();
        int w = in.nextInt();
        long[] a = new long[n];
        for(int a_i=0; a_i < n; a_i++){
            a[a_i] = in.nextInt();
        }
//        n = extractN(p,n,w+p);
//        System.out.println(log(8));
        a = iterFFT(a,p,n,w+p);
        printArray(a,p);
//        long[] b = new long[n];
//        for (int i=0; i < n; i++){
//            b[i] = calc(a, p, i, w);
//            System.out.print(b[i]+" ");
//        }
    }
    private static int extractN(int p, int n, int w){
        Set set = new LinkedHashSet();
        long temp_w = 1;
        while (n > 0){
            temp_w = (w*temp_w) % p;
            set.add(temp_w);
            n --;
        }
        print(set.toString());
        print(set.size()+"");
        return set.size();
    }
    private static int log(int a){
        int count = 0;
        while (a > 1){
            a /= 2;
            count++;
        }
        return count;
    }
    public static long[] iterFFT(long[] a, int p, int n, int w){
        a = bitReverse(a);
        int s = log(n);
        for (int i = 1; i <= s; i++){
//            print("i = "+i +"");
            int m = (int)pow(2,i,Integer.MAX_VALUE);
//            System.out.println(m);
            for (int k = 0; k < n; k+=m){
//                System.out.println("k="+k);
                long temp_w = 1;
                for (int j = 0; j < m/2; j ++){
                    long t = temp_w * (a[(k+j+m/2)]);
                    long t2 = (p - temp_w) *  (a[(k+j+m/2)]);
                    long u = (a[k+j]);
//                    print("t="+t+",u="+u);
                    a[k+j] = (u + t);
                    long temp = (u + t2);
//                    print("temp = "+temp);
                    a[k+j+m/2] = temp;
                    temp_w = (temp_w * w ) % p;
//                    print("");
//                    print("w = "+temp_w);
//                    printArray(a,p);
//                    print("");
                }
            }
        }
        return a;
    }
    private static void print(String a){
        System.out.println(a);
    }
    private static void printArray(long[] a, int p){
        for (int i = 0 ; i < a.length; i++){
            System.out.print((a[i]%p)+" ");
        }
    }
    private static long[] bitReverse(long[] a) {
        int n = a.length;
        int m = log(n);
        long[] b = new long[n];
        for (int i = 0; i < n; i++){
            int r = reverse(i,m);
            b[i]= a[r];
        }
        return b;
    }

    private static int reverse(int a,int m){
        int b = 0;
        while (m > 0){
            b <<= 1;
            b |= (a & 1);
            a >>= 1;
            m--;
        }
        return b;
    }

    public static long pow(long a, long b, int p){
        long init = 1;
        long bound = Long.MAX_VALUE/a/a;
        while (b > 0){
            init = init * a;
            if (init > bound){
                init = init % p;
            }
            b--;
        }
        return init;
    }

    public static long calc(int[] a, int p, int n, int w){
        long result = 0;
        long base = pow(w,n,p);
        base = base % p;
        for (int i =0; i < a.length; i++){
            result += a[i]*(pow(base,i,p));
        }
        result = result % p;
        return (result);
    }
}
```
# Solution 2 (High computational complexity, timed out)
```java
import java.io.*;
import java.util.*;
import java.text.*;
import java.math.*;
import java.util.regex.*;

public class Solution {

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int p = in.nextInt();
        int n = in.nextInt();
        int w = in.nextInt();
        int[] a = new int[n];
        for(int a_i=0; a_i < n; a_i++){
            a[a_i] = in.nextInt();
        }
        long[] b = new long[n];
        for (int i=0; i < n; i++){
            b[i] = calc(a, p, i, w);
            System.out.print(b[i]+" ");
        }
    }

    public static long pow(long a, long b, int p){
        long init = 1;
        while (b > 0){
            init = init * a;
            init = init % p;
            b--;
        }
        return init;
    }

    public static long calc(int[] a, int p, int n, int w){
        long result = 0;
        long base = pow(w,n,p);
        base = base % p;
        for (int i =0; i < a.length; i++){
            result += a[i]*(pow(base,i,p));
        }
        result = result % p;
        return (result);
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
