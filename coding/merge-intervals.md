## Merge Intervals

[Problem statement](https://leetcode.com/problems/merge-intervals/)

### Thought process

* Sort the intervals basis their start and end points.
* Merge consecutive intervals if the end point of the previous interval and start point of the next interval overlap.
* **Time complexity:** O(NlogN)
* **Space complexity:** O(N)

### Enough talk, show me the code

```java
class Solution {
    
    private static final Comparator intervalComparator = new Comparator<int[]>() {
        public int compare(int[] interval1, int[] interval2) {
            if(interval1[0] == interval2[0]) {
                return ((Integer)interval1[1]).compareTo(interval2[1]);
            }
            return ((Integer)interval1[0]).compareTo(interval2[0]);
        }
    };
    
    public int[][] merge(int[][] intervals) {
        
        Arrays.sort(intervals, intervalComparator);
        
        // debug(intervals);
        
        int lastCoveredPoint = -1, tracker = -1;
        
        for(int i=0;i<intervals.length;++i) {
            if(lastCoveredPoint < intervals[i][0]) {
                tracker ++;
                lastCoveredPoint = intervals[i][1];
            } else {
                lastCoveredPoint = Math.max(intervals[i][1], lastCoveredPoint);
            }
        }
        
        int[][] newIntervals = new int[++tracker][2];
        
        lastCoveredPoint = -1;
        tracker = -1;
        
        for(int i=0;i<intervals.length;++i) {
            if(lastCoveredPoint < intervals[i][0]) {
                tracker ++;
                newIntervals[tracker][0] = intervals[i][0];
                newIntervals[tracker][1] = intervals[i][1];
                lastCoveredPoint = newIntervals[tracker][1];
            } else {
                newIntervals[tracker][1] = Math.max(intervals[i][1], newIntervals[tracker][1]);
                lastCoveredPoint = newIntervals[tracker][1];
            }
        }
        
        return newIntervals;
        
    }
    
    private void debug(int[][] intervals) {
        for(int i=0;i<intervals.length;++i) {
            for(int j=0;j<intervals[i].length;++j) {
                System.out.print(intervals[i][j] + " ");
            }
            System.out.println();
        }
    }
}
```