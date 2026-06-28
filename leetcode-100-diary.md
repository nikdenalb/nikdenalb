# LeetCode 100 diary

Personal notes from my [LeetCode 100](https://leetcode.com/) streak: solutions, submission stats, and quick reflections. This diary is an **experiment** — I may stop maintaining it at any time.

Subjective difficulty is the rating I give immediately after the attempt: `1`-`3` easy, `4`-`6` medium, `7`-`9` hard.

Progress rule: solved problems increase the counter by 1; an unsolved task resets it to 0.

Record streak: **4**

## 2026-06-28 · [Maximum Element After Decreasing and Rearranging](https://leetcode.com/problems/maximum-element-after-decreasing-and-rearranging/?envType=daily-question&envId=2026-06-28) · (2 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **2/9**

An easy task for me. The hard part was hitting submit, because a solution this simple cannot be right for a medium problem, so I must have misunderstood something, but I do not see what.

```java
class Solution {
    int maximumElementAfterDecrementingAndRearranging(int[] arr) {
        int n = arr.length;
        Arrays.sort(arr);
        arr[0] = 1;
        for (int i = 1; i < n; i++) {
            if (arr[i] > arr[i - 1] + 1) arr[i] = arr[i - 1] + 1;
        }
        return arr[n - 1];
    }
}
```

Runtime **9 ms** (beats 67.50%) · Memory **77.32 MB** (beats 78.75%) · Time taken **6m 5s**

## 2026-06-27 · [Find the Maximum Number of Elements in Subset](https://leetcode.com/problems/find-the-maximum-number-of-elements-in-subset/?envType=daily-question&envId=2026-06-27) · (1 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **7/9**

Brute force was not an obvious approach. I was afraid I would not solve it at all, which I would rather avoid on medium problems.

```java
class Solution {
    int maximumLength(int[] nums) {
        int max = 46340;
        int n = nums.length;
        Arrays.sort(nums);
        HashSet<Integer> even = new HashSet<>();
        HashSet<Integer> odd = new HashSet<>();
        int cnt = 0;
        for (int m : nums) {
            if (m == 1) cnt++;
            else {
                if (odd.contains(m)) even.add(m);
                else odd.add(m);
            }        
        }
        if (cnt % 2 == 0) cnt--;
        int out = Math.max(1, cnt);
        int j = 0;
        if (nums[0] == 1) {
            while (nums[j] == 0) j++;
        }
        int curr = nums[j];
        for (int i = j + 1; i < n; i++) {
            if (curr != nums[i]) {
                cnt = 1;
                while (curr <= max && even.contains(curr) && odd.contains(curr * curr)) {
                    cnt += 2;
                    curr = curr * curr;
                }
                out = Math.max(out, cnt);
                curr = nums[i];
            }
        }
        return out;
    }
}
```

Runtime **72 ms** (beats 48.84%) · Memory **67.50 MB** (beats 56.98%) · Time taken **48m 33s**

## 2026-06-26 · [Count Subarrays with Majority Element II](https://leetcode.com/problems/count-subarrays-with-majority-element-ii/?envType=daily-question&envId=2026-06-26) · (0 / 100)

LeetCode difficulty **Hard** · Subjective difficulty **5/9**

Did not solve it alone. The prefix-sum reduction was clear, but a Fenwick tree from memory was not. **5/9** sits in the medium band — with those techniques drilled the task is straightforward rather than hard. Segment trees are going on the practice plan.

```java
class Solution {
    long countMajoritySubarrays(int[] nums, int target) {
        int n = nums.length;
        for (int i = 0; i < n; i++) nums[i] = nums[i] == target ? 1 : -1;
        for (int i = 1; i < n; i++) nums[i] = nums[i] + nums[i - 1];
        long out = 0;
        for (int i = 0; i < n; i++) {
            for (int j = -1; j < i; j++) {
                int jval = j == -1 ? 0 : nums[j];
                if (nums[i] - jval > 0) out++;
            }
        }
        return out;
    }
}
```

After a little help from a free LLM:

```java
class Solution {
    long countMajoritySubarrays(int[] nums, int target) {
        int n = nums.length;
        for (int i = 0; i < n; i++) nums[i] = nums[i] == target ? 1 : -1;
        for (int i = 1; i < n; i++) nums[i] += nums[i - 1];

        Fenwick fenwick = new Fenwick(2 * n + 2);
        fenwick.add(n + 1, 1);

        long out = 0;
        for (int i = 0; i < n; i++) {
            int index = nums[i] + n + 1;
            out += fenwick.sum(index - 1);
            fenwick.add(index, 1);
        }
        return out;
    }

    class Fenwick {
        int size;
        long[] tree;

        Fenwick(int size) {
            this.size = size;
            tree = new long[size + 1];
        }

        void add(int index, long delta) {
            for (int i = index; i <= size; i += i & -i) tree[i] += delta;
        }

        long sum(int index) {
            if (index <= 0) return 0;
            long out = 0;
            for (int i = index; i > 0; i -= i & -i) out += tree[i];
            return out;
        }
    }
}
```

## 2026-06-25 · [Count Subarrays with Majority Element I](https://leetcode.com/problems/count-subarrays-with-majority-element-i/) · (1 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **2/9**

I got distracted, so the time is a bit inflated.

```java
class Solution {
    int countMajoritySubarrays(int[] nums, int target) {
        int cnt = 0, out = 0;
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            if (nums[i] == target) {
                cnt = 1;
                out++;
            } else cnt = 0;        
            for (int j = i + 1; j < n; j++) {
                if (nums[j] == target) cnt++;
                if (cnt * 2 > j - i + 1) out++;
            }
        }
        return out;
    }
}
```

Runtime **36 ms** (beats 59.52%) · Memory **46.98 MB** (beats 61.93%) · Time taken **11m 8s**

## 2026-06-24 · [Number of Zigzag Arrays II](https://leetcode.com/problems/number-of-zigzag-arrays-ii/description/?envType=daily-question&envId=2026-06-24) · (0 / 100)

LeetCode difficulty **Hard** · Subjective difficulty **7/9**

I did not solve it. I sat for about 10 minutes and quit. This is a matrix exponentiation problem and I have not met that technique yet. I will practice it.

After reading the [editorial](https://leetcode.com/problems/number-of-zigzag-arrays-ii/editorial/?envType=daily-question&envId=2026-06-24):

```java
class Solution {
    int mod = 1_000_000_007;
    int zigZagArrays(int n, int l, int r) {
        int m = r - l + 1;

        long[][] matrix = new long[m][m];
        for (int i = 0; i < m; i++)
            for (int j = 0; j < m && i + j < m - 1; j++)
                matrix[i][j] = 1;

        long[][] result = new long[m][m];
        for (int i = 0; i < m; i++) result[i][i] = 1;

        long[][] base = matrix;
        int exp = n - 1;
        while (exp > 0) {
            if ((exp & 1) == 1) result = multiply(result, base);
            base = multiply(base, base);
            exp >>= 1;
        }

        long sum = 0;
        for (int i = 0; i < m; i++)
            for (int j = 0; j < m; j++)
                sum += result[i][j];

        return (int) (sum * 2 % mod);
    }

    long[][] multiply(long[][] left, long[][] right) {
        int m = left.length;
        long[][] out = new long[m][m];
        for (int i = 0; i < m; i++)
            for (int j = 0; j < m; j++) {
                long cell = 0;
                for (int k = 0; k < m; k++) cell = (cell + left[i][k] * right[k][j]) % mod;
                out[i][j] = cell;
            }
        return out;
    }
}
```

## 2026-06-23 · [Number of Zigzag Arrays I](https://leetcode.com/problems/number-of-zigzag-arrays-i/description/?envType=daily-question&envId=2026-06-23&roomId=nAywuq) · (3 / 100)

LeetCode difficulty **Hard** · Subjective difficulty **6/9**

My speed suffers from lack of exposure. It took me a long time to realize I needed to count variants with a specific ending number. I also need to drill modulo and type casting — I add too many extra parentheses just to be sure both work correctly.

```java
class Solution {
    int mod = 1_000_000_007;
    int zigZagArrays(int n, int l, int r) {
        int m = r - l + 1;
        int[][] dp = new int[m][n];
        for (int i = 0; i < m; i++) dp[i][0] = 1;
        for (int j = 1; j < n; j++) {
            for (int i = 1; i < m; i++) dp[i][j] = (int) (((long) dp[i - 1][j] + dp[i - 1][j - 1]) % mod);
            if (++j == n) break;
            for (int i = m - 2; i >= 0; i--) dp[i][j] = (int) (((long) dp[i + 1][j] + dp[i + 1][j - 1]) % mod);
        }
        long sum = 0;
        for (int i = 0; i < m; i++) sum += dp[i][n - 1];
        sum *= 2;
        return (int) (sum % mod);
    }
}
/*   n=5, m=6
[0][ ][ ][ ][ ]
[1][ ][ ][ ][ ]
[2][ ][ ][ ][ ]
[3][ ][ ][ ][ ]
[4][ ][ ][ ][ ]
[5][ ][ ][ ][ ]

[1]  [0] [16] [ ][ ]
[1]  [1] [14] [ ][ ]
[1]  [2] [12] [ ][ ]
[1]  [3] [9 ] [ ][ ]
[1]  [4] [5 ] [ ][ ]
[1]  [5] [0 ] [ ][ ]
*/
```

Runtime **262 ms** (beats 92.50%) · Memory **111.45 MB** (beats 12.50%) · Time taken **55m 29s**

Beautification:

```java
class Solution {
    int mod = 1_000_000_007;
    int zigZagArrays(int n, int l, int r) {
        int m = r - l + 1;
        int[][] dp = new int[m][n];
        for (int i = 0; i < m; i++) dp[i][0] = 1;
        for (int j = 1; j < n; j++) {
            for (int i = 1; i < m; i++) dp[i][j] = (dp[i - 1][j] + dp[i - 1][j - 1]) % mod;
            if (++j == n) break;
            for (int i = m - 2; i >= 0; i--) dp[i][j] = (dp[i + 1][j] + dp[i + 1][j - 1]) % mod;
        }
        long sum = 0;
        for (int i = 0; i < m; i++) sum += dp[i][n - 1];
        return (int) (sum * 2 % mod);
    }
}
```

## 2026-06-22 · [Maximum Number of Balloons](https://leetcode.com/problems/maximum-number-of-balloons/description/?envType=daily-question&envId=2026-06-22) · (2 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **2/9**

I did not get to a clean solution right away. I should be less hesitant to write contest-style code first. I cleaned it up afterward.

```java
class Solution {
    int maxNumberOfBalloons(String text) {
        int[] cs = new int[26];
        for (char c : text.toCharArray()) {
            if (c == 'b' || c == 'a' || c == 'l' || c == 'o' || c == 'n') cs[c - 'a']++;
        }
        int out = Integer.MAX_VALUE;
        out = Math.min(out, cs['b' - 'a']);
        out = Math.min(out, cs[0]);
        out = Math.min(out, cs['l' - 'a'] / 2);
        out = Math.min(out, cs['o' - 'a'] / 2);
        out = Math.min(out, cs['n' - 'a']);
        return out;
    }
}
```

Runtime **2 ms** (beats 96.67%) · Memory **43.22 MB** (beats 79.23%) · Time taken **7m 49s**

Beautification:

```java
class Solution {
    int maxNumberOfBalloons(String text) {
        int[] cs = new int[26];
        for (char c : text.toCharArray()) cs[c - 'a']++;

        int[] tgt = new int[26];
        for (char c : "balloon".toCharArray()) tgt[c - 'a']++;

        int out = Integer.MAX_VALUE;
        for (int i = 0; i < 26; i++) if (tgt[i] > 0) out = Math.min(out, cs[i] / tgt[i]);

        return out;
    }
}
```

## 2026-06-21 · [Maximum Ice Cream Bars](https://leetcode.com/problems/maximum-ice-cream-bars/description/?envType=daily-question&envId=2026-06-21) · (1 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **2/9**

I thought it was a knapsack problem. I panicked because I could not recall the solution, so I lost time. I will add knapsack problems for practice.

```java
class Solution {
    int maxIceCream(int[] costs, int coins) {
        int n = costs.length, out = 0;
        Arrays.sort(costs);
        for (int c : costs) {
            coins -= c;
            if (coins >= 0) out++;
            else break;
        }
        return out;
    }
}
```

Runtime **39 ms** (beats 7.12%) · Memory **83.80 MB** (beats 68.89%) · Time taken **4m 59s**

## 2026-06-20 · [Maximum Building Height](https://leetcode.com/problems/maximum-building-height/description/?envType=daily-question&envId=2026-06-20) · (0 / 100)

LeetCode difficulty **Hard** · Subjective difficulty **6/9**

I should have thought about the solution longer instead of coding an approach that would not pass on memory or time anyway. A solution was possible. Also showed I need to drill map, stack, and streams — no streams in what I ended up with, but I spent a long time trying to get a max from an array.

```java
class Solution {
    int maxBuilding(int n, int[][] restrictions) {
        Map<Integer, Integer> map = new HashMap<>();
        map.put(1, 0);
        for (int[] im : restrictions) map.put(im[0], im[1]);
        Deque<Pair> st = new ArrayDeque<>();
        st.add(new Pair(1, 0));
        for (int i = 2; i <= n; i++) {
            if (map.containsKey(i)) {
                Pair p = st.peekLast();
                if (p.rs() + i - p.indx() > map.get(i)) {
                    st.add(new Pair(i, map.get(i)));
                }
            }
        }
        int out = 0;
        int rsr = map.getOrDefault(n, 1_000_000);
        for (int i = n; i >= 1; i--) {
            if (st.peekLast().indx() > i) st.removeLast();
            Pair p = st.peekLast();
            rsr = Math.min(rsr + 1, map.getOrDefault(i, 1_000_000));
            out = Math.max(out, Math.min(p.rs() + i - p.indx(), rsr));
        }
        return out;
    }
    record Pair(int indx, int rs){}
}
```

Final version from [eunice's LeetCode solution](https://leetcode.com/problems/maximum-building-height/solutions/8346225/maximum-building-height-greedy-linear-al-4sar/?envType=daily-question&envId=2026-06-20):

```java
class Solution {
    int maxBuilding(int n, int[][] restrictions) {
        List<int[]> rs = new ArrayList<>();
        for (int[] r : restrictions) rs.add(r);
        rs.add(new int[]{1, 0});
        rs.sort((a, b) -> a[0] - b[0]);
        if (rs.getLast()[0] != n) rs.add(new int[]{n, n - 1});
        int len = rs.size();
        for (int i = 1; i < len; i++) {
            int d = rs.get(i)[0] - rs.get(i - 1)[0];
            rs.get(i)[1] = Math.min(rs.get(i)[1], rs.get(i - 1)[1] + d);
        }
        for (int i = len - 2; i > 0; i--) {
            int d = rs.get(i + 1)[0] - rs.get(i)[0];
            rs.get(i)[1] = Math.min(rs.get(i)[1], rs.get(i + 1)[1] + d);
        }
        int out = 0;
        for (int i = 0; i < len - 1; i++) {
            int[] a = rs.get(i), b = rs.get(i + 1);
            out = Math.max(out, (a[1] + b[1] + b[0] - a[0]) / 2);
        }
        return out;
    }
}
```

## 2026-06-19 · [Find the Highest Altitude](https://leetcode.com/problems/find-the-highest-altitude/description/?envType=daily-question&envId=2026-06-19) · (2 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **1/9**

This is my baseline time for reading the problem and writing the code. I barely had to think on this one. Yes, I am this slow, and I don't really want to optimize for speed.

```java
class Solution {
    int largestAltitude(int[] gain) {
        int h = 0, out = 0;
        for (int g : gain) {
            h += g;
            out = Math.max(out, h);
        }
        return out;
    }
}
```

Runtime **0 ms** (beats 100.00%) · Memory **43.36 MB** (beats 6.14%) · Time taken **2m 21s**

## 2026-06-18 · [Angle Between Hands of a Clock](https://leetcode.com/problems/angle-between-hands-of-a-clock/description/?envType=daily-question&envId=2026-06-18) · (1 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **3/9**

Took longer than I wanted, so not happy with myself. Liked the problem — easy and unfamiliar.

```java
class Solution {
    double angleClock(int hour, int minutes) {
        hour %= 12;
        int h = 30 * hour;
        int m = 6 * minutes;
        double out = Math.abs(h - m + (double) minutes / 2);
        return Math.min(out, 360 - out);
    }
}
```

Runtime **0 ms** (beats 100.00%) · Memory **44.68 MB** (beats 87.91%) · Time taken **16m 19s**

## 2026-06-17 · [Process String with Special Operations II](https://leetcode.com/problems/process-string-with-special-operations-ii/description/?envType=daily-question&envId=2026-06-17) · (0 / 100)

LeetCode difficulty **Hard** · Subjective difficulty **8/9**

An interesting problem — simple once you see the idea, but I couldn't find where to start on my own. I never got to picking off characters on the backward pass and kept trying to tie an index in `s` directly to `k`.

1h solution (stuck on the same approach after 40 minutes):

```java
class Solution {
    record Cmd (char c, long ind){}
    char processStr(String s, long k) {
        StringBuilder res = new StringBuilder();
        long ind = -1;
        List<Cmd> list = new ArrayList<>();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            switch (c) {
                case '*' -> {
                    if (ind == 0) ind = -1;
                    else list.add(new Cmd('*', ind));
                }
                case '#' -> list.add(new Cmd('#', ind));
                case '%' -> list.add(new Cmd('%', ind));
                default -> ind++;
            }
        }
        if (k >= res.length()) return '.';
        return res.charAt((int) k);
    }
}
```

Final version after reviewing the [LeetCode editorial](https://leetcode.com/problems/process-string-with-special-operations-ii/editorial/?envType=daily-question&envId=2026-06-17):

```java
class Solution {
    char processStr(String s, long k) {
        long len = 0;
        for (char c : s.toCharArray()) {
            switch (c) {
                case '*' -> {if (len > 0) len--;}
                case '#' -> len *= 2;
                case '%' -> {}
                default  -> len++;
            }
        }
        if (k >= len) return '.';
        for (int i = s.length() - 1; i >= 0; i--) {
            char c = s.charAt(i);
            switch (c) {
                case '*' -> len++;
                case '#' -> {
                    if (k + 1 > (len + 1) / 2) k -= len / 2;
                    len = (len + 1) / 2;
                }
                case '%' -> k = len - k - 1;
                default -> {
                    if (k + 1 == len) return c;
                    len--;
                }
            }
        }
        throw new RuntimeException();
    }
}
```

## 2026-06-16 · [Process String with Special Operations I](https://leetcode.com/problems/process-string-with-special-operations-i/description/?envType=daily-question&envId=2026-06-16) · (4 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **2/9**

A straightforward implementation task. A reason to practice `switch` — still not in my active vocabulary.

```java
class Solution {
    String processStr(String s) {
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            if (ch == '*') {
                if (!result.isEmpty()) result.setLength(result.length() - 1);
            } else if (ch == '#') {
                result.append(result);
            } else if (ch == '%') {
                result.reverse();
            } else {
                result.append(ch);
            }
        }
        return result.toString();
    }
}
```

Runtime **3 ms** (beats 100.00%) · Memory **54.78 MB** (beats 91.91%) · Time taken **8m 8s**

## 2026-06-15 · [Delete the Middle Node of a Linked List](https://leetcode.com/problems/delete-the-middle-node-of-a-linked-list/description/?envType=daily-question&envId=2026-06-15) · (3 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **3/9**

Dissatisfied with the task resolution time. Got stuck and confused regarding the position of the left node relative to the deleted one.

```java
class Solution {
    ListNode deleteMiddle(ListNode head) {
        if (head.next == null) return null;
        ListNode tail = head.next;
        if (tail.next == null) {
            head.next = null;
            return head;
        }
        tail = tail.next;
        ListNode mid = head;
        while (tail.next != null) {
            tail = tail.next;
            if (tail.next != null) tail = tail.next;
            mid = mid.next;
        }
        mid.next = mid.next.next;
        return head;
    }
}
```

Runtime **3 ms** (beats 99.96%) · Memory **202.57 MB** (beats 59.69%) · Time taken **11m 5s**

## 2026-06-14 · [Maximum Twin Sum of a Linked List](https://leetcode.com/problems/maximum-twin-sum-of-a-linked-list/description/?envType=daily-question&envId=2026-06-14) · (2 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **2/9**

I know the fast-and-slow pointer approach, but an array felt simpler and quicker to write.

```java
class Solution {
    int pairSum(ListNode head) {
        int[] l = new int[100_000];
        int i = 1;
        l[0] = head.val;
        while (head.next != null) {
            head = head.next;
            l[i++] = head.val;
        }
        i--;
        int out = Integer.MIN_VALUE;
        for (int j = 0; j < i; j++, i--) out = Math.max(out, l[j] + l[i]);
        return out;
    }
}
```

Runtime **5 ms** (beats 58.53%) · Memory **107.68 MB** (beats 22.22%) · Time taken **13m 2s**

## 2026-06-13 · [Weighted Word Mapping](https://leetcode.com/problems/weighted-word-mapping/?envType=daily-question&envId=2026-06-14) · (1 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **2/9**

I wasn't in the mood and kept getting distracted, so logged time, not a sprint. On Saturdays I let myself take it easy.

```java
class Solution {
    String mapWordWeights(String[] words, int[] weights) {
        StringBuilder out = new StringBuilder();
        for (String w : words) {
            int sum = 0;
            for (int i = 0; i < w.length(); i++) sum += weights[w.charAt(i) - 'a'];
            sum = 25 - sum % 26;
            out.append((char) (sum + 'a'));
        }
        return out.toString();
    }
}
```

Runtime **2 ms** (beats 96.87%) · Memory **46.38 MB** (beats 82.56%) · Time taken **16m 4s**
