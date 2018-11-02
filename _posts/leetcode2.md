---
layout:     post
title:      5分钟带你看完 WWDC 2018
subtitle:   WWDC 2018 Keynote 全记录
date:       2018-06-05
author:     lsq
header-img: img/post-bg-cook.jpg
catalog: true
tags:
    - leetcode
---

'''c
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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int carry = 0, sum = 0;
        ListNode* res = new ListNode(0);
        ListNode* cur = res;
        while(l1 != nullptr || l2 != nullptr)
        {
            sum = 0;
            if(l1 != nullptr) sum += l1->val, l1 = l1->next;   
            if(l2 != nullptr) sum += l2->val, l2 = l2->next;
            sum += carry;
            carry = sum / 10;
            cur->next = new ListNode(sum % 10);
            cur = cur->next;
        }
        if(carry > 0) cur->next = new ListNode(carry);
        
        return res->next;
    }
};
'''
