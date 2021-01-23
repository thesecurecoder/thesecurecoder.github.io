## Maximum length of common subarray

[Problem statement](https://leetcode.com/problems/maximum-length-of-repeated-subarray/)

### Thought process

* Can be solved as maximum result of maximum common prefix for A and B
* Store values to avoid re-calculation
* **Time complexity:** O(MN) (M,N are lengths of the arrays)
* **Space complexity:** O(MN)

### Enough talk, show me the code

```java
class Solution {
    public int findLength(int[] A, int[] B) {
        
        int[][] dp = new int[A.length][B.length]; // dp[i][j] stores the maximum length of common prefix for A[i...N] and B[j...M]
        int maxLength = 0;
        for(int i=0;i<A.length;++i) {
            for(int j=0;j<B.length;++j) {
                maxLength = Math.max(calculate(dp, i, j, A, B), maxLength);
            }
        }
        
        return maxLength;
    }
    
    private int calculate(int[][] dp, int aInd, int bInd, int[] A, int[] B) {
        if(aInd >= A.length || bInd >= B.length) {
            return 0;
        }
        
        if(dp[aInd][bInd] > 0) {
            return dp[aInd][bInd];
        }
        
        if(A[aInd] == B[bInd]) {
            dp[aInd][bInd] = 1 + calculate(dp, aInd + 1, bInd + 1, A, B);
        } else {
            dp[aInd][bInd] = 0;
        }
        
        return dp[aInd][bInd];
    }
    
}
```