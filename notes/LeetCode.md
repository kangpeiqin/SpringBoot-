<p> <a href="./数据结构&算法.md">返回</a></p>

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
```java
class Solution {
       public String convert(String s, int numRows) {
        //numRows==1的特殊情况！！
        if (numRows == 1) {
            return s;
        }
        List<StringBuilder> list = new ArrayList<>();
        //判断最终的结果有几行,并由此构建几个StringBuilder
        for (int i = 0; i < Math.min(numRows, s.length()); i++) {
//            StringBuilder sb = new StringBuilder();
//            list.add(sb);
            //简洁做法：
            list.add(new StringBuilder());
        }
        //设置遍历的方向为
        int rowIndex = 0;
        boolean goingDonw = false;
        //遍历字符串,判断每个字符属于哪个行当中
        for (int i = 0; i < s.length(); i++) {
            list.get(rowIndex).append(s.charAt(i));
            //这步很关键
            if (rowIndex == 0 || rowIndex == numRows - 1) {
                goingDonw = !goingDonw;
            }
            rowIndex += goingDonw ? 1 : -1;
        }
        StringBuilder ret = new StringBuilder();
        for (StringBuilder sb : list) {
            ret.append(sb);
        }
        return ret.toString();
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