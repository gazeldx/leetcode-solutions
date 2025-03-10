# 392. Is Subsequence
LeetCode link: [392. Is Subsequence](https://leetcode.com/problems/is-subsequence/)

## LeetCode problem description
> Given two strings `s` and `t`, return `true` if `s` is a **subsequence** of `t`, or `false` otherwise.

A **subsequence** of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

```
Example 1:
Input: s = "abc", t = "ahbgdc"
Output: true

Example 2:
Input: s = "axc", t = "ahbgdc"
Output: false

Constraints:
1. 0 <= s.length <= 100
2. 0 <= t.length <= 10^4
3. `s` and `t` consist only of lowercase English letters.
```

## Thoughts
* This problem can be solved by using `two pointers`, but here we will use another way.
* It is a question of comparing two strings. After doing similar questions many times, we will develop an intuition to use dynamic programming with two-dimensional arrays.

### Common steps in dynamic programming
These five steps are a pattern for solving `dynamic programming` problems.

1. Determine the **meaning** of the `dp[i][j]`
    * Since there are two strings, we can use two-dimensional arrays as the default option.
    * At first, try to use the problem's `return` value as the value of `dp[i][j]` to determine the meaning of `dp[i][j]`. If it doesn't work, try another way.
    * `dp[i][j]` represents whether the first `i` letters of `s` are a subsequence of `t`'s first `j` letters.
    * `dp[i][j]` is `true` or `false`.
2. Determine the `dp` array's initial value
    * Use an example:
   ```
   After initialization, the 'dp' array would be:
   s = "abc", t = "ahbgdc"
   #     a h b g d c
   #   T T T T T T T # dp[0]
   # a F F F F F F F
   # b F F F F F F F
   # c F F F F F F F
   ```
    * `dp[0][j] = true` because `dp[0]` represents the empty string, and empty string is a subsequence of any string.
    * `dp[i][j] = false (i != 0)`.
3. Determine the `dp` array's recurrence formula
    * Try to complete the `dp` grid. In the process, you will get inspiration to derive the formula.
   ```
   1. s = "a", t = "ahbgdc" 
   #     a h b g d c
   #   T T T T T T T
   # a F T T T T T T # dp[1]
   ```
   ```
   2. s = "ab", t = "ahbgdc" 
   #     a h b g d c
   #   T T T T T T T
   # a F T T T T T T
   # b F F F T T T T
   ```
   ```
   3. s = "abc", t = "ahbgdc"
   #     a h b g d c
   #   T T T T T T T
   # a F T T T T T T
   # b F F F T T T T
   # c F F F F F F T # dp[3]
   ```
    * When analyzing the sample `dp` grid, remember there are three important points which you should pay special attention to: `dp[i - 1][j - 1]`, `dp[i - 1][j]` and `dp[i][j - 1]`. The current `dp[i][j]` often depends on them.
    * If the question is also true in reverse (swap `s` and `t`), and we need to use `dp[i - 1][j]` or `dp[i][j - 1]`, then we probably need to use both of them.
    * We can derive the `Recurrence Formula`:
   ```
   if s[i - 1] == t[j - 1]
     dp[i][j] = dp[i - 1][j - 1]
   else
     dp[i][j] = dp[i][j - 1]
   ```
4. Determine the `dp` array's traversal order
    * `dp[i][j]` depends on `dp[i - 1][j - 1]` and `dp[i][j - 1]`, so we should traverse the `dp` array from top to bottom, then from left to right.
5. Check the `dp` array's value
    * Print the `dp` to see if it is as expected.

### Complexity
* Time: `O(n * m)`.
* Space: `O(n * m)`.

## Java
```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        var dp = new boolean[s.length() + 1][t.length() + 1];
        Arrays.fill(dp[0], true);

        for (var i = 1; i < dp.length; i++) {
            for (var j = 1; j < dp[0].length; j++) {
                if (s.charAt(i - 1) == t.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = dp[i][j - 1];
                }
            }
        }

        return dp[dp.length - 1][dp[0].length - 1];
    }
}
```

## C#
```c#
public class Solution
{
    public bool IsSubsequence(string s, string t)
    {
        var dp = new bool[s.Length + 1, t.Length + 1];
        for (var j = 0; j < dp.GetLength(1); j++)
            dp[0, j] = true;
        
        for (var i = 1; i < dp.GetLength(0); i++)
        {
            for (var j = 1; j < dp.GetLength(1); j++)
            {
                if (s[i - 1] == t[j - 1])
                {
                    dp[i, j] = dp[i - 1, j - 1];
                }
                else
                {
                    dp[i, j] = dp[i, j - 1];
                }
            }
        }

        return dp[dp.GetUpperBound(0), dp.GetUpperBound(1)];
    }
}
```

## Python
```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        column_count = len(t) + 1
        dp = [[True] * column_count]
        for _ in s:
            dp.append([False] * column_count)
        
        for i in range(1, len(dp)):
            for j in range(1, len(dp[0])):
                if s[i - 1] == t[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1]
                else:
                    dp[i][j] = dp[i][j - 1]
        
        return dp[-1][-1]
```

## C++
```cpp
class Solution {
public:
    bool isSubsequence(string s, string t) {
        vector<vector<bool>> dp(s.size() + 1, vector<bool>(t.size() + 1));
        fill(dp[0].begin(), dp[0].end(), true);

        for (auto i = 1; i < dp.size(); i++) {
            for (auto j = 1; j < dp[0].size(); j++) {
                if (s[i - 1] == t[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = dp[i][j - 1];
                }
            }
        }

        return dp[dp.size() - 1][dp[0].size() - 1];
    }
};
```

## JavaScript
```javascript
var isSubsequence = function (s, t) {
  const dp = Array(s.length + 1).fill().map(
    () => Array(t.length + 1).fill(false)
  )
  dp[0].fill(true)

  for (let i = 1; i < dp.length; i++) {
    for (let j = 1; j < dp[0].length; j++) {
      if (s[i - 1] == t[j - 1]) {
        dp[i][j] = dp[i - 1][j - 1]
      } else {
        dp[i][j] = dp[i][j - 1]
      }
    }
  }

  return dp.at(-1).at(-1)
};
```

## Go
```go
func isSubsequence(s string, t string) bool {
    dp := make([][]bool, len(s) + 1)
    columnSize := len(t) + 1
    dp[0] = slices.Repeat([]bool{true}, columnSize)
    for i := 1; i < len(dp); i++ {
        dp[i] = make([]bool, columnSize)
    }

    for i := 1; i < len(dp); i++ {
        for j := 1; j < len(dp[0]); j++ {
            if s[i - 1] == t[j - 1] {
                dp[i][j] = dp[i - 1][j - 1]
            } else {
                dp[i][j] = dp[i][j - 1]
            }
        }
    }

    return dp[len(dp) - 1][len(dp[0]) - 1]
}
```

## Ruby
```ruby
def is_subsequence(s, t)
  dp = Array.new(s.size + 1) do |i|
    Array.new(t.size + 1, i == 0 ? true : false)
  end

  (1...dp.size).each do |i|
    (1...dp[0].size).each do |j|
      dp[i][j] =
        if s[i - 1] == t[j - 1]
          dp[i - 1][j - 1]
        else
          dp[i][j - 1]
        end
    end
  end

  dp[-1][-1]
end
```

## Rust
```rust
// Welcome to create a PR to complete the code of this language, thanks!
```

## Other languages
```
// Welcome to create a PR to complete the code of this language, thanks!
```
