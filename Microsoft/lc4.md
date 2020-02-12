# LeetCode4. 寻找两个有序数组的中位数

### [题目描述](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。

请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 nums1 和 nums2 不会同时为空

#### 样例

```
示例1：
nums1 = [1, 3]
nums2 = [2]:
则中位数是 2.0

示例2：
nums1 = [1, 2]
nums2 = [3, 4]
则中位数是 (2 + 3)/2 = 2.5
```

----------



### 算法1

#### (归并 + 数组)  `时间：O(n + m) & 空间：O(n + m)`

* 把两个有序的数组归并成一个大的有序的`sorted数组`
* 根据题意， `数组sorted`的大小为n
* 若n是奇数, 数组中第`n / 2 + 1`个数是中位数（下标为0的数的第一个数）
* 若n是偶数， 数组中第`n / 2 `个数 和 `n / 2 + 1`个数的平均值是中位数

#### C++ 代码

```c++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int n1 = nums1.size(), n2 = nums2.size();
        // assert(n1 + n2 != 0)
        vector<int> sorted;
        int i = 0, j = 0;
        while (i < n1 && j < n2) {
            if (nums1[i] <= nums2[j]) {
                sorted.push_back(nums1[i++]);
            } else {
                sorted.push_back(nums2[j++]);
            }
        }
        while (i < n1) sorted.push_back(nums1[i++]);
        while (j < n2) sorted.push_back(nums2[j++]);
        
        int n = sorted.size();
        double ans = sorted[n / 2];  // 下标从0开始， 所以要第n/2+1个数的下标是n/2
        if (n % 2 == 0) ans = (ans + sorted[n / 2 - 1]) / 2;
        return ans;
    }
};
```



### 算法2

#### (归并 优化空间)  `时间：O(n + m) & 空间：O(1)`

* 发现其实可以不需要空间，只要每次做归并操作确定一个当前数cur
* 若是奇数，只要做归并操作`n / 2 + 1`次
* 偶数， 也要做`n / 2 + 1`次，但是还要有其前面的数， 所以用一个变量pre来保存前一个数

#### C++ 代码

```c++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int n1 = nums1.size(), n2 = nums2.size();
        int n = n1 + n2;
        // assert(n != 0)
        int cnt = n / 2 + 1;
        int i = 0, j = 0, pre = -1, cur = -1;
        
        while (cnt--) {
            pre = cur;
            if (i < n1 && j < n2) {
                if (nums1[i] <= nums2[j]) {
                    cur = nums1[i++];
                } else {
                    cur = nums2[j++];
                }
            } else if (i < n1) {
                cur = nums1[i++];
            } else {  // j < n2
                cur = nums2[j++];
            }
        }
        
        if (n & 1) return cur;  // 奇数
        return (pre + cur) / 2.0;   // 偶数
    }
};
```



### 算法3

#### (快速选择分治+ 第k小个数)  `时间：O(log(m+n)) & 空间：O(1)`

* 本题的等价问题为： 在两个有序的数组中，若数组总个数为n； 若n为奇数，则返回第`(n + 1)/2`小的数， 若n为偶数，则返回 第`(n + 1)/2`小的数 和 第`(n + 1)/2 + 1`小的数的平均值。
* 现在就是如何实现在两个有序数组中找第k小的数字
* 思路： 两个数组都看在可用范围内k/2个数， 谁的数小，就把该数以及之前的所有数都删掉，重置待定区间从该数的后面开始重置。 知道可选范围是1的时候， 谁小谁为答案。 注意本题边界条件较多，详细见代码。
* 由于代码是尾递归的形式，编译器可以将空间优化成O(1)

#### C++ 代码

```c++
class Solution {
public:
    // nums1 在[s1, e1]范围;  nums2 在[s2, e2]的范围. 选第k小的数
    int findKMin(vector<int>& nums1, int s1, int e1, vector<int>& nums2, int s2, int e2, int k) {
        // base case 1
        if (s1 > e1) return nums2[s2 + k - 1];
        else if (s2 > e2) return nums1[s1 + k - 1];
        // base case 2
        if (k == 1) return min(nums1[s1], nums2[s2]);
		
        // 主体逻辑
        int len = k / 2;
            // 有可能num1 和 nums2 不够长
        if (s1 + len - 1 > e1) len = e1 - s1 + 1; 
        if (s2 + len - 1 > e2) len = e2 - s2 + 1;
            // 第len个数进行比较
        if (nums1[s1 + len - 1] <= nums2[s2 + len - 1]) { 
            return findKMin(nums1, s1 + len, e1, nums2, s2, e2, k - len);
        } else {
            return findKMin(nums1, s1, e1, nums2, s2 + len, e2, k - len);
        }
    }
    
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int n1 = nums1.size(), n2 = nums2.size();
        int n = n1 + n2;
        // assert(n != 0);
        if (n == 1) return n1 == 0 ? nums2[0] : nums1[0];
        
        int k = (n + 1) / 2;
        
        if (n & 1) {
            return findKMin(nums1, 0, n1 - 1, nums2, 0, n2 - 1, k);
        } else {
            int res1 = findKMin(nums1, 0, n1 - 1, nums2, 0, n2 - 1, k);
            int res2 = findKMin(nums1, 0, n1 - 1, nums2, 0, n2 - 1, k + 1);
            return (res1 + res2) / 2.0;
        }
    }
};
```
