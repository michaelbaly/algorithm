### project
+ parameter F represents input file which contains distinct coordinates range from several to one hundred points
+ according to analyze, output is at most twice the number of OPT
```
POINTS-SEPARATION-AXIS-PARALLEL-LINES(F)
   // let array clist keep the coordinates in instance file
   // array X and Y store the value for horizonal and vertical
   // let set VL/HL contains vertical/horizonal lines picked for each separation
   // use vertical lines to divide the points
   // assuming that the coordinates number n ranges from 4 to 100
1  for i = 3 to n
       // using vertical lines to separate points
       VL <- vertical line with average value for each pair of (X[i], X[i+1])
2  tmp = decreasing value of y-coordinate of first 3 points
   // divide first 3 points with two horizonal lines
3  HL <- average of pair(tmp[1],tmp[2]), (tmp[2],tmp[3])
   // traverse coordinates start with index 4 at step of 3
4  for i = 4 to n // with step of 3
       // sort each 3 points as the first 3 points
5      tmp = {X[i],X[i+1],X[i+2]}
6      reverse tmp
7      for j = 1 to 3
8          pick = true
9          for k = 1 to HL.size
10             if HL[k] in (tmp[j], tmp[j+1])
11                 pick = false
12         if pick == true
13             mid = (tmp[j] + tmp[j+1])/2
14             if mid not in HL
15                 HL <- mid
16 save VL/HL in solution files
```
#### running time:
+ 1 line4, the outer loop, cost O(n/3) for the traverse
+ 2 line7~15, the inner loop, takes O(3*n) to pick all the horizonal line
+ 3 thus, total running time will be O(n^2) for computing the minimum lines contained
in VL or HL

Reference:
1 https://github.com/raj-kotak/Separating-Points-by-Axis-Parallel-Lines
