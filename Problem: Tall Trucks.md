# Solution (Use heapq!)
```python
# Enter your code here. Read input from STDIN. Print output to STDOUT
import sys
import heapq

if __name__ == "__main__":
    n,m = map(int,raw_input().strip().split(' '))
    path = [[] for _ in range(n+1)]
    for p_i in xrange(m):
        p_temp = map(int,raw_input().strip().split(' '))
        path[p_temp[0]].append((p_temp[1],p_temp[2]))
        path[p_temp[1]].append((p_temp[0],p_temp[2]))
result = [0 for _ in range(n+1)]
mask = [0 for _ in range(n+1)]
q = [(0,1)]
mask[1] = 1;
while len(q) > 0:
    (hv,v) = heapq.heappop(q)
    #print q
    #print 'hv:',hv,',v:',v
    hv = -hv
    if hv == result[v]:
        mask[v] = 1
        for (k,h) in path[v]:
#            print 'k:',k,',h:',h
            if mask[k] == 0:
                if h > result[k]:
                    if h > hv and hv > 0:
                        result[k] = hv
#                       print 'result[k]:',result[k],'hv:',hv
                    else:
                        result[k] = h
                    heapq.heappush(q,(-result[k],k))
#    print result
#    print '--------'
del result[0],result[0]
print(' '.join(map(str,result)))
```
## Solution (java with higher complexity)
```java
import java.io.*;
import java.util.*;
import java.text.*;
import java.math.*;
import java.util.regex.*;
import java.util.*;

public class Solution {

    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        int m = in.nextInt();
        //int[][] matrix = new int[n][n];
        LinkedList<Integer[]>[] vts = new LinkedList[n];
        for (int i = 0; i < m; i++){
            int s = in.nextInt() - 1;
            int t = in.nextInt() - 1;
            int h = in.nextInt();
            if (s != t){
                if (vts[s] == null){
                    vts[s] = new LinkedList<>();
                }
                vts[s].add(new Integer[]{t,h});
                if (vts[t] == null){
                    vts[t] = new LinkedList<>();
                }
                vts[t].add(new Integer[]{s,h});
            }
        }
        in.close();
        int[] mask = new int[n];
        int[] result = new int[n];
        for (Integer[] arr: vts[0]) {
            if (result[arr[0]] < arr[1]){
                result[arr[0]] = arr[1];
            }
        }
        int[] max = getMaxIndex(mask,result);
        mask[max[0]] = 1;
        while (max[1] > 0){
            for (Integer[] arr: vts[max[0]]){
                if (mask[arr[0]] == 0){
                    if (result[arr[0]] < arr[1]){
                        if (max[1] < arr[1]){
                            result[arr[0]] = max[1];
                        }else{
                            result[arr[0]] = arr[1];
                        }
                    }
                }
            }
            max = getMaxIndex(mask,result);
            mask[max[0]] = 1;
        }
        for (int i = 1; i < n; i++){
            System.out.print(result[i]+" ");
        }

    }
    public static int[] getMaxIndex(int[] mask,int[] result){
        int[] max = new int[2];
        for (int i = 1; i < mask.length; i ++){
            if (mask[i] == 0){
                if (result[i] > max[1]) {
                    max[1] = result[i];
                    max[0] = i;
                }
            }
        }
        return max;
    }
    static class Vertex{
        int index = 0;
        int height = 0;
        public Vertex(int index, int height){
            this.index = index;
            this.height = height;
        }
        public int getHeight() {
            return this.height;
        }

        public int getIndex() {
            return index;
        }
    }


}
```

