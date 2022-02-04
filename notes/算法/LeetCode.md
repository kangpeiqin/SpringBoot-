<p> <a href="../数据结构&算法.md">返回</a></p>

- [两数之和](#两数之和)
- [两数相加](#两数相加)
- [无重复字符的最长子串](#无重复字符的最长子串)
- [寻找两个正序数组的中位数](#寻找两个正序数组的中位数)
- [最长回文子串](#最长回文子串)
- [Z字形变换](#Z字形变换)
- [整数反转](#整数反转)
- [字符串转换整数](#字符串转换整数)
- [回文数](#回文数)
- [盛最多水的容器](#盛最多水的容器)
- [整数转罗马数字](#整数转罗马数字)
- [删除链表的倒数第N个结点](#删除链表的倒数第N个结点)
- [有效的括号](#有效的括号)
- [合并两个有序链表](#合并两个有序链表)
- [括号生成](#括号生成)
- [两两交换链表中的节点](#两两交换链表中的节点)
- [合并K个升序链表](#合并K个升序链表)
- [K个一组翻转链表](#K个一组翻转链表)
- [删除有序数组中的重复项](#删除有序数组中的重复项)
- [移除元素](#移除元素)
- [最长有效括号](#最长有效括号)
- [下一个排列](#下一个排列)
- [搜索旋转排序数组](#搜索旋转排序数组)
- [在排序数组中查找元素的第一个和最后一个位置](#在排序数组中查找元素的第一个和最后一个位置)
- [搜索插入位置](#搜索插入位置)
- [全排列](#全排列)
- [子集](#子集)
- [汉明距离](#汉明距离)
- [只出现一次的数字](#只出现一次的数字)
- [丢失的数字](#丢失的数字)
- [只出现一次的数字III](#只出现一次的数字III)
## [链表](#链表)
- [奇偶链表](#奇偶链表)
- [分隔链表](#分隔链表)
- [回文链表](#回文链表)
- [链表反转](#链表反转)
- [相交链表](#相交链表)
- [两数相加II](#两数相加II)
## [双指针](#双指针)
- [两数之和II](#两数之和II)
- [平方数之和](#平方数之和)
- [反转字符串中的元音字母](#反转字符串中的元音字母)
- [验证回文字符串Ⅱ](#验证回文字符串Ⅱ)
- [通过删除字母匹配到字典里最长单词](#通过删除字母匹配到字典里最长单词)
- [合并两个有序数组](#合并两个有序数组)
## [树](#树)
- [二叉树的最大深度](#二叉树的最大深度)
- [平衡二叉树](#平衡二叉树)
- [二叉树的直径](#二叉树的直径)
- [翻转二叉树](#翻转二叉树)

### 两数之和
[1. 两数之和](https://leetcode-cn.com/problems/two-sum/)
> 思路：采用哈希表进行求解
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        //值-对应的下标哈希表
        Map<Integer,Integer> map = new HashMap<>();
        for(int i=0;i<nums.length;i++){
            Integer index = map.get(target-nums[i]);
            if(index != null){
                return new int[]{index,i};
            }
            map.put(nums[i],i);
        }
        return new int[0];
    }
}
```
### 两数相加
[2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)
> 考虑进位情况，遍历链表，将结点逐个进行相加
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
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
        return dummy.next;
    }
}
```
### 无重复字符的最长子串
- [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)
> 利用集合中元素的唯一性进行求解
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        //特殊情况判断
        if(s.length() == 0 || s.length() == 1){
            return s.length();
        }
        Set<Character> set = new HashSet<>();
        char[] strs = s.toCharArray();
        //index表示当前正要被加入集合的字符的下标
        int max = 1,index = 0,len = strs.length;;
        for(int i = 0; i < strs.length; i++){
            //表示从 i-index的字串
            if(i > 0){
                set.remove(strs[i-1]);
            }
            while(index < len && !set.contains(strs[index])){
                set.add(strs[index++]);
            }
            max = Math.max(index - i,max);
        }
        return max;
    }
}
```
### 寻找两个正序数组的中位数
[4. 寻找两个正序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)
> 合并数组后求取中位数
```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int len1 = nums1.length, len2 = nums2.length, len = len1 + len2;
        //合并数组
        int[] nums = new int[len];
        //利用双指针合并数组
        int i = 0, j = 0, k = 0;
        while (i < len1 && j < len2) {
            if (nums1[i] <= nums2[j]) {
                nums[k++] = nums1[i++];
            } else {
                nums[k++] = nums2[j++];
            }
        }
        while (i < len1) {
            nums[k++] = nums1[i++];
        }
        while (j < len2) {
            nums[k++] = nums2[j++];
        }
        if (len % 2 == 0) {
            return (nums[len / 2] + nums[len / 2 - 1]) / 2.0;
        } else {
            return nums[len / 2];
        }
    }
}
```
### 最长回文子串
- [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)
> - 暴力解法
```java
class Solution {
    /**
     * 暴力解法：时间复杂度太高
     */
    public String longestPalindrome(String s) {
        //遍历字串
        if (s.length() == 1 || s.length() == 0) {
            return s;
        }
        int ans = 0, len = s.length();
        if (isPalindrome(s, 0, len - 1)) {
            return s;
        }
        String result = s.substring(0, 1);
        for (int i = 0; i < len; i++) {
            //检测字串是否为回文串
            for (int j = i + 1; j < len; j++) {
                if (isPalindrome(s, i, j)) {
                    if (ans < (j - i) + 1) {
                        ans = (j - i) + 1;
                        result = s.substring(i, j + 1);
                    }
                }
            }
        }
        return result;
    }

    /**
     * 判断所给字符串是否为回文串
     */
    private boolean isPalindrome(String s, int left, int right) {
        while (left <= right) {
            if (s.charAt(left++) != s.charAt(right--)) {
                return false;
            }
        }
        return true;
    }
}
```
> - 中心扩展法
```java
//中心扩展法:中心向两边进行扩展
class Solution {
    public String longestPalindrome(String s) {
        int start = 0,end = 0; //字串的开始和结束的位置
        for(int i = 0;i < s.length();i++){ //遍历字串
            int len1 = expandByCenter(s,i,i); //扩展字符长度为奇数情况
            int len2 = expandByCenter(s,i,i+1);   //扩展字符长度为偶数
            int len = Math.max(len1,len2); //选择最长的情况
            if(len > end - start){ //计算字串开始和结束的位置
                start = i - (len-1)/2;
                end = i + len/2;
            }
        }
        return s.substring(start,end + 1);
    }
    //从中心向两边扩展(奇数和偶数情况)
    private int expandByCenter(String s,int left,int right){
        while(left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)){
            left--;
            right++;
        }
        return right-left-1;
    }
}
```
### Z字形变换
[6. Z 字形变换](https://leetcode-cn.com/problems/zigzag-conversion/)
> 判断字符需要放入到哪个容器当中，边界条件要进行判断特殊处理
```java
class Solution {
    public String convert(String s, int numRows) {
        //边界条件需要考虑
        if(s == null||numRows == 1){
            return s;
        }
        List<StringBuilder> list = new ArrayList<>();
        for(int i = 0;i < numRows; i++){
            list.add(new StringBuilder());
        }
        boolean down = false;
        int index = 0;
        //遍历字符串
        for(int i = 0; i < s.length(); i++){
            //获取字符串容器
            StringBuilder sb = list.get(index);
            sb.append(s.charAt(i));
            if(index == numRows - 1 || index == 0){
                down = !down;
            }
            index += down ? 1 :  - 1;
        }
        StringBuilder res = new StringBuilder();
        for(int i = 0; i < list.size(); i++){
            res.append(list.get(i).toString());
        }
        return res.toString();
    }
}
```
### 整数反转
[7.整数反转](https://leetcode-cn.com/problems/reverse-integer/)
```java
class Solution {
    public int reverse(int x) {
        int rev = 0;
        while (x != 0) {
            if (rev < Integer.MIN_VALUE / 10 || rev > Integer.MAX_VALUE / 10) {
                return 0;
            }
            int digit = x % 10;
            x /= 10;
            rev = rev * 10 + digit;
        }
        return rev;
    }
}
```
### 字符串转换整数 
- [8.字符串转换整数](https://leetcode-cn.com/problems/string-to-integer-atoi/)
```java
class Solution {
    public int myAtoi(String s) {
        //去除空字符
        String str = s.trim();
        if (str.length() == 0) {
            return 0;
        }
        //判断首字符
        char first = str.charAt(0);
        //首字符不符合直接返回0
        if (!Character.isDigit(first) && first != '+' && first != '-') {
            return 0;
        }
        //符号的正负
        boolean neg = first == '-';
        long ans = 0L;
        int i = Character.isDigit(first) ? 0 : 1;
        //遍历字符串
        while (i < str.length()) {
            if (Character.isDigit(str.charAt(i))) {
                ans = ans * 10 + (str.charAt(i++) - '0');
                //如果为正整数且超出范围
                if (!neg && ans > Integer.MAX_VALUE) {
                    return Integer.MAX_VALUE;
                }
                if (neg && ans > 1L + Integer.MAX_VALUE) {
                    return Integer.MIN_VALUE;
                }
            } else {
                break;
            }
        }
        return neg ? (int) -ans : (int) ans;
    }
}
```
### 回文数
[11.回文数](https://leetcode-cn.com/problems/container-with-most-water/)
```java
class Solution {
    public boolean isPalindrome(int x) {
        if (x < 0) {
           return false;
        }
        String s = String.valueOf(x);
        return isPalindrome(s);
    }

    private boolean isPalindrome(String s) {
        int low = 0, high = s.length() - 1;
        //写法必须注意：不能写成while(low++<high--)
        while (low < high) {
            if (s.charAt(low++) != s.charAt(high--)) {
                return false;
            }
        }
        return true;
    }
}
```
### 盛最多水的容器
[11.盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)
```java
class Solution {
    public int maxArea(int[] height) {
        int maxArea = 0;
        int i = 0, j = height.length - 1;
        while (i < j) {
            int area = (j - i) * Math.min(height[i], height[j]);
            maxArea = Math.max(maxArea, area);
            if (height[i] <= height[j]) {
                i++;
            } else {
                j--;
            }
        }
        return maxArea;
    }
}
```
### 整数转罗马数字
[12. 整数转罗马数字](https://leetcode-cn.com/problems/integer-to-roman/)
> 采用双数组进行映射
```java
class Solution {
    public String intToRoman(int num) {
        String[] roman = {"M","CM","D","CD","C","XC","L","XL","X","IX","V","IV","III","II","I"};
        int[] arr = {1000,900,500,400,100,90,50,40,10,9,5,4,3,2,1};
        StringBuilder res = new StringBuilder();
        for(int i = 0;i < arr.length; i++){
            if(num == 0){
                break;
            }
            while(num - arr[i] >= 0){
                res.append(roman[i]);
                num -= arr[i];
            }
        }
        return res.toString();
    }
}
```
### 删除链表的倒数第N个结点
[19.删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(),p,q;
        dummy.next = head;
        p = q = dummy;
        while(n-- > 0){
            p = p.next;
        }
        while(p.next != null){
            q = q.next;
            p = p.next;
        }
        q.next = q.next.next;
        return dummy.next;
    }
}
```
### 有效的括号
[20.有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)
> 利用栈作为辅助进行判断
```java
class Solution {
    public boolean isValid(String s) {
        Map<Character,Character> map = new HashMap<>();
        map.put('(',')');
        map.put('[',']');
        map.put('{','}');
        Stack<Character> stack = new Stack<>();
        char[] strs = s.toCharArray();
        for(char c : strs){
            if(c == '(' || c == '[' || c == '{'){
                stack.add(c);
            }else{
                if(stack.isEmpty() || map.get(stack.pop()) != c){
                    return false;
                }
            }
        }
        return stack.isEmpty();
    }
}
```
### 合并两个有序链表
[21.合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)
> Java 题解：
```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(),p = dummy;
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
> python 题解：
```python
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode()
        p = dummy
        while list1 and list2:
            if list1.val < list2.val:
                p.next = list1
                list1 = list1.next
            else:
                p.next = list2
                list2 = list2.next
            p = p.next
        if list1:
            p.next = list1
        if list2:
            p.next = list2
        return dummy.next
```
### 括号生成
[22.括号生成](https://leetcode-cn.com/problems/generate-parentheses/)
> 深度优先遍历
```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList<>();
        dfs(ans, n, 0, 0, "");
        return ans;
    }
    private void dfs(List<String> ans,int n,int left,int right,String str){
        if(left > n){
            return;
        }
        if(left == n && left == right){
            ans.add(str);
            return;
        }
        if(left < n){
            dfs(ans, n, left+1, right, str + "(");
        }
        if(left > right){
            dfs(ans, n, left, right + 1 ,str + ")");
        }    
    }
}
```
### 合并K个升序链表
[23.合并K个升序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)
> 将两两结点进行合并
```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode ans = null;
        for(int i = 0;i < lists.length;i++){
            ans = merge(ans,lists[i]);
        }
        return ans;
    }

    private ListNode merge(ListNode a,ListNode b){
        if(a == null || b == null){
            return a == null? b : a;
        }
        ListNode head = new ListNode(),tail = head,p = a,q= b;
        while(p != null && q != null){
            if(p.val < q.val){
                tail.next = p;
                p = p.next;
            }else{
                tail.next = q;
                q = q.next;
            }
            tail = tail.next;
        }
        tail.next = p == null ? q : p;
        return head.next;
    }
}
```
> 在合并两个链表的基础上进行扩展
```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists.length == 0){
            return null;
        }
        if(lists.length == 1){
            return lists[0];
        }
        ListNode list = lists[0];
        for(int i = 1;i < lists.length; i++){
            list = mergeTwoLists(list,lists[i]);
        }
        return list;
    }
    private ListNode mergeTwoLists(ListNode l1,ListNode l2){
        ListNode dummy = new ListNode(0),p = dummy;
        while(l1 != null && l2 != null){
            if(l1.val < l2.val){
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
### 两两交换链表中的节点
[24.两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)
> 利用双指针法进行求解，需要注意边界条件的判断
```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode p = head,q = head.next;
        ListNode dummy = new ListNode(),k = dummy;
        while(p != null && q != null){
           k.next = new ListNode(q.val);
           k.next.next = new ListNode(p.val);
           k = k.next.next;
           p = q.next;
           if(p != null){
               q = p.next;
           }
        }
        if(q == null){
            k.next = new ListNode(p.val);
        }
        return dummy.next;
    }
}
```
> 虚拟头结点+前置指针
```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        //虚拟头结点
        ListNode dummy = new ListNode(0), prev = dummy;
        dummy.next = head;
        while (prev.next != null && prev.next.next != null) {
            //相邻结点进行两两交换
            ListNode l1 = prev.next, l2 = prev.next.next, next = l2.next;
            l1.next = next;
            l2.next = l1;
            prev.next = l2;
            prev = l1;
        }
        return dummy.next;
    }
}
```
### K个一组翻转链表
[25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)
> K个结点一组翻转后进行重新连接
```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode dummy = new ListNode(0),pre = dummy;
        dummy.next = head;
        while(head != null){
            ListNode tail = pre;
            for(int i = 0; i < k; i++){
                tail = tail.next;
                //不需要翻转，直接返回结果
                if(tail == null){
                    return dummy.next;
                }
            }
            ListNode next = tail.next; 
            ListNode[] node = reverse(head,tail);
            head = node[0];
            tail = node[1];
            pre.next = head;
            tail.next = next;
            head = next;
            pre = tail;
        }
        return dummy.next;
    }
    private ListNode[] reverse(ListNode head,ListNode tail){
        //p为遍历指针
        ListNode prev = tail.next,p = head;
        while(prev != tail){
            ListNode next = p.next;
            p.next = prev;
            prev = p;
            p = next;
        }
        return new ListNode[]{tail,head};
    }
}
```
### 删除有序数组中的重复项
[26.删除有序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int index = 0;
        for(int i = 1; i < nums.length; i++){
            if(nums[index] != nums[i]){
                nums[++index] = nums[i];
            }
        }
        return index + 1;
    }
}
```
### 移除元素
[27.移除元素](https://leetcode-cn.com/problems/remove-element/)
```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int index = 0;
        for(int i = 0; i < nums.length; i++){
            if(nums[i] != val){
                nums[index++] = nums[i];
            }
        }
        return index;
    }
}
```
### 下一个排列
[31.下一个排列](https://leetcode-cn.com/problems/next-permutation/)
> 需要寻找规律，注意边界条件
```java
class Solution {
    public void nextPermutation(int[] nums) {
        int i = nums.length - 2;
        //边界条件需要注意
        while(i >= 0 && nums[i] >= nums[i + 1]){
            i--;
        }
        if(i >= 0){
            int j = nums.length - 1;
            while(j >= 0 && nums[i] >= nums[j]){
                j--;
            }
            swap(nums,i,j);
        }
        reverse(nums,i+1);
    }
    private void swap(int[] nums,int i,int j){
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
    private void reverse(int[] nums,int start){
        int left = start,right = nums.length - 1;
        while(left < right){
            swap(nums,left++,right--);
        }
    }
}
```
### 最长有效括号
[32.最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)
> 遍历字符串，利用栈存储子串的起始位置
```java
class Solution {
    public int longestValidParentheses(String s) {
        Stack<Integer> stack = new Stack<>();
        stack.push(-1);
        int max = 0;
        for(int i = 0; i < s.length(); i++){
            if(s.charAt(i) == '('){
                stack.push(i);
            }else{
                stack.pop();
                if(stack.isEmpty()){
                    stack.push(i);
                }else{
                    max = Math.max(max,i - stack.peek());
                }
            }
        }
        return max;
    }
}
```
### 搜索旋转排序数组
[33.搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)
> 二分查找，并且考虑到极端情况，即数组反转或者不反转的情况 
```java
class Solution {
    public int search(int[] nums, int target) {
        int low = 0,high = nums.length - 1;
        while(low <= high){
            int mid = low + (high - low)/2;
            if(nums[mid] == target){
                return mid;
            }
            if(nums[0] <= nums[mid]){
                if(target >= nums[0] && nums[mid] >= target){
                    high = mid - 1;
                }else{
                    low = mid + 1;
                }
            }else{
                if(target <= nums[nums.length - 1] && nums[mid] < target){
                    low = mid + 1;
                }else{
                    high = mid - 1;
                }
            }
        }
        return -1;
    }
}
```
### 在排序数组中查找元素的第一个和最后一个位置
[34.在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
> 利用二分查找：寻找第一个大于等于`target`的下标和第一个大于`target`的下标
```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int left = binarySearch(nums,target,true);
        int right = binarySearch(nums,target,false) - 1;
        if(left < nums.length && nums[left] == target){
            return new int[]{left,right};
        }
        return new int[]{-1,-1};
    }

    private int binarySearch(int[] nums,int target,boolean lower){
        int low = 0, high = nums.length;
        while(low < high){
            int mid = low + (high - low) / 2;
            if(nums[mid] > target || (lower && nums[mid] >= target)){
                high = mid;
            }else{
                low = mid + 1;
            }
        }
        return low;
    }
}
```
### 搜索插入位置
[35.搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)
> 二分查找：查找第一个大于等于`target`的下标
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
### 全排列
[46.全排列](https://leetcode-cn.com/problems/permutations/)
> 回溯算法：访问标记数组
```java
class Solution {
   public List<List<Integer>> permute(int[] nums) {
        int len = nums.length;
        List<List<Integer>> ans = new ArrayList<>();
        boolean[] visit = new boolean[len];
        Deque<Integer> path = new ArrayDeque<>();
        dfs(nums, len, visit, path, 0, ans);
        return ans;
    }

    private void dfs(int[] nums, int len, boolean[] visit, Deque<Integer> path,
                            int depth, List<List<Integer>> ans) {
        if (depth == len) {
            ans.add(new ArrayList<>(path));
        }
        for (int i = 0; i < len; i++) {
            if (!visit[i]) {
                path.add(nums[i]);
                visit[i] = true;
                dfs(nums, len, visit, path, depth + 1, ans);
                //回溯
                path.removeLast();
                visit[i] = false;
            }
        }
    }
}
```
> 决策的过程，画出决策树，每一步进行选择或者不选
```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
         List<List<Integer>> res = new ArrayList<>();
         backtrack(res, new LinkedList<>(), 0, nums);
         return res;
    }
            //回溯法：深度遍历
    private void backtrack(List<List<Integer>> res, LinkedList<Integer> path, int index, int[] nums) {
        if (path.size() == nums.length) {
            res.add(new ArrayList<>(path));
            return;
        }
            //对每个元素进行深度遍历
        for (int i = 0; i < nums.length; i++) {
            if (!path.contains(nums[i])) {
                path.add(nums[i]);
                //递归，下一层的选择
                backtrack(res, path, i, nums);
                //回溯
                path.removeLast();
            }
        }
    }
}
```
### 子集
[78,子集](https://leetcode-cn.com/problems/subsets/)
```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        res.add(new ArrayList<>());
        for (int i = 1; i <= nums.length; i++) {
            backtrack(res, new LinkedList<>(), i, 0, nums);
        }
        return res;
    }
    //回溯法：深度遍历
    private void backtrack(List<List<Integer>> res, LinkedList<Integer> path, int depth, int index, int[] nums) {
        if (path.size() == depth) {
            res.add(new ArrayList<>(path));
            return;
        }
        //对每个元素进行深度遍历
        for (int i = index; i < nums.length; i++) {
            path.add(nums[i]);
            //递归
            backtrack(res, path, depth, i + 1, nums);
            //回溯
            path.removeLast();
        }
    }
}
```
> 位运算
### 汉明距离
[461.汉明距离](https://leetcode-cn.com/problems/hamming-distance/)
> 亦或求解之后，结果`(z)`进行循环按位与`(z&(z-1))`运算求得最终结果
```java
class Solution {
    public int hammingDistance(int x, int y) {
        //亦或求解
        int z = x ^ y;
        int cnt = 0;
        while (z != 0) {
            cnt++;
            z &= (z - 1);
        }
        return cnt;
    }
}
```
### 只出现一次的数字
[136.只出现一次的数字](https://leetcode-cn.com/problems/single-number/)
> 亦或：x^x = 0(相同为0)，y^0 = y(不同为1)
```java
class Solution {
    public int singleNumber(int[] nums) {
        int ans = 0;
        for (int num : nums) {
            ans ^= num;
        }
        return ans;
    }
}
```
### 丢失的数字
[268.丢失的数字](https://leetcode-cn.com/problems/missing-number/)
> 同上题的思路：nums = [3,0,1] ^ [0 1 2 3] ==> 得到最终结果 2
```java
class Solution {
    public int missingNumber(int[] nums) {
        int ans = 0;
        for (int i = 0; i < nums.length; i++) {
            ans = ans ^ i ^ nums[i];
        }
        //还缺少一个的一个数:num.length
        return ans ^ nums.length;
    }
}
```
### 只出现一次的数字III
[260.只出现一次的数字III](https://leetcode-cn.com/problems/single-number-iii/)
> 如何进行分组问题，亦或求解
```java
class Solution {
    public int[] singleNumber(int[] nums) {
        int x = 0, y = 0, z = 0, m = 1;
        for (int num : nums) {
            z ^= num;
        }
        //找到首个二进制位位1的位置，等于0说明还没找到，需要继续找
        while ((z & m) == 0) {
            m <<= 1;
        }
        //分组
        for (int num : nums) {
            if ((m & num) == 0) {
                x ^= num;
            } else {
                y ^= num;
            }
        }
        return new int[]{x, y};
    }
}
```
---
---
## 树
### 二叉树的最大深度
[二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)
> 广度优先遍历：逐层遍历
```java
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int ans = 0;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            while (size-- > 0) {
                TreeNode node = queue.poll();
                if (node.left != null) {
                    queue.add(node.left);
                }
                if (node.right != null) {
                    queue.add(node.right);
                }
            }
            ans++;
        }
        return ans;
    }
}
```
> 深度优先，递归算法
```java
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null){
            return 0;
        }
        return Math.max(maxDepth(root.left),maxDepth(root.right))+1;
    }
}
```
### 平衡二叉树
[110. 平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)
> 计算最大深度+判断不符合情况的条件
```java
class Solution {
    private boolean result = true;
    public boolean isBalanced(TreeNode root) {
        getMaxDepth(root);
        return result;
    }
    private int getMaxDepth(TreeNode root){
        if(root == null){
            return 0;
        }
        int l = getMaxDepth(root.left);
        int r = getMaxDepth(root.right);
        if(Math.abs(l-r) > 1){
            result = false;
        }
        return Math.max(l,r)+1;
    }
}
```
### 二叉树的直径
[543.二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)
> 左子树的高度 + 右子树的高度
```java
class Solution {
    int max = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        getDepth(root);
        return max;
    }
    private int getDepth(TreeNode root){
        if(root == null){
            return 0;
        }
        int l = getDepth(root.left);
        int r = getDepth(root.right);
        max = Math.max(max,(l+r));
        return Math.max(l,r)+1;
    }
}
```
### 翻转二叉树
[226.翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)
> 临时结点+递归翻转
```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null){
            return null;
        }
        TreeNode temp = root.left;
        root.left = invertTree(root.right);
        root.right = invertTree(temp);
        return root;   
    }
}
```
-----
--------
## 链表
### 奇偶链表
[328.奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)
> 拆分成奇偶链表，然后进行合并
```java
class Solution {
    public ListNode oddEvenList(ListNode head) {
        //奇数
        ListNode odd = new ListNode(0), p = odd;
        //偶数
        ListNode even = new ListNode(0), q = even;
        int cnt = 0;
        while (head != null) {
            if (cnt % 2 == 0) {
                q.next = head;
                q = q.next;
            } else {
                p.next = head;
                p = p.next;
            }
            cnt++;
            head = head.next;
        }
        if (even.next != null && odd.next != null) {
            q.next = odd.next;
        }
        //最后的结点置为空
        p.next = null;
        return even.next;
    }
}
```
### 分隔链表
[725.分隔链表](https://leetcode-cn.com/problems/split-linked-list-in-parts/)
> 计算每段的长度，分割链表(结点最后一个置为空)
```java
class Solution {
    public ListNode[] splitListToParts(ListNode head, int k) {
        //结果集
        ListNode[] res = new ListNode[k];
        if (head == null) {
            return res;
        }
        //链表的长度
        int n = 0;
        ListNode p = head;
        while (p != null) {
            n++;
            p = p.next;
        }
        int mod = n % k, avg = n / k, index = 0;
        //接下来的步骤卡住了，什么原因？认真分析结果
        for (int i = 0; i < k; i++) {
            //每个部分的长度
            int size = avg + ((mod-- > 0) ? 1 : 0);
            //每个部分起始点
            res[i] = head;
            while (size-- > 0) {
                if (size == 0) {
                    p = head;
                    head = head.next;
                    //尾部结点置为空
                    p.next = null;
                    continue;
                }
                head = head.next;
            }
        }
        return res;
    }
}
```
### 回文链表
[234.回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)
> 找到中间结点 + 后半段进行反转
```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        if (head == null || head.next == null) {
            return true;
        }
        int n = 0;
        ListNode p = head, q = head;
        while (p != null) {
            n++;
            p = p.next;
        }
        int mid = n / 2;
        //找到中间结点,中间结点也会被反转
        while (mid-- > 0) {
            if (mid == 0) {
                ListNode tail = q;
                q = q.next;
                tail.next = null;
                continue;
            }
            q = q.next;
        }
        return isPalindrome(head, reverse(q));
    }

    private boolean isPalindrome(ListNode head, ListNode reverse) {
        while (reverse != null && head != null) {
            if (head.val != reverse.val) {
                return false;
            }
            head = head.next;
            reverse = reverse.next;
        }
        return true;
    }

    private ListNode reverse(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode dummy = new ListNode(0), p = head.next, q;
        dummy.next = head;
        head.next = null;
        while (p != null) {
            //用结点 q 保存 p 当前结点
            q = p;
            p = p.next;
            q.next = dummy.next;
            dummy.next = q;
        }
        return dummy.next;
    }
}
```
### 链表反转
[206.反转链表](https://leetcode-cn.com/problems/reverse-linked-list/description/)
> 遍历链表，不断往头部重新链接结点
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode dummy = new ListNode(0), p = head.next, q;
        dummy.next = head;
        head.next = null;
        while (p != null) {
            q = p;
            p = p.next;
            q.next = dummy.next;
            dummy.next = q;
        }
        return dummy.next;
    }
}
```
### 相交链表
[160.相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)
> 交叉遍历
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode p = headA, q = headB;
        while(p != q){
            p = p == null ? headB : p.next;
            q = q == null ? headA : q.next;
        }
        return p;
    }
}
```
### 两数相加II
[445. 两数相加 II](https://leetcode-cn.com/problems/add-two-numbers-ii/)
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
       if(l1 == null || l2 == null){
           return l1 == null ? l2 : l1;
       }
       ListNode node = sum(reverse(l1),reverse(l2));
       return reverse(node);
    }
    private ListNode sum(ListNode l1,ListNode l2){
        ListNode dummy = new ListNode(),p = dummy;
        int carry = 0;
        while(l1 != null && l2 != null){
            int sum = carry + l1.val + l2.val;
            carry = sum/10;
            p.next = new ListNode(sum % 10);
            p = p.next;
            l1 = l1.next;
            l2 = l2.next;
        }
        while(l1 != null){
            int sum = carry + l1.val;
            carry = sum / 10;
            p.next = new ListNode(sum % 10);
            l1 = l1.next;
            p = p.next;
        }
        while(l2 != null){
            int sum = carry + l2.val;
            carry = sum / 10;
            p.next = new ListNode(sum % 10);
            l2 = l2.next;
            p = p.next;
        }
        if(carry != 0){
            p.next = new ListNode(carry);
        }
        return dummy.next;
    }
    private ListNode reverse(ListNode head){
        if(head == null || head.next == null){
            return head;
        }
        ListNode dummy = new ListNode(),p = head.next,q;
        dummy.next = head;
        head.next = null;
        //记得使用 while 语句
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
> 使用栈进行解决
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode();
        Stack<Integer> s1 = buildStack(l1);
        Stack<Integer> s2 = buildStack(l2);
        int carry = 0;
        while(!s1.isEmpty() || !s2.isEmpty() || carry != 0){
            int x = s1.isEmpty()?0:s1.pop();
            int y = s2.isEmpty()?0:s2.pop();
            int sum = x + y + carry;
            ListNode node = new ListNode(sum%10);
            node.next = dummy.next;
            dummy.next = node;
            carry = sum/10;           
        }
        return dummy.next;
    }
    private Stack<Integer> buildStack(ListNode head){
        Stack<Integer> stack = new Stack<>();
        while(head != null){
            stack.add(head.val);
            head = head.next;
        }
        return stack;
    }
}
```
---
---
## 双指针
### 两数之和II
[167.两数之和 II - 输入有序数组](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)
> 有序数组，可以使用二分查找
```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int low = 0,high = numbers.length - 1;
        while(low <= high){
            int sum = numbers[low] + numbers[high];
            if(sum == target){
                return new int[]{low+1,high+1};
            }else if(sum < target){
                low++;
            }else{
                high--;
            }
        }
        return new int[]{};
    }
}
```
### 平方数之和
[633.平方数之和](https://leetcode-cn.com/problems/sum-of-square-numbers/)
> 可能会发生溢出，需要使用 long 型
```java
class Solution {
    public boolean judgeSquareSum(int c) {
        long high = (long)Math.sqrt(c)+1;
        long low = 0;
        while(low <= high){
            long sum = low*low + high*high;
            if(sum == c){
                return true;
            }else if(sum < c){
                low++;
            }else{
                high--;
            }
        }
        return false;
    }
}
```
### 反转字符串中的元音字母
[反转字符串中的元音字母](https://leetcode-cn.com/problems/reverse-vowels-of-a-string/)
```java
class Solution {
    public String reverseVowels(String s) {
        Set<Character> set = new HashSet<>();
        set.addAll(Arrays.asList('a', 'e', 'i', 'o', 'u'));
        int low = 0, high = s.length() - 1;
        char[] arr = s.toCharArray();
        while (low <= high) {
            // arr[low]/arr[high] 出现多次，可以使用变量提取出来。
            if (set.contains(Character.toLowerCase(arr[low]))
                    && set.contains(Character.toLowerCase(arr[high]))) {
                char c = arr[low];
                arr[low] = arr[high];
                arr[high] = c;
                low++;
                high--;
            } else if (set.contains(Character.toLowerCase(arr[low]))) {
                high--;
            } else if (set.contains(Character.toLowerCase(arr[high]))) {
                low++;
            } else {
                low++;
                high--;
            }
        }
        // 可以直接简化成 return new String(arr);
        StringBuilder sb = new StringBuilder();
        for (char c : arr) {
            sb.append(c);
        }
        return sb.toString();
    }
}
```
### 验证回文字符串Ⅱ
[验证回文字符串 Ⅱ](https://leetcode-cn.com/problems/valid-palindrome-ii/)
> 如果两边出现了不相等的情况，则对中间的字符串进行检查
```java
class Solution {
    public boolean validPalindrome(String s) {
        int low = 0,high = s.length()-1;
        while(low <= high){
            if(s.charAt(low) != s.charAt(high)){
                return validPalindrome(s,low+1,high) || validPalindrome(s,low,high-1);
            };
            //条件要写在这边，不能写在if当中
            low++;
            high--;
        }
        return true;
    }
    private boolean validPalindrome(String s,int left,int right){
        while(left < right){
            if(s.charAt(left++) != s.charAt(right--)){
                return false;
            }
        }
        return true;
    }
}
```
### 环形链表
[环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)
> 快慢指针
```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head == null || head.next == null){
            return false;
        }
        ListNode p = head,q = head.next;
        while(p != null && q != null && q.next != null){
            if(p == q){
                return true;
            }
            p = p.next;
            q = q.next.next;
        }
        return false;
    }
}
```
### 通过删除字母匹配到字典里最长单词
[524.通过删除字母匹配到字典里最长单词](https://leetcode-cn.com/problems/longest-word-in-dictionary-through-deleting/)
> 利用双指针判断字串
```java
class Solution {
    public String findLongestWord(String s, List<String> dictionary) {
        String ans = "";
        for(String target:dictionary){
            int l1 = s.length(),l2 = target.length(),l3 = ans.length();
            //将不符合的情况进行排除
            if(l1 < l2 || l2 < l3 || (l2 == l3 && target.compareTo(ans) > 0)){
                continue;
            }
            if(isSubStr(s,target)){
                ans = target;
            }
        }
        return ans;
    }
    private boolean isSubStr(String s,String target){
        int i = 0,j = 0;
        while(i < s.length() && j < target.length()){
            //不匹配则 s 串继续向前查找
            if(s.charAt(i) == target.charAt(j)){
                j++;
            }
            i++;
        }
        return j == target.length();
    }
}
```
### 合并两个有序数组
[88.合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)
> 从尾部进行遍历，然后合并
```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int index = m + n - 1,i = m - 1, j = n - 1;
        while(i >= 0 && j >= 0){
            if(nums1[i] >= nums2[j]){
                nums1[index--] = nums1[i--]; 
            }else{
                nums1[index--] = nums2[j--];
            }
        }
        while(j >= 0){
            nums1[index--] = nums2[j--];
        }
    }
}
```
---
---