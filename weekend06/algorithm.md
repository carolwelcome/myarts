leetcode 4
```Java
class Solution {
	//执行用时 : 15 ms, 在Median of Two Sorted Arrays的Java提交中击败了80.33% 的用户
	//内存消耗 : 52.9 MB, 在Median of Two Sorted Arrays的Java提交中击败了69.29% 的用户
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
       	int[] data3=new int[nums1.length+nums2.length];
        System.arraycopy(nums1, 0, data3, 0, nums1.length);
        System.arraycopy(nums2, 0, data3, nums1.length, nums2.length);
        Arrays.sort(data3);
        if(data3.length%2==1)
            return data3[(data3.length+1)/2-1];
        else
            return (data3[data3.length/2-1]+data3[data3.length/2])/2.0; 
    }

    //官方解法
    public double findMedianSortedArrays(int[] A, int[] B) {
        int m = A.length;
        int n = B.length;
        if (m > n) { // to ensure m<=n
            int[] temp = A; A = B; B = temp;
            int tmp = m; m = n; n = tmp;
        }
        int iMin = 0, iMax = m, halfLen = (m + n + 1) / 2;
        while (iMin <= iMax) {
            int i = (iMin + iMax) / 2;
            int j = halfLen - i;
            if (i < iMax && B[j-1] > A[i]){
                iMin = i + 1; // i is too small
            }
            else if (i > iMin && A[i-1] > B[j]) {
                iMax = i - 1; // i is too big
            }
            else { // i is perfect
                int maxLeft = 0;
                if (i == 0) { maxLeft = B[j-1]; }
                else if (j == 0) { maxLeft = A[i-1]; }
                else { maxLeft = Math.max(A[i-1], B[j-1]); }
                if ( (m + n) % 2 == 1 ) { return maxLeft; }

                int minRight = 0;
                if (i == m) { minRight = B[j]; }
                else if (j == n) { minRight = A[i]; }
                else { minRight = Math.min(B[j], A[i]); }

                return (maxLeft + minRight) / 2.0;
            }
        }
        return 0.0;
    }
}
```