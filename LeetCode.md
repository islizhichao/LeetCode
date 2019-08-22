# LeetCode记录

## 目录

[TOC]

## 15. 3Sum

**QS:**

> Given an array `nums` of *n* integers, are there elements *a*, *b*, *c* in `nums` such that *a* + *b* + *c* = 0? Find all unique triplets in the array which gives the sum of zero.
>
> **Note:**
>
> The solution set must not contain duplicate triplets.
>
> **Example:**
>
> ```c++
> Given array nums = [-1, 0, 1, 2, -1, -4],
> 
> A solution set is:
> [
>   [-1, 0, 1],
>   [-1, -1, 2]
> ]
> ```

**AS:**

```c++
class Solution {
public:
	vector<vector<int>> threeSum(vector<int>& nums) {
		vector<vector<int> > result;
		if (nums.empty() || nums.size()<3) return result;
		sort(nums.begin(), nums.end());
		for (int i = 0; i<nums.size() - 2; i++) {
			if (i>0 && nums[i] == nums[i - 1]) continue;
			int left = i + 1, right = nums.size() - 1;
			while (left<right) {
				int temp = nums[i] + nums[left] + nums[right];
				if (temp == 0) {
					result.push_back(vector<int>{nums[i], nums[left], nums[right]});
					left++;
					right--;
					while (left < right && nums[left] == nums[left - 1]) left++;
					while (left < right && nums[right] == nums[right + 1]) right--;
				}
				else if (temp<0) left++;
				else if (temp>0) right--;
			}
		}
		return result;
	}
};

```

## 23. Merge k Sorted Lists

**QS:**

> Merge *k* sorted linked lists and return it as one sorted list. Analyze and describe its complexity.
>
> **Example:**
>
> ```c++
> Input:
> [
>   1->4->5,
>   1->3->4,
>   2->6
> ]
> Output: 1->1->2->3->4->4->5->6
> ```

**AS:**

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if(lists.empty()) return NULL;
        
        ListNode *L=new ListNode(0);
        L->next=lists[0];
        for(int i=1;i<lists.size();i++){
            ListNode *p,*q,*t;
            p=L->next,q=lists[i],t=L;
            while(p&&q){
                if(p->val<q->val){
                    t->next=p;
                    t=p;
                    p=p->next;
                }else{
                    t->next=q;
                    t=q;
                    q=q->next;
                }
            }
            while(p!=NULL){t->next=p;t=p;p=p->next;}
            while(q!=NULL){t->next=q;t=q;q=q->next;}
        }
        return L->next;
    }
};
```

为使用到哈夫曼树排列的思想

## 27. Remove Element

> Given an array *nums* and a value *val*, remove all instances of that value [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) and return the new length.
>
> 
>
> Do not allocate extra space for another array, you must do this by **modifying the input array in-place** with O(1) extra memory.
>
> 
>
> The order of elements can be changed. It doesn't matter what you leave beyond the new length.
>
> 
>
> **Example 1:**
>
> 
>
> ```c++
> Given nums = [3,2,2,3], val = 3,
> 
> Your function should return length = 2, with the first two elements of nums being 2.
> 
> It doesn't matter what you leave beyond the returned length.
> ```
>
> 
>
> **Example 2:**
>
> 
>
> ```c++
> Given nums = [0,1,2,2,3,0,4,2], val = 2,
> 
> Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4.
> 
> Note that the order of those five elements can be arbitrary.
> 
> It doesn't matter what values are set beyond the returned length.
> ```
>
> 
>
> **Clarification:**
>
> 
>
> Confused why the returned value is an integer but your answer is an array?
>
> 
>
> Note that the input array is passed in by **reference**, which means modification to the input array will be known to the caller as well.
>
> 
>
> Internally you can think of this:
>
> 
>
> ```c++
> // nums is passed in by reference. (i.e., without making a copy)
> int len = removeElement(nums, val);
> 
> // any modification to nums in your function would be known by the caller.
> // using the length returned by your function, it prints the first len elements.
> for (int i = 0; i < len; i++) {
>  print(nums[i]);
> }
> ```

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        if(nums.empty()) return 0;
        int left=0,right=nums.size()-1;
        while(left<right){
            while(left<right && nums[left]!=val) left++;
            while(left<right && nums[right]==val) right--;
            if(left<right){
                std::swap(nums[left],nums[right]);
                left++;right--;
            }
        }
        int i;
        for(i=nums.size()-1;i>=0 && nums[i]==val;i--);
        nums.resize(i+1);
        return i+1;
    }
};
```



