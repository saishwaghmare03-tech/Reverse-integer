# 🔄 Reverse Integer — LeetCode 7

A clean, efficient, and overflow-safe Java solution for LeetCode Problem 7: **Reverse Integer**.

## 📌 Problem Statement

Given a signed 32-bit integer `x`, return `x` with its digits reversed. If reversing `x` causes the value to go outside the signed 32-bit integer range $[-$2,147,483,648, $2,147,483,647]$, then return `0`.

> **Constraint:** Assume the environment does not allow you to store 64-bit integers (longs).

### Examples

| Input ($x$) | Output | Explanation |
| :--- | :--- | :--- |
| `123` | `321` | Standard positive integer reversal |
| `-123` | `-321` | Preserves the negative sign |
| `120` | `21` | Trailing zeros are dropped automatically |

---

## 💡 Approach & Intuition

To reverse an integer without using 64-bit integers (`long`), we pop digits off the end of the input integer one by one using modulo arithmetic (`% 10`) and push them onto our reversed integer (`rev = rev * 10 + digit`).

Because the environment restricts the use of 64-bit integers, we **must check for potential overflow before** performing the multiplication and addition steps:
1. **Positive Overflow Check:** If `rev > Integer.MAX_VALUE / 10`, multiplying by $10$ will strictly exceed the max limit. If `rev == Integer.MAX_VALUE / 10`, adding a `digit > 7` will cause an overflow (since `Integer.MAX_VALUE` is `2147483647`).
2. **Negative Overflow Check:** If `rev < Integer.MIN_VALUE / 10`, multiplying by $10$ will fall below the min limit. If `rev == Integer.MIN_VALUE / 10`, adding a `digit < -8` will cause an underflow (since `Integer.MIN_VALUE` is `-2147483648`).

---

## 💻 Java Implementation

```java
class Solution {
    public int reverse(int x) {
        int rev = 0;

        while (x != 0) {
            int digit = x % 10;
            x /= 10;

            // Check for positive overflow before multiplying by 10 and adding digit
            if (rev > Integer.MAX_VALUE / 10 || 
                (rev == Integer.MAX_VALUE / 10 && digit > 7)) {
                return 0;
            }

            // Check for negative overflow before multiplying by 10 and adding digit
            if (rev < Integer.MIN_VALUE / 10 || 
                (rev == Integer.MIN_VALUE / 10 && digit < -8)) {
                return 0;
            }

            rev = rev * 10 + digit;
        }

        return rev;
    }
}
complexity AnalysisTime Complexity: $O(\log_{10}(x))$ — The number of iterations is proportional to the number of digits in $x$.Space Complexity: $O(1)$ — Uses a constant amount of extra space for pointers and variables.
