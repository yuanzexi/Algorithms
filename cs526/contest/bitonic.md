## Solution (to be continue)
```python
#!/bin/python

import sys

if __name__ == "__main__":
    n = int(input())
    points = []
    for _ in range(n):
        x, y = map(int, raw_input().strip().split(' '))
        points.append((x, y))
        
#calculate distance     
def d(x,y):
    return ((y[0]-x[0])**2+(y[1]-x[1])**2)**0.5

#dynamic programming
inf = float('inf')
tsp = [[0 for _ in range(n)] for _ in range(n)]
back = [[0 for _ in range(n)] for _ in range(n)]
b0 = d(points[0],points[1])
tsp[0][1] = b0
# keep i < j
for j in range(2,n):
    for i in range(j-1):
        tsp[i][j] = tsp[i][j-1] + d(points[j-1],points[j])
        back[i][j] = (i,j-1)
        
    tsp[j-1][j] = inf
    for k in range(j-1):
        t = tsp[k][j-1] + d(points[k],points[j])
        if t < tsp[j-1][j]:
            tsp[j-1][j] = t
            back[j-1][j] = (k,j-1)
tsp[n-1][n-1] = tsp[n-2][n-1] + d(points[n-2],points[n-1])

#backtracking
a = 0
b = 0
total = tsp[n-1][n-1]
(x,y) = (n-2,n-1)
k = 0
last = n-1
flag = True
while True:
    try:
        (i,j) = back[x][y]
        if x != i:
            flag = not flag
            
        if flag:
            #print (i,j)
            b += d(points[j],points[last])
            last = j
        (x,y) = (i,j)
    except:
        break
b += d(points[0],points[last])
a = total - b

print min(a,b)
print max(a,b)
```

# Problem Description
## (https://www.hackerrank.com/contests/cs526f17/challenges/bitonic-tsp/problem by Michelangelo Grigni)
You are given N points in the plane, with increasing x coordinates. A bitonic tour is cyclic tour visiting all the points, with two parts. The first part of the tour goes left to right, from the leftmost point to the rightmost, visiting some of the points. The second part of the tour goes right to left, from the rightmost point back to the leftmost, visiting the remaining points. You want to find a bitonic tour with minimum total length, using Euclidean distances.

Your output is the lengths of the two parts of the tour, shortest first.
```
### Input Format

The first line is an integer:
N
Next there are N lines, each describing a point Pi=(Xi, Yi):
X0 Y0
X1 Y1
...
XN-1 YN-1

### Constraints

All inputs are integers:
3 ≤ N ≤ 3500
0 ≤ Xi, Yi < 3*N

The Xi coordinates are strictly increasing:
X0 < X1 < ... < XN-1
So P0 is leftmost, and PN-1 is rightmost.

### Output Format

Your output is two floating point numbers, one per line. The first is the length of the shorter part of the tour, and the second is the length of the longer part. The sum of these two numbers is the total tour length.

NOTE: the two output numbers both need to be accurate within additive error ±0.0001. Standard (double precision) floating point arithmetic should suffice.

### Sample Input 0

3
3 0
4 3
6 3
### Sample Output 0

4.242640687119285
5.16227766016838
### Explanation 0

The first (shorter) part is [0, 2] (from P0 to P2).
The second (longer) part is [2, 1, 0] (from P2 to P1 to P0).

### Sample Input 1

4
1 8
5 0
6 10
8 0
### Sample Output 1

11.94427190999916
15.583203834320074
### Explanation 1

the first part is [0, 1, 3] (P0 to P1 to P3).
The second part is [3, 2, 0].

### Sample Input 2

6
1 17
2 13
3 14
5 13
10 14
11 16
### Sample Output 2

10.04987562112089
15.10847465658312
Explanation 2

The first part is [0, 5] (directly from P0 to P5).
The second part is [5, 4, 3, 2, 1, 0].

### Sample Input 3

9
2 23
4 12
5 16
6 21
7 22
8 17
12 2
19 19
24 15
### Sample Output 3

24.658790631658505
51.68170388249915
### Explanation 3

The first part is [0, 3, 4, 7, 8].
The second part is [8, 6, 5, 2, 1, 0].

### Sample Input 4

14
1 8
2 4
3 24
4 5
6 10
8 2
11 25
12 21
16 30
19 29
24 16
25 26
31 16
36 40
### Sample Output 4

65.83371207974201
72.7661426312191
### Explanation 4

The first part is [0, 2, 6, 7, 8, 9, 11, 13].
The second part is [13, 12, 10, 5, 4, 3, 1, 0].

### Sample Input 5

22
1 47
2 60
7 9
9 56
17 38
21 3
22 25
26 65
28 47
29 49
30 17
33 15
34 65
35 52
37 40
38 49
40 32
54 29
55 5
59 38
63 53
64 15
### Sample Output 5

128.26400564932734
235.89610926760895
### Explanation 5

The first part is [0, 2, 5, 6, 10, 11, 18, 21].
The second part is [21, 20, 19, 17, 16, 15, 14, 13, 12, 9, 8, 7, 4, 3, 1, 0].
```