```c++
在C++中，当string字符串s为空时，s.size() == 0;
但是，s.size() 的返回值类型为size_type；
s.size() - 1在32位机上为4294967295 即2的32次方-1；但是却不是-1。
```

## 32. Longest Valid Parentheses

**QS:**

> Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.
>
> 
>
> **Example 1:**
>
> 
>
> ```c++
> Input: "(()"
> Output: 2
> Explanation: The longest valid parentheses substring is "()"
> ```
>
> 
>
> **Example 2:**
>
> 
>
> ```c++
> Input: ")()())"
> Output: 4
> Explanation: The longest valid parentheses substring is "()()"
> ```

**AS:**

```c++
class Solution {
public:
    int longestValidParentheses(string s) {
        if(s.empty()) return 0;
        int max_len=0;
        stack<int> st;
        for(int i=0;i<s.size();i++){
            char curr=s[i];
            if(!st.empty() && curr==')' && s[st.top()]=='('){
                st.pop();
                if(st.empty()) max_len=max(max_len,i+1);
                else max_len=max(max_len,i-st.top());
            }else{
                st.push(i);
            }
        }
        return max_len;
    }
    
   
};
```



## 60. Permutation Sequence

> By listing and labeling all of the permutations in order, we get the following sequence for *n* = 3:
>
> 
>
> 1. `"123"`
> 2. `"132"`
> 3. `"213"`
> 4. `"231"`
> 5. `"312"`
> 6. `"321"`
>
> 
>
> Given *n* and *k*, return the *k*th permutation sequence.
>
> 
>
> **Note:**
>
> 
>
> - Given *n* will be between 1 and 9 inclusive.
> - Given *k* will be between 1 and *n*! inclusive.
>
> 
>
> **Example 1:**
>
> 
>
> ```c++
> Input: n = 3, k = 3
> Output: "213"
> ```
>
> 
>
> **Example 2:**
>
> 
>
> ```c++
> Input: n = 4, k = 9
> Output: "2314"
> ```

```c++
class Solution {
public:
    string getPermutation(int n, int k) {
        if(n==1) return to_string(n);
        int num=1;
        for(int i=1;i<n;i++) num*=i;
        int first=0;
        if(n==2) first=1;
        else first=k/(num+1)+1;		//仔细体会这段代码
        vector<int> temp;
        temp.push_back(first);
        for(int i=1;i<=n;i++) 
            if(i!=first) temp.push_back(i);
        int num2=k-(first-1)*num;
        for(int i=1;i<num2;i++)
            next_permutation(temp.begin(),temp.end());
        string result="";
        for(auto i : temp)
            result+=to_string(i);
        return result;
    }
};
```

## 61. Rotate List

