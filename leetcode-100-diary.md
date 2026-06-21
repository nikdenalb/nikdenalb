# LeetCode 100 diary

Personal notes from my [LeetCode 100](https://leetcode.com/) streak: solutions, submission stats, and quick reflections. This diary is an **experiment** — I may stop maintaining it at any time.

Subjective difficulty is the rating I give immediately after the attempt: `1`-`3` easy, `4`-`6` medium, `7`-`9` hard.

Progress rule: solved problems increase the counter by 1; an unsolved task resets it to 0.

Record streak: **4**

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
