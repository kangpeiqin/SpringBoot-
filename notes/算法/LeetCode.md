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
- [搜索旋转排序数组](#搜索旋转排序数组)
- [在排序数组中查找元素的第一个和最后一个位置](#在排序数组中查找元素的第一个和最后一个位置)
- [搜索插入位置](#搜索插入位置)
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