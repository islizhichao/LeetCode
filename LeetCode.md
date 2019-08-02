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