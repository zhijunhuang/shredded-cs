## 402.移除K位数字

给定一个以字符串表示的非负整数 num，移除这个数中的 k 位数字，使得剩下的数字最小。

注意:

num 的长度小于 10002 且 ≥ k。
num 不会包含任何前导零。
示例 1 :

输入: num = "1432219", k = 3
输出: "1219"
解释: 移除掉三个数字 4, 3, 和 2 形成一个新的最小的数字 1219。
示例 2 :

输入: num = "10200", k = 1
输出: "200"
解释: 移掉首位的 1 剩下的数字为 200. 注意输出不能有任何前导零。
示例 3 :

输入: num = "10", k = 2
输出: "0"
解释: 从原数字移除所有的数字，剩余为空就是0。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/remove-k-digits
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

可以使用栈来操作，每次与栈顶元素比较，如果比栈顶大，则当前元素入栈；如果栈顶大，则舍弃栈顶元素; 这样得到递增堆栈（栈底到栈顶递增关系）；如果删除位数不够就不停的出栈直到删除够了。

这题的细节比较多，比如

==1、前导0的处理==

==2、最后为空串的时候得返回字符串0==

==3、考虑整型溢出，得用字符串来拼接结果==

==4、删除计数的时候得检查是否删够了==

```java
class Solution {
    public String removeKdigits(String num, int k) {
        if (k >= num.length()) {
            return "0";
        }
        //so k must be less than num.length()
        Stack<Integer> stack = new Stack();
        char c = num.charAt(0);
        int n = (int)(c - '0');
        stack.push(n);

        int i, m; //i用来遍历，m用来记录删除数
        for (i=1,m=0; m<k && i<num.length(); i++) {
            c = num.charAt(i);
            n = (int)(c - '0');
            while (!stack.empty() && n < stack.peek() && m<k) { //如果之前的数比较大就出栈删除
                stack.pop();
                m++;
            }
            stack.push(n);
        }
        String left = "";
        if (m == k) { //已经删除了k个,那就把剩余元素截取
            left = num.substring(i);
        } else { //没删够，这个时候肯定栈顶最大，依次删栈顶
            assert(stack.size() >= (k-m));
            while(!stack.empty() && m < k) {
                stack.pop();
                m++;
            }
        }
        //规整一下格式，考虑到溢出问题，不能用数字来还原字符串，得用字符串拼接的方式
        StringBuilder sb = new StringBuilder();
        Stack<Integer> reverseStack = new Stack();
        while (!stack.empty()) { //先倒转，方便处理前导0
            reverseStack.push(stack.pop());
        }
        while (!reverseStack.empty()) {
            n = reverseStack.pop();
            if (n == 0 && sb.length() == 0) { //删除前导0
                continue;
            }
            sb.append(n);
        }
        String result = sb.append(left).toString();
        return result.length() == 0 ? "0" : result;
    }
}
```

