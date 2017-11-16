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
#print(points)
def d(x,y):
    return ((y[0]-x[0])**2+(y[1]-x[1])**2)**0.5

inf = float('inf')
tsp = [[0 for _ in range(n)] for _ in range(n)]
back = [[0 for _ in range(n)] for _ in range(n)]
b0 = d(points[0],points[1])
path = [[0 for _ in range(n)] for _ in range(n)]
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
            back[j-1][j] = (k,j)
tsp[n-1][n-1] = tsp[n-2][n-1] + d(points[n-2],points[n-1])
# print tsp
# print '-----'
# print back

a = 0
b = 0
total = tsp[n-1][n-1]
# (i,j) = back[n-2][n-1]
b += d(points[n-2],points[n-1])
x = n-2
(i,j) = (n-2,n-1)
k = 0
while True:
    try:
        (i,j) = back[i][j]
        if j-i == 1:
            if k % 2 == 0:
                b += d(points[j],points[x])
                x = j
            else:
                b += d(points[i],points[x])
                x = i
            k += 1
    except:
        break
a = total - b

print min(a,b)
print max(a,b)
```