> Given a linked list, rotate the list to the right by *k* places, where *k* is non-negative.
>
> 
>
> **Example 1:**
>
> 
>
> ```c++
> Input: 1->2->3->4->5->NULL, k = 2
> Output: 4->5->1->2->3->NULL
> Explanation:
> rotate 1 steps to the right: 5->1->2->3->4->NULL
> rotate 2 steps to the right: 4->5->1->2->3->NULL
> ```
>
> 
>
> **Example 2:**
>
> 
>
> ```c++
> Input: 0->1->2->NULL, k = 4
> Output: 2->0->1->NULL
> Explanation:
> rotate 1 steps to the right: 2->0->1->NULL
> rotate 2 steps to the right: 1->2->0->NULL
> rotate 3 steps to the right: 0->1->2->NULL
> rotate 4 steps to the right: 2->0->1->NULL
> ```

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if(k==0 || head==NULL) return head;
        int len=0;
        ListNode *p,*q,*tail;
        p=head;
        while(p){len++;tail=p;p=p->next;}
        if(k%len==0) return head;
        len=len-k%len;
        p=head;
        for(int i=1;i<len;i++) p=p->next;
        tail->next=head;
        head=p->next;
        p->next=NULL;
        return head;
    }
};
```

## 62. Unique Paths

**QS:**

> A robot is located at the top-left corner of a *m* x *n* grid (marked 'Start' in the diagram below).
>
> 
>
> The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).
>
> 
>
> How many possible unique paths are there?
>
> 
>
> ![img](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)
> Above is a 7 x 3 grid. How many possible unique paths are there?
>
> 
>
> **Note:** *m* and *n* will be at most 100.
>
> 
>
> **Example 1:**
>
> 
>
> ```c++
> Input: m = 3, n = 2
> Output: 3
> Explanation:
> From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
> 1. Right -> Right -> Down
> 2. Right -> Down -> Right
> 3. Down -> Right -> Right
> ```
>
> 
>
> **Example 2:**
>
> 
>
> ```c++
> Input: m = 7, n = 3
> Output: 28
> ```

**AS:**

```C++
int dp[110][110]={0};
class Solution {
public:
    int uniquePaths(int m, int n) {
        if(m==1 || n==1){
            dp[m][n]=1;
            return 1;
        }
        dp[m][n-1]=dp[m][n-1]!=0?dp[m][n-1]:uniquePaths(m,n-1);
        dp[m-1][n]=dp[m-1][n]!=0?dp[m-1][n]:uniquePaths(m-1,n);
        dp[m][n]=dp[m][n-1]+dp[m-1][n];
        return dp[m][n-1]+dp[m-1][n];
    }
};
```

## 64. Minimum Path Sum

**QS:**

> Given a *m* x *n* grid filled with non-negative numbers, find a path from top left to bottom right which *minimizes* the sum of all numbers along its path.
>
> **Note:** You can only move either down or right at any point in time.
>
> **Example:**
>
> ```c++
> Input:
> [
> [1,3,1],
> [1,5,1],
> [4,2,1]
> ]
> Output: 7
> Explanation: Because the path 1→3→1→1→1 minimizes the sum.
> ```

**AS:**

```c++
int dp[110][110]={0};
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        if(grid.empty()) return 0;
        
        for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[0].size();j++){
                if(i>0 && j>0)grid[i][j]=min(grid[i][j]+grid[i-1][j],grid[i][j]+grid[i][j-1]);
                else if(i>0) grid[i][j]=grid[i][j]+grid[i-1][j];
                else if(j>0) grid[i][j]=grid[i][j]+grid[i][j-1];
            }
        }
        return grid[grid.size()-1][grid[0].size()-1];
    }
    
   
    
};
```

## 66. Plus One

**QS:**

> Given a **non-empty** array of digits representing a non-negative integer, plus one to the integer.
>
> 
>
> The digits are stored such that the most significant digit is at the head of the list, and each element in the array contain a single digit.
>
> 
>
> You may assume the integer does not contain any leading zero, except the number 0 itself.
>
> 
>
> **Example 1:**
>
> 
>
> ```c++
> Input: [1,2,3]
> Output: [1,2,4]
> Explanation: The array represents the integer 123.
> ```
>
> 
>
> **Example 2:**
>
> 
>
> ```c++
> Input: [4,3,2,1]
> Output: [4,3,2,2]
> Explanation: The array represents the integer 4321.
> ```

**AS:**

```c++
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int c=0,sum=0;
        for(int i=digits.size()-1;i>=0;i--){
            if(i==digits.size()-1) sum=digits[i]+c+1;
            else sum=digits[i]+c;
            digits[i]=sum%10;
            c=sum/10;
        }
        if(c!=0) digits.insert(digits.begin(),c);
        return digits;
    }
};
```

## 67. Add Binary

**QS:**

> Given two binary strings, return their sum (also a binary string).
>
> 
>
> The input strings are both **non-empty** and contains only characters `1` or `0`.
>
> 
>
> **Example 1:**
>
> 
>
> ```c++
> Input: a = "11", b = "1"
> Output: "100"
> ```
>
> 
>
> **Example 2:**
>
> 
>
> ```c++
> Input: a = "1010", b = "1011"
> Output: "10101"
> ```

**QS:**

```c++
class Solution {
public:
	string addBinary(string a, string b) {
		int c = 0;
		string result = "";
		auto it = a.rbegin(), it2 = b.rbegin();
		while (it != a.rend() || it2 != b.rend() || c != 0) {
			int sum = 0;
			if (it != a.rend() && it2 != b.rend()) sum = (*it-'0') + (*it2-'0') + c;
			else if (it != a.rend()) sum = (*it-'0') + c;
			else if (it2 != b.rend()) sum = (*it2 -'0')+ c;
			else sum += c;
			c = sum / 2;
			result += sum % 2 + '0';
			it != a.rend() ? ++it : it;
			it2 != b.rend() ? ++it2 : it2;
		}
		std::reverse(result.begin(), result.end());
		return result;
	}
};
```

## 71. Simplify Path

**QS:**

Given an **absolute path** for a file (Unix-style), simplify it. Or in other words, convert it to the **canonical path**.

  

In a UNIX-style file system, a period `.` refers to the current directory. Furthermore, a double period `..` moves the directory up a level. For more information, see: [Absolute path vs relative path in Linux/Unix](https://www.linuxnix.com/abslute-path-vs-relative-path-in-linuxunix/)

  

Note that the returned canonical path must always begin with a slash `/`, and there must be only a single slash `/` between two directory names. The last directory name (if it exists) **must not** end with a trailing `/`. Also, the canonical path must be the **shortest** string representing the absolute path.

  

 

  

**Example 1:**

  

```c++
Input: "/home/"
Output: "/home"
Explanation: Note that there is no trailing slash after the last directory name.
```

  

**Example 2:**

  

```c++
Input: "/../"
Output: "/"
Explanation: Going one level up from the root directory is a no-op, as the root level is the highest level you can go.
```

  

**Example 3:**

  

```c++
Input: "/home//foo/"
Output: "/home/foo"
Explanation: In the canonical path, multiple consecutive slashes are replaced by a single one.
```

  

**Example 4:**

  

```c++
Input: "/a/./b/../../c/"
Output: "/c"
```

  

**Example 5:**

  

```c++
Input: "/a/../../b/../c//.//"
Output: "/c"
```

  

**Example 6:**

  

```c++
Input: "/a//b////c/d//././/.."
Output: "/a/b/c"
```

**AS:**

```c++
class Solution {
public:
    string simplifyPath(string path) {
        if(path.empty()) return path;
        vector<string> st;
        
        for(int i=1;i<path.size();i++){
            if(path[i]=='/') continue;
            string temp="";
            while(i<path.size() && path[i]!='/') temp+=path[i++];
            if(temp!=string("..") && temp!=string(".")) st.push_back(temp);
            else if(temp==".."){
                if(!st.empty()) st.pop_back();
            }
        }
        string result="/";
        for(int i=0;i<st.size();i++){
            if(i!=0){
                result+="/";
                
            }
            result+=st[i];
        }
        return result;
    }
};
```

<font color="#dd0000">注意点当st为空的时候，输出应为根目录</font>

## 73. Set Matrix Zeroes

**QS:**

> Given a *m* x *n* matrix, if an element is 0, set its entire row and column to 0. Do it [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm).
>
> 
>
> **Example 1:**
>
> 
>
> ```c++
> Input: 
> [
> [1,1,1],
> [1,0,1],
> [1,1,1]
> ]
> Output: 
> [
> [1,0,1],
> [0,0,0],
> [1,0,1]
> ]
> ```
>
> 
>
> **Example 2:**
>
> 
>
> ```c++
> Input: 
> [
> [0,1,2,0],
> [3,4,5,2],
> [1,3,1,5]
> ]
> Output: 
> [
> [0,0,0,0],
> [0,4,5,0],
> [0,3,1,0]
> ]
> ```
>
> 
>
> **Follow up:**
>
> 
>
> - A straight forward solution using O(*m**n*) space is probably a bad idea.
> - A simple improvement uses O(*m* + *n*) space, but still not the best solution.
> - Could you devise a constant space solution?

**AS:**

```c++
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        if(matrix.empty()) return;
        int row=matrix.size();
        int col=matrix[0].size();
        vector<int> rvis(row+10,0);
        vector<int> cvis(col+10,0);
        for(int i=0;i<row;i++){
            for(int j=0;j<col;j++){
                if(matrix[i][j]==0){
                    rvis[i]=1;
                    cvis[j]=1;
                    
                }
            }
        }
        for(int i=0;i<row;i++){
            for(int j=0;j<col;j++){
                if(rvis[i] || cvis[j]) matrix[i][j]=0;
            }
        }
    }
};
```



## 74. Search a 2D Matrix

**QS:**

> Write an efficient algorithm that searches for a value in an *m* x *n* matrix. This matrix has the following properties:
>
> 
>
> - Integers in each row are sorted from left to right.
> - The first integer of each row is greater than the last integer of the previous row.
>
> 
>
> **Example 1:**
>
> 
>
> ```c++
> Input:
> matrix = [
> [1,   3,  5,  7],
> [10, 11, 16, 20],
> [23, 30, 34, 50]
> ]
> target = 3
> Output: true
> ```
>
> 
>
> **Example 2:**
>
> 
>
> ```c++
> Input:
> matrix = [
> [1,   3,  5,  7],
> [10, 11, 16, 20],
> [23, 30, 34, 50]
> ]
> target = 13
> Output: false
> ```

**AS:**

```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.empty()) return false;
        int row=matrix.size();
        int col=matrix[0].size();
        int left=0,right=row*col-1;
        while(left<=right){
            int mid=(left+right)/2;
            int i=mid/col;
            int j=mid%col;
            if(matrix[i][j]==target) return true;
            else if(matrix[i][j]<target) left=mid+1;
            else right=mid-1;
        }
        return false;
    }
};
```

<font color = "#dd0000">利用二分法的思想</font>



## 77. Combinations

**QS:**

> Given two integers *n* and *k*, return all possible combinations of *k* numbers out of 1 ... *n*.
>
> 
>
> **Example:**
>
> 
>
> ```c++
> Input: n = 4, k = 2
> Output:
> [
> [2,4],
> [3,4],
> [2,3],
> [1,2],
> [1,3],
> [1,4],
> ]
> ```

**AS:**

```c++
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int> > result;
        if(n==0) return result;
        vector<int> temp;
        gene(n,k,0,result,temp,1);
        return result;
    }
    
    void gene(int n,int k,int num,vector<vector<int> > &result,vector<int> &temp,int index){
        if(num==k){
            result.push_back(temp);
            return ;
        }
        if(n-num<k-num) return;
        for(int i=index;i<=n;i++){
            temp.push_back(i);
            gene(n,k,num+1,result,temp,i+1);
            temp.pop_back();
        }
    }
};
```

<font color="#dd0000">思想：利用回溯法</font>

## 78. Subsets

**QS:**

> Given a set of **distinct** integers, *nums*, return all possible subsets (the power set).
>
> 
>
> **Note:** The solution set must not contain duplicate subsets.
>
> 
>
> **Example:**
>
> 
>
> ```c++
> Input: nums = [1,2,3]
> Output:
> [
> [3],
> [1],
> [2],
> [1,2,3],
> [1,3],
> [2,3],
> [1,2],
> []
> ]
> ```

**AS:**

```c++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int> > result;
        vector<int> temp;
        result.push_back(vector<int> (0));
        if(nums.empty()) return result;
        for(int i=1;i<=nums.size();i++){
            temp.clear();
            gene(i,0,nums,temp,result,0);
        }
        return result;
    }
    
    void gene(int k,int num,vector<int> &nums,vector<int> &temp,vector<vector<int> > &result,int index){
        if(num==k){
            result.push_back(temp);
            return ;
        }
        if(nums.size()-num<k-num) return ;
        for(int i=index;i<nums.size();i++){
            temp.push_back(nums[i]);
            gene(k,num+1,nums,temp,result,i+1);
            temp.pop_back();
        }
    }
};
```





## 82. Remove Duplicates from Sorted List II

**QS:**

> Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only *distinct* numbers from the original list.
>
> 
>
> **Example 1:**
>
> 
>
> ```c++
> Input: 1->2->3->3->4->4->5
> Output: 1->2->5
> ```
>
> 
>
> **Example 2:**
>
> 
>
> ```c++
> Input: 1->1->1->2->3
> Output: 2->3
> ```

**AS:**

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(head==NULL) return head;
        ListNode* result=new ListNode(0);
        int pre=head->val;
        ListNode *p,*t,*q;
        p=head;
        q=result;
        while(p){
            int num=0;
            while(p && p->val==pre){num++;t=p;p=p->next;}
            if(p!=NULL) pre=p->val;
            if(t && num<2){
                t->next=q->next;
                q->next=t;
                q=t;
            }
        }
        return result->next;
    }
};
```



