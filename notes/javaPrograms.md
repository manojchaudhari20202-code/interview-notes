
## Array / String

---

### 1. Merge Strings Alternately
**Problem**: Merge two strings by alternating characters, starting with `word1`. Append remaining characters from the longer string.

**Solution**:
```java
public String mergeAlternately(String word1, String word2) {
    StringBuilder merged = new StringBuilder();
    int i = 0, j = 0;
    while (i < word1.length() || j < word2.length()) {
        if (i < word1.length()) merged.append(word1.charAt(i++));
        if (j < word2.length()) merged.append(word2.charAt(j++));
    }
    return merged.toString();
}
```

---

### 2. Greatest Common Divisor of Strings
**Problem**: Find the largest string `x` that divides both `str1` and `str2` (i.e., `str1 = x + x + ...` and same for `str2`).

**Solution**:
```java
public String gcdOfStrings(String str1, String str2) {
    if (!(str1 + str2).equals(str2 + str1)) return "";
    int gcdLen = gcd(str1.length(), str2.length());
    return str1.substring(0, gcdLen);
}

private int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}
```

---

### 3. Kids With the Greatest Number of Candies
**Problem**: After giving `extraCandies` to each kid, determine if that kid can have the greatest number of candies.

**Solution**:
```java
public List<Boolean> kidsWithCandies(int[] candies, int extraCandies) {
    int max = 0;
    for (int c : candies) max = Math.max(max, c);
    List<Boolean> result = new ArrayList<>();
    for (int c : candies) result.add(c + extraCandies >= max);
    return result;
}
```

---

### 4. Can Place Flowers
**Problem**: Can we plant `n` new flowers in a flowerbed without adjacent flowers? (1 = planted, 0 = empty).

**Solution**:
```java
public boolean canPlaceFlowers(int[] flowerbed, int n) {
    int count = 0;
    for (int i = 0; i < flowerbed.length && count < n; i++) {
        if (flowerbed[i] == 0) {
            boolean leftEmpty = (i == 0) || (flowerbed[i - 1] == 0);
            boolean rightEmpty = (i == flowerbed.length - 1) || (flowerbed[i + 1] == 0);
            if (leftEmpty && rightEmpty) {
                flowerbed[i] = 1; // plant
                count++;
                i++; // skip next plot
            }
        }
    }
    return count >= n;
}
```

---

### 5. Reverse Vowels of a String
**Problem**: Reverse only the vowels in the given string.

**Solution**:
```java
public String reverseVowels(String s) {
    char[] chars = s.toCharArray();
    int left = 0, right = chars.length - 1;
    String vowels = "aeiouAEIOU";
    while (left < right) {
        while (left < right && vowels.indexOf(chars[left]) == -1) left++;
        while (left < right && vowels.indexOf(chars[right]) == -1) right--;
        if (left < right) {
            char temp = chars[left];
            chars[left] = chars[right];
            chars[right] = temp;
            left++;
            right--;
        }
    }
    return new String(chars);
}
```

---

### 6. Reverse Words in a String
**Problem**: Reverse the order of words in a string (words are separated by spaces, may have leading/trailing/multiple spaces).

**Solution**:
```java
public String reverseWords(String s) {
    String[] words = s.trim().split("\\s+");
    StringBuilder reversed = new StringBuilder();
    for (int i = words.length - 1; i >= 0; i--) {
        reversed.append(words[i]);
        if (i > 0) reversed.append(" ");
    }
    return reversed.toString();
}
```

---

### 7. Product of Array Except Self
**Problem**: Return an array `output` where `output[i]` is the product of all elements except `nums[i]`, without division and in O(n).

**Solution**:
```java
public int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];
    
    // Left products
    result[0] = 1;
    for (int i = 1; i < n; i++) {
        result[i] = result[i - 1] * nums[i - 1];
    }
    
    // Right products and combine
    int right = 1;
    for (int i = n - 1; i >= 0; i--) {
        result[i] *= right;
        right *= nums[i];
    }
    return result;
}
```

---

### 8. Increasing Triplet Subsequence
**Problem**: Return true if there exists `i < j < k` such that `nums[i] < nums[j] < nums[k]`.

**Solution**:
```java
public boolean increasingTriplet(int[] nums) {
    int first = Integer.MAX_VALUE;
    int second = Integer.MAX_VALUE;
    for (int num : nums) {
        if (num <= first) {
            first = num;          // smallest so far
        } else if (num <= second) {
            second = num;         // second smallest
        } else {
            return true;          // found a number greater than both
        }
    }
    return false;
}
```

---

### 9. String Compression
**Problem**: Compress a character array in-place using counts of consecutive characters. Return the new length.

