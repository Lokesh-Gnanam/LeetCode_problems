# LEETCODE PROBLEMS 2026

## 1888. Minimum Number of Flips to Make the Binary String Alternating [leetcode](https://leetcode.com/problems/minimum-number-of-flips-to-make-the-binary-string-alternating/description/?envType=daily-question&envId=2026-03-07)
Solved
Medium
Topics
premium lock icon
Companies
Hint
You are given a binary string s. You are allowed to perform two types of operations on the string in any sequence:

Type-1: Remove the character at the start of the string s and append it to the end of the string.
Type-2: Pick any character in s and flip its value, i.e., if its value is '0' it becomes '1' and vice-versa.
Return the minimum number of type-2 operations you need to perform such that s becomes alternating.

The string is called alternating if no two adjacent characters are equal.

For example, the strings "010" and "1010" are alternating, while the string "0100" is not.
 

Example 1:

Input: s = "111000"
Output: 2
Explanation: Use the first operation two times to make s = "100011".
Then, use the second operation on the third and sixth elements to make s = "101010".
Example 2:

Input: s = "010"
Output: 0
Explanation: The string is already alternating.
Example 3:

Input: s = "1110"
Output: 1
Explanation: Use the second operation on the second element to make s = "1010".


```
class Solution {
public:
    int minFlips(string s) {
        int n = s.size();
        string t = s + s;

        int ans = n;
        int mis0 = 0;

        for(int i = 0; i < 2*n; i++) {

            char expected = (i % 2 == 0) ? '0' : '1';

            if(t[i] != expected) mis0++;

            if(i >= n) {
                int left = i - n;
                char exp_left = (left % 2 == 0) ? '0' : '1';
                if(t[left] != exp_left) mis0--;
            }

            if(i >= n - 1) {
                int mis1 = n - mis0;
                ans = min(ans, min(mis0, mis1));
            }
        }

        return ans;
    }
};
```

----------------------------
--------------------------

## 1980. Find Unique Binary String [leetcode](https://leetcode.com/problems/find-unique-binary-string/solutions/7633717/beginner-friendly-solution-how-to-approa-u6yf/?envType=daily-question&envId=2026-03-08)
Medium
Topics
premium lock icon
Companies
Hint
Given an array of strings nums containing n unique binary strings each of length n, return a binary string of length n that does not appear in nums. If there are multiple answers, you may return any of them.

 

Example 1:

Input: nums = ["01","10"]
Output: "11"
Explanation: "11" does not appear in nums. "00" would also be correct.
Example 2:

Input: nums = ["00","01"]
Output: "11"
Explanation: "11" does not appear in nums. "10" would also be correct.
Example 3:

Input: nums = ["111","011","001"]
Output: "101"
Explanation: "101" does not appear in nums. "000", "010", "100", and "110" would also be correct.
 

Constraints:

n == nums.length
1 <= n <= 16
nums[i].length == n
nums[i] is either '0' or '1'.
All the strings of nums are unique.

```
class Solution {
public:
    string findDifferentBinaryString(vector<string>& nums) {
        int n = nums.size();
        int size = pow(2, n);

        vector<int> nu(size, 0);

        for(string num : nums){
            int val = stoi(num, nullptr, 2);
            nu[val]++;
        }

        for(int i = 0; i < size; i++){
            if(nu[i] == 0){
                string ans = bitset<32>(i).to_string();
                ans = ans.substr(32 - n);
                return ans;
            }
        }

        return string(n, '0');
    }
};
```
-------------------------
-------------------------

## 3129. Find All Possible Stable Binary Arrays I  [leetcode](https://leetcode.com/problems/find-all-possible-stable-binary-arrays-i/description/?envType=daily-question&envId=2026-03-09)
Medium
Topics
premium lock icon
Companies
Hint
You are given 3 positive integers zero, one, and limit.

A binary array arr is called stable if:

The number of occurrences of 0 in arr is exactly zero.
The number of occurrences of 1 in arr is exactly one.
Each subarray of arr with a size greater than limit must contain both 0 and 1.
Return the total number of stable binary arrays.

Since the answer may be very large, return it modulo 109 + 7.

 

Example 1:

Input: zero = 1, one = 1, limit = 2

Output: 2

Explanation:

The two possible stable binary arrays are [1,0] and [0,1], as both arrays have a single 0 and a single 1, and no subarray has a length greater than 2.

Example 2:

Input: zero = 1, one = 2, limit = 1

Output: 1

Explanation:

The only possible stable binary array is [1,0,1].

Note that the binary arrays [1,1,0] and [0,1,1] have subarrays of length 2 with identical elements, hence, they are not stable.

Example 3:

Input: zero = 3, one = 3, limit = 2

Output: 14

Explanation:

All the possible stable binary arrays are [0,0,1,0,1,1], [0,0,1,1,0,1], [0,1,0,0,1,1], [0,1,0,1,0,1], [0,1,0,1,1,0], [0,1,1,0,0,1], [0,1,1,0,1,0], [1,0,0,1,0,1], [1,0,0,1,1,0], [1,0,1,0,0,1], [1,0,1,0,1,0], [1,0,1,1,0,0], [1,1,0,0,1,0], and [1,1,0,1,0,0].

 

Constraints:

1 <= zero, one, limit <= 200

```
class Solution {
public:
    int numberOfStableArrays(int zero, int one, int limit) {
        static int MOD = 1e9 + 7;
        vector<vector<int>> dp0(zero + 1, vector<int>(one + 1, 0));
        vector<vector<int>> dp1(zero + 1, vector<int>(one + 1, 0));

        for (int i = 1; i <= min(zero, limit); i++) {
            dp0[i][0] = 1;
        }   

        for (int j = 1; j <= min(one, limit); j++) {
            dp1[0][j] = 1;
        }

        for (int i = 1; i <= zero; i++) {
            for (int j = 1; j <= one; j++) {
                // dp0[i][j]:
                // Add one more zero to states with (i-1, j),
                // then remove the invalid states where the zero-run becomes > limit
                long long val0 = (long long)dp0[i - 1][j] + dp1[i - 1][j];
                if (i - limit - 1 >= 0) {
                    val0 -= dp1[i - limit - 1][j];
                }
                dp0[i][j] = (val0 % MOD + MOD) % MOD;

                // dp1[i][j]:
                // Add one more one to states with (i, j-1),
                // then remove the invalid states where the one-run becomes > limit
                long long val1 = (long long)dp0[i][j - 1] + dp1[i][j - 1];
                if (j - limit - 1 >= 0) {
                    val1 -= dp0[i][j - limit - 1];
                }
                dp1[i][j] = (val1 % MOD + MOD) % MOD;
            }
            
        }
        return ((long long)dp0[zero][one] + dp1[zero][one]) % MOD;
    }
};

```

---------------------------------------------
---------------------------------------------

## 

