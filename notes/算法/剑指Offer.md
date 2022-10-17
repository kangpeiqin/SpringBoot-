[返回首页](../README.md)

## 前缀树
> 前缀树，又称为字典树，它用一个树状的数据结构存储一个字典中的所有单词。
### [实现前缀树](https://leetcode-cn.com/problems/QC3q1f/)
```java
class Trie {

    static class TrieNode{
        private TrieNode[] children;
        private boolean isWord;
        public TrieNode(){
            this.children = new TrieNode[26];
        }
    }
    private TrieNode root;
    /** Initialize your data structure here. */
    public Trie() {
        this.root = new TrieNode();
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        TrieNode p = root;
        for(char c : word.toCharArray()){
            if(p.children[c - 'a'] == null){
                p.children[c - 'a'] = new TrieNode();
            }
            p = p.children[c - 'a'];
        }
        p.isWord = true;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        TrieNode p = root;
        for(char c : word.toCharArray()){
            if(p.children[c - 'a'] == null){
                return false;
            }
            p = p.children[c - 'a'];
        }
        return p.isWord;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        TrieNode p = root;
        for(char c : prefix.toCharArray()){
            if(p.children[c - 'a'] == null){
                return false;
            }
            p = p.children[c - 'a'];
        }
        return true;
    }
}
```
### [替换单词](https://leetcode-cn.com/problems/UhWRSj/)
```java
class Solution {
    static class TrieNode{
        private TrieNode[] children;
        private boolean isWord;
        public TrieNode(){
            this.children = new TrieNode[26];
        }
    }
    //利用字典构建一颗前缀树
    private TrieNode buildTrie(List<String> dict){
        TrieNode root = new TrieNode();
        for(String word : dict){
            TrieNode node = root;
            for(char c : word.toCharArray()){
                if(node.children[c - 'a'] == null){
                    node.children[c - 'a'] = new TrieNode();
                }
                node = node.children[c - 'a'];
            }
            node.isWord = true;
        }
        return root;
    }

    private String findPrefix(TrieNode root,String word){
        TrieNode node = root;
        StringBuilder sb = new StringBuilder();
        for(char c : word.toCharArray()){
            if(node.isWord || node.children[c - 'a'] == null){
                break;
            }
            sb.append(c);
            node = node.children[c - 'a'];
        }
        return node.isWord ? sb.toString() : "";
    }

    public String replaceWords(List<String> dictionary, String sentence) {
        TrieNode root = buildTrie(dictionary);
        StringBuilder sb = new StringBuilder();
        String[] words = sentence.split(" ");
        for(int i = 0; i < words.length; i++){
            String prefix = findPrefix(root,words[i]);
            if(!prefix.isEmpty()){
                words[i] = prefix;
            }
        }
        return String.join(" ",words);
    }
}
```
## 二分查找
### [剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)
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
### [剑指 Offer II 068. 查找插入位置](https://leetcode-cn.com/problems/N6YdxV/)
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
### [剑指 Offer II 070. 排序数组中只出现一次的数字](https://leetcode-cn.com/problems/skFtm2/)
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
## 位运算
### [剑指 Offer 15. 二进制中1的个数](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)
![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cb1548c5a8a74dc6903484948d5be077~tplv-k3u1fbpfcp-watermark.image?)
```java
public class Solution {
    public int hammingWeight(int n) {
        int cnt = 0;
        while(n != 0){
            cnt++;
            n &= (n-1);
        }
        return cnt;
    }
}
```
### [剑指 Offer 56 - I. 数组中数字出现的次数](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)
> 描述：前提：数组 `nums` 里除两个数字之外，其他数字都出现了两次；限制：要求时间复杂度是O(n)，空间复杂度是O(1)。
- 采用位运算
![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b65393ea266a417095e5d52b19d83f7c~tplv-k3u1fbpfcp-watermark.image?)
```java
 class Solution {
    public int[] singleNumbers(int[] nums) {
        int z = 0, x = 0,y = 0,m = 1;
        for(int num : nums){
            z ^= num;
        }
        while((z & m) == 0){
            m <<= 1;
        }
        for(int num : nums){
            //分组运算，相当于将x,y连个数分到不同的组当中
            if((num & m) == 0){
                x ^= num;
            }else{
                y ^= num;
            }
        }
        return new int[]{x,y};
    }
}
```
- 因为时间复杂度和空间复杂度的限制，所以不能用哈希表进行解决，但还是写下解决思路：
```java
 class Solution {
    public int[] singleNumbers(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : nums) {
            int cnt = map.getOrDefault(num, 0);
            map.put(num, ++cnt);
        }
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
           if (map.get(nums[i]).equals(1)) {
              list.add(nums[i]);
           }
       }
       return list.stream().mapToInt(Integer::valueOf).toArray();
    }
}
```
## 其他
### [把字符串转换成整数](https://leetcode-cn.com/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/)
> 使用`long`对溢出情况进行判断
```java
class Solution {
    public int strToInt(String str) {
        String trimStr = str.trim();
        if(trimStr == null || trimStr.length() == 0){
            return 0;
        }
        boolean isNegative = trimStr.charAt(0) == '-';
        long res = 0;
        for(int i=0;i<trimStr.length();i++){
            char c = trimStr.charAt(i);
            //符号判定
            if (i == 0 && (c == '+' || c == '-')){
                continue;
            }
            if(c < '0' || c > '9'){
                break;
            }
            res = res*10 + (c-'0');
            if(res > Integer.MAX_VALUE){
                return isNegative ? Integer.MIN_VALUE:Integer.MAX_VALUE;
            }
        }
        return (int)(isNegative ? -res : res);
    }
}
```
## 分治算法
### X[剑指 Offer 07. 重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)
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
### X[剑指 Offer 16. 数值的整数次方](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)
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
### X[剑指 Offer 33. 二叉搜索树的后序遍历序列](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)
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
## 动态规划
### [爬楼梯的最少成本](https://leetcode-cn.com/problems/GzCJIP/)
> 动态转移方程 f(i) = min(f(i-1),f(i-2)) + cost[i]，可以从 i-1 到达 i,也可以从 i-2 到达 i
```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int[] dp = new int[2];
        dp[0] = cost[0];
        dp[1] = cost[1];
        for(int i = 2; i < cost.length; i++){
            dp[i%2] = Math.min(dp[0],dp[1])+cost[i];
        }
        return Math.min(dp[0],dp[1]);
    }
}
```
### [房屋偷盗](https://leetcode-cn.com/problems/Gu0c2T/)
> 动态转移方程：f(i) = max(f(i-1),f(i-2)+nums[i])
```java
class Solution {
    public int rob(int[] nums) {
        int[] dp = new int[2];
        dp[0] = nums[0];
        if(nums.length >= 2){
            dp[1] = Math.max(nums[0],nums[1]);
        }
        for(int i = 2; i < nums.length; i++){
            dp[i % 2] = Math.max(dp[(i-1)%2],dp[(i-2)%2]+nums[i]);
        }
        return Math.max(dp[0],dp[1]);
    }
}
```
### [环形房屋偷盗](https://leetcode-cn.com/problems/PzWKhm/)
> 从 0 ~ n-2 和 1 ~ n-1 进行计算
```java
class Solution {
    public int rob(int[] nums) {
        if(nums.length == 1){
            return nums[0];
        }
        int len = nums.length;
        return Math.max(rob(nums,0,len-2),rob(nums,1,len-1));
    }
    private int rob(int[] nums,int start,int end){
        int[] dp = new int[2];
        dp[0] = nums[start];
        if(start < end){
            dp[1] = Math.max(nums[start],nums[start + 1]);
        }
        for(int i = start + 2;i <= end; i++){
            int j = i - start;
            dp[j % 2] = Math.max(dp[(j-1)%2],dp[(j-2)%2]+nums[i]);
        }
        return Math.max(dp[0],dp[1]);
    }
}
```
### [粉刷房子](https://leetcode-cn.com/problems/JEj789/)
> 画图，找出动态转移方程
```java
class Solution {
    public int minCost(int[][] costs) {
        int[][] dp = new int[3][costs.length];
        for(int i = 0;i < 3;i++){
            dp[i][0] = costs[0][i];
        }
        for(int i = 1; i < costs.length;i++){
            for(int j = 0; j < 3;j++){
                if(j == 0){
                    dp[j][i] = Math.min(dp[j+1][i-1],dp[j+2][i-1]);
                }else if(j == 1){
                    dp[j][i] = Math.min(dp[j-1][i-1],dp[j+1][i-1]);
                }else{
                    dp[j][i] = Math.min(dp[j-1][i-1],dp[j-2][i-1]);
                }
                dp[j][i] += costs[i][j];
            }
        }
        int len = costs.length - 1;
        return Math.min(dp[0][len],Math.min(dp[1][len],dp[2][len]));
    }
}
```
### X[翻转字符](https://leetcode-cn.com/problems/cyJERH/)
```java
class Solution {
    public int minFlipsMonoIncr(String s) {
        int len = s.length();
        int[][] dp = new int[2][2];
        char ch = s.charAt(0);
        dp[0][0] = ch == '0' ? 0 : 1;
        dp[1][0] = ch == '1' ? 0 : 1;
        for(int i = 1; i < len; i++){
            ch = s.charAt(i);
            int prev0 = dp[0][(i-1)%2];
            int prev1 = dp[1][(i-1)%2];
            dp[0][i%2] = prev0 + (ch == '0' ? 0 : 1);
            dp[1][i%2] = Math.min(prev0, prev1)+(ch == '1' ? 0 : 1);
        }
        return Math.min(dp[0][(len-1)%2],dp[1][(len-1)%2]);
    }
}
```
## 双指针
### [剑指 Offer 18. 删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)
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
### [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)
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
### [剑指 Offer 52. 两个链表的第一个公共节点](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)
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
### [剑指 Offer 25. 合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)
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
### [剑指 Offer 58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)
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
### [剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)
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
## 回溯法
### [剑指 Offer II 079. 所有子集](https://leetcode-cn.com/problems/TVdhkn/)
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
### [允许重复选择元素的组合](https://leetcode-cn.com/problems/Ygoe9J/)
> 画出决策树
```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> res = new ArrayList<>();
        backtrack(res,new LinkedList<>(),candidates,target,0);
        return res;
    }

    private void backtrack(List<List<Integer>> res,LinkedList<Integer> path,int[] candidates,int target,int index){
        if(target == 0){
            res.add(new LinkedList(path));
            return;
        }
        if(target < 0){
            return;
        }
        for(int i = index; i < candidates.length; i++){
            path.add(candidates[i]);
            backtrack(res,path,candidates,target-candidates[i],i);
            //回溯
            path.removeLast();
        }

    }
}
```
## 排序
### [剑指 Offer 45. 把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)
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
### [剑指 Offer 61. 扑克牌中的顺子](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)
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
### [剑指 Offer II 074. 合并区间](https://leetcode-cn.com/problems/SsGoHC/)
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
## 搜索与回溯算法
### [剑指 Offer 32 - I. 从上到下打印二叉树](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)
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
### [剑指 Offer 12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)
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
### [剑指 Offer 13. 机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)
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
### [二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)
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
### X[剑指 Offer 36. 二叉搜索树与双向链表](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)
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
### [剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)
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
## 整数
### [剑指 Offer II 002. 二进制加法](https://leetcode-cn.com/problems/JFETK5/)
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
### [前 n 个数字二进制中 1 的个数](https://leetcode-cn.com/problems/w3tCBm/)
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
### [剑指 Offer II 004. 只出现一次的数字](https://leetcode-cn.com/problems/WGki4K/)
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
### [排序数组中两个数字之和](https://leetcode-cn.com/problems/kLl5u1/)
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
## 查找算法
### [二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)
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
### [剑指 Offer 03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)
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
## 栈与队列
### [剑指 Offer 09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)
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
### [剑指 Offer 30. 包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)
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
## 树
### [剑指 Offer 68 - I. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/)
> 根据二叉搜索树的特点：公共祖先 root 满足`root.val >= p.val && root.val <= q.val`
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null){
            return null;
        }
        //往左子树进行递归搜索
        if(root.val > p.val && root.val > q.val){
           return lowestCommonAncestor(root.left,p,q);
        }
        //往右子树进行递归搜索
        if(root.val < p.val && root.val < q.val){
           return lowestCommonAncestor(root.right,p,q);
        }
        // 如果 p.val <= root.val <= q.val，则直接返回结果 root
        return root;
    }
}
```
## 链表
### [删除链表的倒数第 n 个结点](https://leetcode-cn.com/problems/SLwz0R/)
> 采用快慢指针和虚拟头结点
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0),p,q;
        dummy.next = head;
        p = q = dummy;
        //先走 n 个结点
        while(n-- > 0){
            q = q.next;
        }
        while(q.next != null){
            p = p.next;
            q = q.next;
        }
        p.next = p.next.next;
        return dummy.next;
    }
}
```
### * [链表中环的入口节点](https://leetcode-cn.com/problems/c32eOV/)
> 利用快慢指针求出链表中环节点的数目，而后利用双指针求解
```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode node = getNode(head);
        if(node == null){
            return null;
        }
        ListNode p = head;
        while(node != p){
            node = node.next;
            p = p.next;
        }
        return node;
    }

    private ListNode getNode(ListNode head){
        if(head == null || head.next == null){
            return null;
        }
        ListNode slow = head.next,fast = slow.next;
        while(fast != null && slow != null){
            if(fast == slow){
                return slow;
            }
            slow = slow.next;
            fast = fast.next;
            if(fast != null){
                fast = fast.next;
            }
        }
        return null;
    }
}
```
### [两个链表的第一个重合节点](https://leetcode-cn.com/problems/3u1WK4/)
> 双指针，交叉遍历
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode p = headA,q = headB;
        while(p != null || q != null){
            if(p == q){
                return p;
            }
            p = p == null ? headB:p.next;
            q = q == null ? headA:q.next;
        }
        return null;
    }
}
```
### [反转链表](https://leetcode-cn.com/problems/UHnkqh/)
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode dummy = new ListNode(0),p = head.next,q;
        head.next = null;
        dummy.next = head;
        while(p != null){
           //保存前置结点
           q = p;
           p = p.next;
           q.next = dummy.next;
           dummy.next = q;
        }
        return dummy.next;
    }
}
```
### [链表中的两数相加](https://leetcode-cn.com/problems/lMSNwu/)
> 链表反转+结点相加
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        l1 = reverse(l1);
        l2 = reverse(l2);
        if(l1 == null){
            return l2;
        }
        if(l2 == null){
            return l1;
        }
        int carry = 0;
        ListNode dummy = new ListNode();
        ListNode p = dummy;
        while(l1 != null && l2 != null){
            int sum = l1.val + l2.val + carry;
            int val = sum % 10;
            ListNode node = new ListNode(val);
            p.next = node;
            carry = sum/10;
            l1 = l1.next;
            l2 = l2.next;
            p = p.next;
        }
        while(l1 != null){
            int sum = l1.val + carry;
            p.next = new ListNode(sum%10);
            carry = sum/10;
            l1 = l1.next;
            p = p.next;
        }
        while(l2 != null){
            int sum = l2.val + carry;
            p.next = new ListNode(sum%10);
            carry = sum/10;
            l2 = l2.next;
            p = p.next;
        }
        if(carry != 0){
            p.next = new ListNode(carry);
        }
        return reverse(dummy.next);
    }

    private ListNode reverse(ListNode head){
        if(head == null || head.next == null){
            return head;
        }
        ListNode dummy = new ListNode(0),p = head.next,q;
        head.next = null;
        dummy.next = head;
        while(p != null){
           //保存前置结点
           q = p;
           p = p.next;
           q.next = dummy.next;
           dummy.next = q;
        }
        return dummy.next;
    }
}
```
### [回文链表](https://leetcode-cn.com/problems/aMhZSa/)
> 链表反转 + 结点判断，需要注意引用关系，空间复杂度`O(N)`
```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        ListNode node = reverse(head);
        while(node != null){
            if(node.val != head.val){
                return false;
            }
            node = node.next;
            head = head.next;
        }
        return true;
    }
    private ListNode reverse(ListNode head){
        if(head == null || head.next == null){
            return head;
        }
        ListNode dummy = new ListNode(0),p = head.next,node = new ListNode(head.val);
        node.next = null;
        dummy.next = node;
        while(p != null){
            ListNode q = new ListNode(p.val);
            q.next = dummy.next;
            dummy.next = q;
            p = p.next;
        }
        return dummy.next;
    }
}
```
> 快慢指针+后半段链表反转 
```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        if(head == null || head.next == null){
            return true;
        }
        ListNode dummy = new ListNode(0),fast,slow;
        dummy.next = head;
        fast = slow = dummy;
        while(fast != null){
            slow = slow.next;
            fast = fast.next;
            if(fast != null){
                fast = fast.next;
            }
        }
        System.out.print(slow.val);
        return isPalindrome(head,reverse(slow));
    }

    private boolean isPalindrome(ListNode head,ListNode rev){
        while(rev != null){
            if(rev.val != head.val){
                return false;
            }
            rev = rev.next;
            head = head.next;
        }
        return true;
    }

    private ListNode reverse(ListNode head){
        if(head == null || head.next == null){
            return head;
        }
        ListNode dummy = new ListNode(0),p = head.next,q;
        head.next = null;
        dummy.next = head;
        while(p != null){
           //保存前置结点
           q = p;
           p = p.next;
           q.next = dummy.next;
           dummy.next = q;
        }
        return dummy.next;
    }
}
```
### X[重排链表](https://leetcode-cn.com/problems/LGjMqU/)
> 链表后半段进行反转+两链表重新组合
```java
class Solution {
    public void reorderList(ListNode head) {
        if(head == null || head.next == null){
            return;
        }
        ListNode dummy = new ListNode(0),fast,slow;
        dummy.next = head;
        fast = slow = dummy;
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next;
            if(fast.next != null){
                fast = fast.next;
            }
        }
        ListNode temp = slow.next;
        slow.next = null;
        reorderList(head,reverse(temp),dummy);
    }
    //这里需要注意：两个链表如何进行重新组合
    private void reorderList(ListNode head,ListNode rev,ListNode dummy){
        ListNode prev = dummy;
        while(rev != null){
            ListNode temp = head.next;
            prev.next = head;
            head.next = rev;
            prev = rev;
            head = temp;
            rev = rev.next;
        }
        if(head != null){
            prev.next = head;
        }
    }

    private ListNode reverse(ListNode head){
        if(head == null || head.next == null){
            return head;
        }
        ListNode dummy = new ListNode(0),p = head.next,q;
        head.next = null;
        dummy.next = head;
        while(p != null){
            q = p;
            p = p.next;
            q.next = dummy.next;
            dummy.next = q;
        }
        return dummy.next;
    }
}
```
### [剑指 Offer 06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)
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