**Solution**:
```java
public int compress(char[] chars) {
    int write = 0; // position to write compressed result
    int anchor = 0; // start of current group
    for (int read = 0; read < chars.length; read++) {
        // If at end of group or end of array
        if (read + 1 == chars.length || chars[read] != chars[read + 1]) {
            chars[write++] = chars[anchor]; // write the character
            if (read > anchor) { // more than one occurrence
                String count = Integer.toString(read - anchor + 1);
                for (char c : count.toCharArray()) {
                    chars[write++] = c;
                }
            }
            anchor = read + 1; // move anchor to next group
        }
    }
    return write;
}
```
## Two Pointers / Sliding Window

---
### 1. Move Zeroes
**Problem**: Move all zeros to the end while maintaining the relative order of non-zero elements. Do it in-place.

**Solution** (Two pointers):
```java
public void moveZeroes(int[] nums) {
    int lastNonZeroFoundAt = 0;
    // Move all non-zero elements to the front
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != 0) {
            nums[lastNonZeroFoundAt++] = nums[i];
        }
    }
    // Fill the remaining positions with zeros
    for (int i = lastNonZeroFoundAt; i < nums.length; i++) {
        nums[i] = 0;
    }
}
```

---

### 2. Is Subsequence
**Problem**: Given two strings `s` and `t`, check if `s` is a subsequence of `t`.

**Solution** (Two pointers):
```java
public boolean isSubsequence(String s, String t) {
    int i = 0, j = 0;
    while (i < s.length() && j < t.length()) {
        if (s.charAt(i) == t.charAt(j)) i++;
        j++;
    }
    return i == s.length();
}
```

---

### 3. Container With Most Water
**Problem**: Find two lines that together with the x-axis form a container that holds the most water.

**Solution** (Two pointers from ends):
```java
public int maxArea(int[] height) {
    int left = 0, right = height.length - 1;
    int max = 0;
    while (left < right) {
        int area = Math.min(height[left], height[right]) * (right - left);
        max = Math.max(max, area);
        if (height[left] < height[right]) left++;
        else right--;
    }
    return max;
}
```

---

### 4. Max Number of K-Sum Pairs
**Problem**: Find the maximum number of operations to remove pairs that sum to `k` from an array.

**Solution** (Sort + two pointers):
```java
public int maxOperations(int[] nums, int k) {
    Arrays.sort(nums);
    int left = 0, right = nums.length - 1;
    int count = 0;
    while (left < right) {
        int sum = nums[left] + nums[right];
        if (sum == k) {
            count++;
            left++;
            right--;
        } else if (sum < k) left++;
        else right--;
    }
    return count;
}
```

---

### 5. Maximum Average Subarray I
**Problem**: Find a contiguous subarray of length `k` with the maximum average value.

**Solution** (Sliding window):
```java
public double findMaxAverage(int[] nums, int k) {
    int sum = 0;
    for (int i = 0; i < k; i++) sum += nums[i];
    int maxSum = sum;
    for (int i = k; i < nums.length; i++) {
        sum += nums[i] - nums[i - k];
        maxSum = Math.max(maxSum, sum);
    }
    return (double) maxSum / k;
}
```

---

### 6. Maximum Number of Vowels in a Substring of Given Length
**Problem**: Given a string, find the maximum number of vowels in any substring of length `k`.

**Solution** (Sliding window with vowel set):
```java
public int maxVowels(String s, int k) {
    String vowels = "aeiou";
    int count = 0;
    // Initial window
    for (int i = 0; i < k; i++) {
        if (vowels.indexOf(s.charAt(i)) != -1) count++;
    }
    int max = count;
    for (int i = k; i < s.length(); i++) {
        if (vowels.indexOf(s.charAt(i)) != -1) count++;
        if (vowels.indexOf(s.charAt(i - k)) != -1) count--;
        max = Math.max(max, count);
    }
    return max;
}
```

---

### 7. Max Consecutive Ones III
**Problem**: Given a binary array, find the longest subarray with at most `k` zeros that can be flipped to ones.

**Solution** (Sliding window):
```java
public int longestOnes(int[] nums, int k) {
    int left = 0, right;
    for (right = 0; right < nums.length; right++) {
        if (nums[right] == 0) k--;
        if (k < 0) {
            if (nums[left] == 0) k++;
            left++;
        }
    }
    return right - left;
}
```

---

### 8. Longest Subarray of 1's After Deleting One Element
**Problem**: Find the longest subarray containing only 1's after deleting exactly one element (any element).

**Solution** (Sliding window with at most one zero):
```java
public int longestSubarray(int[] nums) {
    int left = 0, zeroCount = 0, maxLen = 0;
    for (int right = 0; right < nums.length; right++) {
        if (nums[right] == 0) zeroCount++;
        while (zeroCount > 1) {
            if (nums[left] == 0) zeroCount--;
            left++;
        }
        maxLen = Math.max(maxLen, right - left); // window length, but we delete one zero, so actual 1's = length-1? Wait careful.
        // Actually we need the length of subarray of 1's after deleting one element.
        // The window can contain at most one zero, and we delete that zero.
        // So the number of 1's = window length - 1 (if window contains a zero) or window length - 1? If no zero, we must delete one 1, so it's length-1 as well.
        // Therefore answer = max window length - 1.
    }
    return maxLen; // we'll compute after loop as maxLen-1, but we need to be precise.
}
// Corrected version:
public int longestSubarray(int[] nums) {
    int left = 0, zeroCount = 0, maxLen = 0;
    for (int right = 0; right < nums.length; right++) {
        if (nums[right] == 0) zeroCount++;
        while (zeroCount > 1) {
            if (nums[left] == 0) zeroCount--;
            left++;
        }
        maxLen = Math.max(maxLen, right - left + 1);
    }
    return maxLen - 1; // delete one element
}
```

