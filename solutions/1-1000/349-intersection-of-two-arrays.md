# 349. Intersection of Two Arrays - LeetCode Solution
LeetCode problem link: [349. Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays),
[349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays)

[中文题解](#中文题解)

## LeetCode problem description
Given two integer arrays `nums1` and `nums2`, return _an array of their **intersection**_.
Each element in the result must be **unique** and you may return the result in **any order**.

Difficulty: **Easy**

### [Example 1]
**Input**: `nums1 = [1,2,2,1], nums2 = [2,2]`

**Output**: `[2]`

### [Example 2]
**Input**: `nums1 = [4,9,5], nums2 = [9,4,9,8,4]`

**Output**: `[9,4]` or `[4,9]`

### [Constraints]
- `1 <= nums1.length, nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 1000`

## Intuition behind the Solution
[中文题解](#中文题解)

1. Convert one of the arrays to a `set`. The elements are unique in a `set`.
2. When traversing the other array, if the an element is found to already exist in the `set`, it means that the element belongs to the intersection, and the element should be added to the `results`.
3. The `results` is also of `set` type because duplicate removal is required.

## Complexity
* Time: `O(n)`.
* Space: `O(n)`.

## Java
```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        var results = new HashSet<Integer>();
        var num1Set = new HashSet<Integer>();

        for (var num : nums1) {
            num1Set.add(num);
        }

        for (var num : nums2) {
            if (num1Set.contains(num)) {
                results.add(num);
            }
        }

        return results.stream().mapToInt(num -> num).toArray();
    }
}
```

## Python
```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        set_of_nums1 = set(nums1)
        results = set()

        for num in nums2:
            if num in set_of_nums1:
                results.add(num)

        return list(results)
```

## C++
```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> results;
        unordered_set<int> set_of_nums1(nums1.begin(), nums1.end());

        for (auto num : nums2) {
            if (set_of_nums1.contains(num)) {
                results.insert(num);
            }
        }

        return vector<int>(results.begin(), results.end());
    }
};
```

## JavaScript
```javascript
var intersection = function (nums1, nums2) {
  let results = new Set()
  let num1Set = new Set(nums1)

  for (let num of nums2) {
    if (num1Set.has(num)) {
      results.add(num)
    }
  }

  return Array.from(results)
};
```

## C#
```c#
public class Solution
{
    public int[] Intersection(int[] nums1, int[] nums2)
    {
        var results = new HashSet<int>();
        var num1Set = new HashSet<int>();

        foreach (int num in nums1)
            num1Set.Add(num);

        foreach (int num in nums2)
        {
            if (num1Set.Contains(num))
            {
                results.Add(num);
            }
        }

        return results.ToArray();
    }
}
```

## Go
```go
func intersection(nums1 []int, nums2 []int) []int {
    results := map[int]bool{}
    num1Set := map[int]bool{}

    for _, num := range nums1 {
        num1Set[num] = true
    }

    for _, num := range nums2 {
        if _, ok := num1Set[num]; ok {
            results[num] = true
        }
    }

    return slices.Collect(maps.Keys(results))
}
```

## Ruby
```ruby
def intersection(nums1, nums2)
  set_of_nums1 = Set.new(nums1)
  results = Set.new

  nums2.each do |num|
    if set_of_nums1.include?(num)
      results << num
    end
  end

  results.to_a
end
```

## C
```c
// Welcome to create a PR to complete the code of this language, thanks!
```

## Kotlin
```kotlin
// Welcome to create a PR to complete the code of this language, thanks!
```

## Swift
```swift
// Welcome to create a PR to complete the code of this language, thanks!
```

## Rust
```rust
// Welcome to create a PR to complete the code of this language, thanks!
```

## Other languages
```
// Welcome to create a PR to complete the code of this language, thanks!
```

## 问题描述
给定两个数组 `nums1` 和 `nums2` ，返回 _它们的 **交集**_ 。输出结果中的每个元素一定是 **唯一** 的。我们可以 **不考虑输出结果的顺序** 。

难度: **容易**

### [示例 1]
**输入**: `nums1 = [1,2,2,1], nums2 = [2,2]`

**输出**: `[2]`

### [示例 2]
**输入**: `nums1 = [4,9,5], nums2 = [9,4,9,8,4]`

**输出**: `[9,4]` 或者 `[4,9]`

# 中文题解
## 思路
1. 把其中一个数组转为`set`，数据结构`set`的特点是元素不重复。
2. 遍历另一个数组时，如果发现当前元素已经存在于`set`中，则说明该元素属于交集，将该元素加入结果集中。
3. 结果集也采用`set`类型，因为需要去重。