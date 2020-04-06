## 215.数组中的第K个最大元素

在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

示例 1:

输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
示例 2:

输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
说明:

你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/kth-largest-element-in-an-array
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

我们用优先队列（堆）来解这个问题，构造长度为K的小堆，堆顶元素就是第K大元素，每次新读入的元素跟堆顶元素比较，如果大于则删掉堆顶元素插入新元素，如此反复直到遍历完所有的元素。此时，堆顶元素就是第K大元素。

```
class Solution {
	public int findKthLargest(int[] nums, int k) {
		if (nums == null || nums.length == 0 || k <= 0 || k > nums.length) {
			throw new IllegalArgumentException();
		}
		PriorityQueue<Integer> heap = new PriorityQueue(k);
		int i;
		for (i=0; i < k; i++) {
			heap.offer(nums[i]);
		}
		for (; i < nums.length; i++) {
			if (nums[i] > heap.peek()) {
				heap.poll();
				heap.offer(nums[i]);
			}
		}
		return heap.peek();
	}
}
```

相比快速选择时间复杂度略有上升（O(Nlogk) vs O(N)），但是代码简洁很多，而且需要处理的边界条件少。



## 440.字典序的第K小数字

给定整数 n 和 k，找到 1 到 n 中字典序第 k 小的数字。

注意：1 ≤ k ≤ n ≤ 10九次方。

示例 :

输入:
n: 13   k: 2

输出:
10

解释:
字典序的排列是 [1, 10, 11, 12, 13, 2, 3, 4, 5, 6, 7, 8, 9]，所以第二小的数字是 10。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/k-th-smallest-in-lexicographical-order
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

我们也可以尝试用优先队列来解这个问题，由于是求第k小，所以我们要构建大堆。

```java
class Solution {
    public int findKthNumber(int n, int k) {
        Comparator<Integer> comp = new Comparator<Integer>() {
            @Override
            public int compare(int n1, int n2) {
                String s1 = ""+n1;
                String s2 = ""+n2;
                if (s1.length() == s2.length()) {
                    return n2-n1;
                }
                int i;
                int minLen = Math.min(s1.length(), s2.length());
                for (i=0; i<minLen; i++) {
                    char c1 = s1.charAt(i);
                    char c2 = s2.charAt(i);
                    if (c1 < c2) {
                        return 1;
                    } else if (c1 > c2) {
                        return -1;
                    }
                }
                if (i == s1.length()) { //s1短，则小，逆序所以返回1
                    return 1;
                } else {
                    return -1;
                }
            }
        };
		PriorityQueue<Integer> heap = new PriorityQueue(k, comp);
        
        int i;
        for (i=1; i<=k; i++) {
            heap.offer(i);
        }
        for (; i<=n; i++) {
            //i字典序小于堆顶
            if (comp.compare(i, heap.peek()) > 0) {
                heap.poll();
                heap.add(i);
            }
        }
        return heap.peek();
    }
}
```

这在n和k都不太的情况下是能工作的。比如n=133，k=20的情况可以正确输入116.但在n=**4289384**，k=**1922239**，却运行超时。正确的解法待我写好，会贴链接到这。



## 面试题40.最小的k个数

输入整数数组 arr ，找出其中最小的 k 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

 

示例 1：

输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]
示例 2：

输入：arr = [0,1,2,1], k = 1
输出：[0]


限制：

0 <= k <= arr.length <= 10000
0 <= arr[i] <= 10000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



跟上题类似，求最小的k个数，我们要构建大堆。

```java
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if (arr == null || arr.length == 0 || k < 1 || k > arr.length) {
            return new int[0];
        }
		PriorityQueue<Integer> maxHeap = new PriorityQueue(k, new Comparator<Integer>(){
            @Override
            public int compare(Integer n1, Integer n2) {
                return n2-n1;
            }
        });
        int i = 0;
        for (; i<k; i++) {
            maxHeap.offer(arr[i]);
        }
        for (; i<arr.length; i++) {
            //把小的数推到大堆里
            if (arr[i] < maxHeap.peek()) {
                maxHeap.poll();
                maxHeap.offer(arr[i]);
            }
        }
        int[] result = new int[k];
        for (i=0; i<k; i++) {
            result[i] = maxHeap.poll();
        }
        return result;
    }
}
```

相同的题还有 https://leetcode-cn.com/problems/smallest-k-lcci/