## Prefix Sum / Hash Set & Map

---

### 1. Find the Highest Altitude
**Problem**: Starting at altitude 0, given an array `gain` where `gain[i]` is the net change between points `i` and `i+1`, return the highest altitude reached.

**Solution** (Prefix sum):
```java
public int largestAltitude(int[] gain) {
    int current = 0, max = 0;
    for (int g : gain) {
        current += g;
        max = Math.max(max, current);
    }
    return max;
}
```

---

### 2. Find Pivot Index
**Problem**: Return the leftmost index where the sum of numbers to the left equals the sum of numbers to the right. If none, return -1.

**Solution** (Prefix sum):
```java
public int pivotIndex(int[] nums) {
    int total = 0;
    for (int num : nums) total += num;
    int leftSum = 0;
    for (int i = 0; i < nums.length; i++) {
        if (leftSum == total - leftSum - nums[i]) return i;
        leftSum += nums[i];
    }
    return -1;
}
```

---

### 3. Find the Difference of Two Arrays
**Problem**: Return two lists: distinct integers in `nums1` not in `nums2`, and distinct integers in `nums2` not in `nums1`.

**Solution** (HashSet):
```java
public List<List<Integer>> findDifference(int[] nums1, int[] nums2) {
    Set<Integer> set1 = new HashSet<>();
    Set<Integer> set2 = new HashSet<>();
    for (int num : nums1) set1.add(num);
    for (int num : nums2) set2.add(num);
    
    List<Integer> onlyIn1 = new ArrayList<>();
    List<Integer> onlyIn2 = new ArrayList<>();
    
    for (int num : set1) if (!set2.contains(num)) onlyIn1.add(num);
    for (int num : set2) if (!set1.contains(num)) onlyIn2.add(num);
    
    return Arrays.asList(onlyIn1, onlyIn2);
}
```

---

### 4. Unique Number of Occurrences
**Problem**: Return `true` if each value in the array has a unique frequency, otherwise `false`.

**Solution** (HashMap + HashSet):
```java
public boolean uniqueOccurrences(int[] arr) {
    Map<Integer, Integer> freq = new HashMap<>();
    for (int num : arr) {
        freq.put(num, freq.getOrDefault(num, 0) + 1);
    }
    Set<Integer> occurrences = new HashSet<>();
    for (int count : freq.values()) {
        if (!occurrences.add(count)) return false;
    }
    return true;
}
```

---

### 5. Determine if Two Strings Are Close
**Problem**: Two strings are close if they have the same set of characters and the same multiset of frequencies (can be rearranged via swapping or transforming characters).

**Solution** (Frequency arrays + sorting):
```java
public boolean closeStrings(String word1, String word2) {
    if (word1.length() != word2.length()) return false;
    
    int[] freq1 = new int[26];
    int[] freq2 = new int[26];
    
    for (char c : word1.toCharArray()) freq1[c - 'a']++;
    for (char c : word2.toCharArray()) freq2[c - 'a']++;
    
    // Check same set of characters
    for (int i = 0; i < 26; i++) {
        if ((freq1[i] == 0 && freq2[i] != 0) || (freq1[i] != 0 && freq2[i] == 0)) {
            return false;
        }
    }
    
    // Compare sorted frequency values
    Arrays.sort(freq1);
    Arrays.sort(freq2);
    return Arrays.equals(freq1, freq2);
}
```

---

### 6. Equal Row and Column Pairs
**Problem**: Given an `n x n` matrix, count pairs `(r, c)` such that row `r` and column `c` are identical.

**Solution** (HashMap with list keys):
```java
public int equalPairs(int[][] grid) {
    int n = grid.length;
    Map<List<Integer>, Integer> rowCount = new HashMap<>();
    
    // Store rows
    for (int i = 0; i < n; i++) {
        List<Integer> row = new ArrayList<>();
        for (int j = 0; j < n; j++) row.add(grid[i][j]);
        rowCount.put(row, rowCount.getOrDefault(row, 0) + 1);
    }
    
    // Compare columns
    int count = 0;
    for (int j = 0; j < n; j++) {
        List<Integer> col = new ArrayList<>();
        for (int i = 0; i < n; i++) col.add(grid[i][j]);
        count += rowCount.getOrDefault(col, 0);
    }
    return count;
}
```

## Stack / Queue

---

### 1. Removing Stars From a String
**Problem**: Remove characters from a string where a star `*` removes the closest non-star character to its left.

