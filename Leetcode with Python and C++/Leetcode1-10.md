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
