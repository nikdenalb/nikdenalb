# LeetCode 100 diary

Personal notes from my [LeetCode 100](https://leetcode.com/) streak: solutions, submission stats, and quick reflections. This is an experiment and is not linked from the README.

Subjective difficulty is the rating I give immediately after solving: `1`-`3` easy, `4`-`6` medium, `7`-`9` hard.

Progress rule: solved problems increase the counter by 1; an unsolved task resets it to 0.

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