**Solution** (Stack):
```java
public String removeStars(String s) {
    StringBuilder stack = new StringBuilder();
    for (char c : s.toCharArray()) {
        if (c == '*') {
            stack.deleteCharAt(stack.length() - 1);
        } else {
            stack.append(c);
        }
    }
    return stack.toString();
}
```

---

### 2. Asteroid Collision
**Problem**: Simulate collisions of asteroids moving left (negative) and right (positive). When they collide, the smaller one explodes; if equal, both explode.

**Solution** (Stack):
```java
public int[] asteroidCollision(int[] asteroids) {
    Stack<Integer> stack = new Stack<>();
    for (int ast : asteroids) {
        boolean exploded = false;
        while (!stack.isEmpty() && ast < 0 && stack.peek() > 0) {
            if (stack.peek() < -ast) {
                stack.pop(); // right asteroid explodes, continue checking
            } else if (stack.peek() == -ast) {
                stack.pop(); // both explode
                exploded = true;
                break;
            } else {
                exploded = true; // current asteroid explodes
                break;
            }
        }
        if (!exploded) stack.push(ast);
    }
    int[] result = new int[stack.size()];
    for (int i = result.length - 1; i >= 0; i--) {
        result[i] = stack.pop();
    }
    return result;
}
```

---

### 3. Decode String
**Problem**: Decode a string encoded as `k[encoded_string]` where the encoded_string inside brackets is repeated k times.

**Solution** (Two stacks or recursion):
```java
public String decodeString(String s) {
    Stack<Integer> countStack = new Stack<>();
    Stack<StringBuilder> stringStack = new Stack<>();
    StringBuilder current = new StringBuilder();
    int k = 0;
    for (char ch : s.toCharArray()) {
        if (Character.isDigit(ch)) {
            k = k * 10 + (ch - '0');
        } else if (ch == '[') {
            countStack.push(k);
            stringStack.push(current);
            current = new StringBuilder();
            k = 0;
        } else if (ch == ']') {
            StringBuilder decoded = stringStack.pop();
            int repeat = countStack.pop();
            for (int i = 0; i < repeat; i++) decoded.append(current);
            current = decoded;
        } else {
            current.append(ch);
        }
    }
    return current.toString();
}
```

---

### 4. Number of Recent Calls
**Problem**: Count the number of recent requests within a time frame of 3000 milliseconds.

**Solution** (Queue):
```java
class RecentCounter {
    private Queue<Integer> queue;
    
    public RecentCounter() {
        queue = new LinkedList<>();
    }
    
    public int ping(int t) {
        queue.add(t);
        while (queue.peek() < t - 3000) {
            queue.poll();
        }
        return queue.size();
    }
}
```
---

### 5. Dota2 Senate
**Problem**: Two parties (Radiant and Dire) vote in rounds. Each senator can ban one senator from the opposite party. The process continues until one party has no senators left. Return the winning party.

**Solution** (Two queues):
```java
public String predictPartyVictory(String senate) {
    Queue<Integer> radiant = new LinkedList<>();
    Queue<Integer> dire = new LinkedList<>();
    int n = senate.length();
    for (int i = 0; i < n; i++) {
        if (senate.charAt(i) == 'R') radiant.add(i);
        else dire.add(i);
    }
    while (!radiant.isEmpty() && !dire.isEmpty()) {
        int r = radiant.poll();
        int d = dire.poll();
        if (r < d) radiant.add(r + n); // Radiant acts first, add back with increased index
        else dire.add(d + n);
    }
    return radiant.isEmpty() ? "Dire" : "Radiant";
}
```

## Linked List
---
### 1. Delete the Middle Node of a Linked List
**Problem**: Delete the middle node of a singly linked list. For even length, delete the second middle node.

**Solution** (Two pointers: slow and fast with a `prev` pointer):
```java
public ListNode deleteMiddle(ListNode head) {
    if (head == null || head.next == null) return null;
    ListNode slow = head, fast = head, prev = null;
    while (fast != null && fast.next != null) {
        prev = slow;
        slow = slow.next;
        fast = fast.next.next;
    }
    prev.next = slow.next; // skip the middle node
    return head;
}
```

---

### 2. Odd Even Linked List
**Problem**: Group all nodes with odd indices together followed by even indices, using O(1) extra space.

**Solution** (Maintain separate odd and even pointers):
```java
public ListNode oddEvenList(ListNode head) {
    if (head == null) return null;
    ListNode odd = head;
    ListNode even = head.next;
    ListNode evenHead = even;
    while (even != null && even.next != null) {
        odd.next = even.next;
        odd = odd.next;
        even.next = odd.next;
        even = even.next;
    }
    odd.next = evenHead;
    return head;
}
```

---

### 3. Reverse Linked List
**Problem**: Reverse a singly linked list.

**Solution** (Iterative with three pointers):
```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode nextTemp = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev;
}
```

---

### 4. Maximum Twin Sum of a Linked List
**Problem**: In a linked list of even length, twin sum is the sum of a node and its symmetric twin (i-th from start and i-th from end). Return the maximum twin sum.

