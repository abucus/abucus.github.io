---
layout: post
title:  "House Robber Problem"
date:   2015-05-31 23:18:20
categories: algorithms java 
---
>You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night. The goal is to give a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

This [solution][original-solution] is given by David Goldstein. Suppose A[i] denote the money we can robber from house i, P(k) denote the maximum amout of money we robber from total k houses, then the general idea is to consider the relationship between of P(k-1) and P(k). Actually there only two values P(k) might have:

P(k) = P(k-1) or P(k) = P(k-2) + A[k]

As initial condition, we have P(0) = A[0], here we assume the index of the house array starts from 0, and we have P(Empty Array) = 0, or you can use P(1) = max(A[0],A[1]).

Then we can progress this to P(1),P(2),P(3),... 

The java implementation is like below. We don't need to store all the values of P(k), to compute the value of P(k), we only need P(k-2), P(k-1) and A[k].

{% highlight java %}
public class Solution {
    private int max(int a, int b){
        return a>b?a:b;
    }
    public int rob(int[] nums) {
        if(nums == null || nums.length == 0){
            return 0;
        } else if(nums.length<3){
            int t = nums[1%nums.length];
            return nums[0]>t? nums[0]:t;
        }
        int[] p = {0,nums[0]};
        int cur = 1;
        while(cur<nums.length){
            p[(cur+1)%2] = this.max(p[cur%2], p[(cur+1)%2]+nums[cur]);
            cur++;
        }
        return this.max(p[0],p[1]);
    }
}
{% endhighlight %}




[original-solution]:http://www.quora.com/A-robber-enters-a-colony-of-houses-numbered-from-1-to-n-Every-house-has-a-number-printed-on-the-top-of-it-That-number-is-the-amount-of-money-inside-that-house-However-there-is-one-constraint-If-the-robber-robs-the-i-th-house-he-cant-rob-house-no-i-1-and-house-no-i+1-How-can-the-robber-maximise-his
