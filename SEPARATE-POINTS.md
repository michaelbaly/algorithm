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

### PSEUDOCODE
* this is an instance of minimal set cover algorithm which follow the greedy method, the the main idea is to repeatedly separate the largest subsets of points until we have no other connection
1. purpose: separate points in plane with axis-parallel lines
2. input: coordinates read from instance file
3. output: set S which contains vertical and horizonal lines of the minimal number
```
SEP-POINTS-BY-AXIS-PARALLEL-LINES()
1     READ-FILE(instance, points_st)
2     SORT-POINTS(points_st)
3     GENERATE-COM-DIGRAPH(points_st)
      // recursively split axis-x and axis-y
4     SPLIT-AXIS(X, 0, n_pts-1)
5     SPLIT-AXIS(Y, 0, n_pts-1)
6     while points_connects > 0 and lx < n_lines_x and ly < n_lines_y
7          flag = EXAM-CONNECTION(x_lines[lx])
8          if flag == true
9              ADD-LINE(x_lines[lx])
10         lx = lx + 1
12         flag = EXAM-CONNECTION(y_lines[ly])
13         if flag == true
14             ADD-LINE(y_lines[ly])
15         ly = ly + 1
16    endwhile
17    GENERATE-SOLUTION(instance)
```
```
// purpose: generates a complete digraph with all the points
// there have n*(n-1)/2 edges follow the property of a complete digraph,
// thus, there exist n*(n-1) connections, takes O(n^2) to generates it
GENERATE-COM-DIGRAPH(points_st)
1     for i = 0 to n_pts
2         trace index of current point <- i
3         for j = 0 to n_pts
4             if i != j // connect to all other points
5                 points_st.cor[i].connection[j] = address(points_st.cor[j])
6                 points_st.cor[i].con_num += 1
7             else
8                 points_st.cor[i].connection[j] = NIL
9         endfor
10    endfor
```
```
// purpose: recursively divide X and Y axis into half, and find a line which
mostly divide the graph and subgraph
// lines_st contains the updated axis parallel lines
SPLIT-AXIS(axis, from, to)
1     if from == to // base case
2         return
3     diff = to - from
4     if diff == 1
5         return
6     else if diff % 2
7         half = (diff - 1) / 2
8     else
9         half = diff / 2
      // update line info
10    if axis == X
11        ls = address of (sorted_x_pts[0])
12    else
13        ls = address of (sorted_y_pts[0])
14    if half != 0
15        pt1 = ls[from + half]
16        pt2 = ls[from + half + 1]
17        pta = x-coordinate or y-coordinate of pt1
18        ptb = x-coordinate or y-coordinate of pt2
19        ptdiff = ptb - pta //ptb no less than pta(sorted)
20        ptmid = pta + ptdiff/2
          // update current line that will be added to lines_st
21        UPDATA-LINE-INFO(axis)
          // recursively find the line which separate the axis
22        if diff != 2
23            SPLIT-AXIS(axis, from, from + half)
24            SPLIT-AXIS(axis, from + half, to)
```
```
ADD-LINE(line)
1     set committed stataus of this line
2     axis = axis of line
3     inter = intercept of this line
4     pos = GET-POINT-CLOSEST-TO-INTER(axis, inter)
5     if axis == X
6         ls = sorted_x_pts
7     else
8         ls = sorted_y_pts
9     for i = 0 to pos
10        j = pos + 1
11        for j to n_pts
12            DISCONNECT_POINTS(ls[i]->self_index, ls[j]->self_index)
13        endfor
14    endfor
```
```
DISCONNECT_POINTS(pt1, pt2)
1    if pt1 connect to pt2
2        disconnect connection from pt1 to pt2
3        disconnect connection from pt2 to pt1
4        decrease connect count of pt1 by 1
5        decrease connect count of pt2 by 1
6        decrease total connect count of the graph by 2
```
```
UPDATE-LINE-INFO(axis, intercept_cor)
1    if axis == X
2        cur_line = x_lines[n_x_lines]
3        n_x_lines = n_x_lines + 1
4    else
5        cur_line = y_lines[n_y_lines]
6        n_y_lines = n_y_lines + 1
7    axis of cur_line <- axis
8    intercept of cur_line <- intercept_cor
     // init commit status of cur_line
9    committed <- 0
```
```
// Exam if there are any of line of the left of cur_line has pt_connections
// to any points on the right of cur_line
EXAM-CONNECTION(cur_line)
1     axis = axis of cur_line
2     inter = intercept of cur_line
3     position = GET-POINT-CLOSEST-TO-INTER(axis, inter)
4     if axis == X
5         ls = address of (sorted_x_pts[0])
6     else
7         ls = address of (sorted_y_pts[0])
8     for i = 0 to (postion + 1)
9         j = postion + 1
10        for j to n_pts
11            if points[ls[i]->self_index].pt_connections[ls[i]->self_index] != NIL
12                return true
13        endfor
14    endfor
15    return false
```
```
GET-POINTS-CLOSEST-TO-INTER(axis, inter)
1     if axis == X
2         ls = sorted_x_pts
3     else
4         ls = sorted_y_pts
5     for i = 0 to n_pts
6         if axis == X
7             cor = pt_x of ls[i]
8         else
9             cor = pt_y of ls[i]
10        endif
11        if cor > inter
12            return (i-1)
13    endfor
14    return -1 // there's no point to the left of the intersect
```
## Running time analyse
+ 1. for a specific instance file, there's have n coordinates in the plane
+ 2. line2 use quick sort to nondecreasing sort both x-axis and y-axis, thus have an upper bound O(n^2) to complete the process
+ 3. line3 takes O(n*(n-1)) to generates a complete digraph
+ 4. line4~5, it takes O(n*logn)for each recursive call on x-axis and y-axis
+ 5. line6~16, the outer loop takes n times to find a maximum connection line picked, and ADD-LINE process takes O(n^2) to disconnect all the connections with all other points, thus takes O(n^3) to pick all axis-parallel lines
+ 6. we have instance file which contains points ranging from 3 to 100, where 100 represent an upper bound for the value of n.
+ 7. so for each instance file, it takes n^2 + n*(n-1) + nlogn + nlogn + n^3 to find the minimum lines, if considering to find all instance files, thus it takes n(n^2 + n*(n-1) + 2nlogn + n^3) for the total running time, omit the lower bound, we got the worst cas O(n^4) for the running time
Reference:
+ 1 https://github.com/nickziv/sep_pts
