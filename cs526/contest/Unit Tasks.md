# Solution (java)
```java
import java.io.*;
import java.util.*;
import java.text.*;
import java.math.*;
import java.util.regex.*;
public class Solution {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        int[] d = new int[n];
        for(int d_i = 0; d_i < n; d_i++){
            d[d_i] = in.nextInt();
        }
        int[] v = new int[n];
        for(int v_i = 0; v_i < n; v_i++){
            v[v_i] = in.nextInt();
        }
        in.close();
        int result = calc(n,d,v);
        System.out.print(result);
    }    
    public static int calc(int n, int[] d, int[] v){
        int[] s = new int[n];
        int[] idx = new int[n];
        for (int i = 0; i < n; i++){
            idx[i] = i;
        }
        quicksort(v,idx,0,n-1);
        int sum = 0;
        for (int i = 0; i < n; i++){
            for (int j = d[idx[i]]; j > 0; j-- ){
                if (s[j] == 0){
                    sum += v[i];
                    s[j] = 1;
                    break;
                }
            }
        }
        return sum;
    } 
    public static void exchange(int[] arr,int i,int j){
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }        
    public static int partition(int[] arr,int[] idx, int p,int r){
        int x = arr[r];
        int i = p-1;
        for (int j = p; j < r; j++){
            if (arr[j] >= x){
                i = i+1;
                exchange(arr,i,j);
                exchange(idx,i,j);
            }
                
        }
            
        exchange(arr,i+1,r);
        exchange(idx,i+1,r);
        return i+1;
    
    }  
    public static void quicksort(int[] arr,int[] idx, int p,int r){
        if (p < r){
            int q = partition(arr,idx,p,r);
            quicksort(arr,idx,p,q-1);
            quicksort(arr,idx,q+1,r);
        }
    }
}
```

# Problem Description  
## (https://www.hackerrank.com/contests/cs526f17/challenges/unit-tasks/problem by Michelangelo Grigni)
You have N tasks to perform in some order. Each task takes one minute. Task i has a deadline Di and a value Vi. If you finish task i within the first Di minutes, then the task is on-time and you earn a profit of Vi dollars. Otherwise the task is late, and you earn nothing for this task.

A schedule is an ordering (or permutation) of the N tasks, and its value is the sum of the profits earned for its on-time tasks. Your goal is to find the maximum possible value of a schedule.

### Input Format

The first line is a single positive integer: 
   N
The second line is N space-separated integers, the task deadlines: 
   D1 D2 ... DN
The third line is N space-separated integers, the task values: 
   V1 V2 ... VN

### Constraints

1 ≤ N ≤ 7000
For each i, 0 ≤ Di ≤ N
For each i, 0 ≤ Vi ≤ 100000

### Notes

If Di==0, then task i is late in any schedule.
If Di==N, then task i is on-time in any schedule.
If Vi==0, then it does not matter whether task i is on-time.

A solution running in O(N2) time should be acceptable. If you manage to implement this in a better time bound, contact me regarding possible extra credit.

### Output Format

The output is an integer, the maximum possible value of a task schedule.

### Sample Input 0

5
1 2 2 3 3
2 2 4 2 6
### Sample Output 0

12
### Explanation 0

One optimal schedule is (3, 2, 5, 4, 1), where tasks 2, 3, and 5 are on-time, with value 2+4+6=12. Another optimal schedule is (1, 3, 5, 2, 4), where 1, 3 and 5 are on-time, with value 2+4+6=12.

### Sample Input 1

7
1 1 2 2 4 4 4
4 5 2 3 1 2 6
### Sample Output 1

16
### Explanation 1

An optimal schedule is (2, 4, 6, 7, 1, 3, 5), where tasks 2, 4, 6, 7 are on-time, with value 5+3+2+6=16. Another optimal schedule is (2, 4, 7, 6, 5, 3, 1), with the same on-time tasks and value.

### Sample Input 2

13
4 5 0 5 0 3 1 0 0 4 7 3 1
20 17 3 17 7 23 16 8 2 23 18 7 17
### Sample Output 2

118
### Sample Input 3

17
6 4 6 2 3 0 0 0 2 0 0 1 3 6 6 2 0
11 11 7 23 20 20 32 28 8 9 9 28 8 24 0 2 15
### Sample Output 3

117
### Sample Input 4

22
1 4 5 4 1 5 15 8 1 3 3 2 8 9 13 8 3 3 4 1 4 6
34 22 38 37 22 38 14 13 12 13 34 27 23 16 12 20 14 18 35 8 16 13
### Sample Output 4

280