## 119. [Pascal's Triangle II](https://leetcode-cn.com/problems/pascals-triangle-ii)

**QS:**

Given a non-negative index k where k ≤ 33, return the kth index row of the Pascal's triangle.

Note that the row index starts from 0.

  

![img](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)

In Pascal's triangle, each number is the sum of the two numbers directly above it.

Example:

Input: 3
Output: [1,3,3,1]

Follow up:

Could you optimize your algorithm to use only O(k) extra space?

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/pascals-triangle-ii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

**AS:**

```c++
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector<int> result;
        
        
            for(int i=0;i<=rowIndex;i++){
                result.push_back(getNum(rowIndex,min(i,rowIndex-i)));
            }
        
        return result;
    }
    
    int getNum(int n,int m){
        if(m==0) return 1;
        double sum1=1;
        for(int i=1;i<=m;i++)
            sum1*=(double)(n+1-i)/i;
         return (int)(sum1+0.05);
    }
   
};
```

==杨辉三角的关键解决思路，第i行第j个数，相当于组合排列数为 *C(n-1，m-1)*==



## 150. [Evaluate Reverse Polish Notation](https://leetcode-cn.com/problems/evaluate-reverse-polish-notation)

**QS:**

> Evaluate the value of an arithmetic expression in Reverse Polish Notation.
>
> Valid operators are +, -, *, /. Each operand may be an integer or another expression.
>
> Note:
>
> ```
> Division between two integers should truncate toward zero.
> The given RPN expression is always valid. That means the expression would always evaluate to a result and there won't be any divide by zero operation.
> ```
>
> Example 1:
>
> Input: ["2", "1", "+", "3", "*"]
> Output: 9
> Explanation: ((2 + 1) * 3) = 9
>
> Example 2:
>
> Input: ["4", "13", "5", "/", "+"]
> Output: 6
> Explanation: (4 + (13 / 5)) = 6
>
> Example 3:
>
> Input: ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
> Output: 22
> Explanation: 
>   ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
> = ((10 * (6 / (12 * -11))) + 17) + 5
> = ((10 * (6 / -132)) + 17) + 5
> = ((10 * 0) + 17) + 5
> = (0 + 17) + 5
> = 17 + 5
> = 22
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/evaluate-reverse-polish-notation
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



**AS：**

```c++
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> st;
        for(int i=0;i<tokens.size();i++){
            if(tokens[i]==string("+") || tokens[i]==string("-") || tokens[i]==string("*") || tokens[i]==string("/")){
                int num1=st.top();st.pop();
                int num2=st.top();st.pop();
                if(tokens[i]==string("+")) st.push(num1+num2);
                else if(tokens[i]==string("*")) st.push(num1*num2);
                else if(tokens[i]==string("/")) st.push(num2/num1);
                else st.push(num2-num1);
            }else{
                st.push(str2i(tokens[i]));
            }
        }
        return st.top();
    }
    
    
    int str2i(string &str){
        bool flag=false;
        if(str.front()=='-') {
            flag=true;
            str.erase(str.begin());
        }
        int sum=0;
        for(int i=0;i<str.size();i++){
            sum=sum*10+str[i]-'0';
        }
        return flag?-sum:sum;
    }
};
```

