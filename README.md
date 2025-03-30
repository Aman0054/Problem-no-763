# Partition Labels

## Problem Statement
Given a string `s`, we want to partition it into as many parts as possible so that each letter appears in at most one part. The partition should be done in such a way that after concatenating all parts in order, the resultant string should be `s`.

### Example 1:
**Input:**
```plaintext
s = "ababcbacadefegdehijhklij"
```
**Output:**
```plaintext
[9,7,8]
```
**Explanation:**
The partition is `["ababcbaca", "defegde", "hijhklij"]`. Each letter appears in at most one part.

### Example 2:
**Input:**
```plaintext
s = "eccbbbbdec"
```
**Output:**
```plaintext
[10]
```

---
## Approach
The problem is solved using **Greedy Algorithm** and **Hashing**.

### Steps:
1. **Find Last Occurrence of Each Character:**
   - Traverse the string and store the last index of each character in an array `lastIndex[26]` (since only lowercase English letters are given).

2. **Partition the String:**
   - Use two pointers: `start` (beginning of the current partition) and `end` (furthest index the current partition should reach).
   - Iterate through the string:
     - For each character, update `end` to be the maximum of `end` and its last occurrence index.
     - If `i` reaches `end`, a partition is found.
     - Store the length of the partition and update `start` for the next partition.

3. **Return the Partition Sizes:**
   - The result is stored in a list and returned.

---
## Code Implementation
```java
class Solution {
    public List<Integer> partitionLabels(String s) {
        int n = s.length();
        List<Integer> result = new ArrayList<>();
        
        // Last occurrence of each character
        int[] lastIndex = new int[26];
        for (int i = 0; i < n; i++) {
            lastIndex[s.charAt(i) - 'a'] = i;
        }
        
        int i = 0;
        int start = 0;
        int end = 0;
        while (i < n) {
            end = Math.max(end, lastIndex[s.charAt(i) - 'a']);
            if (i == end) {
                result.add(end - start + 1);
                start = end + 1;
            }
            i++;
        }
        
        return result;
    }
}
```

---
## Complexity Analysis
- **Time Complexity:** `O(N)`
  - The first pass (to store last occurrences) takes `O(N)`.
  - The second pass (to find partitions) takes `O(N)`.
  - So, overall complexity is `O(N)`.

- **Space Complexity:** `O(1)`
  - The `lastIndex` array has a fixed size of 26 (constant space).
  - The result list contains at most 26 partitions in the worst case, which is still `O(1)`.

---
## Intuition
- By tracking the last appearance of each character, we ensure that a character does not appear outside its designated partition.
- We extend the current partition until we reach the furthest last occurrence of any character encountered so far.
- When we reach the `end` of the partition, we know no further characters in this segment appear elsewhere, so we finalize it.
- This ensures **maximum partitions** while keeping the constraint that each character appears in only one partition.

---
## Edge Cases Considered
- Single character string (`s = "a"`) → Output `[1]`.
- String with all identical characters (`s = "aaaaa"`) → Output `[5]`.
- String where each character appears only once (`s = "abcdef"`) → Output `[1,1,1,1,1,1]`.