**Solution** (Find middle, reverse second half, then compute sums):
```java
public int pairSum(ListNode head) {
    // Find middle (slow will be head of second half)
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    // Reverse second half
    ListNode secondHalf = reverse(slow);
    ListNode firstHalf = head;
    int maxSum = 0;
    while (secondHalf != null) {
        maxSum = Math.max(maxSum, firstHalf.val + secondHalf.val);
        firstHalf = firstHalf.next;
        secondHalf = secondHalf.next;
    }
    return maxSum;
}

private ListNode reverse(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

## Binary Tree
---
### 1. Maximum Depth of Binary Tree
**Problem**: Return the maximum depth (number of nodes along the longest path from root to leaf).

**Solution** (DFS recursion):
```java
public int maxDepth(TreeNode root) {
    if (root == null) return 0;
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

---

### 2. Leaf-Similar Trees
**Problem**: Check if two trees have the same leaf value sequence.

**Solution** (DFS to collect leaves):
```java
public boolean leafSimilar(TreeNode root1, TreeNode root2) {
    List<Integer> leaves1 = new ArrayList<>();
    List<Integer> leaves2 = new ArrayList<>();
    collectLeaves(root1, leaves1);
    collectLeaves(root2, leaves2);
    return leaves1.equals(leaves2);
}

private void collectLeaves(TreeNode node, List<Integer> leaves) {
    if (node == null) return;
    if (node.left == null && node.right == null) {
        leaves.add(node.val);
        return;
    }
    collectLeaves(node.left, leaves);
    collectLeaves(node.right, leaves);
}
```

---

### 3. Count Good Nodes in Binary Tree
**Problem**: Count nodes where no node on the path from root to that node has a greater value.

**Solution** (DFS with `maxSoFar`):
```java
public int goodNodes(TreeNode root) {
    return dfs(root, Integer.MIN_VALUE);
}

private int dfs(TreeNode node, int maxSoFar) {
    if (node == null) return 0;
    int count = 0;
    if (node.val >= maxSoFar) {
        count = 1;
        maxSoFar = node.val;
    }
    count += dfs(node.left, maxSoFar);
    count += dfs(node.right, maxSoFar);
    return count;
}
```

---

### 4. Path Sum III
**Problem**: Count paths that sum to a target value (paths can start and end anywhere but must go downward).

**Solution** (Prefix sum with backtracking):
```java
public int pathSum(TreeNode root, int targetSum) {
    Map<Long, Integer> prefixCount = new HashMap<>();
    prefixCount.put(0L, 1);
    return dfs(root, 0L, targetSum, prefixCount);
}

private int dfs(TreeNode node, long currentSum, int target, Map<Long, Integer> prefixCount) {
    if (node == null) return 0;
    currentSum += node.val;
    int count = prefixCount.getOrDefault(currentSum - target, 0);
    prefixCount.put(currentSum, prefixCount.getOrDefault(currentSum, 0) + 1);
    count += dfs(node.left, currentSum, target, prefixCount);
    count += dfs(node.right, currentSum, target, prefixCount);
    prefixCount.put(currentSum, prefixCount.get(currentSum) - 1); // backtrack
    return count;
}
```

---

### 5. Longest ZigZag Path in a Binary Tree
**Problem**: Find the longest path where direction alternates (left‑right‑left…). Return the number of edges.

**Solution** (DFS with direction tracking):
```java
int maxZig = 0;

public int longestZigZag(TreeNode root) {
    dfs(root, 0, 0);
    return maxZig;
}

private void dfs(TreeNode node, int leftLen, int rightLen) {
    if (node == null) return;
    maxZig = Math.max(maxZig, Math.max(leftLen, rightLen));
    dfs(node.left, rightLen + 1, 0);  // going left: continue zigzag from right
    dfs(node.right, 0, leftLen + 1);  // going right: continue zigzag from left
}
```

---

### 6. Lowest Common Ancestor of a Binary Tree
**Problem**: Find the lowest node that has both `p` and `q` as descendants.

**Solution** (Recursive DFS):
```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null || root == p || root == q) return root;
    TreeNode left = lowestCommonAncestor(root.left, p, q);
    TreeNode right = lowestCommonAncestor(root.right, p, q);
    if (left != null && right != null) return root;
    return left != null ? left : right;
}
```

---

### 7. Binary Tree Right Side View
**Problem**: Return the values visible when looking at the tree from the right side (top to bottom).

**Solution** (BFS level order – take last node of each level):
```java
public List<Integer> rightSideView(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    if (root == null) return result;
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            TreeNode node = queue.poll();
            if (i == size - 1) result.add(node.val);
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
    }
    return result;
}
```

---

### 8. Maximum Level Sum of a Binary Tree
**Problem**: Return the smallest level (1‑indexed) whose sum of node values is maximum.

**Solution** (BFS level order):
```java
public int maxLevelSum(TreeNode root) {
    if (root == null) return 0;
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    int maxSum = Integer.MIN_VALUE;
    int maxLevel = 1;
    int level = 1;
    while (!queue.isEmpty()) {
        int size = queue.size();
        int sum = 0;
        for (int i = 0; i < size; i++) {
            TreeNode node = queue.poll();
            sum += node.val;
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
        if (sum > maxSum) {
            maxSum = sum;
            maxLevel = level;
        }
        level++;
    }
    return maxLevel;
}
```

---

### 9. Search in a Binary Search Tree
**Problem**: Return the subtree rooted with the node having the given value, or `null` if not found.

**Solution** (Recursive BST search):
```java
public TreeNode searchBST(TreeNode root, int val) {
    if (root == null || root.val == val) return root;
    return val < root.val ? searchBST(root.left, val) : searchBST(root.right, val);
}
```

---

### 10. Delete Node in a BST
**Problem**: Delete the node with the given key and return the new root, preserving BST properties.

**Solution** (Recursive deletion with successor replacement):
```java
public TreeNode deleteNode(TreeNode root, int key) {
    if (root == null) return null;
    if (key < root.val) {
        root.left = deleteNode(root.left, key);
    } else if (key > root.val) {
        root.right = deleteNode(root.right, key);
    } else {
        // Node to delete found
        if (root.left == null) return root.right;
        if (root.right == null) return root.left;
        // Two children: find successor (minimum in right subtree)
        TreeNode minNode = findMin(root.right);
        root.val = minNode.val;
        root.right = deleteNode(root.right, minNode.val);
    }
    return root;
}

private TreeNode findMin(TreeNode node) {
    while (node.left != null) node = node.left;
    return node;
}
```

## Graphs & Search (BFS/DFS)
---
### 1. Keys and Rooms
**Problem**: There are `n` rooms labeled from `0` to `n-1`. You start in room `0` and each room contains keys to other rooms. Determine if you can visit all rooms.

**Solution** (DFS / BFS to traverse reachable rooms):
```java
public boolean canVisitAllRooms(List<List<Integer>> rooms) {
    boolean[] visited = new boolean[rooms.size()];
    dfs(rooms, 0, visited);
    for (boolean v : visited) if (!v) return false;
    return true;
}

private void dfs(List<List<Integer>> rooms, int room, boolean[] visited) {
    visited[room] = true;
    for (int key : rooms.get(room)) {
        if (!visited[key]) dfs(rooms, key, visited);
    }
}
```

---

### 2. Number of Provinces
**Problem**: Given an `n x n` adjacency matrix `isConnected` where `isConnected[i][j] = 1` if cities `i` and `j` are directly connected, return the total number of provinces (connected components).

**Solution** (DFS on each unvisited node):
```java
public int findCircleNum(int[][] isConnected) {
    int n = isConnected.length;
    boolean[] visited = new boolean[n];
    int provinces = 0;
    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            dfs(isConnected, i, visited);
            provinces++;
        }
    }
    return provinces;
}

private void dfs(int[][] isConnected, int city, boolean[] visited) {
    visited[city] = true;
    for (int j = 0; j < isConnected.length; j++) {
        if (isConnected[city][j] == 1 && !visited[j]) {
            dfs(isConnected, j, visited);
        }
    }
}
```

---

### 3. Reorder Routes to Make All Paths Lead to the City Zero
**Problem**: There are `n` cities with `n-1` directed roads (forming a tree). Find the minimum number of roads that need to be reversed so that every city can reach city `0`.

**Solution** (BFS from city `0` on a graph with both directions, count edges going away):
```java
public int minReorder(int n, int[][] connections) {
    List<int[]>[] graph = new ArrayList[n];
    for (int i = 0; i < n; i++) graph[i] = new ArrayList<>();
    for (int[] conn : connections) {
        graph[conn[0]].add(new int[]{conn[1], 1}); // original direction (needs reverse if away from 0)
        graph[conn[1]].add(new int[]{conn[0], 0}); // reverse direction (already towards 0)
    }
    boolean[] visited = new boolean[n];
    Queue<Integer> queue = new LinkedList<>();
    queue.offer(0);
    visited[0] = true;
    int changes = 0;
    while (!queue.isEmpty()) {
        int city = queue.poll();
        for (int[] neighbor : graph[city]) {
            int next = neighbor[0];
            int dir = neighbor[1]; // 1 means original direction is city->next
            if (!visited[next]) {
                visited[next] = true;
                changes += dir; // if original direction is away from 0, we need to reverse
                queue.offer(next);
            }
        }
    }
    return changes;
}
```

---

### 4. Evaluate Division
**Problem**: Given equations like `["a/b","b/c"]` with values `[2.0,3.0]` and queries like `["a","c"]`, return answers for `a/c` (or `-1.0` if unknown).

**Solution** (Graph with weighted edges, DFS with backtracking):
```java
public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
    Map<String, Map<String, Double>> graph = new HashMap<>();
    for (int i = 0; i < equations.size(); i++) {
        String a = equations.get(i).get(0);
        String b = equations.get(i).get(1);
        double val = values[i];
        graph.computeIfAbsent(a, k -> new HashMap<>()).put(b, val);
        graph.computeIfAbsent(b, k -> new HashMap<>()).put(a, 1.0 / val);
    }
    double[] results = new double[queries.size()];
    for (int i = 0; i < queries.size(); i++) {
        String from = queries.get(i).get(0);
        String to = queries.get(i).get(1);
        if (!graph.containsKey(from) || !graph.containsKey(to)) {
            results[i] = -1.0;
        } else {
            Set<String> visited = new HashSet<>();
            results[i] = dfs(graph, from, to, 1.0, visited);
        }
    }
    return results;
}

private double dfs(Map<String, Map<String, Double>> graph, String curr, String target, double product, Set<String> visited) {
    if (curr.equals(target)) return product;
    visited.add(curr);
    for (Map.Entry<String, Double> neighbor : graph.get(curr).entrySet()) {
        if (!visited.contains(neighbor.getKey())) {
            double result = dfs(graph, neighbor.getKey(), target, product * neighbor.getValue(), visited);
            if (result != -1.0) return result;
        }
    }
    visited.remove(curr);
    return -1.0;
}
```

---

### 5. Nearest Exit from Entrance in Maze
**Problem**: In an `m x n` maze (`'+'` = wall, `'.'` = empty), start at `entrance`. Find the shortest number of steps to any empty cell on the border (exit), excluding the entrance itself if it happens to be on the border.

**Solution** (BFS):
```java
public int nearestExit(char[][] maze, int[] entrance) {
    int m = maze.length, n = maze[0].length;
    int[][] dirs = {{-1,0},{1,0},{0,-1},{0,1}};
    Queue<int[]> queue = new LinkedList<>();
    queue.offer(new int[]{entrance[0], entrance[1], 0}); // row, col, steps
    maze[entrance[0]][entrance[1]] = '+'; // mark visited
    
    while (!queue.isEmpty()) {
        int[] cell = queue.poll();
        int r = cell[0], c = cell[1], steps = cell[2];
        // Check if exit (border and not entrance)
        if ((r == 0 || r == m-1 || c == 0 || c == n-1) && !(r == entrance[0] && c == entrance[1])) {
            return steps;
        }
        for (int[] d : dirs) {
            int nr = r + d[0], nc = c + d[1];
            if (nr >= 0 && nr < m && nc >= 0 && nc < n && maze[nr][nc] == '.') {
                maze[nr][nc] = '+'; // mark visited
                queue.offer(new int[]{nr, nc, steps + 1});
            }
        }
    }
    return -1;
}
```

---

### 6. Rotting Oranges
**Problem**: In a grid, `0` empty, `1` fresh, `2` rotten. Every minute, any fresh orange adjacent (4-directionally) to a rotten one becomes rotten. Return minutes until no fresh oranges, or `-1` if impossible.

**Solution** (Multi-source BFS):
```java
public int orangesRotting(int[][] grid) {
    int m = grid.length, n = grid[0].length;
    Queue<int[]> queue = new LinkedList<>();
    int freshCount = 0;
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == 2) queue.offer(new int[]{i, j});
            else if (grid[i][j] == 1) freshCount++;
        }
    }
    if (freshCount == 0) return 0;
    
    int minutes = 0;
    int[][] dirs = {{-1,0},{1,0},{0,-1},{0,1}};
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            int[] cell = queue.poll();
            for (int[] d : dirs) {
                int nr = cell[0] + d[0], nc = cell[1] + d[1];
                if (nr >= 0 && nr < m && nc >= 0 && nc < n && grid[nr][nc] == 1) {
                    grid[nr][nc] = 2;
                    freshCount--;
                    queue.offer(new int[]{nr, nc});
                }
            }
        }
        if (!queue.isEmpty()) minutes++;
    }
    return freshCount == 0 ? minutes : -1;
}
```
## Heaps, Bit Manipulation, & DP

---
### 1. Kth Largest Element in an Array
**Problem**: Find the kth largest element in an unsorted array (not the kth distinct).

**Solution** (Min‑heap of size k):
```java
public int findKthLargest(int[] nums, int k) {
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();
    for (int num : nums) {
        minHeap.offer(num);
        if (minHeap.size() > k) minHeap.poll();
    }
    return minHeap.peek();
}
```

---

### 2. Smallest Number in Infinite Set
**Problem**: Design a class that maintains an infinite set of positive integers, supports `popSmallest()` and `addBack(num)`.

**Solution** (Min‑heap + set for added‑back numbers):
```java
class SmallestInfiniteSet {
    private PriorityQueue<Integer> heap;
    private Set<Integer> inHeap;
    private int current;
    
    public SmallestInfiniteSet() {
        heap = new PriorityQueue<>();
        inHeap = new HashSet<>();
        current = 1;
    }
    
    public int popSmallest() {
        if (!heap.isEmpty()) {
            int val = heap.poll();
            inHeap.remove(val);
            return val;
        }
        return current++;
    }
    
    public void addBack(int num) {
        if (num < current && !inHeap.contains(num)) {
            heap.offer(num);
            inHeap.add(num);
        }
    }
}
```

---

### 3. Maximum Subsequence Score
**Problem**: Given two arrays `nums1` and `nums2`, choose a subsequence of indices of length `k` to maximize `sum(nums1[i]) * min(nums2[i])`.

**Solution** (Sort by nums2 descending + min‑heap for nums1):
```java
public long maxScore(int[] nums1, int[] nums2, int k) {
    int n = nums1.length;
    int[][] pairs = new int[n][2];
    for (int i = 0; i < n; i++) {
        pairs[i] = new int[]{nums1[i], nums2[i]};
    }
    Arrays.sort(pairs, (a, b) -> b[1] - a[1]); // sort by nums2 descending
    
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();
    long sum = 0;
    long max = 0;
    for (int[] p : pairs) {
        minHeap.offer(p[0]);
        sum += p[0];
        if (minHeap.size() > k) sum -= minHeap.poll();
        if (minHeap.size() == k) max = Math.max(max, sum * p[1]);
    }
    return max;
}
```

---

### 4. Total Cost to Hire K Workers
**Problem**: Hire `k` workers from two arrays `costs` and follow a hiring rule: pick the cheapest from either the first or last `candidates` remaining.

**Solution** (Two min‑heaps, left and right):
```java
public long totalCost(int[] costs, int k, int candidates) {
    int n = costs.length;
    PriorityQueue<Integer> leftHeap = new PriorityQueue<>();
    PriorityQueue<Integer> rightHeap = new PriorityQueue<>();
    int left = 0, right = n - 1;
    long total = 0;
    
    for (; left < candidates && left <= right; left++) leftHeap.offer(costs[left]);
    for (; right >= n - candidates && right >= left; right--) rightHeap.offer(costs[right]);
    
    while (k-- > 0) {
        if (rightHeap.isEmpty() || (!leftHeap.isEmpty() && leftHeap.peek() <= rightHeap.peek())) {
            total += leftHeap.poll();
            if (left <= right) leftHeap.offer(costs[left++]);
        } else {
            total += rightHeap.poll();
            if (left <= right) rightHeap.offer(costs[right--]);
        }
    }
    return total;
}
```

---

### 5. Counting Bits
**Problem**: For each `i` from `0` to `n`, count the number of 1's in its binary representation.

**Solution** (DP using the pattern `dp[i] = dp[i >> 1] + (i & 1)`):
```java
public int[] countBits(int n) {
    int[] dp = new int[n + 1];
    for (int i = 1; i <= n; i++) {
        dp[i] = dp[i >> 1] + (i & 1);
    }
    return dp;
}
```

---

### 6. Single Number
**Problem**: Every element appears twice except one. Find that single one.

**Solution** (XOR all numbers – cancels duplicates):
```java
public int singleNumber(int[] nums) {
    int result = 0;
    for (int num : nums) result ^= num;
    return result;
}
```

---

### 7. N-th Tribonacci Number
**Problem**: Tribonacci: `T0 = 0, T1 = 1, T2 = 1`, and `Tn+3 = Tn + Tn+1 + Tn+2`. Compute the nth.

**Solution** (DP with three variables):
```java
public int tribonacci(int n) {
    if (n == 0) return 0;
    if (n <= 2) return 1;
    int a = 0, b = 1, c = 1;
    for (int i = 3; i <= n; i++) {
        int next = a + b + c;
        a = b;
        b = c;
        c = next;
    }
    return c;
}
```

---

### 8. Min Cost Climbing Stairs
**Problem**: You can start at index 0 or 1. Each step costs `cost[i]`, and you can climb 1 or 2 steps. Find minimum cost to reach the top (past the last index).

**Solution** (DP bottom‑up):
```java
public int minCostClimbingStairs(int[] cost) {
    int n = cost.length;
    int[] dp = new int[n + 1]; // dp[i] = min cost to reach step i
    for (int i = 2; i <= n; i++) {
        dp[i] = Math.min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
    }
    return dp[n];
}
```
Space‑optimized version (just two variables).

---

### 9. House Robber
**Problem**: Rob houses without robbing two adjacent ones. Maximize sum.

**Solution** (DP – either rob current or skip):
```java
public int rob(int[] nums) {
    int prev2 = 0, prev1 = 0; // prev2 = dp[i-2], prev1 = dp[i-1]
    for (int num : nums) {
        int curr = Math.max(prev1, prev2 + num);
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

---

### 10. Longest Common Subsequence
**Problem**: Return length of longest common subsequence of `text1` and `text2`.

**Solution** (2D DP):
```java
public int longestCommonSubsequence(String text1, String text2) {
    int m = text1.length(), n = text2.length();
    int[][] dp = new int[m + 1][n + 1];
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }
    return dp[m][n];
}
```





