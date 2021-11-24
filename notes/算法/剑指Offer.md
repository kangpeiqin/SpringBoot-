<p> <a href="../数据结构&算法.md">返回</a></p>

<div>
  <li><a href="./type/前缀树.md">前缀树</a> </li>
  <li><a href="./type/动态规划.md">动态规划</a> </li>
  <li><a href="./type/链表.md">链表</a> </li>
</div>

## [整数](#整数)
- [二进制加法](#二进制加法)
- [前n个数字二进制中1的个数](#前n个数字二进制中1的个数)
- [只出现一次的数字](#只出现一次的数字)
- [排序数组中两个数字之和](#排序数组中两个数字之和)
## [二分查找](#二分查找)
- [在排序数组中查找数字I](#在排序数组中查找数字I)
- [查找插入位置](#查找插入位置)
- [排序数组中只出现一次的数字](#排序数组中只出现一次的数字)
## [栈与队列](#栈与队列) 
- [用两个栈实现队列](#用两个栈实现队列) 
- [包含min函数的栈](#包含min函数的栈)
## [链表](#链表)
- [从尾到头打印链表](#从尾到头打印链表)
- [反转链表](#反转链表)
## [查找算法](#查找算法)
- [二维数组中的查找](#二维数组中的查找)
- [数组中重复的数字](#数组中重复的数字)
## [双指针](#双指针)
- [删除链表的节点](#删除链表的节点)
- [链表中倒数第k个节点](#链表中倒数第k个节点)
- [合并两个排序的链表](#合并两个排序的链表)
- [两个链表的第一个公共节点](#两个链表的第一个公共节点)
- [翻转单词顺序](#翻转单词顺序)
- [调整数组顺序使奇数位于偶数前面](#调整数组顺序使奇数位于偶数前面)
## [搜索与回溯算法](#搜索与回溯算法)
- [从上到下打印二叉树](#从上到下打印二叉树)
- [矩阵中的路径](#矩阵中的路径)
- [机器人的运动范围](#机器人的运动范围)
- [二叉树中和为某一值的路径](#二叉树中和为某一值的路径)
- [二叉搜索树与双向链表](#二叉搜索树与双向链表)
- [二叉搜索树的第k大节点](#二叉搜索树的第k大节点)
## [排序](#排序)
- [把数组排成最小的数](#把数组排成最小的数) 
- [扑克牌中的顺子](#扑克牌中的顺子)
- [合并区间](#合并区间)
## [分治算法](#分治算法)
- [重建二叉树](#重建二叉树)
- [数值的整数次方](#数值的整数次方)
- [二叉搜索树的后序遍历序列](#二叉搜索树的后序遍历序列)
## [回溯法](#回溯法)
- [所有子集](#所有子集)
## [图算法](#图算法)
- [二分图](#二分图)
- [所有路径](#所有路径)
- [课程顺序](#课程顺序)

## 整数
### 二进制加法
[剑指 Offer II 002. 二进制加法](https://leetcode-cn.com/problems/JFETK5/)
```java
class Solution {
    public String addBinary(String a, String b) {
        StringBuilder result = new StringBuilder();
        int i = a.length() - 1,j = b.length() - 1;
        int carry = 0;
        while(i >= 0 || j >= 0){
            int digitA = i >= 0 ? a.charAt(i--) - '0' : 0;
            int digitB = j >= 0 ? b.charAt(j--) - '0' : 0;
            int sum = digitA + digitB + carry;
            carry = sum >= 2 ? 1:0;
            sum = sum >= 2 ? sum - 2 : sum;
            result.append(sum); 
        }
        if(carry == 1){
            result.append(1);
        }
        return result.reverse().toString();
    }
}
```
### 前n个数字二进制中1的个数
[前 n 个数字二进制中 1 的个数](https://leetcode-cn.com/problems/w3tCBm/)
> 采用位运算：i & (i-1) 将整数最右边的 1 变成 0
```java
class Solution {
    public int[] countBits(int n) {
        int[] ans = new int[n+1];
        for(int i = 0;i <= n;i++){
            int j = i;
            while(j != 0){
                ans[i]++;
                //将整数最右边的 1 变成 0
                j = j & (j-1);
            }
        }
        return ans;
    }
}
```
> 简化算法
```java
class Solution {
    public int[] countBits(int n) {
        int[] ans = new int[n+1];
        for(int i = 1;i <= n;i++){
           ans[i] = ans[i & (i-1)] + 1;
        }
        return ans;
    }
}
```
### 只出现一次的数字 
[剑指 Offer II 004. 只出现一次的数字](https://leetcode-cn.com/problems/WGki4K/)
```java
class Solution {
    public int singleNumber(int[] nums) {
        Map<Integer,Integer> map = new HashMap<>();
        for(int num:nums){
            map.put(num,map.getOrDefault(num,0)+1);
        }
        int ans = 0;
        for(Map.Entry<Integer,Integer> entry:map.entrySet()){
            int key = entry.getKey(),val = entry.getValue();
            if(val == 1){
                ans = key;
                break;
            }
        }
        return ans;
    }
}
```
### 排序数组中两个数字之和
[排序数组中两个数字之和](https://leetcode-cn.com/problems/kLl5u1/)
> 双指针
```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int i = 0, j = numbers.length - 1;
        while(i < j){
            int sum = numbers[i] + numbers[j];
            if(sum==target){
                return new int[]{i,j};
            }else if(sum<target){
                i++;
            }else{
                j--;
            }
        }
        return new int[0];
    }
}
```
## 二分查找
### 在排序数组中查找数字I
[剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)
> 题目描述：统计一个数字在排序数组中出现的次数。
```java
class Solution {
    public int search(int[] nums, int target) {
        int left = binarySearch(nums,target);
        int right = binarySearch(nums,target + 1);
        return right - left;
    }
    private int binarySearch(int[] nums,int target){
        int low = 0, high = nums.length;
        while(low < high){
            int mid = low + (high - low)/2;
            if(nums[mid] >= target){
                high = mid;
            }else{
                low = mid + 1;
            }
        }
        return low;
    }
}
```
### 查找插入位置
[剑指 Offer II 068. 查找插入位置](https://leetcode-cn.com/problems/N6YdxV/)
> 利用二分查找在有序数组中查找第一个大于等于`target`的数值下标
```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int low = 0,high = nums.length;
        while(low < high){
            int mid = low + (high - low)/2;
            if(nums[mid] >= target){
                high = mid;
            }else{
                low = mid + 1;
            }
        }
        return low;
    }
}
```
### 排序数组中只出现一次的数字
[剑指 Offer II 070. 排序数组中只出现一次的数字](https://leetcode-cn.com/problems/skFtm2/)
```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        if(nums.length == 1){
            return nums[0];
        }
        for(int i = 1;i < nums.length-1;i+=2){
            if(nums[i] != nums[i-1]){
                return nums[i-1];
            }
        }
        return nums[nums.length - 1];
    }
}
```
## 栈与队列
### 用两个栈实现队列
[剑指 Offer 09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)
> 采用两个栈：数据栈和操作栈
```java
class CQueue {
    private Stack<Integer> data,op;
    public CQueue() {
        //初始化栈时需要注意不能写成data=op这种形式，不然就指向同一个对象了
        data = new Stack<>();
        op = new Stack<>();
    }
    
    public void appendTail(int value) {
        data.add(value);
    }
    
    public int deleteHead() {
        if(!op.isEmpty()){
            return op.pop();
        }else{
            while(!data.isEmpty()){
                op.add(data.pop());
            }
            return !op.isEmpty()?op.pop():-1;
        }
       
    }
}
```
### 包含min函数的栈
[剑指 Offer 30. 包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)
> 采用两个栈：数据栈和最小栈
```java
class MinStack {
   private Stack<Integer> data,min;
    /** initialize your data structure here. */
    public MinStack() {
        data = new Stack<>();
        min = new Stack<>();
    }
    
    public void push(int x) {
        data.add(x);
        if(!min.isEmpty()){
            //判断条件需要注意！
            if(min.peek() >= x){
                min.add(x);
            }
        }else{
            min.add(x);
        }
    }
    
    public void pop() {
       int x = data.pop();
       if(x == min.peek()){
           min.pop();
       }
    }
    
    public int top() {
        return data.peek();
    }
    
    public int min() {
        return min.peek();
    }
}

```
## 链表
### 从尾到头打印链表
[剑指 Offer 06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)
```java
class Solution {
    public int[] reversePrint(ListNode head) {
        if(head==null){
            return new int[]{};
        }
        List<Integer> list = reverse(head);
        int len = list.size();
        int[] res = new int[len];
        int j = 0;
        for(int i = len-1;i >= 0;i--){
            res[j++] = list.get(i);
        }
        return res;
    }

    private List<Integer> reverse(ListNode head){
        List<Integer> list = new ArrayList<>();
        while(head != null){
            list.add(head.val);
            head = head.next;
        }
        return list;
    }

}
```
### 反转链表
[剑指 Offer 24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)
> 采用头插法进行求解
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode dummy = new ListNode(0),p = head.next;
        head.next = null;
        dummy.next = head;
        while(p != null){
            ListNode node = new ListNode(p.val);
            node.next = dummy.next;
            dummy.next = node;
            p = p.next;
        }
        return dummy.next;
    }
}
```

## 查找算法
### 二维数组中的查找
[二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

```java
//缩小查找范围
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        if(matrix == null || matrix.length == 0){
            return false;
        }
        int row = matrix.length,col = matrix[0].length;
        int i = 0,j = col-1;
        while(i < row && j >= 0){
            if(matrix[i][j] == target){
                return true;
            }else if(matrix[i][j] > target){
                j--;
            }else{
                i++;
            }
        }
        return false;
    }
}
```
### 数组中重复的数字
[剑指 Offer 03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)
```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for(int num : nums){
            if(set.contains(num)){
                return num;
            }
            set.add(num);
        }
        return -1;
    }
}
```
## 双指针
### 删除链表的节点
[剑指 Offer 18. 删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)
> 问题描述：删除给定结点，返回头结点
```java
class Solution {
    public ListNode deleteNode(ListNode head, int val) {
        //判断头结点是否为空
        if(head == null){
            return null;
        }
        if(head.val == val){
            return head.next;
        }
        ListNode p = head,q = head.next;
        while(q!=null){
            if(q.val == val){
                p.next = q.next;
                break;
            }
            p = p.next;
            q = q.next;
        }
        return head;
    }
}
```
### 链表中倒数第k个节点
[剑指 Offer 22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)
> 解法：采用双指针，让一个指针先走k个结点，之后两个指针同步走
```java
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        //边界情况判断
        if(head == null){
            return null;
        }
        ListNode p = head, q = head;
        while(k-- > 0){
            q = q.next;
        }
        while(q != null){
            p = p.next;
            q = q.next;
        }
        return p;
    }
}
```
### 两个链表的第一个公共节点
[剑指 Offer 52. 两个链表的第一个公共节点](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)
> 交叉遍历：假设 la = a+c; lb = b+c,那么可得a+c+b=b+c+a(交叉遍历)
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA == null || headB == null){
            return null;
        }
        ListNode p = headA,q = headB;
        while(p != q){
            /*
            * 之前的错误写法：会出现两个指针没有同时进行遍历的情况
            *      while(p != q){
            *              if(p.next == null){
            *                  p.next = headB;
            *              }
            *              if(q.next == null){
            *                  q.next = headA;
            *              }
            *              //同时到达某尾结点，就符合p==q这样的条件
            *              if(p.next == null && q.next == null){
            *                  return null;
            *              }
            *              p = p.next;
            *              q = q.next;
            *        }
            * */
            p = p == null ? headB : p.next;
            q = q == null ? headA : q.next;
        }
        return p;
    }
}
```
### 合并两个排序的链表
[剑指 Offer 25. 合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)
```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
       if(l1 == null){
           return l2;
       }
       if(l2 == null){
           return l1;
       }
       ListNode dummy = new ListNode(0);
       ListNode p = dummy;
       while(l1 != null && l2 != null){
           if(l1.val <= l2.val){
               p.next = l1;
               l1 = l1.next;
           }else{
               p.next = l2;
               l2 = l2.next;
           }
           p = p.next;
       }
       if(l1 != null){
           p.next = l1;
       }
       if(l2 != null){
           p.next = l2;
       }
       return dummy.next;
    }
}
```
### 翻转单词顺序
[剑指 Offer 58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)
```java
class Solution {
    public String reverseWords(String s) {
        if(s == null || s.length() == 0){
            return "";
        }
        String str = s.trim();
        String[] arr = s.split(" ");
        StringBuilder sb = new StringBuilder();
        for(int i = arr.length-1;i >= 0;i--){
             //做的时候没有考虑到出现空串的情况，中间可能包含空的字符串，需要过滤
            if("".equals(arr[i])){
                continue;
            }
            sb.append(arr[i]+" ");
        }
        return sb.toString().trim();
    }
}
```
### 调整数组顺序使奇数位于偶数前面
[剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)
```java
class Solution {
     public int[] exchange(int[] nums) {
            int start = 0, end = nums.length - 1;
            while (start < end) {
                if (nums[start] % 2 == 0 && nums[end] % 2 == 1) {
                    swap(nums, start, end);
                    start++;
                    end--;
                }
                while (nums[start] % 2 == 1 && start < end) {
                    start++;
                }
                while (nums[end] % 2 == 0 && start < end) {
                    end--;
                }
            }
            return nums;
        }

        private int[] swap(int[] nums, int start, int end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            return nums;
        }
}
```    
## 搜索与回溯算法
### 从上到下打印二叉树
[剑指 Offer 32 - I. 从上到下打印二叉树](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)
```java
class Solution {
  public int[] levelOrder(TreeNode root) {
    if (root == null) {
      return new int[]{};
    }
    List<Integer> list = new ArrayList<>();
    Queue<TreeNode> queue = new LinkedList<>();
    queue.add(root);
    while (!queue.isEmpty()) {
      TreeNode node = queue.poll();
      list.add(node.val);
      if (node.left != null) {
        queue.add(node.left);
      }
      if (node.right != null) {
        queue.add(node.right);
      }
    }
    //List转成int数组
    return list.stream().mapToInt(Integer::valueOf).toArray();
  }
}
```
### 矩阵中的路径
- [剑指 Offer 12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)
```java
class Solution {
    public boolean exist(char[][] board, String word) {
        char[] words = word.toCharArray();
        for(int i=0;i<board.length;i++){
            for(int j=0;j<board[0].length;j++){
                //从当前位置进行深度优先遍历
                if(dfs(board,words,i,j,0)){
                    return true;
                }
            }
        }
        return false;
    }
    private boolean dfs(char[][] board,char[] word,int i,int j,int k){
        //边界条件
        if(i >= board.length || i < 0 || j >= board[0].length || j < 0 || board[i][j] != word[k]){
            return false;
        }
        //找到最后一个字符，返回true
        if(k == word.length-1){
            return true;
        }
        //标记被访问
        board[i][j]='\0';
        boolean res = dfs(board,word,i+1,j,k+1) || dfs(board,word,i-1,j,k+1) || 
        dfs(board,word,i,j+1,k+1) || dfs(board,word,i,j-1,k+1);
        //回溯
        board[i][j] = word[k];
        return res;
    }
}
```
### 机器人的运动范围
- [剑指 Offer 13. 机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)
```java
class Solution {
    public int movingCount(int m, int n, int k) {
        boolean[][] visited = new boolean[m][n];
        return dfs(0, 0, m, n, k, visited);
    }

    private int dfs(int i, int j, int m, int n, int k, boolean[][] visited) {
        if (i >= m || j >= n || (sum(i) + sum(j) > k || visited[i][j])) {
            return 0;
        }
        //标记访问
        visited[i][j] = true;
        //递归进行求解
        return 1 + dfs(i + 1, j, m, n, k, visited) + dfs(i, j + 1, m, n, k, visited);
    }
    private int sum(int num) {
        int sum = 0;
        while (num > 0) {
            sum += num % 10;
            num /= 10;
        }
        return sum;
    }
}
```
### 二叉树中和为某一值的路径
- [二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)
```java
class Solution {
    List<List<Integer>> result = new LinkedList<>();

    public List<List<Integer>> pathSum(TreeNode root, int target) {
        LinkedList<Integer> list = new LinkedList<>();
        dfs(root,target,list);
        return result;
    }

    public void dfs(TreeNode root, int target, LinkedList<Integer> list) {
        if (root == null) {
            return;
        }
        //队尾加入
        list.offerLast(root.val);
        if (root.left == null && root.right == null && target == root.val) {
            result.add(new ArrayList<>(list));
        }
        dfs(root.left, target - root.val, list);
        dfs(root.right, target - root.val, list);
        list.removeLast();
    }
}
```
### 二叉搜索树与双向链表
* [剑指 Offer 36. 二叉搜索树与双向链表](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)
> 采用中序遍历，在遍历过程中连接各个结点
```java
class Solution {
    private Node head, pre;
    public Node treeToDoublyList(Node root) {
        if(root == null){
            return null;
        }
        inorder(root);
        head.left = pre;
        pre.right = head;
        return head;
    }

    private void inorder(Node root){
        if(root == null){
            return;
        }
        inorder(root.left);
        if(pre != null){
            pre.right = root;
        }else{
            head = root;
        }
        root.left = pre;
        pre = root;
        inorder(root.right);
    }
}
```
### 二叉搜索树的第k大节点
- [剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)
```java
class Solution {
    private int result,k;
    public int kthLargest(TreeNode root, int k) {
        this.k = k;
        postOrder(root);
        return result;
    }

    private void postOrder(TreeNode root){
        if(root == null){
            return;
        }
        postOrder(root.right);
        k--;
        if(k == 0){
            result = root.val;
        }
        postOrder(root.left);
    } 
}
```
## 排序
### 把数组排成最小的数
- [剑指 Offer 45. 把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)
> 由于数组中的数据都是非负整数，定义字符串排序规则：拼接字符串 x + y > y + x ,x “大于” y,如[3][30]>[30][3] 
```java
class Solution {
    public String minNumber(int[] nums) {
        String[] arr = new String[nums.length];
        for(int i = 0;i < nums.length;i++){
            arr[i] = String.valueOf(nums[i]);
        }
        Arrays.sort(arr,(x,y)->(x+y).compareTo(y+x));
        StringBuilder sb = new StringBuilder();
        for(int i=0;i<arr.length;i++){
            sb.append(arr[i]);
        }
        return sb.toString();
    }
}
```
### 扑克牌中的顺子
- [剑指 Offer 61. 扑克牌中的顺子](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)
> 从不同的角度进行考虑：除大小王外，所有牌无重复;除大小王必须要满足 max−min < 5
```java
class Solution {
    public boolean isStraight(int[] nums) {
        Arrays.sort(nums);
        int joker = 0;
        for(int i = 0;i < 4;i++){
            if(nums[i] == 0){
                joker++;
            }else{
                if(nums[i] == nums[i+1]){
                    return false;
                }
            }
        }
        return nums[4] - nums[joker] < 5;
    }
}
```
### 合并区间
[剑指 Offer II 074. 合并区间](https://leetcode-cn.com/problems/SsGoHC/)
```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if (intervals == null || intervals.length == 0) {
            return intervals;
        }
        Arrays.sort(intervals, Comparator.comparing(o -> o[0]));
        List<int[]> res = new ArrayList<>();
        res.add(intervals[0]);
        for (int i = 1; i < intervals.length; i++) {
            int[] element = res.get(res.size() - 1);
            if (element[1] >= intervals[i][0]) {
                element[1] = Math.max(intervals[i][1], element[1]);
            } else {
                res.add(intervals[i]);
            }
        }
        return res.toArray(new int[res.size()][2]);
    }
}
```
## 分治算法
### 重建二叉树
* [剑指 Offer 07. 重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)
> 分治算法，递归求解
```java
class Solution {
    private Map<Integer,Integer> map = new HashMap<>();
    private int[] preorder;
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        this.preorder = preorder;
        for(int i = 0;i < inorder.length;i++){
            map.put(inorder[i],i);
        }
        TreeNode root = buildTree(0,0,preorder.length-1);
        return root;
    }
    private TreeNode buildTree(int root,int left,int right){
        if(left > right){
            return null;
        }
        TreeNode node = new TreeNode(preorder[root]);
        //求出左子树的个数
        int index = map.get(preorder[root]);
        //左子树的范围left-index-1
        node.left = buildTree(root+1,left,index-1);
        //右子树的范围
        node.right = buildTree(root+index-left+1,index+1,right);
        return node;
    }
}
```
### 数值的整数次方
* [剑指 Offer 16. 数值的整数次方](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)
> 采用快速幂思想
```java
class Solution {
    public double myPow(double x, int n) {
        if(x == 0){
            return 0;
        }
        long b = n;
        double res = 1.0;
        if(b < 0){
            x = 1/x;
            b = -b;
        }
        while(b > 0){
            if((b&1) == 1){
                res *= x;
            }
            x*=x;
            b >>= 1;
        }
        return res;
    }
}
```
### 二叉搜索树的后序遍历序列
* [剑指 Offer 33. 二叉搜索树的后序遍历序列](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)
> 根据二叉搜索树和后序遍历的特点进行求解
```java
class Solution {
    public boolean verifyPostorder(int[] postorder) {
        return recur(postorder,0,postorder.length-1);
    }

    private boolean recur(int[] postorder,int i,int j){
        if(i >= j){
            return true;
        }
        int p = i;
        while(postorder[p] < postorder[j]){
            p++;
        }
        int m = p;
        while(postorder[p] > postorder[j]){
            p++;
        }
        return p==j && recur(postorder,i,m-1)&&recur(postorder,m,j-1);
    }
}
```
## 回溯法
### 所有子集
[剑指 Offer II 079. 所有子集](https://leetcode-cn.com/problems/TVdhkn/)
```java
class Solution {
   public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        if (nums.length == 0) {
            return res;
        }
        Deque<Integer> subset = new LinkedList<>();
        backtracking(nums, 0, res, subset);
        return res;
    }

    private void backtracking(int[] nums, int deep, List<List<Integer>> res, Deque<Integer> subset) {
        //遍历到根结点，加入结果集合，到达指定的深度
        if (deep == nums.length) {
            res.add(new ArrayList<>(subset));
            return;
        }
        //由于不打算将该数字添加到子集中，因此不对子集进行任何操作，
        // 只需要调用递归函数backtracking处理数组nums中的下一个数字（下标增加1）就可以。
        backtracking(nums, deep + 1, res, subset);
        //考虑将下标为index的数字添加到子集subset的情形
        subset.add(nums[deep]);
        //接下来调用递归函数处理数组nums中的下一个数字（下标增加1）
        backtracking(nums, deep + 1, res, subset);
        //在回溯到父节点之前，应该清除已经对子集状态进行的修改。
        subset.removeLast();
    }
}
```
## 图算法
### 二分图
[剑指 Offer II 106. 二分图](https://leetcode-cn.com/problems/vEAB3K/)
> 题目描述：
```java

```
### 所有路径
[剑指 Offer II 110. 所有路径](https://leetcode-cn.com/problems/bP4bmD/)
> 题目描述：
```java

```
### 课程顺序
[剑指 Offer II 113. 课程顺序](https://leetcode-cn.com/problems/QA2IGt/)
```java

```