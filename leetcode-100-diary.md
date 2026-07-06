# LeetCode 100 diary

Personal notes from my [LeetCode 100](https://leetcode.com/) streak: solutions, submission stats, and quick reflections. This diary is an **experiment** — I may stop maintaining it at any time.

Subjective difficulty is the rating I give immediately after the attempt: `1`-`3` easy, `4`-`6` medium, `7`-`9` hard.

Progress rule: solved problems increase the counter by 1; an unsolved task resets it to 0.

Record streak: **4**

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
