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

基本上优先队列可以成为求解第k大或第k小问题的标准套路。除了直接分配长度k的队列，还可以通过k次poll得到第k大的数，再比如下面这道。

## 面试题17.09.第k个数

有些数的素因子只有 3，5，7，请设计一个算法找出第 k 个数。注意，不是必须有这些素因子，而是必须不包含其他的素因子。例如，前几个数按顺序应该是 1，3，5，7，9，15，21。

示例 1:

输入: k = 5

输出: 9

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/get-kth-magic-number-lcci
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

这题虽然没有明确写明是求第K大的数，但是观察数列可以知道这是递增数列。这次我们采用最小堆，不停poll的方式，当第k次poll的时候就是要求的那个数。之所以要用这种方式，因为poll的结果要参与新数的运算。每次poll出的数都是当前最小因子，它跟{3,5,7}分别相乘就能得到满足题意的新数。

注意每次新生成的数有可能之前就生成过，如3x5=5x3；另外每次生成的数都是等比递增，这会给人一个错觉，会以为去重后每次追加到普通队列后面即可，这是不对的。举个反例：

比如当前poll的元素是7，它乘以素数后得到21,35,49，前两个重复；下个poll的元素是9，它乘以素数后得到27,45,63；可以看到如果是普通队列27是跟在49后面的，这样的队列每次poll的数就不是当前最小的，当然最后的求的k也是错误的。

```java
class Solution {
    public int getKthMagicNumber(int k) {
		int[] primes = {3,5,7};
        //为了避免整型溢出，所以用long存储
        PriorityQueue<Long> heap = new PriorityQueue();
        HashSet<Long> set = new HashSet();
        
        heap.offer(1L);
        long num = 1L;
        for (int i=0; i<k; i++) {
            num = heap.poll(); //每次log2k，因为每次新增3个数，减少1个数，一共k次，klog2k
            long result;
            for (int p : primes) { 
                result = num * p; //执行3k次
                if (!set.contains(result)) {
                    heap.offer(result); //执行klog2k~3klog2k
                    set.add(result);
                }
            }
        }
        return (int)num;
    }
}
```

时间复杂度：O(NlogN)。我们还会在动态规划看到这题。

虽然优先队列是解第K大问题的标准套路，但正如440题看到的，这不一定是最优方案。再比如下面这题，也是如此。

## 719.找出第k小的距离对

给定一个整数数组，返回所有数对之间的第 k 个最小距离。一对 (A, B) 的距离被定义为 A 和 B 之间的绝对差值。

示例 1:

输入：
nums = [1,3,1]
k = 1
输出：0 
解释：
所有数对如下：
(1,3) -> 2
(1,1) -> 0
(3,1) -> 2
因此第 1 个最小距离的数对是 (1,1)，它们之间的距离为 0。
提示:

2 <= len(nums) <= 10000.
0 <= nums[i] < 1000000.
1 <= k <= len(nums) * (len(nums) - 1) / 2.

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/find-k-th-smallest-pair-distance
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

这题我们先排下序，因为要计算两两之间的距离，这本身就是O(N²)的时间复杂度，排完序后，(i, i+1)的距离不可能大于(i, i+2)，所以我们先将(i, i+1)入最小堆，然后依次poll k次，每次poll后，将poll元素索引的距离增大1再插入队列，这样平均时间复杂度有望降低。但是最坏情况时间复杂度仍为O(N²logN)

```java
class Solution {
    public int smallestDistancePair(int[] nums, int k) {
        Arrays.sort(nums);
		PriorityQueue<Node> heap = new PriorityQueue(new Comparator<Node>(){
            @Override
            public int compare(Node n1, Node n2) {
                return n1.dist-n2.dist;
            }
        });
        for (int i=0; i+1<nums.length; i++) {
            heap.offer(new Node(i, i+1, nums[i+1]-nums[i]));
        }
        Node n = null;
        for (; k > 0; k--) {
            n = heap.poll();
            if (n.j+1 < nums.length) {
                heap.offer(new Node(n.i, n.j+1, nums[n.j+1]-nums[n.i]));
            }
        }
        return n.dist;
    }
    
    private static class Node {
        int i;
        int j;
        int dist;
        
        public Node(int i, int j, int dist) {
            this.i = i;
            this.j = j;
            this.dist = dist;
        }
    }
}
```

