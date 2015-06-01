---
layout: post
title:  "Maximum Subarray Problem"
date:   2015-06-01 00:08:00
categories: algorithms java 
---
>Given a array of numeric values, including both positive and negative values. Find out the sub-array (consecutive elements in the array) whose sum has the maximum value. 

The problem is given as an example of divide and conquer approach in the book "Introduction to Algorithms". The author gave a solution with time complexity of nLog(n). However as a exercise in the textbook, it also could be solved with linear time complexity.

The hint is to consider the relationship between the maximum sub-array ending at i and the maximum sub-array ending at i+1. Suppose we already have maximum sub-array ending at i with sum S(i), then it's not hard to realize the sum of maximum sub-array ending at i+1 will satisfy:

S(i+1) = Max(S(i)+A[i+1], A[i+1])

With initial condition S(0) = A[0], suppose empty sub-array is not allowed, we then can compute S(1), S(2), S(3), ...

The java implementation of the solution is like below.
{% highlight java %}
public class MaximumSubArraySolution {

    /**
     * @param a
     *            the original array
     * @return a array of three elements, the start index, the exclusive end
     *         index, the sum of the sub-array
     */
    public int[] getMaximumSubArray(int[] a) {

        if (a == null || a.length == 0) {
            return new int[] { -1, -1, 0 };
        } else if (a.length == 1) {
            return new int[] { 0, 1, a[0] };
        }

        int start = -1, end = -1, sum = 0;
        // s[i] = [start_idx, value]
        int[][] s = new int[a.length][2];
        s[0][0] = 0;
        s[0][1] = a[0];
        for (int i = 1, len = a.length; i < len; i++) {
            if (s[i - 1][1] > 0) {
                s[i][0] = s[i - 1][0];
                s[i][1] = s[i - 1][1] + a[i];
            } else {
                s[i][0] = i;
                s[i][1] = a[i];
            }
            if (s[i][1] > sum) {
                sum = s[i][1];
                start = s[i][0];
                end = i + 1;
            }
        }
        return new int[] { start, end, sum };
    }
}
{% endhighlight %}

