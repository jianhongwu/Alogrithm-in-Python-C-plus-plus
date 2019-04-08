1. Two Sum
查找表法，O(n)空间O(n)时间
```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> record;
        for(int i = 0; i < nums.size(); i++){
            if(record.find(target-nums[i]) != record.end())
                return {i,record[target-nums[i]]};
            record[nums[i]] = i;
        }
        return {};
    }
};
```
```
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        record = {}
        for i in range(len(nums)):
            if record.get(target-nums[i]) is not None:
                return [i,record[target-nums[i]]]
            record[nums[i]] = i
        return []
```

2. Add Two Numbers
链表的大数加法
```
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        if(!l1)
            return l2;
        if(!l2)
            return l1;
        
        ListNode* dummyhead = new ListNode(-1);
        ListNode* cur = dummyhead;
        int carry = 0;
        while(l1 || l2){
            int add1 = l1 ? l1->val:0;
            int add2 = l2 ? l2->val:0;
            int temp_sum = add1+add2+carry;
            carry = temp_sum/10;
            
            cur->next = new ListNode(temp_sum%10);
            cur = cur->next;
            
            if(l1)
                l1 = l1->next;
            if(l2)
                l2 = l2->next;
        }
        if(carry)
            cur->next = new ListNode(carry);
        
        return dummyhead->next;
    }
};
```
- python3 //整除
- if else  python三元表达式
```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        if not l1:
            return l2
        if not l2:
            return l1
        
        dummyhead = ListNode(-1)
        cur = dummyhead
        carry = 0
        
        while l1 or l2:
            add1 = l1.val if l1 else 0
            add2 = l2.val if l2 else 0
            temp_sum = add1+add2+carry
            
            carry = temp_sum//10
            cur.next = ListNode(temp_sum%10)
            cur = cur.next
            
            if l1:
                l1 = l1.next
            if l2:
                l2 = l2.next
        if carry:
            cur.next = ListNode(carry)
            
        return dummyhead.next
```

3. Longest substring without repeating characters
最长不重复子串，hash+滑动窗口
```
//hash记录出现次数
class Solution {
public:
	int lengthOfLongestSubstring(string s) {
        if(s.size() <= 1)
            return s.size();
        
        int left = 0, right = 0;
        int res = 0;
        vector<int> freq(256,0);
        for(right; right < s.size(); right++){
            while(freq[s[right]] != 0){
                left++;
                freq[s[left-1]]--;
            }
            res = max(res,right-left+1);
            freq[s[right]]++;
        }
        return res;
	}
};

//hash记录下标
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        if(s.size() <= 1)
            return s.size();
        vector<int> hash(256,-1);
        
        int res = 0;
        int left = -1, right = 0;
        for(right; right < s.size(); right++){
            left = max(left,hash[s[right]]);
            hash[s[right]] = right;
            res = max(res,right-left);
        }
        return res;
    }
};
```

```
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        
        dic, res, start = {}, 0, 0
        for i, c in enumerate(s):
            start = max(start, dic.get(c,-1)+1) # advance start if necessary
            res = max(res, i-start+1) # update result
            dic[c] = i
        return res
```

4. Median of Two Sorted Arrays 
归并排序解法
```
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(), n = nums2.size();
        int left = (m+n-1)/2, right = (m+n)/2;
        
        return (findK(nums1,nums2,left)+findK(nums1,nums2,right))/2.0;
    }
    
    int findK(vector<int>& nums1, vector<int>& nums2, int k){
        int index1 = 0, index2 = 0;
        k = k+1;
        int res = 0;
        while(k--){
            if(index1 >= nums1.size())
                res = nums2[index2++];
            else if(index2 >= nums2.size())
                res = nums1[index1++];
            else if(nums1[index1] <= nums2[index2])
                res = nums1[index1++];
            else
                res = nums2[index2++];
        }
        return res;
    }
};
```

```
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        m, n = len(nums1), len(nums2)
        left = (m+n-1)//2
        right = (m+n)//2
        
        return (self.findK(nums1,nums2,left)+self.findK(nums1,nums2,right))/2
    
    def findK(self,nums1,nums2,k):
        index1, index2 = 0, 0
        k = k+1
        res = 0
        
        while k:
            if index1 >= len(nums1):
                res = nums2[index2]
                index2 += 1
            elif index2 >= len(nums2):
                res = nums1[index1]
                index1 += 1
            elif nums1[index1] <= nums2[index2]:
                res = nums1[index1]
                index1 += 1
            else:
                res = nums2[index2]
                index2 += 1
            k -= 1
        return res
```

5. Longest palindromic Substring
