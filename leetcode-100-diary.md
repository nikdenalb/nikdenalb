# LeetCode 100 diary

Personal notes from my [LeetCode 100](https://leetcode.com/) streak: solutions, submission stats, and quick reflections. I may stop maintaining it at any time.

Subjective difficulty is the rating I give immediately after the attempt: `1`-`3` easy, `4`-`6` medium, `7`-`9` hard.

Progress rule: solved problems increase the counter by 1; an unsolved task resets it to 0.

Record streak: **8**

## 2026-09-02 · [Construct Uniform Parity Array I](https://leetcode.com/problems/construct-uniform-parity-array-i/?envType=daily-question&envId=2026-09-02) · (1 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **2/9**

```java
class Solution {
    boolean uniformArray(int[] nums1) {
        return true;
    }
}
```

Runtime **0 ms** (beats 100.00%) · Memory **44.78 MB** (beats 97.04%) · Time taken **10m 29s**

## 2026-09-01 · [Minimum Moves to Clean the Classroom](https://leetcode.com/problems/minimum-moves-to-clean-the-classroom/?envType=daily-question&envId=2026-09-01) · (0 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **7/9**

Today is a rest day, but I tried solving it on my own for half an hour, and it was all in vain.

```java
class Solution {
    int[][] dirs = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};

    int minMoves(String[] classroom, int energy) {
        int m = classroom.length, n = classroom[0].length(), cnt = 0, sx = 0, sy = 0;
        int[][] id = new int[m][n];
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++) {
                char c = classroom[i].charAt(j);
                if (c == 'S') {
                    sx = i;
                    sy = j;
                } else if (c == 'L') id[i][j] = cnt++;
            }

        if (cnt == 0) return 0;

        int[][][] vis = new int[m][n][1 << cnt];
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                Arrays.fill(vis[i][j], -1);

        Queue<int[]> q = new ArrayDeque<>();
        q.offer(new int[]{sx, sy, energy, (1 << cnt) - 1, 0});
        vis[sx][sy][(1 << cnt) - 1] = energy;

        while (!q.isEmpty()) {
            int[] p = q.poll();
            int i = p[0], j = p[1], e = p[2], mask = p[3], step = p[4];
            if (mask == 0) return step;
            if (e == 0) continue;

            for (int[] d : dirs) {
                int x = i + d[0], y = j + d[1];
                if (x < 0 || x >= m || y < 0 || y >= n) continue;

                char c = classroom[x].charAt(y);
                if (c == 'X') continue;

                int nextE = c == 'R' ? energy : e - 1;
                int nextMask = mask;
                if (c == 'L') nextMask &= ~(1 << id[x][y]);

                if (vis[x][y][nextMask] >= nextE) continue;
                vis[x][y][nextMask] = nextE;
                q.offer(new int[]{x, y, nextE, nextMask, step + 1});
            }
        }
        return -1;
    }
}
```

## 2026-08-31 · [Find the Minimum and Maximum Number of Nodes Between Critical Points](https://leetcode.com/problems/find-the-minimum-and-maximum-number-of-nodes-between-critical-points/?envType=daily-question&envId=2026-08-31) · (0 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **5/9**

Closed the problem with minimal effort.

```java
class Solution {
    int[] nodesBetweenCriticalPoints(ListNode head) {
        int[] out = {-1, -1};
        int first = -1, last = -1, i = 1, min = Integer.MAX_VALUE;
        ListNode prev = head, cur = head.next;
        while (cur != null && cur.next != null) {
            if (cur.val > prev.val && cur.val > cur.next.val || cur.val < prev.val && cur.val < cur.next.val) {
                if (first == -1) first = i;
                else min = Math.min(min, i - last);
                last = i;
            }
            prev = cur;
            cur = cur.next;
            i++;
        }
        if (first != last) {
            out[0] = min;
            out[1] = last - first;
        }
        return out;
    }
}
```

## 2026-08-30 · [Removing Minimum and Maximum from Array](https://leetcode.com/problems/removing-minimum-and-maximum-from-array/?envType=daily-question&envId=2026-08-30) · (0 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **3/9**

Closed the problem with minimal effort.

```java
class Solution {
    int minimumDeletions(int[] nums) {
        int n = nums.length, min = 0, max = 0;
        for (int i = 1; i < n; i++) {
            if (nums[i] < nums[min]) min = i;
            if (nums[i] > nums[max]) max = i;
        }
        int i = Math.min(min, max), j = Math.max(min, max);
        return Math.min(Math.min(j + 1, n - i), i + 1 + n - j);
    }
}
```

## 2026-08-29 · [Make Lexicographically Smallest Array by Swapping Elements](https://leetcode.com/problems/make-lexicographically-smallest-array-by-swapping-elements/?envType=daily-question&envId=2026-08-29) · (0 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **4/9**

Closed the problem with minimal effort.

```java
class Solution {
    int[] lexicographicallySmallestArray(int[] nums, int limit) {
        int n = nums.length;
        int[][] a = new int[n][2];
        for (int p = 0; p < n; p++) {
            a[p][0] = nums[p];
            a[p][1] = p;
        }
        Arrays.sort(a, (p, r) -> p[0] - r[0]);
        int[] out = new int[n];
        for (int i = 0; i < n; ) {
            int j = i + 1;
            while (j < n && a[j][0] - a[j - 1][0] <= limit) j++;
            int[] pos = new int[j - i];
            for (int k = 0; k < j - i; k++) pos[k] = a[i + k][1];
            Arrays.sort(pos);
            for (int k = 0; k < j - i; k++) out[pos[k]] = a[i + k][0];
            i = j;
        }
        return out;
    }
}
```

## 2026-08-28 · [Lexicographically Smallest Palindromic Permutation Greater than Target](https://leetcode.com/problems/lexicographically-smallest-palindromic-permutation-greater-than-target/?envType=daily-question&envId=2026-08-28) · (0 / 100)

LeetCode difficulty **Hard** · Subjective difficulty **8/9**

Closed the problem with minimal effort.

```java
class Solution {
    String lexPalindromicPermutation(String s, String target) {
        int n = s.length(), half = n / 2, odd = 0;
        int[] cs = new int[26];
        for (char c : s.toCharArray()) cs[c - 'a']++;
        char mid = 0;
        for (int c = 0; c < 26; c++)
            if (cs[c] % 2 == 1) {
                odd++;
                mid = (char) ('a' + c);
            }

        if (odd > 1) return "";
        for (int c = 0; c < 26; c++) cs[c] /= 2;
        char[] left = new char[half];
        int i = 0;
        for (; i < half; i++) {
            int t = target.charAt(i) - 'a';
            if (cs[t] == 0) break;
            cs[t]--;
            left[i] = target.charAt(i);
        }
        if (i == half) {
            String out = palindrome(left, mid, n);
            if (out.compareTo(target) > 0) return out;
        }
        for (int j = i == half ? half - 1 : i; j >= 0; j--) {
            if (j < i) cs[target.charAt(j) - 'a']++;
            char nxt = 0;
            for (char c = (char) (target.charAt(j) + 1); c <= 'z'; c++)
                if (cs[c - 'a'] > 0) {
                    nxt = c;
                    break;
                }

            if (nxt == 0) continue;
            cs[nxt - 'a']--;
            left[j] = nxt;
            int p = j + 1;
            for (int c = 0; c < 26; c++)
                while (cs[c] > 0) {
                    left[p++] = (char) ('a' + c);
                    cs[c]--;
                }

            return palindrome(left, mid, n);
        }
        return "";
    }

    String palindrome(char[] left, char mid, int n) {
        StringBuilder out = new StringBuilder();
        out.append(left);
        if (n % 2 == 1) out.append(mid);
        for (int i = left.length - 1; i >= 0; i--) out.append(left[i]);
        return out.toString();
    }
}
```

## 2026-08-27 · [Lexicographically Smallest Permutation Greater than Target](https://leetcode.com/problems/lexicographically-smallest-permutation-greater-than-target/?envType=daily-question&envId=2026-08-27) · (0 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **5/9**

I used the hints, although I probably could have solved it myself. I just wasn't in the mood.

```java
class Solution {
    String lexGreaterPermutation(String s, String target) {
        int n = s.length();
        Map<Character, Integer> map = new HashMap<>();
        for (char c : s.toCharArray()) map.put(c, map.getOrDefault(c, 0) + 1);
        int i = 0;
        for (; i < n; i++) {
            char t = target.charAt(i);
            if (map.getOrDefault(t, 0) == 0) break;
            map.put(t, map.get(t) - 1);
        }
        for (int j = i == n ? n - 1 : i; j >= 0; j--) {
            if (j < i) map.put(target.charAt(j), map.getOrDefault(target.charAt(j), 0) + 1);
            char nxt = 0;
            for (char c = (char) (target.charAt(j) + 1); c <= 'z'; c++)
                if (map.getOrDefault(c, 0) > 0) {
                    nxt = c;
                    break;
                }
                
            if (nxt == 0) continue;
            map.put(nxt, map.get(nxt) - 1);
            StringBuilder out = new StringBuilder(target.substring(0, j));
            out.append(nxt);
            for (char c = 'a'; c <= 'z'; c++) {
                int cnt = map.getOrDefault(c, 0);
                while (cnt-- > 0) out.append(c);
            }
            return out.toString();
        }
        return "";
    }
}
```



## 2026-08-26 · [Shortest and Lexicographically Smallest Beautiful String](https://leetcode.com/problems/shortest-and-lexicographically-smallest-beautiful-string/?envType=daily-question&envId=2026-08-26) · (2 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **3/9**

```java
class Solution {
    String shortestBeautifulSubstring(String s, int k) {
        int cnt = 0, len = 101, n = s.length();
        String out = "";
        for (int i = 0, j = 0; i < n; i++) {
            if (s.charAt(i) == '1') cnt++;
            while (cnt > k) if (s.charAt(j++) == '1') cnt--;
            while (j < n && s.charAt(j) == '0') j++;
            if (cnt == k) {
                if (len > i - j + 1) {
                    out = s.substring(j, i + 1);
                    len = i - j + 1;
                } else if (len == i - j + 1 && s.substring(j, i + 1).compareTo(out) < 0) out = s.substring(j, i + 1);
            }
        }
        return out;
    }
}
```

Runtime **1 ms** (beats 100.00%) · Memory **43.49 MB** (beats 99.32%) · Time taken **20m 58s**

## 2026-08-25 · [Smallest Missing Multiple of K](https://leetcode.com/problems/smallest-missing-multiple-of-k/?envType=daily-question&envId=2026-08-25) · (1 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **2/9**

```java
class Solution {
    int missingMultiple(int[] nums, int k) {
        Set<Integer> set = new HashSet<>();
        for (int n : nums) if (n % k == 0) set.add(n);
        int out = k;
        while (true) {
            if (!set.contains(out)) return out;
            out += k;
        }
    }
}
```

Runtime **1 ms** (beats 82.00%) · Memory **44.50 MB** (beats 99.08%) · Time taken **4m 22s**

## 2026-08-24 · [Stone Game VIII](https://leetcode.com/problems/stone-game-viii/?envType=daily-question&envId=2026-08-24) · (0 / 100)

LeetCode difficulty **Hard** · Subjective difficulty **6/9**

Solved according to the manual from eunice

```java
class Solution {
    int stoneGameVIII(int[] stones) {
        int n = stones.length;
        for (int i = 1; i < n; i++) stones[i] += stones[i - 1];
        int out = stones[n - 1];
        for (int i = n - 2; i > 0; i--) out = Math.max(out, stones[i] - out);
        return out;
    }
}
```



## 2026-08-23 · [Sum Game](https://leetcode.com/problems/sum-game/?envType=daily-question&envId=2026-08-23) · (2 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **5/9**

```java
class Solution {
    boolean sumGame(String num) {
        int sumL = 0, sumR = 0, cntL = 0, cntR = 0;
        for (int i = 0; i < num.length() / 2; i++) {
            if (num.charAt(i) == '?') cntL++;
            else sumL += Integer.parseInt("" + num.charAt(i));
        }
        for (int i = num.length() / 2; i < num.length(); i++) {
            if (num.charAt(i) == '?') cntR++;
            else sumR += Integer.parseInt("" + num.charAt(i));
        }        
        int cnt = -cntL + cntR;
        int sum = sumL - sumR;

        return 2 * sum != 9 * cnt;
    }
}
```

Runtime **20 ms** (beats 5.11%) · Memory **47.13 MB** (beats 26.28%) · Time taken **15m 59s**

## 2026-08-22 · [Check Divisibility by Digit Sum and Product](https://leetcode.com/problems/check-divisibility-by-digit-sum-and-product/?envType=daily-question&envId=2026-08-22) · (1 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **2/9**

```java
class Solution {
    boolean checkDivisibility(int n) {
        int c = n, s = 0, p = 1;
        while (c > 0) {
            s += c % 10;
            p *= c % 10;
            c /= 10;
        }
        return n % (s + p) == 0;
    }
}
```

Runtime **1 ms** (beats 25.54%) · Memory **42.45 MB** (beats 42.42%) · Time taken **5m 9s**

## 2026-08-21 · [Kth Smallest Amount With Single Denomination Combination](https://leetcode.com/problems/kth-smallest-amount-with-single-denomination-combination/?envType=daily-question&envId=2026-08-21) · (0 / 100)

LeetCode difficulty **Hard** · Subjective difficulty **7/9**

I analyzed and solved the problem with AI support. I'm shifting my work time to other tasks as much as possible.

```java
class Solution {
    long findKthSmallest(int[] coins, int k) {
        int n = coins.length;
        long[] lcms = new long[1 << n];
        int[] sign = new int[1 << n];
        lcms[0] = 1;
        sign[0] = -1;
        int min = coins[0];
        for (int i = 0; i < n; i++) {
            min = Math.min(min, coins[i]);
            int bit = 1 << i;
            for (int mask = 0; mask < bit; mask++) {
                lcms[mask | bit] = lcm(lcms[mask], coins[i]);
                sign[mask | bit] = -sign[mask];
            }
        }
        long l = 1, r = (long) min * k;
        while (l < r) {
            long mid = l + (r - l) / 2, cnt = 0;
            for (int mask = 1; mask < 1 << n; mask++) cnt += mid / lcms[mask] * sign[mask];
            if (cnt >= k) r = mid;
            else l = mid + 1;
        }
        return l;
    }

    long lcm(long a, long b) {
        return a / gcd(a, b) * b;
    }

    long gcd(long a, long b) {
        while (b > 0) {
            long t = a % b;
            a = b;
            b = t;
        }
        return a;
    }
}
```



## 2026-08-21 · [Distribute Elements Into Two Arrays II](https://leetcode.com/problems/distribute-elements-into-two-arrays-ii/?envType=daily-question&envId=2026-08-21) · (2 / 100)

LeetCode difficulty **Hard** · Subjective difficulty **5/9**

```java
class Solution {
    int[] resultArray(int[] nums) {
        int n = nums.length, m = 1;
        int[] uniq = nums.clone();
        Arrays.sort(uniq);
        for (int i = 1; i < n; i++) if (uniq[i] != uniq[m - 1]) uniq[m++] = uniq[i];
        Fenwick a = new Fenwick(m);
        Fenwick b = new Fenwick(m);
        a.add(rank(uniq, m, nums[0]), 1);
        b.add(rank(uniq, m, nums[1]), 1);
        int[] arr = new int[n];
        arr[0] = nums[1];
        int i = 2, j = 0, k = 0;
        for ( ; i < n; i++) {
            int r = rank(uniq, m, nums[i]);
            int ga = j + 1 - a.sum(r);
            int gb = k + 1 - b.sum(r);
            if (ga > gb || (ga == gb && j <= k)) {
                nums[++j] = nums[i];
                a.add(r, 1);
            } else {
                arr[++k] = nums[i];
                b.add(r, 1);
            }
        }
        j++;
        k = 0;
        for ( ; j < n; j++, k++) nums[j] = arr[k];
        return nums;
    }

    int rank(int[] uniq, int m, int x) {
        int lo = 0, hi = m;
        while (lo < hi) {
            int mid = (lo + hi) >>> 1;
            if (uniq[mid] < x) lo = mid + 1;
            else hi = mid;
        }
        return lo + 1;
    }

    class Fenwick {
        int[] t;
        Fenwick(int n) { t = new int[n + 2]; }
        void add(int i, int v) { for (; i < t.length; i += i & -i) t[i] += v; }
        int sum(int i) {
            int s = 0;
            for (; i > 0; i -= i & -i) s += t[i];
            return s;
        }
    }
}
```

Runtime **37 ms** (beats 75.86%) · Memory **58.72 MB** (beats 75.86%) · Time taken **35m 38s**

## 2026-08-20 · [Distribute Elements Into Two Arrays I](https://leetcode.com/problems/distribute-elements-into-two-arrays-i/?envType=daily-question&envId=2026-08-20) · (1 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **2/9**

```java
class Solution {
    int[] resultArray(int[] nums) {
        int i = 2, j = 0, k = 0, n = nums.length;
        int[] arr = new int[n];
        arr[0] = nums[1];
        for ( ; i < n; i++) {
            if (nums[j] > arr[k]) nums[++j] = nums[i];
            else arr[++k] = nums[i];
        }
        j++;
        k = 0;
        for ( ; j < n; j++, k++) nums[j] = arr[k];
        return nums;
    }
}
```

Runtime **1 ms** (beats 98.08%) · Memory **46.72 MB** (beats 42.79%) · Time taken **16m 51s**

## 2026-08-19 · [Cinema Seat Allocation](https://leetcode.com/problems/cinema-seat-allocation/?envType=daily-question&envId=2026-08-19) · (0 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **5/9**

Not an independent solution. Used the LeetCode Editorial.

```java
class Solution {
    int maxNumberOfFamilies(int n, int[][] reservedSeats) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int[] s : reservedSeats)
            map.put(s[0], map.getOrDefault(s[0], 0) | 1 << (s[1] - 1));
        int out = 2 * n;
        for (int mask : map.values()) {
            int cnt = 0;
            if ((mask & 0b0000011110) == 0) cnt++;
            if ((mask & 0b0111100000) == 0) cnt++;
            if ((mask & 0b0001111000) == 0) cnt++;
            if (cnt == 0) out -= 2;
            else if (cnt < 3) out--;
        }
        return out;
    }
}
```



## 2026-08-18 · [Find the Largest Almost Missing Integer](https://leetcode.com/problems/find-the-largest-almost-missing-integer/?envType=daily-question&envId=2026-08-18) · (2 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **3/9**

```java
class Solution {
    int largestInteger(int[] nums, int k) {
        int n = nums.length;
        if (k == 1) {
            Map<Integer, Integer> map = new HashMap<>();
            for (int num : nums) map.put(num, map.getOrDefault(num, 0) + 1);
            return map.keySet().stream().filter(e -> map.get(e) == 1).max(Integer::compare).orElse(-1);
        }

        if (k == n) return Arrays.stream(nums).max().orElse(-1);

        int b = Math.min(nums[0], nums[n - 1]);
        int a = Math.max(nums[0], nums[n - 1]);
        if (a == b) return -1;
        boolean f = true;
        for (int i = 1; i < n - 1; i++) {
            if (nums[i] == a) {
                f = false;
                break;
            }
        }
        if (f) return a;
        for (int i = 1; i < n - 1; i++) {
            if (nums[i] == b) return -1; 
        }        
        return b;
    }
}
```

Runtime **7 ms** (beats 14.74%) · Memory **45.07 MB** (beats 57.19%) · Time taken **21m 54s**

## 2026-08-17 · [Stone Game V](https://leetcode.com/problems/stone-game-v/?envType=daily-question&envId=2026-08-17) · (1 / 100)

LeetCode difficulty **Hard** · Subjective difficulty **6/9**

```java
class Solution {
    int[] ps;
    int[][] mem;

    int stoneGameV(int[] stoneValue) {
        int n = stoneValue.length;
        ps = new int[n + 1];
        ps[0] = 0;
        for (int i = 1; i <= n; i++) ps[i] = ps[i - 1] + stoneValue[i - 1];
        mem = new int[n][n];
        for (int i = 0; i < n; i++) Arrays.fill(mem[i], -1);
        return calcAns(0, n - 1);
    }

    int calcAns(int l, int r) {
        if (l == r) return 0;
        if (mem[l][r] > -1) return mem[l][r];
        int out = 0;
        for (int x = l, add = 0; x < r; x++) {
            int sL = calcSum(l, x);
            int sR = calcSum(x + 1, r);

            if (sL < sR) add = sL + calcAns(l, x);
            else if (sL > sR) add = sR + calcAns(x + 1, r);
            else add = sL + Math.max(calcAns(l, x), calcAns(x + 1, r));

            out = Math.max(out, add);
        }
        return mem[l][r] = out;
    }

    int calcSum(int l, int r) {
        return ps[r + 1] - ps[l];
    }
}
```

Runtime **223 ms** (beats 90.38%) · Memory **47.45 MB** (beats 87.74%) · Time taken **35m 59s**

## 2026-08-16 · [Stone Game IX](https://leetcode.com/problems/stone-game-ix/?envType=daily-question&envId=2026-08-16) · (0 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **6/9**

Did not figure it out in an hour.

```java
class Solution {
    boolean stoneGameIX(int[] stones) {
        int[] div = new int[3];
        for (int s : stones) div[s % 3]++;

        if (div[0] % 2 == 0) return Math.min(div[1], div[2]) > 0;
        
        return Math.abs(div[1] - div[2]) > 2;
    }
}
```



## 2026-08-15 · [Longest Subsequence With Non-Zero Bitwise XOR](https://leetcode.com/problems/longest-subsequence-with-non-zero-bitwise-xor/?envType=daily-question&envId=2026-08-15) · (2 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **2/9**

This looks like one of the easiest medium problems. I will add it to the collection. Yes, my time really was 40 minutes. A good problem.

```java
class Solution {
    int longestSubsequence(int[] nums) {
        int n = nums.length, xor = nums[0];
        for (int i = 1; i < n; i++) xor ^= nums[i];      
        if (xor > 0) return n;
        boolean isZero = true;
        for (int i = 0; i < n; i++) {
            if (nums[i] != 0) {
                isZero = false;
                break;
            } 
        }     
        return isZero ? 0 : n - 1;
    }
}
```

Runtime **1 ms** (beats 100.00%) · Memory **133.63 MB** (beats 18.38%) · Time taken **40m 0s**

## 2026-08-14 · [Maximum Length Substring With Two Occurrences](https://leetcode.com/problems/maximum-length-substring-with-two-occurrences/?envType=daily-question&envId=2026-08-14) · (1 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **2/9**

```java
class Solution {
    int maximumLengthSubstring(String s) {
        int n = s.length(), out = 0;
        Map<Character, Integer> map = new HashMap<>();
        for (int i = 0, j = 0; i < n; i++) {
            map.put(s.charAt(i), map.getOrDefault(s.charAt(i), 0) + 1);
            while (map.get(s.charAt(i)) > 2) map.put(s.charAt(j), map.get(s.charAt(j++)) - 1);
            out = Math.max(out, i - j + 1);
        }
        return out;
    }
}
```

Runtime **3 ms** (beats 43.02%) · Memory **44.15 MB** (beats 33.61%) · Time taken **6m 31s**

## 2026-08-13 · [Longest Substring of One Repeating Character](https://leetcode.com/problems/longest-substring-of-one-repeating-character/?envType=daily-question&envId=2026-08-13) · (0 / 100)

LeetCode difficulty **Hard** · Subjective difficulty **6/9**

Did not try to solve it. Together with an AI I assembled the most academically clean solution, to build the skill. Segment-tree understanding is already there, and I do not rate such problems high on difficulty, yet they still need time spent on a hand-written solution, which I keep putting off. Time feels especially costly now, since I do not expect problems at this level in my own interviews. I will put this type of problem first in the practice queue once the pet project is no longer the main focus.

```java
class Solution {
    char[] letters;

    int[] longestRepeating(String s, String queryCharacters, int[] queryIndices) {
        letters = s.toCharArray();
        Node root = new Node(0, letters.length - 1);
        int k = queryCharacters.length();
        int[] out = new int[k];
        for (int i = 0; i < k; i++) {
            letters[queryIndices[i]] = queryCharacters.charAt(i);
            root.update(queryIndices[i]);
            out[i] = root.maxRun;
        }
        return out;
    }

    class Node {
        int leftBound, rightBound;
        int leftRun, rightRun, maxRun;
        Node left, right;

        Node(int leftBound, int rightBound) {
            this.leftBound = leftBound;
            this.rightBound = rightBound;
            if (leftBound == rightBound) {
                leftRun = rightRun = maxRun = 1;
                return;
            }
            int mid = (leftBound + rightBound) / 2;
            left = new Node(leftBound, mid);
            right = new Node(mid + 1, rightBound);
            merge();
        }

        void update(int index) {
            if (leftBound == rightBound) return;
            if (index <= left.rightBound) left.update(index);
            else right.update(index);
            merge();
        }

        void merge() {
            int leftLen = left.rightBound - left.leftBound + 1;
            int rightLen = right.rightBound - right.leftBound + 1;
            maxRun = Math.max(left.maxRun, right.maxRun);
            leftRun = left.leftRun;
            rightRun = right.rightRun;
            if (letters[left.rightBound] == letters[right.leftBound]) {
                maxRun = Math.max(maxRun, left.rightRun + right.leftRun);
                if (left.leftRun == leftLen) leftRun += right.leftRun;
                if (right.rightRun == rightLen) rightRun += left.rightRun;
            }
        }
    }
}
```



## 2026-08-12 · [Length of Longest Subarray With at Most K Frequency](https://leetcode.com/problems/length-of-longest-subarray-with-at-most-k-frequency/?envType=daily-question&envId=2026-08-12) · (4 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **4/9**

A familiar problem — not happy with how I solved it. Took too long, and the code wants a tweak.

```java
class Solution {
    int maxSubarrayLength(int[] nums, int k) {
        int n = nums.length, j = 0, out = 0;
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            map.put(nums[i], map.getOrDefault(nums[i], 0) + 1);
            while(map.get(nums[i]) > k && j < n) map.put(nums[j], map.get(nums[j++]) - 1);
            out = Math.max(out, i - j + 1);
        }
        return out;
    }
}
```

Runtime **70 ms** (beats 40.08%) · Memory **87.61 MB** (beats 96.15%) · Time taken **8m 59s**

## 2026-08-11 · [Smallest Missing Integer Greater Than Sequential Prefix Sum](https://leetcode.com/problems/smallest-missing-integer-greater-than-sequential-prefix-sum/?envType=daily-question&envId=2026-08-11) · (3 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **2/9**

I do not find complaints that the statement is confusing to be fair. I have seen that kind of comment often enough, and in the vast majority of cases I disagreed as well.

```java
class Solution {
    int missingInteger(int[] nums) {
        int n = nums.length;
        int sum = nums[0];
        for (int i = 1; i < n; i++) {
            if (nums[i] != nums[i - 1] + 1) break;
            sum += nums[i];
        }
        Set<Integer> set = new HashSet<>();
        for (int e : nums) set.add(e);
        while (set.contains(sum)) sum++;
        return sum;
    }
}
```

Runtime **2 ms** (beats 52.86%) · Memory **43.97 MB** (beats 77.96%) · Time taken **9m 17s**

## 2026-08-10 · [Stone Game IV](https://leetcode.com/problems/stone-game-iv/?envType=daily-question&envId=2026-08-10) · (2 / 100)

LeetCode difficulty **Hard** · Subjective difficulty **3/9**

A problem with intimidating (and confusing) difficulty. I felt like collecting extreme problems — the easiest or hardest ones in their difficulty band. I will start that list with this one.

```java
class Solution {
    boolean winnerSquareGame(int n) {
        boolean[] dp = new boolean[n + 1];
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j * j <= i; j++) {
                if (j * j == i || (i - j * j > 0 && !dp[i - j * j])) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[n];
    }
}
```

Runtime **15 ms** (beats 64.49%) · Memory **42.56 MB** (beats 68.41%) · Time taken **18m 38s**

## 2026-08-09 · [Stone Game II](https://leetcode.com/problems/stone-game-ii/?envType=daily-question&envId=2026-08-09) · (1 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **5/9**

This is a brute-force problem via recursion. Memoization is added so the solution does not TL.

```java
class Solution {
    int[][] mem;

    int stoneGameII(int[] piles) {
        int n = piles.length;
        mem = new int[n][n];
        for (int[] row : mem) Arrays.fill(row, Integer.MIN_VALUE);
        int total = Arrays.stream(piles).sum();
        return (total + cnt(piles, 0, 1)) / 2;
    }

    int cnt(int[] piles, int i, int m) {
        int n = piles.length;
        if (i == n) return 0;
        if (mem[i][m - 1] != Integer.MIN_VALUE) return mem[i][m - 1];
        int max = Integer.MIN_VALUE;
        for (int k = 0; k < 2 * m && i + k < n; k++) {
            int stns = 0;
            for (int j = 0; j <= k; j++) stns += piles[i + j];
            max = Math.max(max, stns - cnt(piles, i + k + 1, Math.max(m, k + 1)));
        }
        return mem[i][m - 1] = max;
    }
}
```

Runtime **12 ms** (beats 36.12%) · Memory **44.56 MB** (beats 56.71%) · Time taken **42m 34s**

## 2026-08-08 · [Find the Lexicographically Smallest Valid Sequence](https://leetcode.com/problems/find-the-lexicographically-smallest-valid-sequence/?envType=daily-question&envId=2026-08-08) · (0 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **7/9**

Did not solve it. Only figured out the main idea toward the end of the hour. A developmental problem that fits my current level.

```java
class Solution {
    int[] validSequence(String word1, String word2) {
        int m = word1.length(), n = word2.length();
        int[] last = new int[n];
        Arrays.fill(last, -1);
        for (int i = m - 1, j = n - 1; i >= 0 && j >= 0; i--)
            if (word1.charAt(i) == word2.charAt(j)) last[j--] = i;

        int[] out = new int[n];
        boolean isChanged = false;
        int j = 0;
        for (int i = 0; i < m && j < n; i++) {
            if (word1.charAt(i) == word2.charAt(j)) out[j++] = i;
            else if (!isChanged && (j == n - 1 || i < last[j + 1])) {
                out[j++] = i;
                isChanged = true;
            }
        }
        return j == n ? out : new int[0];
    }
}
```



## 2026-08-07 · [Smallest Divisible Digit Product II](https://leetcode.com/problems/smallest-divisible-digit-product-ii/?envType=daily-question&envId=2026-08-07) · (0 / 100)

LeetCode difficulty **Hard** · Subjective difficulty **8/9**

Did not try to solve it — went straight to working through the solution with an AI from the Editorial. Would not have solved it alone. The problem stays on the long shelf until there is time to practice it properly.

```java
class Solution {
    String smallestNumber(String num, long t) {
        long x = t;
        for (int i = 2; i <= 9; i++) while (x % i == 0) x /= i;
        if (x > 1) return "-1";

        int n = num.length();
        char[] a = num.toCharArray();
        long[] rem = new long[n + 1];
        rem[0] = t;
        int pos = n - 1;

        for (int i = 0; i < n; i++) {
            if (a[i] == '0') {
                pos = i;
                break;
            }
            rem[i + 1] = rem[i] / gcd(rem[i], a[i] - '0');
        }
        if (rem[n] == 1) return num;

        for (int i = pos; i >= 0; i--) {
            while (++a[i] <= '9') {
                long need = rem[i] / gcd(rem[i], a[i] - '0');
                int k = 9;
                for (int j = n - 1; j > i; j--) {
                    while (need % k != 0) k--;
                    need /= k;
                    a[j] = (char) ('0' + k);
                }
                if (need == 1) return new String(a);
            }
        }

        StringBuilder out = new StringBuilder();
        x = t;
        for (int i = 9; i > 1; i--) while (x % i == 0) {
                out.append((char) ('0' + i));
                x /= i;
            }
        
        int pad = Math.max(n + 1 - out.length(), 0);
        for (int i = 0; i < pad; i++) out.append('1');
        return out.reverse().toString();
    }

    long gcd(long a, long b) {
        while (b > 0) {
            long t = a % b;
            a = b;
            b = t;
        }
        return a;
    }
}
```



## 2026-08-06 · [Smallest Divisible Digit Product I](https://leetcode.com/problems/smallest-divisible-digit-product-i/?envType=daily-question&envId=2026-08-06) · (8 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **2/9**

```java
class Solution {
    int smallestNumber(int n, int t) {
        while (prodD(n) % t != 0) n++;
        return n;
    }
    
    int prodD(int n) {
        int out = 1;
        while (n > 0) {
            out *= n % 10;
            n /= 10;
        }
        return out;
    }
}
```

Runtime **1 ms** (beats 100.00%) · Memory **42.56 MB** (beats 65.90%) · Time taken **5m 18s**

## 2026-08-05 · [Remove Methods From Project](https://leetcode.com/problems/remove-methods-from-project/?envType=daily-question&envId=2026-08-05) · (7 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **5/9**

```java
class Solution {
    List<Integer> remainingMethods(int n, int k, int[][] invocations) {
        List<Integer>[] list = new ArrayList[n];
        for (int i = 0; i < n; i++) list[i] = new ArrayList<>();
        for (int[] g : invocations) list[g[0]].add(g[1]);

        boolean[] susp = new boolean[n];
        Queue<Integer> q = new ArrayDeque<>();
        q.offer(k);
        while (!q.isEmpty()) {
            int v = q.poll();
            susp[v] = true;
            for (int u : list[v]) {
                if (susp[u]) continue;
                q.offer(u);
                susp[u] = true;
            }
        }
        
        for (int[] g : invocations) if (!susp[g[0]] && susp[g[1]])
            return IntStream.range(0, n).boxed().toList();

        return IntStream.range(0, n).boxed().filter(e -> !susp[e]).toList();
    }
}
```

Runtime **58 ms** (beats 82.73%) · Memory **283.80 MB** (beats 60.00%) · Time taken **41m 55s**

## 2026-08-04 · [Find Missing Elements](https://leetcode.com/problems/find-missing-elements/?envType=daily-question&envId=2026-08-04) · (6 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **2/9**

```java
class Solution {
    List<Integer> findMissingElements(int[] nums) {
        Arrays.sort(nums);
        List<Integer> out = new ArrayList<>();
        int curr = nums[0];
        for (int i = 1; i < nums.length; i++) {
            curr++;
            while (curr != nums[i]) out.add(curr++);
        }
        return out;
    }
}
```

Runtime **7 ms** (beats 22.50%) · Memory **47.42 MB** (beats 5.33%) · Time taken **5m 51s**

## 2026-08-03 · [Stone Game III](https://leetcode.com/problems/stone-game-iii/?envType=daily-question&envId=2026-08-03) · (5 / 100)

LeetCode difficulty **Hard** · Subjective difficulty **6/9**

About 20–25 minutes went into the solution itself. The rest of the time went into realizing I should solve from the end.

```java
class Solution {
    String stoneGameIII(int[] stoneValue) {  
        int n = stoneValue.length - 1;
        int[] da = new int[n + 1];
        int[] db = new int[n + 1];
        if (n == 0) da[0] = stoneValue[0];
        if (n == 1) da[0] = Math.max(stoneValue[0] + stoneValue[1], stoneValue[0] - stoneValue[1]);
        if (n > 1) {
            da[n] = stoneValue[n];
            db[n] = -stoneValue[n];
            
            da[n - 1] = Math.max(stoneValue[n - 1] + db[n], stoneValue[n - 1] + stoneValue[n]);
            db[n - 1] = Math.min(-stoneValue[n - 1] - stoneValue[n], -stoneValue[n - 1] + da[n]);
            
            da[n - 2] = Math.max(Math.max(
                stoneValue[n - 2] + stoneValue[n - 1] + stoneValue[n],
                stoneValue[n - 2] + stoneValue[n - 1] + db[n]),
                stoneValue[n - 2] + db[n - 1]
            );
            db[n - 2] = Math.min(Math.min(
                -stoneValue[n - 2] - stoneValue[n - 1] - stoneValue[n],
                -stoneValue[n - 2] - stoneValue[n - 1] + da[n]),
                -stoneValue[n - 2] + da[n - 1]
            );
            
            for (int i = n - 3; i >= 0; i--) {
                da[i] = Math.max(Math.max(
                    stoneValue[i] + stoneValue[i + 1] + stoneValue[i + 2] + db[i + 3],
                    stoneValue[i] + stoneValue[i + 1] + db[i + 2]),
                    stoneValue[i] + db[i + 1]
                );
                db[i] = Math.min(Math.min(
                    -stoneValue[i] - stoneValue[i + 1] - stoneValue[i + 2] + da[i + 3],
                    -stoneValue[i] - stoneValue[i + 1] + da[i + 2]),
                    -stoneValue[i] + da[i + 1]
                );                
            }
            /*
                 0  1  2  3  4  5  6  7  8  9  n
                [ ][ ][ ][ ][ ][ ][ ][i][*][*][*]
                [ ][ ][ ][ ][ ][ ][ ][i][*][*][*]
            */                    
        }
        if (da[0] > 0) return "Alice";
        if (da[0] < 0) return "Bob";
        return "Tie";
    }
}
```

Runtime **6 ms** (beats 92.61%) · Memory **85.52 MB** (beats 65.91%) · Time taken **54m 14s**

## 2026-08-02 · [Stone Game](https://leetcode.com/problems/stone-game/?envType=daily-question&envId=2026-08-02) · (4 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **5/9**

I worked through the non-recursive approach in [eunice](https://leetcode.com/problems/predict-the-winner/solutions/8433447/solution-by-la_castille-ia0j)'s write-up for yesterday's daily. Today I only needed to repeat it.

```java
class Solution {
    boolean stoneGame(int[] piles) {
        int n = piles.length;
        if (n % 2 == 0) return true;
        int[] dp = new int[n];
        for (int i = n - 1; i >= 0; i--) {
            dp[i] = piles[i];
            for (int j = i + 1; j < n; j++) dp[j] = Math.max(piles[i] - dp[j], piles[j] - dp[j - 1]);
        }
        return dp[n - 1] > 0;
    }
}
```

Runtime **0 ms** (beats 100.00%) · Memory **43.27 MB** (beats 35.84%) · Time taken **12m 5s**

## 2026-08-01 · [Predict the Winner](https://leetcode.com/problems/predict-the-winner/?envType=daily-question&envId=2026-08-01) · (3 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **5/9**

I could not implement a solution without recursion, after sitting a long time on attempts at that. Solved it poorly just to submit.

```java
class Solution {
    boolean predictTheWinner(int[] nums) {
        return diff(nums, 0, nums.length - 1) >= 0;
    }

    int diff(int[] nums, int i, int j) {
        if (i == j) return nums[i];
        if (j - i == 1) return Math.max(nums[i] - nums[j], nums[j] - nums[i]);
        return Math.max(nums[i] - diff(nums, i + 1, j), nums[j] - diff(nums, i, j - 1));
    }
}
```

Runtime **37 ms** (beats 31.03%) · Memory **42.92 MB** (beats 23.56%) · Time taken **29m 39s**

## 2026-07-31 · [Minimum Number of Pushes to Type Word II](https://leetcode.com/problems/minimum-number-of-pushes-to-type-word-ii/?envType=daily-question&envId=2026-07-31) · (2 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **3/9**

```java
class Solution {
    int minimumPushes(String word) {
        int[] arr = new int[26];
        for (char c : word.toCharArray()) arr[c - 'a']++;
        Arrays.sort(arr);
        int out = 0;
        for (int i = 25, cnt = 8; i >= 0; i--, cnt++) out += arr[i] * (cnt / 8);
        return out;
    }
}
```

Runtime **9 ms** (beats 94.22%) · Memory **48.20 MB** (beats 32.08%) · Time taken **7m 31s**

## 2026-07-30 · [Minimum Number of Pushes to Type Word I](https://leetcode.com/problems/minimum-number-of-pushes-to-type-word-i/?envType=daily-question&envId=2026-07-30) · (1 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **2/9**

```java
class Solution {
    int minimumPushes(String word) {
        int n = word.length();
        int out = 0;
        for (int i = 1; n > 0; i++) {
            out += Math.min(n, 8) * i;
            n -= 8;
        }
        return out;
    }
}
```

Runtime **0 ms** (beats 100.00%) · Memory **43.34 MB** (beats 28.23%) · Time taken **8m 58s**

## 2026-07-29 · [Smallest Palindromic Rearrangement II](https://leetcode.com/problems/smallest-palindromic-rearrangement-ii/?envType=daily-question&envId=2026-07-29) · (0 / 100)

LeetCode difficulty **Hard** · Subjective difficulty **7/9**

I got scared of a hard problem and read this one before solving it. The excuse is lack of time — a hard task can stretch to 3–4 hours. There was a chance to solve this one, but I cannot count it.

```java
class Solution {
    String smallestPalindrome(String s, int k) {
        int len = s.length() / 2;
        int[] lts = new int[26];
        for (int i = 0; i < len; i++) lts[s.charAt(i) - 'a']++;

        if (cntPerms(lts, k) < k) return "";

        StringBuilder out = new StringBuilder();
        for (int p = 0; p < len; p++) {
            for (int i = 0; i < 26; i++) {
                if (lts[i] == 0) continue;

                lts[i]--;
                int cnt = cntPerms(lts, k);
                if (cnt >= k) {
                    out.append((char) (i + 'a'));
                    break;
                }

                k -= cnt;
                lts[i]++;
            }
        }

        StringBuilder h = new StringBuilder(out);
        if (s.length() % 2 == 1) out.append(s.charAt(len));
        out.append(h.reverse());
        return out.toString();
    }

    int cntPerms(int[] lts, int k) {
        int bag = 0;
        for (int e : lts) bag += e;

        long out = 1;
        for (int e : lts) {
            int r = Math.min(e, bag - e);
            for (int i = 1; i <= r; i++) {
                out = out * (bag - i + 1) / i;
                if (out >= k) return k;
            }
            bag -= e;
        }
        return (int) out;
    }
}
```



## 2026-07-28 · [Smallest Palindromic Rearrangement I](https://leetcode.com/problems/smallest-palindromic-rearrangement-i/?envType=daily-question&envId=2026-07-28) · (6 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **3/9**

```java
class Solution {
    String smallestPalindrome(String s) {
        int len = (s.length()) / 2;
        int[] lts = new int[26];
        for (int i = 0; i < len; i++) lts[s.charAt(i) - 'a']++;
        StringBuilder out = new StringBuilder();
        for (int i = 0; i < 26; i++) for (int j = 0; j < lts[i]; j++) out.append((char) (i + 'a'));
        StringBuilder h = new StringBuilder(out);
        if (s.length() % 2 == 1) out.append(s.charAt(len));
        out.append(h.reverse());
        return out.toString();
    }
}
```

Runtime **25 ms** (beats 76.71%) · Memory **48.49 MB** (beats 10.04%) · Time taken **20m 26s**

## 2026-07-27 · [Maximum Product of Two Elements in an Array](https://leetcode.com/problems/maximum-product-of-two-elements-in-an-array/?envType=daily-question&envId=2026-07-27) · (5 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **2/9**

```java
class Solution {
    int maxProduct(int[] nums) {
        int a = 0, b = 0;
        for (int n : nums) {
            if (n > a) {
                b = a;
                a = n;
            } else if (n > b) b = n;
        }
        return (a - 1) * (b - 1);
    }
}
```

Runtime **0 ms** (beats 100.00%) · Memory **44.41 MB** (beats 73.61%) · Time taken **2m 46s**

## 2026-07-26 · [Maximum Product of Three Numbers](https://leetcode.com/problems/maximum-product-of-three-numbers/?envType=daily-question&envId=2026-07-26) · (4 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **3/9**

I had already solved a problem like this on CodeRun when I was just starting to get interested in algorithms. One of the best easy problems.

```java
class Solution {
    int maximumProduct(int[] nums) {
        int max1 = Integer.MIN_VALUE, max2 = Integer.MIN_VALUE, max3 = Integer.MIN_VALUE;
        int min1 = Integer.MAX_VALUE, min2 = Integer.MAX_VALUE;
        for (int n : nums) {
            if (n > max1) {
                max3 = max2;
                max2 = max1;
                max1 = n;
            } else if (n > max2) {
                max3 = max2;
                max2 = n;
            } else if (n > max3) max3 = n;
            if (n < min1) {
                min2 = min1;
                min1 = n;
            } else if (n < min2) min2 = n;
        }
        return Math.max(min1 * min2 * max1, max1 * max2 * max3);
    }
}
```

Runtime **2 ms** (beats 99.61%) · Memory **47.05 MB** (beats 97.34%) · Time taken **6m 11s**

## 2026-07-25 · [Maximum Product of Two Digits](https://leetcode.com/problems/maximum-product-of-two-digits/?envType=daily-question&envId=2026-07-25) · (3 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **2/9**

```java
class Solution {
    int maxProduct(int n) {
        int a = 0, b = 0;
        while (n > 0) {
            int t = n % 10;
            if (t > a) {
                b = a;
                a = t;
            } else if (t > b) b = t;
            n /= 10;
        }
        return a * b;
    }
}
```

Runtime **1 ms** (beats 100.00%) · Memory **42.49 MB** (beats 85.43%) · Time taken **4m 3s**

## 2026-07-24 · [Number of Unique XOR Triplets II](https://leetcode.com/problems/number-of-unique-xor-triplets-ii/?envType=daily-question&envId=2026-07-24) · (2 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **5/9**

```java
class Solution {
    int uniqueXorTriplets(int[] nums) {
        int n = nums.length;
        boolean[] set1 = new boolean[2048];
        boolean[] set2 = new boolean[2048];
        for (int e : nums) set1[e] = true;
        for (int i = 0; i < 2048; i++) {
            if (!set1[i]) continue;
            for (int j = 0; j < 2048; j++) {
                if (!set1[j]) continue;
                set2[i ^ j] = true;
            }
        } 
        int out = 0;
        boolean[] set3 = new boolean[2048];
        for (int i = 0; i < 2048; i++) {
            if (!set1[i]) continue;
            for (int j = 0; j < 2048; j++) {
                if (!set2[j]) continue;
                int x = i ^ j;
                if (!set3[x]) out++;
                set3[x] = true;
            }
        }
        return out;
    }
}
```

Runtime **359 ms** (beats 43.64%) · Memory **46.59 MB** (beats 98.18%) · Time taken **26m 17s**

## 2026-07-23 · [Number of Unique XOR Triplets I](https://leetcode.com/problems/number-of-unique-xor-triplets-i/?envType=daily-question&envId=2026-07-23) · (1 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **4/9**

A good problem. Little code and a lot of thinking.

```java
class Solution {
    int uniqueXorTriplets(int[] nums) {
        int n = nums.length;
        if (n <= 2) return n;
        int out = 1;
        while (out <= n) out <<= 1;
        return out;
    }
}
```

Runtime **0 ms** (beats 100.00%) · Memory **124.94 MB** (beats 98.04%) · Time taken **15m 4s**

## 2026-07-22 · [Maximize Active Section with Trade II](https://leetcode.com/problems/maximize-active-section-with-trade-ii/?envType=daily-question&envId=2026-07-22) · (0 / 100)

LeetCode difficulty **Hard** · Subjective difficulty **8/9**

Far from an independent solution.

```java
class Solution {
    List<Integer> maxActiveSectionsAfterTrade(String s, int[][] queries) {
        int n = s.length();
        List<Integer> list = new ArrayList<>();
        List<Integer> pos = new ArrayList<>();
        int[] blockAt = new int[n];
        char p = s.charAt(0);
        int cnt = 0, ones = 0, block = 0;

        for (int i = 0; i < n; i++) {
            char c = s.charAt(i);
            if (c == '1') ones++;
            if (c == p) cnt++;
            else {
                list.add(cnt);
                pos.add(block);
                p = c;
                cnt = 1;
                block = i;
            }
            blockAt[i] = list.size();
        }
        list.add(cnt);
        pos.add(block);

        int m = queries.length;
        List<Integer> out = new ArrayList<>(m);

        int z0 = s.charAt(0) == '0' ? 0 : 1;
        int zCnt = 0;
        for (int i = z0; i < list.size(); i += 2) zCnt++;
        if (zCnt < 2) {
            for (int i = 0; i < m; i++) out.add(ones);
            return out;
        }

        int[] merge = new int[zCnt - 1];
        for (int i = z0, t = 0; i + 2 < list.size(); i += 2, t++)
            merge[t] = list.get(i) + list.get(i + 2);
        SparseTable st = new SparseTable(merge);

        int[] groupAt = new int[n];
        int cur = -1;
        for (int i = 0; i < n; i++) {
            int bi = blockAt[i];
            if (bi >= z0 && (bi - z0) % 2 == 0)
                cur = (bi - z0) / 2;
            groupAt[i] = cur;
        }

        for (int qi = 0; qi < m; qi++) {
            int l = queries[qi][0], r = queries[qi][1];
            int gL = groupAt[l], gR = groupAt[r];

            int leftCnt = -1, rightCnt = -1;
            if (s.charAt(l) == '0') {
                int bi = blockAt[l];
                leftCnt = pos.get(bi) + list.get(bi) - l;
            }
            if (s.charAt(r) == '0') {
                int bi = blockAt[r];
                rightCnt = r - pos.get(bi) + 1;
            }

            int left = gL + 1;
            int right = s.charAt(r) == '0' ? gR - 1 : gR;

            int best = ones;
            if (left <= right - 1)
                best = Math.max(best, ones + st.query(left, right - 1));
            if (s.charAt(l) == '0' && s.charAt(r) == '0' && gL + 1 == gR)
                best = Math.max(best, ones + leftCnt + rightCnt);
            if (s.charAt(l) == '0' && gL + 1 <= right)
                best = Math.max(best, ones + leftCnt + list.get(z0 + 2 * (gL + 1)));
            if (s.charAt(r) == '0' && left <= gR - 1)
                best = Math.max(best, ones + rightCnt + list.get(z0 + 2 * (gR - 1)));
            out.add(best);
        }
        return out;
    }

    class SparseTable {
        int[][] table;
        int[] log;

        SparseTable(int[] a) {
            int n = a.length;
            log = new int[n + 1];
            for (int i = 2; i <= n; i++) log[i] = log[i / 2] + 1;
            int k = log[n] + 1;
            table = new int[k][n];
            for (int i = 0; i < n; i++) table[0][i] = a[i];
            for (int j = 1; j < k; j++)
                for (int i = 0; i + (1 << j) <= n; i++)
                    table[j][i] = Math.max(table[j - 1][i], table[j - 1][i + (1 << (j - 1))]);
        }

        int query(int l, int r) {
            int j = log[r - l + 1];
            return Math.max(table[j][l], table[j][r - (1 << j) + 1]);
        }
    }
}
```



## 2026-07-21 · [Maximize Active Section with Trade I](https://leetcode.com/problems/maximize-active-section-with-trade-i/?envType=daily-question&envId=2026-07-21) · (2 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **3/9**

Sometimes I struggle to invent comments. Next time I will just leave that field empty.

```java
class Solution {
    int maxActiveSectionsAfterTrade(String s) {
        List<Integer> list = new ArrayList<>();
        char p = s.charAt(0);
        int cnt = 0, ones = 0;
        for (char c : s.toCharArray()) {
            if (c == '1') ones++;
            if (c == p) cnt++;
            else {
                list.add(cnt);
                p = c;
                cnt = 1;
            }
        }
        list.add(cnt);

        int maxSwap = 0;
        for (int i = s.charAt(0) == '0' ? 0 : 1; i < list.size(); i += 2)
            if (i + 2 < list.size())
                maxSwap = Math.max(maxSwap, list.get(i) + list.get(i + 2));

        return ones + maxSwap;
    }
}
```

Runtime **71 ms** (beats 76.66%) · Memory **48.10 MB** (beats 31.80%) · Time taken **21m 55s**

## 2026-07-20 · [Shift 2D Grid](https://leetcode.com/problems/shift-2d-grid/?envType=daily-question&envId=2026-07-20) · (1 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **2/9**

If you do not try to solve it in-place, it is a fairly simple problem despite the bulky code.

```java
class Solution {
    List<List<Integer>> shiftGrid(int[][] grid, int k) {
        int m = grid.length, n = grid[0].length;
        int mod = m * n;
        k %= mod;
        int[][] out = new int[m][n];
        for (int d = 0; d < mod; d++) out[((k + d) % mod) / n][(k + d) % n] = grid[(d % mod) / n][d % n];
        List<List<Integer>> outList = new ArrayList<>();
        for (int i = 0; i < m; i++) {
            List<Integer> list = new ArrayList();
            for (int j = 0; j < n; j++) list.add(out[i][j]);
            outList.add(list);
        }
        return outList;
    }
}
```

Runtime **5 ms** (beats 88.49%) · Memory **47.22 MB** (beats 38.71%) · Time taken **10m 31s**

## 2026-07-19 · [Smallest Subsequence of Distinct Characters](https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/?envType=daily-question&envId=2026-07-19) · (0 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **7/9**

I am writing this entry five days later. I did not solve the problem and no longer remember the details. I need to try to log entries right away, but this was a stretch when I wanted to drop the whole thing.

```java
class Solution {
    String smallestSubsequence(String s) {
        int[] freq = new int[26];
        boolean[] seen = new boolean[26];
        Deque<Character> st = new ArrayDeque<>();

        for (int i = 0; i < s.length(); i++)
            freq[s.charAt(i) - 'a']++;

        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            int x = c - 'a';
            freq[x]--;
            if (seen[x]) continue;

            while (!st.isEmpty() && st.peek() > c && freq[st.peek() - 'a'] > 0) {
                seen[st.peek() - 'a'] = false;
                st.pop();
            }
            st.push(c);
            seen[x] = true;
        }

        StringBuilder out = new StringBuilder();
        while (!st.isEmpty()) out.append(st.pop());
        return out.reverse().toString();
    }
}
```



## 2026-07-18 · [Find Greatest Common Divisor of Array](https://leetcode.com/problems/find-greatest-common-divisor-of-array/?envType=daily-question&envId=2026-07-18) · (1 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **1/9**

The problem is unpleasant because of its inconsistency.

```java
class Solution {
    int findGCD(int[] nums) {
        int max = 1, min = 1000;
        for (int num : nums) {
            max = Math.max(max, num);
            min = Math.min(min, num);
        }
        return gcd(max, min);
    }
    
    int gcd(int a, int b) {
        while (b > 0) {
            int t = a % b;
            a = b;
            b = t;
        }
        return a;
    }
}
```

Runtime **0 ms** (beats 100.00%) · Memory **44.92 MB** (beats 73.70%) · Time taken **3m 29s**

## 2026-07-17 · [Sorted GCD Pair Queries](https://leetcode.com/problems/sorted-gcd-pair-queries/?envType=daily-question&envId=2026-07-17) · (0 / 100)

LeetCode difficulty **Hard** · Subjective difficulty **7/9**

Did not solve it. In a good state I might have managed it, but in the daily routine it is far from always possible to solve in such a state.

Final solution based on [eunice](https://leetcode.com/problems/sorted-gcd-pair-queries/solutions/8402129/solution-by-la_castille-9n7e/?envType=daily-question&envId=2026-07-17) solution:

```java
class Solution {
    int[] gcdValues(int[] nums, long[] queries) {
        int maxValue = 0;
        for (int num : nums) maxValue = Math.max(maxValue, num);

        int[] freq = new int[maxValue + 1];
        long[] gcdCount = new long[maxValue + 1];

        for (int num : nums) freq[num]++;

        for (int d = maxValue; d >= 1; d--) {
            long multiples = 0;
            long higher = 0;
            for (int j = d; j <= maxValue; j += d) {
                multiples += freq[j];
                higher += gcdCount[j];
            }
            gcdCount[d] = multiples * (multiples - 1) / 2 - higher;
        }

        for (int d = 1; d <= maxValue; d++)
            gcdCount[d] += gcdCount[d - 1];

        int m = queries.length;
        int[] out = new int[m];
        for (int i = 0; i < m; i++) {
            long query = queries[i];
            int left = 0, right = maxValue + 1;
            while (left < right) {
                int mid = (left + right) >>> 1;
                if (gcdCount[mid] <= query) left = mid + 1;
                else right = mid;
            }
            out[i] = left;
        }
        return out;
    }
}
```



## 2026-07-16 · [Sum of GCD of Formed Pairs](https://leetcode.com/problems/sum-of-gcd-of-formed-pairs/?envType=daily-question&envId=2026-07-16) · (2 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **2/9**

An implementation problem. I only set the difficulty high because of needing prefix sums and GCD.

```java
class Solution {
    long gcdSum(int[] nums) {
        int n = nums.length;
        int[] prefixGcd = new int[n];
        prefixGcd[0] = nums[0];
        for (int i = 1, max = nums[0]; i < n; i++) {
            max = Math.max(max, nums[i]);
            prefixGcd[i] = gcd(max, nums[i]);
        }
        Arrays.sort(prefixGcd);
        long out = 0;
        for (int i = 0, j = n - 1; i < j; i++, j--) out += gcd(prefixGcd[i], prefixGcd[j]);
        return out;
    }

    int gcd(int a, int b) {
        while (b > 0) {
            int t = a % b;
            a = b;
            b = t;
        }
        return a;
    }
}
```

Runtime **52 ms** (beats 99.50%) · Memory **107.66 MB** (beats 97.00%) · Time taken **11m 58s**

## 2026-07-15 · [GCD of Odd and Even Sums](https://leetcode.com/problems/gcd-of-odd-and-even-sums/?envType=daily-question&envId=2026-07-15) · (1 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **2/9**

Shameful for me. There will be no beautification on purpose.

```java
class Solution {
    int gcdOfOddEvenSums(int n) {
        int sumOdd = n * n;
        int sumEven = (n * 2 + 2) * n / 2;
        return gcd(sumOdd, sumEven);
    }
    
    int gcd(int a, int b) {
        while (b > 0) {
            int t = a % b;
            a = b;
            b = t;
        }
        return a;
    }
}
```

Runtime **1 ms** (beats 83.02%) · Memory **42.80 MB** (beats 6.14%) · Time taken **10m 33s**

## 2026-07-14 · [Find the Number of Subsequences With Equal GCD](https://leetcode.com/problems/find-the-number-of-subsequences-with-equal-gcd/?envType=daily-question&envId=2026-07-14) · (0 / 100)

LeetCode difficulty **Hard** · Subjective difficulty **7/9**

The problem seems easy, but thinking in three dimensions turned out too hard. Closed it with a few critical hints from an LLM. I will come back to it in two months and see how hard it is to write from scratch then. I know there is a more optimal solution, and I will look at that even later.

```java
class Solution {
    int mod = 1_000_000_007;

    int subsequencePairCount(int[] nums) {
        int n = nums.length;
        int[][][] dp = new int[n + 1][201][201];
        dp[0][0][0] = 1;

        for (int i = 0; i < n; i++) {
            int num = nums[i];
            for (int g1 = 0; g1 < 201; g1++) {
                for (int g2 = 0; g2 < 201; g2++) {
                    int cur = dp[i][g1][g2];
                    if (cur == 0) continue;

                    dp[i + 1][g1][g2] = (int) ((long) dp[i + 1][g1][g2] + cur) % mod;

                    int ng1 = g1 == 0 ? num : gcd(g1, num);
                    dp[i + 1][ng1][g2] = (int) ((long) dp[i + 1][ng1][g2] + cur) % mod;

                    int ng2 = g2 == 0 ? num : gcd(g2, num);
                    dp[i + 1][g1][ng2] = (int) ((long) dp[i + 1][g1][ng2] + cur) % mod;
                }
            }
        }

        int out = 0;
        for (int g = 1; g < 201; g++)
            out = (int) ((long) out + dp[n][g][g]) % mod;
        return out;
    }

    int gcd(int a, int b) {
        while (b > 0) {
            int t = a % b;
            a = b;
            b = t;
        }
        return a;
    }
}
```



## 2026-07-13 · [Sequential Digits](https://leetcode.com/problems/sequential-digits/?envType=daily-question&envId=2026-07-13) · (2 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **3/9**

Coming up with a generator is not that easy, but it is not needed either.

```java
class Solution {
    List<Integer> sequentialDigits(int low, int high) {
        List<Integer> out = new ArrayList<>();
        int[] val = new int[]{
            12,         23,         34,         45,     56,     67,     78,     89,
            123,        234,        345,        456,    567,    678,    789,
            1234,       2345,       3456,       4567,   5678,   6789,
            12345,      23456,      34567,      45678,  56789,
            123456,     234567,     345678,     456789,
            1234567,    2345678,    3456789,
            12345678,   23456789,
            123456789 
        };
        for (int v : val) {
            if (v > high) break;
            if (v >= low) out.add(v);
        }
        return out;
    }
}
```

Runtime **0 ms** (beats 100.00%) · Memory **42.23 MB** (beats 73.12%) · Time taken **30m 35s**

## 2026-07-12 · [Rank Transform of an Array](https://leetcode.com/problems/rank-transform-of-an-array/?envType=daily-question&envId=2026-07-12) · (1 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **3/9**

The time was not competitive. This is not an easy task for me, and not solving it quickly upset me.

```java
class Solution {
    int[] arrayRankTransform(int[] arr) {
        int n = arr.length;
        if (n == 0) return new int[0];
        int[] sort = arr.clone(); 
        Arrays.sort(sort);
        int[] rank = new int[n];
        rank[0] = 1;
        for (int i = 1; i < n; i++) rank[i] = sort[i] == sort[i - 1] ? rank[i - 1] : rank[i - 1] + 1;
        int[] out = new int[n];
        for (int i = 0; i < n; i++) out[i] = rank[Arrays.binarySearch(sort, arr[i])];
        return out;
    }
}
```

Runtime **29 ms** (beats 84.49%) · Memory **68.80 MB** (beats 97.17%) · Time taken **24m 58s**

## 2026-07-11 · [Count the Number of Complete Components](https://leetcode.com/problems/count-the-number-of-complete-components/?envType=daily-question&envId=2026-07-11) · (0 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **5/9**

Did not finish on time and did not really try. Solved it myself with a few small hints from an LLM.

```java
class Solution {
    int countCompleteComponents(int n, int[][] edges) {
        int[] nods = new int[n];
        int[] comp = new int[n];
        for (int i = 0; i < n; i++) comp[i] = i;

        for (int i = 0; i < edges.length; i++) {
            int a = Math.min(edges[i][0], edges[i][1]);
            int b = Math.max(edges[i][0], edges[i][1]);
            nods[a]++;
            nods[b]++;
            edges[i][0] = a;
            edges[i][1] = b;
        }
        Arrays.sort(edges, (a, b) -> Integer.compare(a[0], b[0]));

        for (int[] ed : edges) {
            int ra = ed[0];
            while (comp[ra] != ra) ra = comp[ra];
            int rb = ed[1];
            while (comp[rb] != rb) rb = comp[rb];
            int root = Math.min(ra, rb);
            comp[ra] = root;
            comp[rb] = root;
        }

        for (int i = 0; i < n; i++) {
            int r = i;
            while (comp[r] != r) r = comp[r];
            comp[i] = r;
        }

        int[] cnt = new int[n];
        int[] ecnt = new int[n];
        for (int i = 0; i < n; i++) cnt[comp[i]]++;
        for (int[] ed : edges) ecnt[comp[ed[0]]]++;

        int out = 0;
        for (int i = 0; i < n; i++) {
            if (cnt[i] == 0 || comp[i] != i) continue;
            int m = cnt[i];
            boolean ok = true;
            for (int j = 0; j < n; j++)
                if (comp[j] == i && nods[j] != m - 1) ok = false;
            if (ok && ecnt[i] == m * (m - 1) / 2) out++;
        }
        return out;
    }
}
```



## 2026-07-10 · [Path Existence Queries in a Graph II](https://leetcode.com/problems/path-existence-queries-in-a-graph-ii/?envType=daily-question&envId=2026-07-10) · (0 / 100)

LeetCode difficulty **Hard**

This is above me. I think I should add 1 year to ETA.

Final solution based on [Manjeet Dhayal](https://leetcode.com/problems/path-existence-queries-in-a-graph-ii/solutions/8387529/how-to-convert-problem-to-a-standard-pro-114m/?envType=daily-question&envId=2026-07-10) solution:

```java
class Solution {
    int[] pathExistenceQueries(int n, int[] nums, int maxDiff, int[][] queries) {
        int[][] sortedByValue = new int[n][2];
        int[] positionInSorted = new int[n];

        for (int i = 0; i < n; i++) {
            sortedByValue[i][0] = nums[i];
            sortedByValue[i][1] = i;
        }
        Arrays.sort(sortedByValue, (a, b) -> Integer.compare(a[0], b[0]));

        for (int i = 0; i < n; i++)
            positionInSorted[sortedByValue[i][1]] = i;

        int[] componentRoot = new int[n];
        componentRoot[0] = 0;
        for (int i = 1; i < n; i++)
            if (sortedByValue[i][0] - sortedByValue[i - 1][0] <= maxDiff)
                componentRoot[i] = componentRoot[i - 1];
            else
                componentRoot[i] = i;

        int[] furthestReach = new int[n];
        int left = 0, right = 0;
        while (left < n) {
            while (right < n && sortedByValue[right][0] - sortedByValue[left][0] <= maxDiff) right++;
            furthestReach[left] = right - 1;
            left++;
        }

        int levels = 17;
        int[][] jumpTable = new int[levels][n];
        for (int i = 0; i < n; i++) jumpTable[0][i] = furthestReach[i];

        for (int level = 1; level < levels; level++)
            for (int node = 0; node < n; node++)
                jumpTable[level][node] = jumpTable[level - 1][jumpTable[level - 1][node]];

        int m = queries.length;
        int[] out = new int[m];
        for (int i = 0; i < m; i++) {
            int leftIdx = queries[i][0];
            int rightIdx = queries[i][1];
            int leftPos = positionInSorted[leftIdx];
            int rightPos = positionInSorted[rightIdx];

            if (componentRoot[leftPos] != componentRoot[rightPos]) {
                out[i] = -1;
                continue;
            }
            if (leftPos > rightPos) {
                int temp = leftPos;
                leftPos = rightPos;
                rightPos = temp;
            }
            if (leftPos == rightPos) {
                out[i] = 0;
                continue;
            }

            int hops = 0;
            for (int level = levels - 1; level >= 0; level--)
                if (jumpTable[level][leftPos] < rightPos) {
                    hops += 1 << level;
                    leftPos = jumpTable[level][leftPos];
                }
            out[i] = hops + 1;
        }
        return out;
    }
}
```



## 2026-07-09 · [Path Existence Queries in a Graph I](https://leetcode.com/problems/path-existence-queries-in-a-graph-i/?envType=daily-question&envId=2026-07-09) · (1 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **3/9**

I did it late, and I think when fresh I could have done it about five minutes faster.

```java
class Solution {
    boolean[] pathExistenceQueries(int n, int[] nums, int maxDiff, int[][] queries) {
        boolean[] out = new boolean[queries.length];
        int t = nums[0];
        nums[0] = 0;
        for (int i = 1; i < n; i++) {
            if (nums[i] - t <= maxDiff) {
                t = nums[i];
                nums[i] = nums[i - 1];
            } else {
                t = nums[i];
                nums[i] = i;
            }
        }
        for (int i = 0; i < queries.length; i++) out[i] = nums[queries[i][0]] == nums[queries[i][1]];
        return out;
    }
}
```

Runtime **2 ms** (beats 100.00%) · Memory **167.86 MB** (beats 17.48%) · Time taken **14m 18s**

## 2026-07-08 · [Concatenate Non-Zero Digits and Multiply by Sum II](https://leetcode.com/problems/concatenate-non-zero-digits-and-multiply-by-sum-ii/?envType=daily-question&envId=2026-07-08) · (0 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **6/9**

I solved it myself, but it took somewhere around one and a half hours.

```java
class Solution {
    int mod = 1_000_000_007;
    
    int[] sumAndMultiply(String s, int[][] queries) {
        int n = s.length();
        int[] psum = new int[n + 1];
        int[] x = new int[n + 1];
        int[] pow10 = new int[n + 1];
        int[] pw = new int[n + 1];
        pw[0] = 1;
        for (int i = 1; i <= n; i++) pw[i] = (int) (10L * pw[i - 1] % mod);
        
        for (int i = 1; i <= n; i++) {
            if (s.charAt(i - 1) == '0') {
                psum[i] = psum[i - 1]; 
                x[i] = x[i - 1];
                pow10[i] = pow10[i - 1];
                continue;
            }
            int d = Integer.parseInt(s.substring(i - 1, i));
            psum[i] = psum[i - 1] + d;
            pow10[i] = pow10[i - 1] + 1;
            x[i] = (int) ((10L * x[i - 1] + d) % mod);
        }
        
        int m = queries.length;
        int[] out = new int[m];
        
        for (int i = 0; i < m; i++) {
            int l = queries[i][0];
            int r = queries[i][1];
            int p = pw[pow10[r + 1] - pow10[l]];
            int xout = (int) (((long) mod + x[r + 1] - (long) x[l] * p % mod) % mod);
            int sum = psum[r + 1] - psum[l];
            out[i] = (int) ((long) xout * sum % mod);
        }
        
        return out;
    }
}
```



## 2026-07-07 · [Concatenate Non-Zero Digits and Multiply by Sum I](https://leetcode.com/problems/concatenate-non-zero-digits-and-multiply-by-sum-i/?envType=daily-question&envId=2026-07-07) · (4 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **2/9**

I will skip beautification — the problem is too simple. It leaves me wondering why I think so messy.

```java
class Solution {
    long sumAndMultiply(int n) {
        if (n == 0) return 0;
        int x = 0, sum = 0, cnt = -1;
        while (n > 0) {
            int t = n % 10;
            if (t > 0) {
                cnt++;
                sum += t;
                for (int i = 0; i < cnt; i++) t *= 10;
                x += t;
            }
            n /= 10;
        }
        return (long) x * sum;     
    }
}
```

Runtime **1 ms** (beats 99.85%) · Memory **42.79 MB** (beats 35.05%) · Time taken **8m 1s**

## 2026-07-06 · [Remove Covered Intervals](https://leetcode.com/problems/remove-covered-intervals/?envType=daily-question&envId=2026-07-06) · (3 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **2/9**

I broke my head uselessly over a more optimized solution. Judging by the timer, my brute force is the ideal option at these constraints.

```java
class Solution {
    int removeCoveredIntervals(int[][] intervals) {
        int n = intervals.length;
        int cnt = n;
        boolean[] rvd = new boolean[n];
        for (int i = 0; i < n; i++) {
            int a = intervals[i][0], b = intervals[i][1];                
            for (int j = 0; j < i; j++) {
                if (rvd[j]) continue;
                int c = intervals[j][0], d = intervals[j][1];        
                if (c <= a && b <= d) {
                    rvd[i] = true;
                    cnt--;
                    break;
                } else if (c >= a && b >= d)  {
                    rvd[j] = true;
                    cnt--;
                }
            }
        }
        return cnt;
    }
}
```

Runtime **3 ms** (beats 100.00%) · Memory **46.46 MB** (beats 62.65%) · Time taken **15m 1s**

PS [eunice](https://leetcode.com/problems/remove-covered-intervals/solutions/8378466/solution-by-la_castille-0b55) illustrated and implemented my vague thoughts.

## 2026-07-05 · [Number of Paths with Max Score](https://leetcode.com/problems/number-of-paths-with-max-score/?envType=daily-question&envId=2026-07-05) · (2 / 100)

LeetCode difficulty **Hard** · Subjective difficulty **3/9**

The problem is easy to understand and medium to implement. I was not in the mood, and that showed on the timer too.

```java
class Solution {
    int mod = 1_000_000_007;

    int[] pathsWithMaxScore(List<String> board) {
        int n = board.size();
        int[][][] dp = new int[n][n][2];

        dp[n - 1][n - 1][0] = 0;
        dp[n - 1][n - 1][1] = 1;

        for (int j = n - 2; j >= 0; j--) {
            char c = board.get(n - 1).charAt(j);
            if (c == 'X') break;
            dp[n - 1][j][0] = dp[n - 1][j + 1][0] + Integer.parseInt(String.valueOf(c));
            dp[n - 1][j][1] = 1;
        }

        for (int i = n - 2; i >= 0; i--) {
            char c = board.get(i).charAt(n - 1);
            if (c == 'X') break;
            dp[i][n - 1][0] = dp[i + 1][n - 1][0] + Integer.parseInt(String.valueOf(c));
            dp[i][n - 1][1] = 1;
        }

        int[][] dirs = {{1, 0}, {0, 1}, {1, 1}};
        for (int i = n - 2; i >= 0; i--) {
            for (int j = n - 2; j >= 0; j--) {
                char c = board.get(i).charAt(j);
                if (c == 'X') continue;
                int v = c == 'E' ? 0 : Integer.parseInt(String.valueOf(c));
                int max = -1, cnt = 0;

                for (int[] d : dirs) {
                    int x = i + d[0], y = j + d[1];
                    if (dp[x][y][1] == 0) continue;
                    if (dp[x][y][0] + v > max) {
                        max = dp[x][y][0] + v;
                        cnt = dp[x][y][1];
                    } else if (dp[x][y][0] + v == max) {
                        cnt = (cnt + dp[x][y][1]) % mod;
                    }
                }
                if (max >= 0) {
                    dp[i][j][0] = max;
                    dp[i][j][1] = cnt;
                }
            }
        }
        return dp[0][0];
    }
}
```

Runtime **15 ms** (beats 47.13%) · Memory **47.27 MB** (beats 19.54%) · Time taken **42m 19s**

## 2026-07-04 · [Minimum Score of a Path Between Two Cities](https://leetcode.com/problems/minimum-score-of-a-path-between-two-cities/?envType=daily-question&envId=2026-07-04) · (1 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **3/9**

Union-Find was on this year's Yandex summer school contest. I was very afraid I would not remember even a weak solution. Emotions without exam nerves only slow me down. I will add the algorithm for practice in two months.

```java
class Solution {
    int[] p;
    int minScore(int n, int[][] roads) {
        p = new int[n + 1];
        for (int i = 1; i <= n; i++) p[i] = i;
        
        for (int[] r : roads) setParent(r[0], r[1]);
               
        int out = Integer.MAX_VALUE;
        for (int[] r : roads) {
            int a = r[0];
            while (p[a] != a) a = p[a];
            if (a == 1) out = Math.min(out, r[2]);
        }        
        return out;
    }
    
    void setParent(int a, int b) {
        while (p[a] != a) a = p[a];
        while (p[b] != b) b = p[b];
        p[Math.max(a, b)] = Math.min(a, b);
    }
}
```

Runtime **100 ms** (beats 6.53%) · Memory **127.86 MB** (beats 90.27%) · Time taken **51m 21s**

## 2026-07-03 · [Network Recovery Pathways](https://leetcode.com/problems/network-recovery-pathways/?envType=daily-question&envId=2026-07-03) · (0 / 100)

LeetCode difficulty **Hard** · Subjective difficulty **6/9**

I am somewhat overloaded right now, so drilling this problem is pushed back two months. I hope there will not be more like it in that window.

TL solution:

```java
class Solution {
    int findMaxPathScore(int[][] edges, boolean[] online, long k) {
        n = online.length;
        this.k = k;
        list = new ArrayList<>(n);
        for (int i = 0; i < n; i++) list.add(new ArrayList<>());

        for (int[] edg : edges) {
            int u = edg[0], v = edg[1], cost = edg[2];
            if (!online[u] || !online[v]) continue;
            list.get(u).add(new Pair(v, cost));
        }

        dfs(0, 0, Integer.MAX_VALUE, 0);

        return out;
    }

    List<List<Pair>> list;
    int n = 0;
    int out = -1;
    long k = 0;

    void dfs(int v, int cost, int min, long total) {  
        if (total + cost > k) return;
        if (v != 0) min = Math.min(min, cost);    
        
        if (v == n - 1) {
            out = Math.max(out, min);
            return;
        }

        total = total + cost;
        for (Pair p : list.get(v)) dfs(p.v(), p.cost(), min, total);
    }

    record Pair(int v, int cost){}
}
```

LeetCode AI verdict (I wanted to buy premium after this try, maybe with bonus points I will):

**Approach**

Current: Depth-First Search / Recursion  
Suggested: Binary Search / Dijkstra's Algorithm / Shortest Path  
Key Idea: Maximize the minimum edge weight in a path subject to a total cost constraint.

**Efficiency**

Time Complexity

Current complexity: `O(m × 2^n)`  
Suggested complexity: `O((n + m) log m)`  
Suggestions: Replace DFS with binary search on the answer combined with a shortest path check.

Space Complexity

Current complexity: `O(N + M)`  
Suggested complexity: `O(N + M)`  
Suggestions: Replace DFS with binary search on the answer combined with a shortest path check.

Final solution:

```java
class Solution {
    record Edge(int to, int cost) {}

    List<List<Edge>> graph;
    List<Integer> topologicalOrder;
    int nodeCount;
    long costLimit;

    int findMaxPathScore(int[][] edges, boolean[] online, long k) {
        nodeCount = online.length;
        costLimit = k;
        graph = new ArrayList<>(nodeCount);
        for (int i = 0; i < nodeCount; i++)
            graph.add(new ArrayList<>());

        int minCost = Integer.MAX_VALUE, maxCost = 0;
        for (int[] edge : edges) {
            int from = edge[0], to = edge[1], weight = edge[2];
            if (!online[from] || !online[to]) continue;
            graph.get(from).add(new Edge(to, weight));
            minCost = Math.min(minCost, weight);
            maxCost = Math.max(maxCost, weight);
        }

        topologicalOrder = buildTopologicalOrder();

        while (minCost < maxCost) {
            int mid = (minCost + maxCost + 1) >>> 1;
            if (canReachWithMinEdge(mid)) minCost = mid;
            else maxCost = mid - 1;
        }
        return canReachWithMinEdge(minCost) ? minCost : -1;
    }

    List<Integer> buildTopologicalOrder() {
        int[] inDegree = new int[nodeCount];
        for (int from = 0; from < nodeCount; from++)
            for (Edge edge : graph.get(from))
                inDegree[edge.to()]++;

        Queue<Integer> queue = new ArrayDeque<>();
        for (int i = 0; i < nodeCount; i++)
            if (inDegree[i] == 0) queue.offer(i);

        List<Integer> order = new ArrayList<>();
        while (!queue.isEmpty()) {
            int from = queue.poll();
            order.add(from);
            for (Edge edge : graph.get(from))
                if (--inDegree[edge.to()] == 0) queue.offer(edge.to());
        }
        return order;
    }

    boolean canReachWithMinEdge(int minEdgeCost) {
        long unreachable = Long.MAX_VALUE / 4;
        long[] distance = new long[nodeCount];
        for (int i = 1; i < nodeCount; i++)
            distance[i] = unreachable;

        for (int from : topologicalOrder) {
            if (distance[from] == unreachable) continue;
            for (Edge edge : graph.get(from)) {
                if (edge.cost() < minEdgeCost) continue;
                int to = edge.to();
                distance[to] = Math.min(distance[to], distance[from] + edge.cost());
            }
        }
        return distance[nodeCount - 1] <= costLimit;
    }
}
```



## 2026-07-02 · [Find a Safe Walk Through a Grid](https://leetcode.com/problems/find-a-safe-walk-through-a-grid/?envType=daily-question&envId=2026-07-02) · (1 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **4/9**

A lighter version of the previous problem.

```java
class Solution {
    int[][] dirs = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};

    boolean findSafeWalk(List<List<Integer>> grid, int health) {
        int m = grid.size(), n = grid.get(0).size();
        int[][] f  =  new int[m][n];
        for (int i = 0; i < m; i++) 
            for (int j = 0; j < n; j++)
                f[i][j] = -grid.get(i).get(j);
        
        PriorityQueue<int[]> pq = new PriorityQueue<>((e1, e2) -> e2[2] - e1[2]);
        pq.offer(new int[]{0, 0, f[0][0] + health});

        while (!pq.isEmpty()) {
            int[] poll = pq.poll();
            int i = poll[0], j = poll[1], v = poll[2];
            for (int[] d : dirs) {
                int x = i + d[0], y = j + d[1];
                if (x >= 0 && x < m && y >= 0 && y < n && f[x][y] < 1000) {
                    if (x == m - 1 && y == n - 1) return f[x][y] + v > 0;
                    pq.offer(new int[]{x, y, f[x][y] + v});                    
                    f[x][y] = 1000;
                }
            }
        }
        
        throw new RuntimeException();
    }
}
```

Runtime **14 ms** (beats 49.99%) · Memory **46.81 MB** (beats 78.41%) · Time taken **33m 27s**

## 2026-07-01 · [Find the Safest Path in a Grid](https://leetcode.com/problems/find-the-safest-path-in-a-grid/?envType=daily-question&envId=2026-07-01) · (0 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **5/9**

I understood the approach in about fifteen minutes, but I could not implement it. I have not written BFS in a long time and I still do not write it cleanly — I used to create a new list instead of one queue. I am not good at working with coordinates by hand either. I am not too worried about the medium label this time, the algorithm itself is clear to me. A long stretch of drilling manual writes lies ahead.

Final solution based on [eunice](https://leetcode.com/problems/find-the-safest-path-in-a-grid/solutions/8368584/solution-by-la_castille-6y31) solution:

```java
class Solution {
    int[][] dirs = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};

    int maximumSafenessFactor(List<List<Integer>> grid) {
        int n = grid.size();

        int[][] a = new int[n][n];
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                a[i][j] = grid.get(i).get(j);

        Queue<int[]> q = new ArrayDeque<>();
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                if (a[i][j] == 1)
                    q.offer(new int[]{i, j});

        while (!q.isEmpty()) {
            int[] cur = q.poll();
            int i = cur[0], j = cur[1];
            int v = a[i][j];
            for (int[] d : dirs) {
                int x = i + d[0], y = j + d[1];
                if (Math.min(x, y) >= 0 && Math.max(x, y) < n && a[x][y] == 0) {
                    a[x][y] = v + 1;
                    q.offer(new int[]{x, y});
                }
            }
        }

        PriorityQueue<int[]> pq = new PriorityQueue<>((e1, e2) -> e2[2] - e1[2]);
        pq.offer(new int[]{0, 0, a[0][0]});

        while (!pq.isEmpty()) {
            int[] cur = pq.poll();
            int i = cur[0], j = cur[1], sf = cur[2];
            if (i == n - 1 && j == n - 1)
                return sf - 1;
            for (int[] d : dirs) {
                int x = i + d[0], y = j + d[1];
                if (Math.min(x, y) >= 0 && Math.max(x, y) < n && a[x][y] > 0) {
                    pq.offer(new int[]{x, y, Math.min(sf, a[x][y])});
                    a[x][y] *= -1;
                }
            }
        }
        throw new RuntimeException();
    }
}
```



## 2026-06-30 · [Number of Substrings Containing All Three Characters](https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/?envType=daily-question&envId=2026-06-30) · (4 / 100)

LeetCode difficulty **Medium** · Subjective difficulty **3/9**

I was not in the best state of mind, but I decided it would do for a medium. I panicked and looked at the problem hints, which only made my solution worse, yet they did lead me to the answer.

```java
class Solution {
    int numberOfSubstrings(String s) {
        int n = s.length();
        int out = 0;
        Deque<Integer> stA = new ArrayDeque<>();
        Deque<Integer> stB = new ArrayDeque<>();
        Deque<Integer> stC = new ArrayDeque<>();
        for (int j = 0; j < n; j++) {
            switch (s.charAt(j)) {
                case 'a' -> stA.push(j);
                case 'b' -> stB.push(j);
                case 'c' -> stC.push(j);
            }
            if (!stA.isEmpty() && !stB.isEmpty() && !stC.isEmpty()) {
                int i = Math.min(Math.min(stA.peek(), stB.peek()), stC.peek());
                out += i + 1;
            }
        }
        return out;
    }
}
```

Runtime **29 ms** (beats 18.45%) · Memory **47.43 MB** (beats 5.43%) · Time taken **39m 10s**

TL solution:

```java
class Solution {
    int numberOfSubstrings(String s) {
        int n = s.length();
        int out = 0;
        for (int i = 0; i < n; i++) {
            int j = i;
            int cntA = 0, cntB = 0, cntC = 0;
            for ( ; j < n && (cntA == 0 || cntB == 0 || cntC == 0); j++) {
                switch (s.charAt(j)) {
                    case 'a' -> cntA++;
                    case 'b' -> cntB++;
                    case 'c' -> cntC++;
                }
            }
            if (cntA > 0 && cntB > 0 && cntC > 0) out += n - j + 1;
            else break;
        }
        return out;
    }
}
```

Beautification — I already had this shape when I submitted, but I was afraid to spend the time on it.

```java
class Solution {
    int numberOfSubstrings(String s) {
        int n = s.length();
        int out = 0;
        int a = -1, b = -1, c = -1;
        for (int i = 0; i < n; i++) {
            switch (s.charAt(i)) {
                case 'a' -> a = i;
                case 'b' -> b = i;
                case 'c' -> c = i;
            }
            if (a >= 0 && b >= 0 && c >= 0)
                out += Math.min(Math.min(a, b), c) + 1;
        }
        return out;
    }
}
```



## 2026-06-29 · [Number of Strings That Appear as Substrings in Word](https://leetcode.com/problems/number-of-strings-that-appear-as-substrings-in-word/?envType=daily-question&envId=2026-06-29) · (3 / 100)

LeetCode difficulty **Easy** · Subjective difficulty **2/9**

I knew there was a String method for this, but not its exact name, so I had already started on a raw brute-force check before `contains` came back.

```java
class Solution {
    int numOfStrings(String[] patterns, String word) {
        int cnt = 0;
        for (String p : patterns) if (word.contains(p)) cnt++;
        return cnt;
    }
}
```

Runtime **1 ms** (beats 75.75%) · Memory **42.80 MB** (beats 98.28%) · Time taken **6m 2s**

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

After a little help from an LLM:

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