# Problem Decription 
## (https://www.hackerrank.com/contests/cs526f17/challenges/tall-trucks by Michelangelo Grigni)
There are N cities, and you want to send trucks from your factory (in city 1) to the other cities, the taller the better. There are M two-way roads connecting pairs of cities. Each road has a maximum truck height, determined by bridges and tunnels along the way. For each city V, you want to work out the maximum height of a truck that can travel from city 1 to city V. You do not care about the length of the route, just the best height.
```
### Input Format

The first line is "N M", where N is the number cities, and M is the number of roads.

Each of the next M lines is "U V H". It describes a road between cities U and V, with max truck height H.

### Constraints

All inputs are positive integers.

1 ≤ N ≤ 50000
1 ≤ M ≤ 200000
1 ≤ U ≤ N
1 ≤ V ≤ N
1 ≤ H ≤ 1000

### Notes

Possibly U=V (such roads are useless). If there are multiple roads between two cities, use the best road.

This has a solution resembling Dijkstra's algorithm, running in similar time bounds.

### Output Format

For each city V from 2 to N, print the maximum height of a truck that can travel from city 1 to city V. Or if there is no path from 1 to V, print 0.

Your output is N-1 integers, separated by spaces.


Sample Input 0

4 3
1 2 72
1 4 96
2 4 80
Sample Output 0

80 0 96
Explanation 0

The best path to city 2 is via city 4. There is no path to city 3.

Sample Input 1

5 6
1 2 70
1 3 75
2 3 80
2 5 72
3 4 65
4 5 70
Sample Output 1

75 75 70 72
Explanation 1

The best path from 1 to 5 is 1-3-2-5, with height 72.

Sample Input 2

5 10
1 5 90
2 5 160
3 5 134
2 5 1000
1 3 126
3 4 128
1 3 139
1 5 1000
1 2 111
1 5 176
Sample Output 2

1000 139 128 1000
Sample Input 3

10 20
1 9 90
4 9 160
6 9 134
3 10 1000
2 5 126
6 7 128
2 6 139
1 10 1000
2 4 111
2 9 176
2 7 85
1 7 85
7 10 161
5 8 1000
3 9 1000
4 7 1000
2 10 113
4 7 95
3 8 86
8 9 61
Sample Output 3

176 1000 161 126 139 161 126 1000 1000
Sample Input 4

20 40
2 17 90
7 17 160
12 17 134
5 19 1000
4 9 126
11 14 128
4 11 139
1 19 1000
3 7 111
4 18 176
3 13 85
1 14 85
14 19 161
10 15 1000
6 17 1000
8 14 1000
4 19 113
7 13 95
5 16 86
15 18 61
15 16 1000
4 17 1000
16 17 154
14 18 106
1 8 83
1 15 156
4 15 1000
5 9 1000
10 15 163
6 10 178
10 15 138
2 3 113
10 14 85
3 15 179
15 17 1000
1 6 171
3 9 81
5 6 170
10 15 88
7 20 180
Sample Output 4

113 171 171 1000 171 160 161 1000 171 139 134 95 161 171 171 171 171 1000 160
Sample Input 5

50 100
3 44 1000
8 33 111
23 26 1000
19 38 97
17 39 87
17 27 143
7 18 143
2 20 1000
13 40 78
8 13 1000
25 30 78
7 27 64
7 38 168
26 49 172
20 43 1000
11 33 1000
15 27 1000
37 47 87
14 25 109
9 41 1000
7 36 1000
1 30 176
31 39 90
31 34 1000
24 32 169
12 35 60
6 16 66
25 30 175
8 44 1000
17 42 92
19 31 175
19 26 103
29 30 137
20 44 71
5 14 139
44 49 167
7 30 77
30 33 175
2 43 104
28 42 130
5 6 92
11 28 133
8 30 111
31 40 1000
5 28 1000
10 41 62
25 40 1000
19 41 70
35 37 1000
17 23 113
17 33 141
2 4 131
14 42 76
13 19 1000
30 49 1000
9 21 1000
17 36 138
42 48 1000
19 34 1000
33 37 169
4 31 117
3 10 177
8 33 1000
7 17 179
25 45 1000
21 22 154
12 29 144
7 47 1000
34 43 175
27 40 70
6 24 141
24 28 177
38 47 153
3 19 127
6 18 117
3 26 103
11 31 1000
36 46 87
39 46 1000
3 6 1000
3 28 1000
9 40 143
16 35 1000
3 18 108
10 16 134
8 38 173
5 9 1000
20 29 1000
26 38 117
23 48 86
16 44 126
9 23 177
15 36 1000
20 29 75
29 38 91
13 40 76
9 32 158
18 34 1000
23 29 72
28 34 66
Sample Output 5

175 175 131 175 175 168 175 175 175 175 144 175 139 168 169 168 175 175 175 175 154 175 175 175 175 168 175 175 176 175 169 175 175 169 168 169 173 90 175 175 130 175 175 175 90 168 130 176 0
```
