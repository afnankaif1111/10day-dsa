# Day 1 DSA Mastery Tutorial (C++17)

**10-Day, 500-Problem Interview Preparation Bootcamp, Day 1 of 10 (Problems 1 to 50)**

> **How to use this document.** Parts 1, 2, 4, 5, 6 are theory you read once. Part 3 is the 50-problem workbook: attempt each problem *before* reading its section (use the time budgets in Part 1). Part 8's answer key is deliberately separated from the recall questions. Part 9 is your tracker.
>
> **Code conventions.** Every full solution lists its own headers. Short template snippets in Parts 2 and 5 assume `#include <bits/stdc++.h>` and `using namespace std;`. Solutions are written LeetCode-style (`class Solution`). "O(1) expected" for hash containers means *expected* time; worst case per operation is O(n) with adversarial keys or collisions, so worst-case totals are quoted separately where it matters.
>
> **Problem numbers.** LeetCode numbers and difficulties were written from memory of the LeetCode catalogue. Spot-check each number once while you attempt the problem; Problem 38 is explicitly *not* an official LeetCode title (see its section).

---

## Table of Contents

- [Part 1: Day 1 Roadmap](#part-1-day-1-roadmap)
- [Part 2: Pattern Theory and Reusable C++ Templates](#part-2-pattern-theory-and-reusable-c-templates)
- [Part 3: Complete Tutorial for All 50 Problems](#part-3-complete-tutorial-for-all-50-problems)
  - [Section 1: Arrays and Hashing (1 to 10)](#section-1-arrays-and-hashing)
  - [Section 2: Two Pointers and Array Manipulation (11 to 20)](#section-2-two-pointers-and-array-manipulation)
  - [Section 3: Sliding Window (21 to 30)](#section-3-sliding-window)
  - [Section 4: Prefix Sums and Subarray Problems (31 to 40)](#section-4-prefix-sums-and-subarray-problems)
  - [Section 5: Mixed Patterns and Transfer (41 to 50)](#section-5-mixed-patterns-and-transfer)
- [Part 4: Pattern Comparison Tables](#part-4-pattern-comparison-tables)
- [Part 5: C++17 Template Library](#part-5-c17-template-library)
- [Part 6: How to Start Coding Any DSA Problem](#part-6-how-to-start-coding-any-dsa-problem)
- [Part 7: Day 1 Revision Sheet](#part-7-day-1-revision-sheet)
- [Part 8: Self-Assessment and Answer Key](#part-8-self-assessment-and-answer-key)
- [Part 9: Personal Revision Tracker](#part-9-personal-revision-tracker)

---

## Part 1: Day 1 Roadmap

### 1.1 Learning objectives

By the end of Day 1 you should be able to:

1. Read an unfamiliar array/string problem and name the likely pattern from its **clues** (sorted input, "contiguous", "in place", "at most K", "longest/shortest", "count subarrays").
2. State the brute force, its cost, and the **specific repeated work** that the optimization removes.
3. Write from memory the skeletons for: complement lookup, opposite-end pointers, slow/fast compaction, fixed and variable windows, prefix-sum + hash map, Kadane, interval merge.
4. Say *why* a method is valid (the invariant) and *when it breaks* (negatives, unsorted input, non-monotone validity).
5. Handle edge cases mechanically: empty/size-1 input, duplicates, negatives, overflow, off-by-one in prefix arrays.

### 1.2 The five sections and their patterns

| Section | Problems | Core patterns | Single most important idea |
|---|---|---|---|
| 1. Arrays and Hashing | 1 to 10 | Hash set/map, frequency counting, complement lookup, bucket sort, prefix/suffix, in-place index tricks | Trade O(n) memory (or the array itself) for removing an inner loop |
| 2. Two Pointers and Array Manipulation | 11 to 20 | Opposite-end pointers, slow/fast compaction, sort + pointers, boundary reasoning | Sorted order or a monotone quantity lets you discard candidates in pairs |
| 3. Sliding Window | 21 to 30 | Fixed window, variable window, frequency-map window | The window state is updated incrementally in O(1) per step |
| 4. Prefix Sums and Subarrays | 31 to 40 | Prefix sums, prefix + hash map, prefix/suffix products | `sum(l..r) = P[r+1] - P[l]` turns range questions into pair questions |
| 5. Mixed Patterns and Transfer | 41 to 50 | Kadane, product DP, window with product, merge-from-back, reverse trick, Floyd, matrix marking, intervals | Same tools, disguised statements |

### 1.3 Compact pattern-selection guide

| If the statement says... | Think first of... |
|---|---|
| "find two numbers that...", unsorted | Hash map complement lookup |
| "exists duplicate", "is anagram", "frequency" | Set / frequency array |
| input is **sorted** (or you may sort) and you need pairs/triplets | Sort + opposite-end pointers |
| "in place", "O(1) extra space", "remove/move elements" | Slow/fast (read/write) pointers |
| "longest/shortest **contiguous** ... with a property" and values are **non-negative** / property is monotone | Variable sliding window |
| "substring of length K", "every window of size K" | Fixed window |
| "count subarrays with sum = K" (negatives allowed) | Prefix sum + frequency map |
| "longest subarray with sum = K" (negatives allowed) | Prefix sum + earliest-index map |
| "range sum queries", "split where left vs right sums" | Prefix sums |
| "max sum/product of contiguous subarray" | Kadane (sum) / max-min tracking (product) |
| "all except self", "left of i and right of i" | Prefix/suffix arrays |
| "intervals overlap/merge/insert" | Sort by start, sweep |
| "numbers in range [1..n] in array of size n(+1)" | Index-as-hash or cycle detection |
| "matrix, in place, spiral, zero rows" | Boundary pointers / use matrix border as markers |

### 1.4 Realistic workflow for 50 problems

Fifty problems at full budget could exceed 9 hours of pure attempt time, so **triage**:

| Block | Problems | Suggested approach | Target time |
|---|---|---|---|
| A | 1 to 10 | Mastered ones: 3-minute drill (see 1.6). New ones: full attempt | ~75 min |
| B | 11 to 20 | Same | ~75 min |
| C | 21 to 30 | Windows deserve full attempts on 24, 25, 28, 30 | ~90 min |
| D | 31 to 40 | Prefix + hash map (34 to 38) deserve full attempts | ~75 min |
| E | 41 to 50 | 42, 46, 47, 49 deserve full attempts | ~75 min |
| Review | Tracker + revision sheet | Update Part 9, re-solve flagged problems blind | ~45 min |

For each problem the loop is: **read (2 min) → write brute force in words (2 min) → find the repeated work → code → test on edge cases → compare with this document → fill the tracker.**

### 1.5 Time budgets and hint rules

The budgets are **maximum first-attempt limits**, not equal allocations. An easy problem you solve in 3 minutes is a success; don't pad it.

| Difficulty | Max first-attempt time |
|---|---|
| Easy | 10 minutes |
| Medium | 25 minutes |
| Hard | 45 minutes |

**When to stop and take a hint (escalating):**

1. **No brute force written after ~20% of the budget** → read only the "Pattern recognition" part (C) of the problem section.
2. **Brute force exists but no optimization idea at ~60% of the budget** → read "Optimization reasoning" (E), then close it and code.
3. **Budget exhausted** → read the full solution, then re-implement from a blank file without looking. Mark the tracker as *Hint used* or *Failed*.
4. **Wrong answer and you've been debugging 10 minutes** → write the invariant in a comment; if the code violates it, fix; otherwise compare against the problem's "Common mistakes" (J).

Never read the solution *and* move on without re-implementing. Reading is not learning.

### 1.6 Strategy for already-mastered problems

If you can solve a problem cold, don't spend its full budget:

1. **Say it out loud (30 s):** pattern, brute force, optimized idea, complexity.
2. **Write the core loop from memory (2 min)** in a scratch file, ignoring boilerplate.
3. **Test one edge case mentally** (empty/size-1, duplicates, negatives).
4. **Read only sections J and K** of that problem for traps and variations.
5. If you hesitate more than 2 minutes at step 2 on *any* problem, it is **not mastered**: reclassify it as *Hint used* and give it a full attempt.

Mark such problems **Already mastered** in the tracker, but still blind re-solve one or two from each section tomorrow.

---

## Part 2: Pattern Theory and Reusable C++ Templates

Each pattern below follows the same fields: **Idea · Recognize · Clue · Why brute force fails · Algorithm · Template · Invariant · Complexity · Fails when.**

### 2.1 Arrays and in-place operations

- **Idea:** Treat the array as both input and output. A *read* index scans; a *write* index marks the finished prefix.
- **Recognize / Clue:** "in place", "O(1) extra space", "return the new length", "relative order must be preserved".
- **Why brute force fails:** Copying to a new array costs O(n) auxiliary space; repeated `erase` in the middle is O(n) each, so O(n²) total.
- **Algorithm:** Scan with `read`; copy kept elements to `write`; return `write`.
- **Template:**

```cpp
// Keeps elements for which keep(x) is true, preserving order. Returns new length.
template <class T, class Keep>
int compactInPlace(vector<T>& a, Keep keep) {          // CUSTOMIZE: keep
    int write = 0;
    for (int read = 0; read < (int)a.size(); ++read)
        if (keep(a[read])) a[write++] = a[read];
    return write;
}
```

- **Invariant:** `a[0..write)` holds exactly the kept elements seen in `a[0..read)`, in order.
- **Complexity:** O(n) time, O(1) auxiliary space.
- **Fails when:** The decision for `a[read]` depends on elements already *overwritten*. Compare against `a[write-1]` (already final), never against `a[read-1]` if it may be stale.

### 2.2 Hash maps and hash sets

- **Idea:** Constant-time membership/lookup replaces an O(n) scan.
- **Recognize / Clue:** "have we seen X before", "does value Y exist", "group by property".
- **Why brute force fails:** Checking each pair or scanning for membership gives O(n²).
- **Algorithm:** Maintain a container of what you've seen; query before or after inserting depending on whether an element may pair with itself.
- **Template:**

```cpp
bool hasDuplicate(const vector<int>& a) {
    unordered_set<int> seen;
    for (int x : a)
        if (!seen.insert(x).second) return true;  // insert fails => already present
    return false;
}
```

- **Invariant:** `seen` equals the set of elements in the processed prefix.
- **Complexity:** O(n) expected time, O(n) space. Worst case O(n²) time if hashing degenerates. If worst-case guarantees matter, use `std::set`/sorting for O(n log n).
- **Fails when:** Keys are not hashable by default (pairs, vectors need a custom hash or conversion to a string/encoded key), or memory is restricted.

### 2.3 Frequency counting

- **Idea:** Reduce a sequence to a multiset of counts, making comparisons order-independent.
- **Recognize / Clue:** "anagram", "permutation of", "most frequent", "majority", "at most K distinct".
- **Why brute force fails:** Sorting or comparing every pair of strings costs extra logs or quadratic time.
- **Algorithm:** `count[x]++` for each element; compare/aggregate counts.
- **Template:**

```cpp
array<int, 26> countLower(const string& s) {   // CUSTOMIZE: alphabet size / use unordered_map for large domain
    array<int, 26> cnt{};
    for (char c : s) ++cnt[c - 'a'];
    return cnt;
}
```

- **Invariant:** `cnt[c]` equals occurrences of `c` in the scanned part.
- **Complexity:** O(n) time; O(Σ) space where Σ is the alphabet size.
- **Fails when:** The alphabet is huge or unknown (use a map), or order matters.

### 2.4 Complement lookup

- **Idea:** For each element `x`, the partner is determined (`target - x`); look it up instead of searching.
- **Recognize / Clue:** "two elements that satisfy `a + b = T`" on unsorted data.
- **Why brute force fails:** Trying all pairs is O(n²).
- **Algorithm:** For each `x`: check if `T - x` is in the map; then insert `x`.
- **Template:**

```cpp
// Returns indices of a pair summing to target, or {-1,-1}.
pair<int,int> complementLookup(const vector<int>& a, long long target) {
    unordered_map<long long, int> seen;               // value -> index
    for (int i = 0; i < (int)a.size(); ++i) {
        auto it = seen.find(target - a[i]);           // CUSTOMIZE: the "needed" value
        if (it != seen.end()) return {it->second, i};
        seen[a[i]] = i;                               // insert AFTER query: avoids reusing a[i]
    }
    return {-1, -1};
}
```

- **Invariant:** `seen` has every element before `i`; so a hit is a distinct earlier index.
- **Complexity:** O(n) expected, O(n) space.
- **Fails when:** You need *all* pairs without duplicates (sort + pointers is cleaner), or the relation isn't an equation (use ordered structures).

### 2.5 Sorting-based optimization

- **Idea:** Sorting exposes structure: duplicates become adjacent, pairs can be found by pointers, intervals align by start.
- **Recognize / Clue:** Order of the original array is irrelevant (or indices aren't needed), and you need pairs/groups/overlaps.
- **Why brute force fails:** Pairwise comparison is O(n²); sorting costs O(n log n) but enables an O(n) scan.
- **Algorithm:** Sort; scan with pointers or adjacent comparisons.
- **Template:**

```cpp
vector<int> sortedUnique(vector<int> a) {
    sort(a.begin(), a.end());
    a.erase(unique(a.begin(), a.end()), a.end());
    return a;
}
```

- **Invariant:** After sorting, equal values are contiguous; every pairwise relation becomes a monotone relation along the array.
- **Complexity:** O(n log n) time; O(1) to O(n) space depending on the sort (`std::sort` is introsort, O(log n) stack).
- **Fails when:** You must return *original indices* (store `(value, index)` pairs first), or the input is a stream.

### 2.6 Opposite-end two pointers

- **Idea:** Place `l = 0`, `r = n-1`; at each step a comparison tells you one end can *never* be part of an answer, so discard it.
- **Recognize / Clue:** Sorted array, palindrome check, "maximize area between two indices", "pair with target sum".
- **Why brute force fails:** O(n²) pairs; the monotone relation lets each step eliminate an entire row/column of pairs.
- **Algorithm:** Compute the current pair's value; move the pointer whose move *might* improve the answer.
- **Template:**

```cpp
// Sorted a. Find pair with sum == target (0-based).
pair<int,int> pairSumSorted(const vector<int>& a, long long target) {
    int l = 0, r = (int)a.size() - 1;
    while (l < r) {
        long long s = (long long)a[l] + a[r];
        if (s == target) return {l, r};
        if (s < target) ++l;      // CUSTOMIZE: which side to move, per problem's monotonicity
        else --r;
    }
    return {-1, -1};
}
```

- **Invariant:** Any valid answer lies within `[l, r]`. Moving `l` right when `s < target` is safe because `a[l]` plus *any* element ≤ `a[r]` is still `< target`.
- **Complexity:** O(n) after sorting; O(1) auxiliary.
- **Fails when:** The array is not sorted/monotone; the compared quantity isn't monotone in pointer movement.

### 2.7 Slow/fast pointers (read/write pointers)

- **Idea:** Same as 2.1 but emphasises two pointers moving in the same direction at different rates.
- **Recognize / Clue:** Remove duplicates, remove value, move zeros; also merging from the back.
- **Why brute force fails:** Shifting elements left after each deletion is O(n) per deletion.
- **Template (sorted dedupe):**

```cpp
int dedupeSorted(vector<int>& a) {
    if (a.empty()) return 0;
    int write = 1;                                   // a[0] always kept
    for (int read = 1; read < (int)a.size(); ++read)
        if (a[read] != a[write - 1]) a[write++] = a[read];   // compare with last KEPT
    return write;
}
```

- **Invariant:** `a[0..write)` is the deduplicated prefix of `a[0..read)`.
- **Complexity:** O(n) time, O(1) space.
- **Fails when:** Input not sorted (equal elements not adjacent). Use a set.

### 2.8 Fixed-size sliding window

- **Idea:** Window length `k` is constant; slide by adding the new right element and removing the left element that fell out.
- **Recognize / Clue:** "subarray/substring of length k", "every window of size k".
- **Why brute force fails:** Recomputing the window from scratch is O(nk).
- **Template:**

```cpp
// Max sum over all windows of size k. Requires 1 <= k <= n.
long long maxWindowSum(const vector<int>& a, int k) {
    long long cur = 0, best;
    for (int i = 0; i < k; ++i) cur += a[i];
    best = cur;
    for (int i = k; i < (int)a.size(); ++i) {
        cur += a[i];            // ADD the entering element   (CUSTOMIZE)
        cur -= a[i - k];        // REMOVE the leaving element (CUSTOMIZE)
        best = max(best, cur);
    }
    return best;
}
```

- **Invariant:** After processing index `i`, `cur` is the aggregate of `a[i-k+1..i]`.
- **Complexity:** O(n) time, O(1) space (more if state is a frequency table).
- **Fails when:** The aggregate is not *invertible* (e.g., max/min needs a deque or multiset to remove).

### 2.9 Variable-size sliding window

- **Idea:** Expand `right` always; shrink `left` only as needed to restore validity. Each index enters and leaves at most once.
- **Recognize / Clue:** "longest/shortest/count subarray or substring such that ...", with a **monotone** validity condition: shrinking a valid window keeps it valid (or growing an invalid window keeps it invalid).
- **Why brute force fails:** O(n²) windows; monotonicity says once `[l, r]` is invalid, every `[l', r]` with `l' < l`... is invalid too, so `left` never moves backward.
- **Templates:**

```cpp
// LONGEST window that is valid. valid() must be monotone: shrinking never breaks validity.
template <class Add, class Remove, class Valid>
int longestValidWindow(int n, Add add, Remove rem, Valid valid) {
    int left = 0, best = 0;
    for (int right = 0; right < n; ++right) {
        add(right);                                  // CUSTOMIZE: update state
        while (!valid()) rem(left++);                // shrink until valid again
        best = max(best, right - left + 1);
    }
    return best;
}

// SHORTEST window that is valid. valid() must be monotone: growing never breaks validity.
template <class Add, class Remove, class Valid>
int shortestValidWindow(int n, Add add, Remove rem, Valid valid) {
    int left = 0, best = INT_MAX;
    for (int right = 0; right < n; ++right) {
        add(right);
        while (left <= right && valid()) {           // record, then try to shrink
            best = min(best, right - left + 1);
            rem(left++);
        }
    }
    return best == INT_MAX ? 0 : best;
}
```

- **Invariant:** *Longest:* after the `while`, `[left, right]` is the longest valid window ending at `right`. *Shortest:* after the `while`, `[left, right]` is no longer valid, so every window starting at or before `left - 1` that ends at `right` has been considered.
- **Complexity:** O(n) time (each pointer moves ≤ n times); state-dependent space.
- **Fails when:** Values can be **negative** (sum validity is not monotone), or the validity predicate is not monotone. See Part 4.

### 2.10 Frequency-map sliding window

- **Idea:** Window state is a count table. Keep a derived scalar (`missing`, `distinct`, `maxFreq`) updated in O(1) so validity is checked without scanning the table.
- **Recognize / Clue:** "contains all characters of t", "at most K distinct", "permutation/anagram window", "replace at most k characters".
- **Why brute force fails:** Comparing two 26-entry tables per window is O(26n), acceptable, but for "covers all of t" with large alphabets or repeated shrinking checks, a maintained counter avoids rescanning.
- **Template (cover-all-of-t):**

```cpp
// Smallest window of s containing all chars of t (with multiplicity).
string coverWindow(const string& s, const string& t) {
    if (t.empty() || s.size() < t.size()) return "";
    vector<int> need(256, 0);
    for (unsigned char c : t) ++need[c];
    int missing = (int)t.size();                     // chars still required
    int bestLen = INT_MAX, bestStart = 0, left = 0;
    for (int right = 0; right < (int)s.size(); ++right) {
        if (need[(unsigned char)s[right]]-- > 0) --missing;   // was still needed
        while (missing == 0) {
            if (right - left + 1 < bestLen) { bestLen = right - left + 1; bestStart = left; }
            if (++need[(unsigned char)s[left]] > 0) ++missing; // now needed again
            ++left;
        }
    }
    return bestLen == INT_MAX ? "" : s.substr(bestStart, bestLen);
}
```

- **Invariant:** `need[c] > 0` means window still lacks `c`; `need[c] <= 0` means surplus. `missing` counts total deficit.
- **Complexity:** O(|s| + |t|) time, O(Σ) space.
- **Fails when:** Validity is not monotone (e.g., "exactly K distinct" must be computed as `atMost(K) - atMost(K-1)`).

### 2.11 Prefix sums

- **Idea:** `P[i]` = sum of the first `i` elements; any range sum is a difference of two prefixes.
- **Recognize / Clue:** Many range-sum queries, "sum of left part vs right part", "running total".
- **Why brute force fails:** O(n) per query → O(nq).
- **Template:**

```cpp
vector<long long> buildPrefix(const vector<int>& a) {
    vector<long long> P(a.size() + 1, 0);            // P[0] = 0 is the empty prefix
    for (size_t i = 0; i < a.size(); ++i) P[i + 1] = P[i] + a[i];
    return P;
}
// sum of a[l..r] inclusive = P[r + 1] - P[l]
```

- **Invariant:** `P[i] = a[0] + ... + a[i-1]`.
- **Complexity:** O(n) build, O(1) query, O(n) space.
- **Fails when:** The array is **mutated between queries** (use a Fenwick/segment tree, not covered Day 1).

### 2.12 Prefix sums combined with hash maps

- **Idea:** `sum(l..r) = K` ⇔ `P[r+1] - P[l] = K` ⇔ `P[l] = P[r+1] - K`. This is *complement lookup on prefix values*.
- **Recognize / Clue:** Subarray sum/balance/remainder conditions on arrays that may contain **negatives or zeros**.
- **Why brute force fails:** Enumerating all `(l, r)` pairs is O(n²); sliding windows are invalid with negatives.
- **Templates:**

```cpp
// COUNT subarrays with sum == K.
int countSubarraysSumK(const vector<int>& a, long long K) {
    unordered_map<long long, int> freq{{0, 1}};      // empty prefix
    long long pre = 0; int count = 0;
    for (int x : a) {
        pre += x;
        auto it = freq.find(pre - K);                // CUSTOMIZE: the needed prefix
        if (it != freq.end()) count += it->second;
        ++freq[pre];
    }
    return count;
}

// LONGEST subarray with sum == K: store EARLIEST index of each prefix value.
int longestSubarraySumK(const vector<int>& a, long long K) {
    unordered_map<long long, int> first{{0, -1}};    // prefix 0 occurs "before index 0"
    long long pre = 0; int best = 0;
    for (int i = 0; i < (int)a.size(); ++i) {
        pre += a[i];
        auto it = first.find(pre - K);
        if (it != first.end()) best = max(best, i - it->second);
        first.emplace(pre, i);                       // emplace keeps the earliest index
    }
    return best;
}
```

- **Invariant:** Before processing `i`, the map describes all prefixes `P[0..i]` (up to index `i-1` of elements).
- **Complexity:** O(n) expected time, O(n) space.
- **Fails when:** The condition is not an equation on prefix values (e.g., "sum ≥ K" with negatives needs a monotonic deque, outside Day 1).

### 2.13 Prefix/suffix products (and prefix/suffix aggregates)

- **Idea:** Answer for `i` depends on everything to its left and everything to its right, each of which can be accumulated in one pass.
- **Recognize / Clue:** "except self", "without division", "left max and right max".
- **Why brute force fails:** Recomputing left/right aggregates per index is O(n²).
- **Template:**

```cpp
vector<int> productExceptSelfTemplate(const vector<int>& a) {
    int n = a.size();
    vector<int> out(n, 1);
    int left = 1;
    for (int i = 0; i < n; ++i) { out[i] = left; left *= a[i]; }     // out[i] = product of a[0..i-1]
    int right = 1;
    for (int i = n - 1; i >= 0; --i) { out[i] *= right; right *= a[i]; } // times product of a[i+1..]
    return out;
}
```

- **Invariant:** After pass 1, `out[i]` = exclusive prefix product. After pass 2, `out[i]` = prefix × suffix.
- **Complexity:** O(n) time, O(1) auxiliary (output array excluded).
- **Fails when:** The operation has no identity or is non-associative; or when overflow is possible (the problem must guarantee fit).

### 2.14 Kadane's algorithm

- **Idea:** Best subarray *ending at i* is either `a[i]` alone or `a[i]` appended to the best ending at `i-1`.
- **Recognize / Clue:** "maximum sum of contiguous subarray".
- **Why brute force fails:** O(n²) subarrays (or O(n³) naive).
- **Template:**

```cpp
int kadane(const vector<int>& a) {                   // requires a non-empty
    int cur = a[0], best = a[0];
    for (size_t i = 1; i < a.size(); ++i) {
        cur = max(a[i], cur + a[i]);                 // extend or restart
        best = max(best, cur);
    }
    return best;
}
```

- **Invariant:** `cur` = max sum of a subarray ending exactly at `i`; `best` = max over all `i` so far.
- **Complexity:** O(n) time, O(1) space.
- **Fails when:** The subarray must be non-empty and all values negative *and you initialise with 0* (bug), or when a length constraint exists (then use prefix sums with a sliding minimum).

### 2.15 Monotonic and boundary reasoning (where relevant)

- **Idea:** The answer at a position is determined by the nearest/maximal *boundary* on each side. A pointer on the *smaller* side is safe to process because the other side can only be at least as large.
- **Recognize / Clue:** "water trapped", "area limited by the shorter side", "first missing positive in range [1..n]".
- **Why brute force fails:** Scanning left and right for each index is O(n²).
- **Template (two-sided boundary):**

```cpp
// Generic: process the side whose boundary is the binding constraint.
int boundaryTwoPointer(const vector<int>& h) {
    int l = 0, r = (int)h.size() - 1, leftMax = 0, rightMax = 0, total = 0;
    while (l < r) {
        if (h[l] < h[r]) { leftMax = max(leftMax, h[l]); total += leftMax - h[l]; ++l; }
        else             { rightMax = max(rightMax, h[r]); total += rightMax - h[r]; --r; }
    }
    return total;
}
```

- **Invariant:** When `h[l] < h[r]`, the right boundary for `l` is at least `h[r] > h[l]`, so `leftMax` alone determines the water at `l`.
- **Complexity:** O(n) time, O(1) space.
- **Fails when:** Boundaries are not simply "max so far" (use a monotonic stack; not needed Day 1).

### 2.16 Matrix traversal and in-place marking

- **Idea:** Walk a 2D grid using index boundaries (`top, bottom, left, right`) or reuse row 0/column 0 of the matrix as marker storage.
- **Recognize / Clue:** "spiral", "rotate", "set zeros in place".
- **Why brute force fails:** A copy of the matrix is O(mn) space; naive in-place zeroing corrupts later decisions (zeros you wrote look like original zeros).
- **Template (spiral boundaries):**

```cpp
vector<int> spiral(const vector<vector<int>>& M) {
    vector<int> out;
    if (M.empty() || M[0].empty()) return out;
    int top = 0, bottom = (int)M.size() - 1, left = 0, right = (int)M[0].size() - 1;
    while (top <= bottom && left <= right) {
        for (int c = left; c <= right; ++c) out.push_back(M[top][c]);
        ++top;
        for (int r = top; r <= bottom; ++r) out.push_back(M[r][right]);
        --right;
        if (top <= bottom) { for (int c = right; c >= left; --c) out.push_back(M[bottom][c]); --bottom; }
        if (left <= right) { for (int r = bottom; r >= top; --r) out.push_back(M[r][left]); ++left; }
    }
    return out;
}
```

- **Invariant:** The unvisited cells are exactly the sub-rectangle `[top..bottom] × [left..right]`.
- **Complexity:** O(mn) time; O(1) auxiliary (excluding output).
- **Fails when:** You forget the two `if` guards, which causes duplicate visits in single-row/single-column leftovers.

### 2.17 Interval sorting and merging

- **Idea:** After sorting by start, overlapping intervals are consecutive; maintain the current merged interval and extend its end.
- **Recognize / Clue:** "merge overlapping", "insert interval", "meeting rooms" style.
- **Why brute force fails:** Comparing every pair repeatedly is O(n²) (and iterating until no merges remain is worse).
- **Template:**

```cpp
vector<vector<int>> mergeIntervals(vector<vector<int>> iv) {
    sort(iv.begin(), iv.end());                      // by start, then end
    vector<vector<int>> out;
    for (auto& cur : iv) {
        if (out.empty() || out.back()[1] < cur[0]) out.push_back(cur);   // gap: new group
        else out.back()[1] = max(out.back()[1], cur[1]);                 // overlap/touch: extend
    }
    return out;
}
```

- **Invariant:** `out` holds disjoint, sorted merged intervals covering all intervals processed so far.
- **Complexity:** O(n log n) time (sort dominates); O(1) auxiliary besides the output.
- **Fails when:** Touching intervals (`[1,4]` and `[4,5]`) have different problem semantics. The condition `<` vs `<=` is a **requirement**, not a style choice.

### 2.18 Supplementary techniques used once or twice on Day 1

| Technique | Used in | One-line idea |
|---|---|---|
| Bucket sort by frequency | 5 | Frequencies ∈ [1, n], so index buckets by frequency |
| Boyer–Moore voting | 8 | Cancel unlike pairs; a true majority survives |
| Dutch national flag (3-way partition) | 9 | Maintain `[0,low)=0`, `[low,mid)=1`, `(high,n)=2` |
| Index-as-hash / cyclic placement | 10 | Value `v` belongs at index `v-1`; place it there with swaps |
| Merge from the back | 44 | Write from the end so unread data is never overwritten |
| Reverse-three-times rotation | 45 | Rotation = reverse all, then reverse the two blocks |
| Floyd's cycle detection | 46 | Treat `i → nums[i]` as a linked list; the duplicate is the cycle entry |

---
## Part 3: Complete Tutorial for All 50 Problems

Every problem uses the same lettered structure (A to L). **Attempt the problem first.** Recall-question answers are in Part 8.

---

## Section 1: Arrays and Hashing

### Problem 1: Two Sum

**A. Identification:** LeetCode **1** · Easy · Primary: complement lookup (hash map) · Secondary: sorting + pointers (variant)

**B. Statement:** Given an integer array `nums` and an integer `target`, return the indices of two *different* elements whose sum is `target`. Exactly one solution exists. Order of the returned indices does not matter.

**C. Pattern recognition**
- Clues: "two elements", fixed sum, **unsorted** input, *indices* required.
- Strong candidates notice that for a fixed `nums[i]`, the partner value is *determined*: `target - nums[i]`.
- First question: "Given one element, what exactly must the other one be?"
- **Rule:** *Pair with an equation + unsorted + indices ⇒ hash map of value → index.*

**D. Brute force:** For every `i < j`, check `nums[i] + nums[j] == target`. Time O(n²), auxiliary space O(1). Inefficient because the inner loop *searches* for a value we could look up directly.

**E. Optimization reasoning:** Replace the inner search by a lookup. Keep `seen[value] = index` for earlier elements. At index `i`, look for `target - nums[i]` in `seen`. Insert `nums[i]` *after* the lookup so an element can't pair with itself. Duplicates work: `[3,3]`, target 6: at `i=1` the map holds `3→0`.

**F. Algorithm**
1. `seen = {}`.
2. For each `i`: `need = target - nums[i]`.
3. If `need` in `seen`, return `{seen[need], i}`.
4. Else `seen[nums[i]] = i`.

**G. C++17 solution**

```cpp
#include <vector>
#include <unordered_map>
using namespace std;

class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<long long, int> seen;                 // value -> index
        for (int i = 0; i < (int)nums.size(); ++i) {
            long long need = (long long)target - nums[i];   // long long: no overflow risk
            auto it = seen.find(need);
            if (it != seen.end()) return {it->second, i};
            seen[nums[i]] = i;
        }
        return {};                                          // unreachable per constraints
    }
};
```

Dry run: `[2,7,11,15]`, target 9. i=0: need 7, not seen, store 2→0. i=1: need 2, found at 0 → `{0,1}`.

**H. Correctness and complexity:** *Invariant:* before step `i`, `seen` contains all elements at indices `< i`. If a valid pair `(j, i)` with `j < i` exists, `nums[j]` is in `seen` when we reach `i`, so the first such `i` finds it. Time O(n) expected (O(n²) worst case under adversarial hashing); space O(n).

**I. Reusable template:** *Complement lookup* (Part 2.4). Init: empty map. Loop: one pass. Invariant: map = prefix of processed elements. Update: insert after query. Termination: first hit. **What generalizes:** any "find earlier element satisfying a relation determined by the current one". **What changes:** the *needed value* formula and what you store (index, count, first index).

**J. Common mistakes:** inserting before querying (reuse same element); using the same index twice; storing counts instead of indices; assuming sorted input; returning values instead of indices.

**K. Variation:** *Return all unique value pairs, or array is sorted and O(1) space required.* Hash map still works for existence, but uniqueness handling gets messy. For sorted input use opposite-end pointers (Problem 12). Assumption making the map approach valid: you only need *one* pair and extra space is allowed. If space is O(1) and order is free, sort + pointers (O(n log n)), but you lose original indices unless you pair them with their indices first.

**L. Recall questions**
- **Q1.1:** Why must the insertion into the map come after the lookup?
- **Q1.2:** Array is sorted and you must use O(1) extra space; which approach and what's its time?

---

### Problem 2: Contains Duplicate

**A. Identification:** LeetCode **217** · Easy · Primary: hash set · Secondary: sorting

**B. Statement:** Return `true` if any value appears at least twice in `nums`.

**C. Pattern recognition**
- Clue: "appears at least twice" = membership question.
- First question: "Can I remember what I've seen in O(1) per check?"
- **Rule:** *"Seen before?" ⇒ set.*

**D. Brute force:** Compare all pairs: O(n²) time, O(1) space. Wasteful because each element re-scans earlier ones.

**E. Optimization reasoning:** A set answers "seen before?" in O(1) expected. Alternative: sort (O(n log n)) so equal values are adjacent and compare neighbors (O(1) extra if you may modify the input). `unordered_set::insert` returns `{iterator, bool}`; the bool is `false` if already present.

**F. Algorithm**
1. `seen = {}`.
2. For each `x`: if `x` already in `seen` return true; else insert.
3. Return false.

**G. C++17 solution**

```cpp
#include <vector>
#include <unordered_set>
using namespace std;

class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> seen;
        seen.reserve(nums.size() * 2);                // reduces rehashing
        for (int x : nums)
            if (!seen.insert(x).second) return true;  // insert fails => duplicate
        return false;
    }
};
```

Dry run: `[1,2,3,1]` → inserts 1,2,3; inserting 1 fails → true.

**H. Correctness and complexity:** *Invariant:* `seen` = set of elements in the processed prefix. A duplicate exists iff some element is already in `seen` when processed. Time O(n) expected; space O(n). Sorting alternative: O(n log n), O(1) extra (ignoring sort stack).

**I. Reusable template:** *Seen-set scan* (Part 2.2). Init: empty set. Loop: single pass. Update: insert. Termination: early exit on repeat. Generalizes to "first repeated element", "cycle detection by visited set". Changes: what you test and what you insert (e.g., window-limited set for LC 219).

**J. Common mistakes:** sorting a `const` input; forgetting early return; using `vector<bool>` of fixed size when values are huge/negative.

**K. Variation:** *Duplicates within distance k (LC 219).* Same set idea but the set must hold only the last `k` elements (remove `nums[i-k]` as you slide). Assumption: set membership is the only question. If you need *how many* duplicates, switch to a frequency map.

**L. Recall questions**
- **Q2.1:** What does `unordered_set::insert(x).second` tell you?
- **Q2.2:** How do you adapt the solution for "duplicates within index distance ≤ k"?

---

### Problem 3: Valid Anagram

**A. Identification:** LeetCode **242** · Easy · Primary: frequency counting · Secondary: sorting

**B. Statement:** Given strings `s` and `t` (lowercase English letters), return whether `t` is a rearrangement of `s`.

**C. Pattern recognition**
- Clue: "rearrangement/anagram" = same multiset of characters.
- First question: "What is the order-independent signature of a string?"
- **Rule:** *Anagram ⇒ equal frequency tables (or equal sorted strings).*

**D. Brute force:** Sort both strings and compare: O(n log n) time. Not "bad", but the log factor is avoidable for a tiny fixed alphabet.

**E. Optimization reasoning:** The alphabet has 26 letters, so use a fixed count array. Increment for `s`, decrement for `t`; all zeros ⇔ anagram. Check lengths first: different lengths ⇒ false immediately.

**F. Algorithm**
1. If lengths differ, return false.
2. `cnt[26] = {0}`; for `i`: `++cnt[s[i]]`, `--cnt[t[i]]`.
3. Return true iff all counts are zero.

**G. C++17 solution**

```cpp
#include <string>
#include <array>
using namespace std;

class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.size() != t.size()) return false;
        array<int, 26> cnt{};                    // zero-initialised
        for (size_t i = 0; i < s.size(); ++i) {
            ++cnt[s[i] - 'a'];
            --cnt[t[i] - 'a'];
        }
        for (int c : cnt) if (c != 0) return false;
        return true;
    }
};
```

Dry run: `s="ab"`, `t="ba"`: cnt[a]: +1 −1 = 0; cnt[b]: +1 −1 = 0 → true.

**H. Correctness and complexity:** Equal multisets ⇔ every net count is zero. Equal lengths guarantee no hidden leftover. Time O(n); space O(1) (26 ints).

**I. Reusable template:** *Frequency table* (Part 2.3). Init: zeroed array sized to the alphabet. Loop: increment/decrement. Termination: check zeros. Generalizes to any "same multiset" test. Changes: alphabet size/key type (map for Unicode).

**J. Common mistakes:** skipping the length check; forgetting `'a'` offset; assuming lowercase only when the input has uppercase/Unicode.

**K. Variation:** *Unicode characters.* Same idea, but use `unordered_map<char32_t,int>` (after decoding) or `map`. Assumption: counts are a complete signature of equality. Alternative: sort both strings.

**L. Recall questions**
- **Q3.1:** Why is the length check necessary in the single-array increment/decrement approach?
- **Q3.2:** What changes if input may contain arbitrary Unicode?

---

### Problem 4: Group Anagrams

**A. Identification:** LeetCode **49** · Medium · Primary: hash map with canonical key · Secondary: frequency counting / sorting

**B. Statement:** Given an array of strings, group the strings that are anagrams of each other. Return the groups in any order.

**C. Pattern recognition**
- Clue: "group by equivalence class" + anagram equivalence.
- First question: "What canonical representation is identical for all anagrams?"
- **Rule:** *Group by property ⇒ map from canonical key → list.*

**D. Brute force:** For each string, compare against existing groups' representatives using an anagram check: O(n · g · K) where `g` = number of groups and `K` = max length; up to O(n²K).

**E. Optimization reasoning:** Compute a canonical key per string, then bucket by key in a hash map. Key options: sorted string (O(K log K)) or a count-signature (O(K) with a 26-entry encoding). Key is the *only* part that changes between approaches.

**F. Algorithm**
1. `groups: map<string, vector<string>>`.
2. For each `s`: `key = sorted(s)`; `groups[key].push_back(s)`.
3. Move all map values into the answer.

**G. C++17 solution**

```cpp
#include <vector>
#include <string>
#include <unordered_map>
#include <algorithm>
using namespace std;

class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> groups;
        for (const string& s : strs) {
            string key = s;
            sort(key.begin(), key.end());          // canonical form of the anagram class
            groups[key].push_back(s);
        }
        vector<vector<string>> result;
        result.reserve(groups.size());
        for (auto& entry : groups) result.push_back(move(entry.second));
        return result;
    }
};
```

Dry run: `["eat","tea","tan"]` → keys `aet, aet, ant` → `[["eat","tea"],["tan"]]`.

**H. Correctness and complexity:** Two strings are anagrams ⇔ their sorted forms are equal, so the map partitions strings exactly into anagram classes. Time O(N · K log K) (N strings, max length K) expected hashing; space O(N·K) for the keys and output. Count-signature key reduces to O(N·K).

**I. Reusable template:** *Group-by-canonical-key.* Init: `map<Key, vector<T>>`. Loop: compute key, append. Termination: harvest values. **What generalizes:** any equivalence-class grouping (rotations, normalized paths). **What changes:** the key function; make sure it is *complete* (same class ⇔ same key).

**J. Common mistakes:** using the original string as part of the key; count key without separators (e.g., concatenating counts `1`,`11` vs `11`,`1`: ambiguous), which needs a delimiter; assuming output order matters.

**K. Variation:** *Group by "shift-equivalent" strings (LC 249).* Same pattern; the canonical key is the sequence of differences between consecutive letters (mod 26). Assumption: a complete canonical key exists. If none exists (fuzzy equivalence), you need union-find or pairwise comparison.

**L. Recall questions**
- **Q4.1:** Why must a count-based key include a delimiter between counts?
- **Q4.2:** What canonical key would you use to group strings that are rotations of each other?

---

### Problem 5: Top K Frequent Elements

**A. Identification:** LeetCode **347** · Medium · Primary: frequency counting + bucket sort · Secondary: heap

**B. Statement:** Given integer array `nums` and `k`, return the `k` most frequent elements in any order. The answer is guaranteed unique.

**C. Pattern recognition**
- Clues: "most frequent", "top k", requirement often better than O(n log n).
- First question: "Once I have frequencies, what is the range of those frequencies?"
- **Rule:** *Top-k by frequency ⇒ count, then bucket by frequency (range 1..n) or use a size-k min-heap.*

**D. Brute force:** Count, put `(freq, value)` pairs in a vector, sort descending, take `k`. O(n log n) time, O(n) space.

**E. Optimization reasoning:** A frequency is an integer in `[1, n]`, so build `buckets[f]` = values with frequency `f` and scan from `n` down. This is O(n). Heap alternative: min-heap of size `k` gives O(n log k), better when `k` is small and you stream.

**F. Algorithm**
1. Count frequencies in `unordered_map`.
2. `buckets` of size `n+1`; for each `(value, f)` push `value` into `buckets[f]`.
3. For `f = n..1`, append bucket values until `k` collected.

**G. C++17 solution**

```cpp
#include <vector>
#include <unordered_map>
using namespace std;

class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> freq;
        for (int x : nums) ++freq[x];

        vector<vector<int>> buckets(nums.size() + 1);   // buckets[f] = values with frequency f
        for (const auto& [value, f] : freq) buckets[f].push_back(value);

        vector<int> result;
        for (int f = (int)nums.size(); f >= 1 && (int)result.size() < k; --f)
            for (int value : buckets[f]) {
                result.push_back(value);
                if ((int)result.size() == k) break;
            }
        return result;
    }
};
```

Dry run: `[1,1,1,2,2,3]`, k=2 → freq `1:3, 2:2, 3:1`; buckets[3]={1}, buckets[2]={2}; scan from 6 down → result `[1,2]`.

**H. Correctness and complexity:** Scanning buckets in descending frequency yields values in non-increasing frequency order, so the first `k` collected are the top `k`. Time O(n) expected; auxiliary space O(n) (map + buckets), output O(k).

**I. Reusable template:** *Count then bucket* (Part 2.18). Init: freq map; buckets sized by max possible frequency. Loop: distribute, then scan. **What generalizes:** any "sort by small-range integer key". **What changes:** bucket range (needs a bounded key).

**J. Common mistakes:** bucket array size `n` instead of `n+1`; breaking only the inner loop (then the outer continues; here the outer condition handles it); using a max-heap of all items when a size-`k` min-heap is needed for O(n log k).

**K. Variation:** *Top-k frequent words with lexicographic tie-break (LC 692).* Frequencies can tie, so bucket then sort within bucket, or heap with a custom comparator. The bucket trick still gives the frequency order; tie-break adds a sort inside buckets. Assumption: ties unspecified here (answer unique).

**L. Recall questions**
- **Q5.1:** Why is the bucket array size `n + 1` and what is the maximum useful index?
- **Q5.2:** When is a size-`k` min-heap preferable to bucket sort?

---

### Problem 6: Product of Array Except Self

**A. Identification:** LeetCode **238** · Medium · Primary: prefix/suffix products · Secondary: in-place output reuse

**B. Statement:** For each `i`, return the product of all elements except `nums[i]`, without using division, in O(n) time. The product of any prefix or suffix fits in 32 bits.

**C. Pattern recognition**
- Clues: "except self", "no division", O(n).
- First question: "What is the answer in terms of what lies left and right of `i`?"
- **Rule:** *Everything except `i` = left part × right part ⇒ prefix/suffix pass.*

**D. Brute force:** For each `i`, multiply all others: O(n²) time, O(1) auxiliary. Division version (total product / `nums[i]`) fails with zeros and is forbidden.

**E. Optimization reasoning:** `out[i] = (product of a[0..i-1]) × (product of a[i+1..n-1])`. Compute the left products in pass 1, storing them directly in `out`. In pass 2 sweep from the right with a running `right` product and multiply it in. This reuses `out`, so auxiliary space is O(1).

**F. Algorithm**
1. `out[i] = 1`; `left = 1`; for `i = 0..n-1`: `out[i] = left; left *= nums[i]`.
2. `right = 1`; for `i = n-1..0`: `out[i] *= right; right *= nums[i]`.

**G. C++17 solution**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = (int)nums.size();
        vector<int> out(n, 1);
        int left = 1;
        for (int i = 0; i < n; ++i) {        // out[i] = product of nums[0..i-1]
            out[i] = left;
            left *= nums[i];
        }
        int right = 1;
        for (int i = n - 1; i >= 0; --i) {   // multiply by product of nums[i+1..n-1]
            out[i] *= right;
            right *= nums[i];
        }
        return out;
    }
};
```

Dry run: `[1,2,3,4]`. Pass 1: `out=[1,1,2,6]`. Pass 2 (right: 1,4,12,24): `out=[24,12,8,6]`.

**H. Correctness and complexity:** After pass 1, `out[i]` = exclusive prefix product. During pass 2 at index `i`, `right` = product of `nums[i+1..]`. So `out[i]` becomes prefix × suffix. Time O(n); auxiliary space O(1) (output excluded). Note `left`/`right` updates after the last element may overflow *unused* values only if products exceed int; guaranteed not by constraints.

**I. Reusable template:** *Prefix/suffix aggregate* (Part 2.13). Init: identity element. Pass 1 left→right writes exclusive aggregate; pass 2 right→left combines. **What generalizes:** any associative operation (sum, max, gcd). **What changes:** identity and combine operator. See Problem 40 for the deeper prefix/suffix perspective.

**J. Common mistakes:** using division; including `nums[i]` in its own prefix; forgetting identity `1`; confusing exclusive vs inclusive prefix.

**K. Variation:** *Division allowed, zeros allowed.* Count zeros: if ≥2 zeros all outputs 0; if exactly 1, only the zero position is non-zero (product of the others); if 0 zeros, `total / nums[i]`. Assumption of the main method: no division/inverse needed. Operations without inverses (max, gcd) still need prefix/suffix.

**L. Recall questions**
- **Q6.1:** Why does storing `left` before multiplying by `nums[i]` give an *exclusive* prefix?
- **Q6.2:** How would you compute "max of all elements except self" with the same pattern?

---

### Problem 7: Longest Consecutive Sequence

**A. Identification:** LeetCode **128** · Medium · Primary: hash set · Secondary: sequence-start detection

**B. Statement:** Given an unsorted integer array, return the length of the longest run of consecutive integers (values like `x, x+1, x+2...`) that appear in the array, in O(n) time.

**C. Pattern recognition**
- Clues: "consecutive values" (not positions), unsorted, O(n) demanded (so no sorting).
- First question: "Which elements can be the *start* of a run?"
- **Rule:** *Consecutive-values run + O(n) ⇒ set + expand only from starts (x with x−1 absent).*

**D. Brute force:** Sort and scan runs: O(n log n). Or for each `x` expand upward with membership checks: O(n²) worst case (each element re-walks long runs).

**E. Optimization reasoning:** Expanding from every element repeats work: a run `[1..1000]` is walked 1000 times. Only expand when `x - 1` is **not** in the set (run start). Then each element is visited by at most one expansion, total O(n).

**F. Algorithm**
1. Put all numbers in `unordered_set`.
2. For each `x` **in the set** (not the array: avoids duplicate work): if `x-1` in set, skip.
3. Else count upward while `x+len` in set.
4. Track the max length.

**G. C++17 solution**

```cpp
#include <vector>
#include <unordered_set>
#include <algorithm>
using namespace std;

class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<long long> present(nums.begin(), nums.end());
        int best = 0;
        for (long long x : present) {
            if (present.count(x - 1)) continue;        // not a start of a run
            int length = 1;
            while (present.count(x + length)) ++length;
            best = max(best, length);
        }
        return best;
    }
};
```

Dry run: `[100,4,200,1,3,2]`: starts are 100, 200, 1. From 1: 2,3,4 present → length 4. Answer 4.

**H. Correctness and complexity:** Every maximal run has exactly one start (its minimum). We expand from each start to full length. Each element is counted in exactly one expansion, because non-starts skip. Time O(n) expected; space O(n). Using `long long` avoids `x-1`/`x+1` overflow at `INT_MIN`/`INT_MAX`.

**I. Reusable template:** *Set + start-only expansion.* Init: set. Loop: iterate unique values; guard with "is this a boundary?". **What generalizes:** any structure where each component is walked once from a canonical entry. **What changes:** the "start" definition (e.g., x-1 absent) and the step (x+1).

**J. Common mistakes:** iterating the array (duplicates cause repeated expansion; still correct but may degrade); skipping the start check → O(n²); overflow on `x-1`; sorting and forgetting to skip duplicate values (they should not extend runs).

**K. Variation:** *Streaming inserts, query the longest run at any time.* Use a hash map storing run length at endpoints, or union-find. Assumption: static data. For static data with small value range, a boolean array/bitset can replace the hash set.

**L. Recall questions**
- **Q7.1:** What exactly stops the "expand from every element" idea from being O(n²) here?
- **Q7.2:** How would you handle values stream-inserted one at a time?

---

### Problem 8: Majority Element

**A. Identification:** LeetCode **169** · Easy · Primary: Boyer–Moore voting · Secondary: hash map / sorting

**B. Statement:** Given an array of size `n`, return the element appearing more than `⌊n/2⌋` times. Such an element always exists.

**C. Pattern recognition**
- Clues: "more than n/2" and "always exists", O(1) space in follow-up.
- First question: "If I cancel pairs of different elements, what survives?"
- **Rule:** *Strict majority guaranteed ⇒ Boyer–Moore cancellation.*

**D. Brute force:** Count each candidate: O(n²); or hash-map counts: O(n) time, O(n) space; or sort and take `nums[n/2]`: O(n log n).

**E. Optimization reasoning:** Keep a `candidate` and a `count`. If `count == 0`, adopt the current element. Matching element increments, different element decrements. Each decrement cancels one majority copy with one non-majority copy; since the majority has more than half, it can't be fully cancelled.

**F. Algorithm**
1. `candidate = none`, `count = 0`.
2. For each `x`: if `count == 0` → `candidate = x`; then `count += (x == candidate) ? 1 : -1`.
3. Return `candidate`.

**G. C++17 solution**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int candidate = 0, count = 0;
        for (int x : nums) {
            if (count == 0) candidate = x;
            count += (x == candidate) ? 1 : -1;
        }
        return candidate;       // valid because a majority element is guaranteed
    }
};
```

Dry run: `[2,2,1,1,1,2,2]`: cand 2 (1), 2 (2), 1 (1), 1 (0), then cand=1 (1), 2 (0), cand=2 (1) → 2.

**H. Correctness and complexity:** *Proof idea:* pair every removal of a "different" element with a distinct counterpart. The majority appears > n/2 times, so more than half the elements are the majority, and it cannot be paired away entirely. Time O(n), space O(1).

**I. Reusable template:** *Boyer–Moore.* Init: `count=0`. Loop: reset candidate on zero, vote. **What generalizes:** frequent items > n/k need k−1 candidates (Misra–Gries). **What changes:** number of candidates, plus a **verification pass** if existence isn't guaranteed.

**J. Common mistakes:** omitting verification when majority isn't guaranteed; resetting `count` incorrectly; assuming "most frequent" = "more than n/2".

**K. Variation:** *Elements appearing more than n/3 times (LC 229).* Two candidates with two counters, then a verification pass. Assumption in the original: the majority is guaranteed. If not guaranteed, the survivor is only a candidate and must be verified by counting.

**L. Recall questions**
- **Q8.1:** Why is the final candidate correct only because a majority is guaranteed?
- **Q8.2:** How many candidates for "more than n/3"?

---

### Problem 9: Sort Colors

**A. Identification:** LeetCode **75** · Medium · Primary: Dutch national flag (3-way partition) · Secondary: counting sort

**B. Statement:** Sort an array of `0`s, `1`s, `2`s in place so that same values are adjacent, ordered `0,1,2`, without using the library sort.

**C. Pattern recognition**
- Clues: three distinct values, in place, one pass wanted.
- First question: "Where do 0s, 1s, 2s end up, and can I place each as I see it?"
- **Rule:** *Three-valued array, in place ⇒ Dutch flag with `low, mid, high`.*

**D. Brute force:** Counting sort: count 0s,1s,2s, then overwrite: two passes, O(n) time, O(1) space. Sorting: O(n log n).

**E. Optimization reasoning:** One pass with three zones: `[0,low)` = 0s, `[low,mid)` = 1s, `[mid,high]` = unknown, `(high,n)` = 2s. Examine `nums[mid]`: if 0, swap with `low` and advance both; if 1, advance `mid`; if 2, swap with `high` and shrink `high` **without** advancing `mid` (swapped-in element is unexamined).

**F. Algorithm**
1. `low = 0, mid = 0, high = n-1`.
2. While `mid <= high`: case 0 → `swap(low++, mid++)`; case 1 → `mid++`; case 2 → `swap(mid, high--)`.

**G. C++17 solution**

```cpp
#include <vector>
#include <utility>
using namespace std;

class Solution {
public:
    void sortColors(vector<int>& nums) {
        int low = 0, mid = 0, high = (int)nums.size() - 1;
        while (mid <= high) {
            if (nums[mid] == 0)      swap(nums[low++], nums[mid++]);
            else if (nums[mid] == 1) ++mid;
            else                     swap(nums[mid], nums[high--]);  // do NOT advance mid
        }
    }
};
```

Dry run: `[2,0,1]`: mid=0 is 2 → swap with high → `[1,0,2]`, high=1. mid=0 is 1 → mid=1. mid=1 is 0 → swap with low(0) → `[0,1,2]`, low=1, mid=2 > high → done.

**H. Correctness and complexity:** *Invariant:* `[0,low)` all 0, `[low,mid)` all 1, `(high,n)` all 2. Each case preserves it and shrinks the unknown zone `[mid, high]`. When `mid > high` nothing is unknown. Time O(n), space O(1). When swapping with `low`, the element at `low` is always a `1` (or `low == mid`), so `mid` can advance safely.

**I. Reusable template:** *3-way partition.* Init: three indices. Loop: classify `nums[mid]`. Termination: `mid > high`. **What generalizes:** partition around a pivot (quicksort with duplicates), "move all X to front". **What changes:** classification predicate (`< pivot`, `== pivot`, `> pivot`).

**J. Common mistakes:** advancing `mid` after swapping with `high`; wrong loop condition (`mid < high` skips the last element); using `<` instead of `<=`.

**K. Variation:** *K distinct colors (K > 3).* Dutch flag no longer works directly; use counting sort (O(n + K)) or recursive partitioning. Assumption: constant number (three) of classes allows a fixed zone structure.

**L. Recall questions**
- **Q9.1:** Why isn't `mid` advanced after swapping with `high`?
- **Q9.2:** What do you use when the array has values 0 to K−1 with K large?

---

### Problem 10: First Missing Positive

**A. Identification:** LeetCode **41** · Hard · Primary: index-as-hash (cyclic placement) · Secondary: boundary reasoning on answer range

**B. Statement:** Given an unsorted integer array, return the smallest missing positive integer, in O(n) time and O(1) extra space.

**C. Pattern recognition**
- Clues: "smallest missing positive", O(1) space, array of size `n`.
- Key observation before coding: the answer is in `[1, n+1]`.
- First question: "If the answer must be in 1..n+1, can the array itself serve as the presence table?"
- **Rule:** *Smallest missing in range [1..n] + O(1) space ⇒ place value `v` at index `v−1`.*

**D. Brute force:** For `x = 1,2,...`, scan for `x`: O(n²). Or hash set: O(n) time but O(n) space. Or sort: O(n log n).

**E. Optimization reasoning:** With `n` elements, the answer ≤ `n+1` (pigeonhole). Values outside `[1, n]` are irrelevant. Put each value `v ∈ [1,n]` at index `v-1` via swaps. Then the first index `i` where `nums[i] != i+1` gives answer `i+1`; if none, answer `n+1`. Each swap puts one element in its final position, so total swaps ≤ n.

**F. Algorithm**
1. For each `i`: while `1 ≤ nums[i] ≤ n` and `nums[nums[i]-1] != nums[i]`: swap `nums[i]` with `nums[nums[i]-1]`.
2. Scan: first `i` with `nums[i] != i+1` → return `i+1`.
3. Return `n+1`.

**G. C++17 solution**

```cpp
#include <vector>
#include <utility>
using namespace std;

class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n = (int)nums.size();
        for (int i = 0; i < n; ++i) {
            while (nums[i] >= 1 && nums[i] <= n && nums[nums[i] - 1] != nums[i]) {
                int target = nums[i] - 1;          // compute index first, then swap
                swap(nums[i], nums[target]);
            }
        }
        for (int i = 0; i < n; ++i)
            if (nums[i] != i + 1) return i + 1;
        return n + 1;
    }
};
```

Dry run: `[3,4,-1,1]`: i=0: 3 → swap with idx2: `[-1,4,3,1]`; -1 stops. i=1: 4 → swap with idx3: `[-1,1,3,4]`; now 1 → swap with idx0: `[1,-1,3,4]`. Scan: idx1 has −1 ≠ 2 → answer 2.

**H. Correctness and complexity:** *Invariant:* Each swap places a value into its home index, and the `nums[nums[i]-1] != nums[i]` check prevents infinite loops on duplicates. After the pass, every value in `[1,n]` that exists sits at its home index. The first non-matching index is the smallest absent positive. Time O(n) (each swap finalizes one element, ≤ n swaps), space O(1).

**I. Reusable template:** *Cyclic placement.* Init: none. Loop: `while` misplaced and in range → swap. Termination: element placed, out of range, or duplicate. **What generalizes:** "find duplicate/missing in 1..n". **What changes:** home index formula (`v-1` vs `v`), what you scan for afterwards.

**J. Common mistakes:** `if` instead of `while`; swapping without the duplicate check (infinite loop); evaluating `swap(nums[i], nums[nums[i]-1])` inline is OK for `std::swap`, but manual temp-based swaps with a changing `nums[i]` are not; forgetting to return `n+1`.

**K. Variation:** *Array is read-only.* The in-place trick is invalid. Use a hash set (O(n) space) or sort a copy. Assumption: you may mutate the input. Another variation: smallest missing *non-negative* integer, which shifts the home index to `v` and the answer range to `[0, n]`.

**L. Recall questions**
- **Q10.1:** Why is the answer always within `[1, n+1]`?
- **Q10.2:** What goes wrong if you use `if` instead of `while` in the placement loop?

---
## Section 2: Two Pointers and Array Manipulation

### Problem 11: Valid Palindrome

**A. Identification:** LeetCode **125** · Easy · Primary: opposite-end two pointers · Secondary: character filtering

**B. Statement:** After converting uppercase to lowercase and removing all non-alphanumeric characters, decide whether the string reads the same forwards and backwards. An empty cleaned string is a palindrome.

**C. Pattern recognition**
- Clue: palindrome = compare mirrored positions.
- First question: "Can I compare mirror pairs without building a cleaned copy?"
- **Rule:** *Palindrome check ⇒ pointers from both ends moving inward.*

**D. Brute force:** Build the cleaned lowercase string, reverse it, compare. O(n) time, O(n) space. Fine, but the extra copy is unnecessary.

**E. Optimization reasoning:** Skip non-alphanumeric characters with the pointers themselves. At each step both pointers sit on alphanumeric characters; compare lowercase versions. Mismatch → false. Pointers cross → true.

**F. Algorithm**
1. `l = 0`, `r = n-1`.
2. While `l < r`: advance `l` while not alnum; retreat `r` while not alnum.
3. If `l < r` and lowercase differ → false; else `l++`, `r--`.
4. Return true.

**G. C++17 solution**

```cpp
#include <string>
#include <cctype>
using namespace std;

class Solution {
public:
    bool isPalindrome(string s) {
        int l = 0, r = (int)s.size() - 1;
        while (l < r) {
            while (l < r && !isalnum((unsigned char)s[l])) ++l;   // cast: avoid UB on negative char
            while (l < r && !isalnum((unsigned char)s[r])) --r;
            if (tolower((unsigned char)s[l]) != tolower((unsigned char)s[r])) return false;
            ++l; --r;
        }
        return true;
    }
};
```

Dry run: `"A man, a plan"`: l→'A', r→'n'; `a` vs `n` mismatch → false. `"a,a"`: l=0 'a', r=2 'a' equal; then l=1,r=1 loop ends → true.

**H. Correctness and complexity:** *Invariant:* all mirror pairs outside `[l, r]` already matched. Skipping non-alnum characters mirrors the filter step. Time O(n); space O(1).

**I. Reusable template:** *Opposite-end pointers with skipping.* Init: `l=0, r=n-1`. Loop: normalise both pointers, compare, move inward. Termination: `l >= r`. **What generalizes:** palindrome variants (Valid Palindrome II). **What changes:** the skip predicate and the comparison.

**J. Common mistakes:** forgetting `l < r` in the inner skip loops (out of bounds on all-punctuation input); not lowercasing; `isalnum` on a negative `char`; returning false for an empty cleaned string.

**K. Variation:** *Allow deleting at most one character (LC 680).* Opposite pointers still work, but on mismatch you branch: check `[l+1, r]` or `[l, r-1]` using the plain palindrome check. Assumption: pure comparison of mirrored positions. With at most one deletion the branching is bounded (O(n)); with `k` deletions it becomes a DP.

**L. Recall questions**
- **Q11.1:** Why do the inner skip loops also test `l < r`?
- **Q11.2:** How does the algorithm change if one deletion is allowed?

---

### Problem 12: Two Sum II: Input Array Is Sorted

**A. Identification:** LeetCode **167** · Medium (LeetCode's label) · Primary: opposite-end two pointers · Secondary: binary search

**B. Statement:** Given a **1-indexed non-decreasing** array `numbers` and `target`, return the 1-based indices of two different elements with sum `target`. Exactly one solution exists; use only O(1) extra space.

**C. Pattern recognition**
- Clues: sorted, pair sum, O(1) space.
- First question: "If the current sum is too big, which element can't help any more?"
- **Rule:** *Sorted + pair sum ⇒ squeeze from both ends.*

**D. Brute force:** All pairs: O(n²). Hash map: O(n) time but O(n) space, violating the requirement. Binary search for each complement: O(n log n).

**E. Optimization reasoning:** If `a[l] + a[r] > target`, then `a[r]` plus *any* element ≥ `a[l]` is also > target, so `r` is useless: drop it. If `< target`, `a[l]` plus any element ≤ `a[r]` is smaller: drop `l`. Each step eliminates one element permanently.

**F. Algorithm**
1. `l = 0`, `r = n-1`.
2. `s = a[l] + a[r]`: equal → return `{l+1, r+1}`; smaller → `l++`; larger → `r--`.

**G. C++17 solution**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int l = 0, r = (int)numbers.size() - 1;
        while (l < r) {
            long long sum = (long long)numbers[l] + numbers[r];
            if (sum == target) return {l + 1, r + 1};   // 1-indexed output
            if (sum < target) ++l;
            else --r;
        }
        return {};
    }
};
```

Dry run: `[2,7,11,15]`, 9: 2+15=17>9 → r=2; 2+11=13>9 → r=1; 2+7=9 → `{1,2}`.

**H. Correctness and complexity:** *Invariant:* if a solution exists it lies in `[l, r]`. Each pointer move discards an index that provably cannot be part of any solution given the sorted order. Time O(n); space O(1).

**I. Reusable template:** *Sorted pair-sum* (Part 2.6). Init: ends. Loop: compare sum with target. Termination: `l >= r` or hit. **What generalizes:** 3Sum, 4Sum, closest-sum, count-pairs-with-sum-less-than. **What changes:** target relation and what to do on equality (record + move both for "all pairs").

**J. Common mistakes:** 0-based output; `l <= r` (allows the same element twice); moving the wrong pointer; int overflow when values are near the limits.

**K. Variation:** *Unsorted input.* The elimination argument fails; use a hash map (O(n) space) or sort a copy with indices. Assumption: monotone relationship between pointer movement and sum. Also: "count all pairs with sum < target" still uses the squeeze, adding `r - l` pairs when `a[l]+a[r] < target`.

**L. Recall questions**
- **Q12.1:** Why is discarding `a[r]` safe when `a[l]+a[r] > target`?
- **Q12.2:** How do you count pairs with sum strictly less than `target` in a sorted array?

---

### Problem 13: 3Sum

**A. Identification:** LeetCode **15** · Medium · Primary: sort + two pointers · Secondary: duplicate skipping

**B. Statement:** Return all *unique* triplets `[a,b,c]` (from distinct indices) with `a + b + c = 0`. The solution set must not contain duplicate triplets.

**C. Pattern recognition**
- Clues: triplets, sum zero, uniqueness of *values*.
- First question: "If I fix one element, what remains?" → Two Sum on the rest.
- **Rule:** *k-sum with unique results ⇒ sort, fix elements, squeeze, skip duplicates.*

**D. Brute force:** Three nested loops with a set of sorted triplets for uniqueness: O(n³) time, extra space for dedup.

**E. Optimization reasoning:** Sort. Fix `nums[i]`, then solve sorted two-sum for `-nums[i]` on `(i, n)`. Sorting makes duplicates adjacent, so skip equal values at the fixed position and after recording a triplet. Early exit: if `nums[i] > 0` no triplet is possible.

**F. Algorithm**
1. Sort `nums`.
2. For `i` from 0: skip if `i>0` and `nums[i]==nums[i-1]`; stop if `nums[i] > 0`.
3. `l=i+1`, `r=n-1`. While `l<r`: sum smaller → `l++`; larger → `r--`; equal → record, advance both, then skip duplicates on both sides.

**G. C++17 solution**

```cpp
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        int n = (int)nums.size();
        for (int i = 0; i < n - 2; ++i) {
            if (nums[i] > 0) break;                            // sorted: remaining all positive
            if (i > 0 && nums[i] == nums[i - 1]) continue;     // skip duplicate anchor
            int l = i + 1, r = n - 1;
            while (l < r) {
                long long sum = (long long)nums[i] + nums[l] + nums[r];
                if (sum < 0) ++l;
                else if (sum > 0) --r;
                else {
                    result.push_back({nums[i], nums[l], nums[r]});
                    ++l; --r;
                    while (l < r && nums[l] == nums[l - 1]) ++l;   // skip dup second
                    while (l < r && nums[r] == nums[r + 1]) --r;   // skip dup third
                }
            }
        }
        return result;
    }
};
```

Dry run: `[-1,0,1,2,-1,-4]` sorted `[-4,-1,-1,0,1,2]`. i=1 (−1): l=2(−1), r=5(2): sum 0 → record `[-1,-1,2]`; then l=3(0), r=4(1): sum 0 → record `[-1,0,1]`. i=2 skipped (duplicate).

**H. Correctness and complexity:** Fixing the smallest element `nums[i]` and squeezing finds every pair `(l, r)` to its right with the right sum (Problem 12's argument). Duplicate skipping at the anchor and after each hit ensures each distinct triple of values is emitted once. Time O(n²) (sort is O(n log n)); auxiliary space O(1) besides sorting and output.

**I. Reusable template:** *Sort + anchor + squeeze.* Init: sort. Loop: anchor, pointers. Update: move by sum comparison; on hit record and skip duplicates. **What generalizes:** 3Sum Closest, 3Sum Smaller, 4Sum. **What changes:** target, early-exit rule (only valid when all remaining numbers move the sum in one direction), duplicate-handling.

**J. Common mistakes:** skipping duplicates *before* recording (loses `[-1,-1,2]`); forgetting to advance both pointers after a hit; `nums[i] > 0` break when target isn't 0; int overflow on sum; dedup via `set` (works but slower).

**K. Variation:** *Target `T` instead of 0.* The sorted/squeeze structure is unchanged, but the early exit `nums[i] > 0` is only valid for `T = 0`. In general, all three chosen values are ≥ `nums[i]`, so the sum is ≥ `3*nums[i]`; break when `3LL * nums[i] > T`. Assumption: sorted order tells you which pointer to move. If you need *counts* of index triples rather than unique value triples, hashing/counting of pair sums is cleaner (see Problem 20's variation).

**L. Recall questions**
- **Q13.1:** Why are duplicates skipped *after* recording a triplet, not before?
- **Q13.2:** What is the complexity for k-Sum with this approach?

---

### Problem 14: Container With Most Water

**A. Identification:** LeetCode **11** · Medium · Primary: opposite-end two pointers · Secondary: greedy proof

**B. Statement:** Given heights `h[0..n-1]`, choose two indices `i < j` maximizing `min(h[i], h[j]) * (j - i)`. Return the maximum area.

**C. Pattern recognition**
- Clues: two indices, area = width × limiting height.
- First question: "What happens to the area when I shrink the width?"
- **Rule:** *Maximize `f(i,j)` where width shrinks as pointers approach ⇒ start widest and move the shorter side.*

**D. Brute force:** All pairs: O(n²) time, O(1) space.

**E. Optimization reasoning:** Start with the widest container. Area is limited by the shorter line. If you move the *taller* pointer inward, width decreases and the limiting height can't increase (still ≤ the shorter), so area cannot improve. So move the shorter pointer; only that can possibly increase the limiting height.

**F. Algorithm**
1. `l=0, r=n-1, best=0`.
2. `best = max(best, min(h[l],h[r]) * (r-l))`.
3. Move the pointer at the smaller height inward. Repeat until `l >= r`.

**G. C++17 solution**

```cpp
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    int maxArea(vector<int>& height) {
        int l = 0, r = (int)height.size() - 1;
        long long best = 0;
        while (l < r) {
            long long area = (long long)min(height[l], height[r]) * (r - l);
            best = max(best, area);
            if (height[l] < height[r]) ++l;
            else --r;
        }
        return (int)best;
    }
};
```

Dry run: `[1,8,6,2,5,4,8,3,7]`: area(0,8)=1·8=8 → move l (1<7). Then (1,8): min(8,7)·7=49 → best 49; move r (7<8)...

**H. Correctness and complexity:** *Proof idea:* Suppose `h[l] ≤ h[r]`. Any pair `(l, j)` with `j < r` has width smaller and height ≤ `h[l]`, so area ≤ the current area; hence index `l` can be discarded safely. Time O(n); space O(1).

**I. Reusable template:** *Opposite-end greedy elimination.* Init: ends. Loop: evaluate, discard the dominated side. **What generalizes:** any objective where one endpoint is *dominated* by the other. **What changes:** the objective function and the dominance rule. Not the same as the sorted-sum squeeze because the array isn't sorted; correctness comes from the dominance argument.

**J. Common mistakes:** moving the taller pointer; using `max` instead of `min`; wrong width (`r-l+1` vs `r-l`); thinking this is "sorted array two pointers" (it isn't).

**K. Variation:** *Maximize `min(h[i],h[j])*(j-i)` with a constraint `j - i ≥ d`.* Start from `r - l = n-1` and stop when `r - l < d`; the dominance argument still holds. If areas depend on *all* heights between (like trapping water), the pair-only argument fails and boundary maxima are needed (Problem 19).

**L. Recall questions**
- **Q14.1:** Why can the taller pointer's side be left in place?
- **Q14.2:** Which condition would stop the dominance argument from working?

---

### Problem 15: Remove Duplicates from Sorted Array

**A. Identification:** LeetCode **26** · Easy · Primary: slow/fast pointers · Secondary: in-place compaction

**B. Statement:** Given a sorted array, remove duplicates **in place** so each unique element appears once; return the count `k` of unique elements. The first `k` positions must hold them in order; the rest is irrelevant.

**C. Pattern recognition**
- Clues: sorted, "in place", "return new length".
- First question: "What is the write position, and what do I compare the next candidate to?"
- **Rule:** *Sorted dedupe in place ⇒ write pointer, compare with last kept.*

**D. Brute force:** Copy unique elements into a new vector then copy back: O(n) time, O(n) space. Or erase duplicates one at a time: O(n²).

**E. Optimization reasoning:** Equal values are adjacent. Keep `write` as the next free slot; a new value is any `a[read] != a[write-1]`. Copy it into `a[write++]`.

**F. Algorithm**
1. If empty return 0.
2. `write = 1`.
3. For `read = 1..n-1`: if `a[read] != a[write-1]`: `a[write++] = a[read]`.
4. Return `write`.

**G. C++17 solution**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.empty()) return 0;
        int write = 1;
        for (int read = 1; read < (int)nums.size(); ++read)
            if (nums[read] != nums[write - 1]) nums[write++] = nums[read];
        return write;
    }
};
```

Dry run: `[0,0,1,1,2]`: write=1; read=1 (0==0) skip; read=2 (1≠0) → a[1]=1, write=2; read=3 skip; read=4 (2≠1) → a[2]=2, write=3 → `[0,1,2,...]`, return 3.

**H. Correctness and complexity:** *Invariant:* `a[0..write)` is strictly increasing and equals the distinct values of `a[0..read)`. Comparing with `a[write-1]` (last kept, not `a[read-1]`) is safe because kept values are never overwritten later. Time O(n); space O(1).

**I. Reusable template:** *Slow/fast compaction* (Part 2.7). Init: `write` at the first slot to fill. Loop: read, test, write. Termination: `read` reaches end. **What generalizes:** keep-at-most-twice (LC 80: compare with `a[write-2]`). **What changes:** the keep predicate and the lookback distance.

**J. Common mistakes:** comparing with `a[read-1]` after overwriting; starting `write=0` without special-casing; returning `write-1`; assuming the tail must be cleaned.

**K. Variation:** *Allow each value at most twice (LC 80).* Same pattern: keep `a[read]` if `write < 2 || a[read] != a[write-2]`. Assumption: sorted so duplicates are adjacent. If unsorted, use a frequency map; in-place O(1) space is no longer possible in linear time.

**L. Recall questions**
- **Q15.1:** Why compare with `nums[write-1]` rather than `nums[read-1]`?
- **Q15.2:** What single change allows each value to appear at most twice?

---

### Problem 16: Remove Element

**A. Identification:** LeetCode **27** · Easy · Primary: slow/fast pointers · Secondary: swap with end (alternative)

**B. Statement:** Remove all occurrences of `val` from `nums` in place; return the count `k` of remaining elements. The first `k` entries must be the non-`val` elements (any order is allowed by the problem).

**C. Pattern recognition**
- Clues: remove a value, in place, return length.
- First question: "Does order of the remaining elements need to be preserved?" (Here, no, but preserving is free.)
- **Rule:** *Filter in place ⇒ `compactInPlace(keep: x != val)`.*

**D. Brute force:** Use `erase` for each match: O(n²) worst case. Or copy into a new vector: O(n) space.

**E. Optimization reasoning:** Same as Problem 15 with predicate `x != val`. Alternative when matches are rare/order free: swap each match with the last element and shrink `n`: fewer writes.

**F. Algorithm**
1. `write = 0`.
2. For each `x`: if `x != val` → `nums[write++] = x`.
3. Return `write`.

**G. C++17 solution**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int write = 0;
        for (int read = 0; read < (int)nums.size(); ++read)
            if (nums[read] != val) nums[write++] = nums[read];
        return write;
    }
};
```

Dry run: `[3,2,2,3]`, val=3 → write: 2,2 → return 2, array starts `[2,2,...]`.

**H. Correctness and complexity:** *Invariant:* `nums[0..write)` = all non-`val` elements from `nums[0..read)` in order. Time O(n); space O(1).

**I. Reusable template:** `compactInPlace` (Part 2.1). **What generalizes:** any filter. **What changes:** predicate. *Swap-with-last variant:* `i=0, n=size; while i<n: if a[i]==val: a[i]=a[--n]; else ++i;` which does not preserve order and does **not** advance `i` after the swap.

**J. Common mistakes:** advancing `i` after swapping with the last element (skips the swapped-in value); returning the original size; using `erase` in a loop.

**K. Variation:** *Order of survivors must be preserved and writes should be minimized.* The read/write version preserves order; swap-with-last doesn't. Assumption for swap-with-last: order is free. For stable removal with minimum writes you only write when `read != write`.

**L. Recall questions**
- **Q16.1:** In the swap-with-last variant, why don't you advance the index after a swap?
- **Q16.2:** Which variant must you use if survivor order matters?

---

### Problem 17: Move Zeroes

**A. Identification:** LeetCode **283** · Easy · Primary: slow/fast pointers · Secondary: stable partition

**B. Statement:** Move all `0`s to the end of the array in place while keeping the relative order of non-zero elements.

**C. Pattern recognition**
- Clues: stable relative order, in place.
- First question: "Is this just compaction with a different tail rule?"
- **Rule:** *Stable partition in place ⇒ write non-zeros forward, then fill zeros (or swap).*

**D. Brute force:** Copy non-zeros then zeros into a new array: O(n) time and space. Or repeatedly shift: O(n²).

**E. Optimization reasoning:** Compact the non-zeros with `write`, then fill `[write, n)` with zeros. Single-pass swap version: swap `nums[read]` with `nums[write]` whenever `nums[read] != 0`, which also minimizes the zero-fill pass.

**F. Algorithm**
1. `write = 0`.
2. For each `read`: if `nums[read] != 0`, `swap(nums[write++], nums[read])`.

**G. C++17 solution**

```cpp
#include <vector>
#include <utility>
using namespace std;

class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int write = 0;
        for (int read = 0; read < (int)nums.size(); ++read)
            if (nums[read] != 0) swap(nums[write++], nums[read]);   // self-swap is harmless
    }
};
```

Dry run: `[0,1,0,3]`: read=1 (1): swap(0,1) → `[1,0,0,3]`, write=1; read=3 (3): swap(1,3) → `[1,3,0,0]`.

**H. Correctness and complexity:** *Invariant:* `nums[0..write)` = the non-zeros seen so far in order; `nums[write..read)` are all zeros. Swapping puts the non-zero at `write` and the zero (or itself) at `read`. Time O(n); space O(1).

**I. Reusable template:** *Stable partition by predicate (swap form).* Init: `write=0`. Loop: if predicate true → swap into the front zone. **What generalizes:** move all negatives first (stable), move odds after evens, etc. **What changes:** predicate. Partitions that must be stable on *both* sides need the swap form with two zones, or `stable_partition`.

**J. Common mistakes:** swapping when `nums[read]==0` (breaks order); not realizing that `write == read` self-swap is fine; filling zeros in an extra pass inefficiently.

**K. Variation:** *Move zeros to the front, keeping order of non-zeros.* Mirror: scan from the right with a write pointer at the end. Assumption: stable order needed for only one class; if stability is needed for neither, a two-pointer opposite-end swap is enough.

**L. Recall questions**
- **Q17.1:** Why is it OK that the swap sometimes swaps an element with itself?
- **Q17.2:** How would you move zeros to the front, keeping the order of the others?

---

### Problem 18: Squares of a Sorted Array

**A. Identification:** LeetCode **977** · Easy · Primary: opposite-end two pointers · Secondary: merge-like fill from the back

**B. Statement:** Given a **non-decreasing** integer array (may contain negatives), return the squares of each number, sorted non-decreasingly.

**C. Pattern recognition**
- Clues: sorted input but squaring breaks order; negatives make the largest squares sit at the ends.
- First question: "Where does the largest square live?" → at one of the two ends.
- **Rule:** *Sorted with negatives, transformation by |x| ⇒ compare ends, fill output from the back.*

**D. Brute force:** Square all, sort: O(n log n) time.

**E. Optimization reasoning:** The squares' maxima are at the ends of the array: `max(|a[0]|, |a[n-1]|)`. Compare the two end squares; the larger goes to the *last open* slot of the output; move that pointer inward. This is a merge of the descending square sequences of the negative and non-negative parts.

**F. Algorithm**
1. `l=0, r=n-1`, `pos=n-1`.
2. While `l ≤ r`: compare `a[l]²` and `a[r]²`; place the larger at `out[pos--]`; move its pointer.

**G. C++17 solution**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int n = (int)nums.size();
        vector<int> out(n);
        int l = 0, r = n - 1;
        for (int pos = n - 1; pos >= 0; --pos) {
            long long left = (long long)nums[l] * nums[l];
            long long right = (long long)nums[r] * nums[r];
            if (left > right) { out[pos] = (int)left; ++l; }
            else              { out[pos] = (int)right; --r; }
        }
        return out;
    }
};
```

Dry run: `[-4,-1,0,3,10]`: 16 vs 100 → out[4]=100, r=3; 16 vs 9 → out[3]=16, l=1; 1 vs 9 → out[2]=9, r=2; 1 vs 0 → out[1]=1, l=2; 0 vs 0 → out[0]=0. Result `[0,1,9,16,100]`.

**H. Correctness and complexity:** *Invariant:* the unplaced elements lie in `[l, r]` and all unplaced squares ≤ both end squares; thus the larger end square is the maximum unplaced. Filling from the back puts the maximum in its final place. Time O(n); auxiliary space O(1) besides the output.

**I. Reusable template:** *Two-end merge, fill from back.* Init: pointers + output position. Loop: pick extreme, place, shrink. **What generalizes:** "sorted by |x − c|" for any center `c`. **What changes:** the key function (square, `abs(x - c)`).

**J. Common mistakes:** filling the output from the front; comparing raw values instead of squares; `int` overflow for squares of values up to 10⁴ is fine, but be careful with larger ranges; `l < r` instead of fixed loop count.

**K. Variation:** *Apply `f(x) = ax² + bx + c` to a sorted array and return the sorted results (LC 360).* If `a > 0` fill from the back (extremes are largest); if `a < 0` fill from the front. Assumption: the transformed sequence is unimodal over the sorted order. For general `f`, sort after transform.

**L. Recall questions**
- **Q18.1:** Why can the output be filled from the back with only the end pointers?
- **Q18.2:** For `f(x) = -x²`, which end is filled first and in which direction?

---

### Problem 19: Trapping Rain Water

**A. Identification:** LeetCode **42** · Hard · Primary: boundary reasoning with two pointers · Secondary: prefix/suffix maxima

**B. Statement:** Given non-negative bar heights, compute how much water is trapped after raining. Water above bar `i` has height `min(maxLeft(i), maxRight(i)) - h[i]` when positive.

**C. Pattern recognition**
- Clues: water depends on the tallest bar to the left and right.
- First question: "What determines the water level above index `i`?"
- **Rule:** *Local value depends on global left and right maxima ⇒ prefix/suffix maxima, or two pointers on the smaller side.*

**D. Brute force:** For each `i`, scan left and right to find maxima: O(n²) time, O(1) space.

**E. Optimization reasoning:** Precompute `leftMax[i]` and `rightMax[i]` (O(n) space). Reduce to O(1) space: maintain `l, r, leftMax, rightMax`. If `h[l] < h[r]`, then the true right boundary of `l` is ≥ `h[r] > h[l]`, so the level above `l` is determined by `leftMax` alone; process `l`. Otherwise process `r` symmetrically.

**F. Algorithm**
1. `l=0, r=n-1, leftMax=rightMax=0, water=0`.
2. While `l<r`: if `h[l]<h[r]`: `leftMax=max(leftMax,h[l])`; `water += leftMax-h[l]`; `l++`. Else symmetric on `r`.

**G. C++17 solution**

```cpp
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    int trap(vector<int>& height) {
        int l = 0, r = (int)height.size() - 1;
        int leftMax = 0, rightMax = 0;
        long long water = 0;
        while (l < r) {
            if (height[l] < height[r]) {
                leftMax = max(leftMax, height[l]);
                water += leftMax - height[l];       // bounded by leftMax; right side is taller
                ++l;
            } else {
                rightMax = max(rightMax, height[r]);
                water += rightMax - height[r];
                --r;
            }
        }
        return (int)water;
    }
};
```

Dry run: `[0,1,0,2,1,0,1,3,2,1,2,1]` → total 6 (e.g., index 2 holds `1-0=1`, indices 4,5 hold `2-1`, `2-0`, etc.).

**H. Correctness and complexity:** *Invariant:* `leftMax` is the true maximum of `h[0..l]`, `rightMax` the true maximum of `h[r..n-1]`, and bars outside `[l, r]` have been accounted for. Suppose `h[l] < h[r]`. Every bar processed on the left was processed while smaller than some right bar at an index ≥ the current `r`, so `leftMax ≤` the true maximum to the right of `l`. Hence `min(maxLeft, maxRight) = leftMax` for index `l`, and the water above it is `leftMax - h[l]`. The case `h[l] ≥ h[r]` is symmetric. Time O(n); space O(1).

**I. Reusable template:** *Two-sided boundary squeeze* (Part 2.15). Init: ends, running maxima. Loop: process the smaller side. **What generalizes:** "limit by min of left and right aggregate". **What changes:** the aggregate (max) and the per-index contribution.

**J. Common mistakes:** using `<=` inconsistently (both versions work, but be consistent); updating the max after adding water (gives negatives); not realizing water above a bar can't be negative; using `int` for total in larger variants.

**K. Variation:** *Trapping Rain Water II (2D grid, LC 407).* The two-pointer argument fails because boundaries are a closed curve; use a min-heap Dijkstra-like expansion from the border. Assumption: 1D line with only two sides. Alternative 1D approach: monotonic stack.

**L. Recall questions**
- **Q19.1:** Why is `leftMax` enough to decide the water above `l` when `h[l] < h[r]`?
- **Q19.2:** What does the water formula look like with explicit prefix/suffix maxima arrays?

---

### Problem 20: 4Sum

**A. Identification:** LeetCode **18** · Medium · Primary: sort + two pointers · Secondary: duplicate skipping, overflow care

**B. Statement:** Return all unique quadruplets `[a,b,c,d]` (distinct indices) with `a+b+c+d = target`.

**C. Pattern recognition**
- Clues: k-sum, uniqueness.
- First question: "Is this 3Sum with one more fixed element?"
- **Rule:** *k-Sum ⇒ sort, fix k−2 elements with nested loops, squeeze the last two.*

**D. Brute force:** Four nested loops: O(n⁴), with dedup through sorted-tuple set.

**E. Optimization reasoning:** Fix `i < j`, then use pointers `l, r` over `(j, n)`. Sums use `long long` since four values up to 10⁹ overflow `int`. Skip duplicates at each level. Optional pruning: if the smallest possible sum for this `i` exceeds `target` (and values are... careful) break.

**F. Algorithm**
1. Sort.
2. For `i`: skip duplicates. For `j > i`: skip duplicates.
3. `l=j+1`, `r=n-1`; compare `sum` to `target`; on hit record and skip duplicates.

**G. C++17 solution**

```cpp
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        int n = (int)nums.size();
        if (n < 4) return result;
        sort(nums.begin(), nums.end());
        for (int i = 0; i < n - 3; ++i) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            for (int j = i + 1; j < n - 2; ++j) {
                if (j > i + 1 && nums[j] == nums[j - 1]) continue;
                int l = j + 1, r = n - 1;
                while (l < r) {
                    long long sum = (long long)nums[i] + nums[j] + nums[l] + nums[r];
                    if (sum < target) ++l;
                    else if (sum > target) --r;
                    else {
                        result.push_back({nums[i], nums[j], nums[l], nums[r]});
                        ++l; --r;
                        while (l < r && nums[l] == nums[l - 1]) ++l;
                        while (l < r && nums[r] == nums[r + 1]) --r;
                    }
                }
            }
        }
        return result;
    }
};
```

Dry run: `[1,0,-1,0,-2,2]`, target 0 → sorted `[-2,-1,0,0,1,2]` → `[-2,-1,1,2]`, `[-2,0,0,2]`, `[-1,0,0,1]`.

**H. Correctness and complexity:** Same argument as 3Sum with one more anchor: for fixed `(i, j)`, the squeeze finds every `(l, r)`; duplicate skipping on every level yields unique value-quadruplets. Time O(n³); auxiliary space O(1) (plus sort stack and output).

**I. Reusable template:** *k-Sum by nesting.* Init: sort. Loops: k−2 anchors with duplicate skipping; squeeze last two. **What generalizes:** k-Sum via recursion `kSum(start, k, target)`; the base case is 2-Sum squeeze. **What changes:** `k`, `target` handling; for general `k` write the recursive version.

**J. Common mistakes:** `int` overflow in the sum; dedup condition `j > 0` instead of `j > i+1`; forgetting the `n < 4` guard; skipping duplicates incorrectly after a hit.

**K. Variation:** *4Sum II (LC 454): four arrays, count tuples with sum 0.* Not sorted two-pointers: use the hash map over pair sums `A+B` and look up `-(C+D)`: O(n²) time, O(n²) space. Assumption of the sorted approach: unique value-quadruplets from one array. Counting index tuples across arrays favors hashing.

**L. Recall questions**
- **Q20.1:** Why must the sum be accumulated in `long long`?
- **Q20.2:** Which approach is used when four separate arrays are given and you count tuples?

---
## Section 3: Sliding Window

### Problem 21: Best Time to Buy and Sell Stock

**A. Identification:** LeetCode **121** · Easy · Primary: running minimum (window whose left end is the best buy day) · Secondary: Kadane-style reasoning

**B. Statement:** `prices[i]` is a stock price on day `i`. Choose one day to buy and a *later* day to sell; return the max profit, or `0` if no profit is possible.

**C. Pattern recognition**
- Clues: ordered pair `(i < j)`, maximize `prices[j] - prices[i]`.
- First question: "For a fixed sell day, which buy day is best?" → the cheapest day before it.
- **Rule:** *Maximize `a[j] - a[i]` with `i < j` ⇒ track the minimum so far.*

**D. Brute force:** All pairs `(i, j)`: O(n²) time, O(1) space.

**E. Optimization reasoning:** For sell day `j`, the best `i` is `argmin prices[0..j-1]`. Maintain `minPrice` as you scan; at each day, `profit = prices[j] - minPrice`. This equals a window `[buy, sell]` whose left pointer jumps to `j` whenever a new minimum appears.

**F. Algorithm**
1. `minPrice = +inf`, `best = 0`.
2. For each price `p`: `best = max(best, p - minPrice)`; `minPrice = min(minPrice, p)`.

**G. C++17 solution**

```cpp
#include <vector>
#include <algorithm>
#include <climits>
using namespace std;

class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int minPrice = INT_MAX, best = 0;
        for (int p : prices) {
            if (minPrice != INT_MAX) best = max(best, p - minPrice);  // sell today
            minPrice = min(minPrice, p);                              // candidate buy day
        }
        return best;
    }
};
```

Dry run: `[7,1,5,3,6,4]`: min 7; p=1 → min 1; p=5 → profit 4; p=3 → 2; p=6 → 5; p=4 → 3. Answer 5.

**H. Correctness and complexity:** *Invariant:* before day `j`, `minPrice` = min of `prices[0..j-1]`, so `p - minPrice` is the best profit selling on day `j`. The max over `j` is the global best. Time O(n); space O(1).

**I. Reusable template:** *Running extreme + best difference.* Init: neutral extreme and best. Loop: evaluate with the previous extreme, then update it. **What generalizes:** "max `a[j]-a[i]`", max drop/rise, best pair with order. **What changes:** which extreme (min/max) and the combiner.

**J. Common mistakes:** updating `minPrice` before computing profit (allows same-day buy/sell: profit 0, harmless here but conceptually wrong); returning negative profit; initializing `best` with `INT_MIN`; `INT_MAX - ` overflow when subtracting from `INT_MAX`.

**K. Variation:** *Unlimited transactions (LC 122).* Sliding-window thinking changes to summing every positive consecutive difference. Assumption of this version: exactly one buy/sell. With at most `k` transactions, you need DP (LC 188).

**L. Recall questions**
- **Q21.1:** Why does profit use the minimum over *earlier* days only?
- **Q21.2:** How does the solution change for unlimited transactions?

---

### Problem 22: Maximum Average Subarray I

**A. Identification:** LeetCode **643** · Easy · Primary: fixed-size sliding window · Secondary: prefix sums

**B. Statement:** Given `nums` and an integer `k`, find the contiguous subarray of length exactly `k` with the maximum average; return that average.

**C. Pattern recognition**
- Clues: length *exactly* `k`, contiguous.
- First question: "Maximizing the average = maximizing what, since `k` is fixed?" → the sum.
- **Rule:** *Fixed length `k` ⇒ slide, add new, subtract old.*

**D. Brute force:** Recompute each window sum: O(nk).

**E. Optimization reasoning:** Consecutive windows share `k−1` elements. Moving one step adds `nums[i]` and removes `nums[i-k]`. Track the max sum and divide by `k` at the end (avoid floating-point accumulation). Use `long long`.

**F. Algorithm**
1. Sum the first `k`; `best = sum`.
2. For `i = k..n-1`: `sum += nums[i] - nums[i-k]`; `best = max(best, sum)`.
3. Return `best / k` as `double`.

**G. C++17 solution**

```cpp
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        long long window = 0;
        for (int i = 0; i < k; ++i) window += nums[i];
        long long best = window;
        for (int i = k; i < (int)nums.size(); ++i) {
            window += nums[i] - nums[i - k];     // add entering, drop leaving
            best = max(best, window);
        }
        return static_cast<double>(best) / k;
    }
};
```

Dry run: `[1,12,-5,-6,50,3]`, k=4: sums 2, 51, 42 → best 51 → 12.75.

**H. Correctness and complexity:** *Invariant:* after processing index `i ≥ k-1`, `window = sum(nums[i-k+1..i])`. Every length-`k` window is examined once. Time O(n); space O(1). Requires `1 ≤ k ≤ n`.

**I. Reusable template:** *Fixed window* (Part 2.8). Init: first window. Loop: add right, remove `i-k`. **What generalizes:** any invertible aggregate (sum, count, frequency). **What changes:** aggregate and its add/remove operations.

**J. Common mistakes:** dividing early as integers; `int` overflow; accumulating averages as doubles (precision); starting the loop at `i = 0` without handling the first window.

**K. Variation:** *Maximum average over windows of length ≥ k (LC 644).* Fixed window no longer suffices (length varies); binary search on the answer + prefix sums. Assumption: window length fixed. If you need window max/min (not invertible), use a monotonic deque.

**L. Recall questions**
- **Q22.1:** Why is it enough to compare sums rather than averages?
- **Q22.2:** Which aggregate prevents the "add new, remove old" update?

---

### Problem 23: Maximum Number of Vowels in a Substring of Given Length

**A. Identification:** LeetCode **1456** · Medium · Primary: fixed-size sliding window · Secondary: counting

**B. Statement:** For string `s` (lowercase letters) and integer `k`, return the maximum number of vowels in any substring of length `k`.

**C. Pattern recognition**
- Clues: "substring of length k", count of a property.
- First question: "What do I add/remove when the window moves?"
- **Rule:** *Property count over fixed-length windows ⇒ fixed window with an integer counter.*

**D. Brute force:** Count vowels in each window: O(nk).

**E. Optimization reasoning:** Maintain `count` of vowels in the current window. When sliding: `+1` if the entering char is a vowel, `-1` if the leaving char is a vowel. Early exit when `count == k`.

**F. Algorithm**
1. Count vowels in the first `k` chars; `best = count`.
2. For `i=k..n-1`: update with `s[i]` and `s[i-k]`; update `best`.

**G. C++17 solution**

```cpp
#include <string>
#include <algorithm>
using namespace std;

class Solution {
    static bool isVowel(char c) {
        return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u';
    }
public:
    int maxVowels(string s, int k) {
        int count = 0;
        for (int i = 0; i < k; ++i) count += isVowel(s[i]);
        int best = count;
        for (int i = k; i < (int)s.size() && best < k; ++i) {
            count += isVowel(s[i]) - isVowel(s[i - k]);
            best = max(best, count);
        }
        return best;
    }
};
```

Dry run: `"abciiidef"`, k=3: "abc"→1; "bci"→1; "cii"→2; "iii"→3 → best 3 (early exit).

**H. Correctness and complexity:** *Invariant:* `count` = vowels in `s[i-k+1..i]`. Time O(n); space O(1).

**I. Reusable template:** *Fixed window with a predicate counter.* Init: first window count. Update: `count += pred(in) - pred(out)`. **What generalizes:** max 1s in a binary window, windows with at most X bad characters. **What changes:** predicate.

**J. Common mistakes:** mismatched window length (using `i-k+1`); counting uppercase if present; forgetting `k ≤ n`.

**K. Variation:** *Longest substring with at most `m` consonants (variable length).* Becomes a variable window (Problem 29 pattern). Assumption: the length is fixed so the shrink step is mechanical.

**L. Recall questions**
- **Q23.1:** What invariant does `count` maintain?
- **Q23.2:** How does the pattern change when length is not fixed but consonants ≤ m?

---

### Problem 24: Longest Substring Without Repeating Characters

**A. Identification:** LeetCode **3** · Medium · Primary: variable sliding window · Secondary: last-seen index map

**B. Statement:** Find the length of the longest substring of `s` with all distinct characters.

**C. Pattern recognition**
- Clues: longest contiguous substring with a validity property (no repeats).
- Validity is monotone: shrinking a valid window stays valid.
- First question: "When a duplicate enters, where must `left` move?"
- **Rule:** *Longest substring with uniqueness ⇒ window + last-seen index (jump) or counts (shrink).*

**D. Brute force:** Check all substrings with a set: O(n²) to O(n³) time.

**E. Optimization reasoning:** Keep `last[c]` = last index of character `c`. When `s[right]` was seen at index ≥ `left`, move `left = last[c] + 1` in one jump. The window `[left, right]` always has distinct characters. `max(left, ...)` is required because `last[c]` may point left of the window.

**F. Algorithm**
1. `last[256] = -1`, `left = 0`, `best = 0`.
2. For `right`: `left = max(left, last[c]+1)`; `last[c] = right`; `best = max(best, right-left+1)`.

**G. C++17 solution**

```cpp
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        vector<int> last(256, -1);               // last index of each byte value
        int left = 0, best = 0;
        for (int right = 0; right < (int)s.size(); ++right) {
            unsigned char c = s[right];
            left = max(left, last[c] + 1);       // jump past the previous occurrence if inside window
            last[c] = right;
            best = max(best, right - left + 1);
        }
        return best;
    }
};
```

Dry run: `"abba"`: r=0 → len1; r=1 → len2; r=2 (b, last=1) → left=2, len1; r=3 (a, last=0) → `max(2,1)=2`, len2. Answer 2.

**H. Correctness and complexity:** *Invariant:* `s[left..right]` has distinct characters, and `left` is the smallest such index. Without the `max`, `left` could move backwards (bug on `"abba"`). Time O(n); space O(Σ) = O(1) for 256 bytes.

**I. Reusable template:** *Variable window with jump* (Part 2.9 variant). Init: `left=0`. Loop: expand, adjust `left`, record. **What generalizes:** "longest with all distinct", "longest with unique elements array" (map instead of vector). **What changes:** the jump rule vs. a `while` shrink with a count map.

**J. Common mistakes:** forgetting `max(left, ...)`; using `r-l` instead of `r-l+1`; using a set but not removing in the right order; assuming only lowercase.

**K. Variation:** *At most `K` distinct characters (LC 340, premium).* The last-seen jump no longer works; use a count map and shrink while distinct > K. Assumption: validity depends on single-character repetition. "At most K distinct" is still a monotone predicate, so a variable window applies with counts.

**L. Recall questions**
- **Q24.1:** Why is `left = max(left, last[c] + 1)` needed instead of `left = last[c] + 1`?
- **Q24.2:** What data structure replaces `last[]` for "at most K distinct"?

---

### Problem 25: Longest Repeating Character Replacement

**A. Identification:** LeetCode **424** · Medium · Primary: frequency-map sliding window · Secondary: non-shrinking window trick

**B. Statement:** Given an uppercase string `s` and integer `k`, you may change at most `k` characters to any other uppercase letter. Return the length of the longest substring of identical letters you can obtain.

**C. Pattern recognition**
- Clues: "at most k changes", longest substring.
- First question: "For a window, how many replacements are needed?" → `windowLen − maxFreq`.
- **Rule:** *Window valid iff `len - maxFreq ≤ k`*.

**D. Brute force:** For each substring, compute max frequency: O(n²·26) or O(n²).

**E. Optimization reasoning:** The window is valid when `(right-left+1) - maxFreqInWindow ≤ k`. Expand `right`, increment its count, update `maxFreq`. If invalid, shrink by one (`left++`). **Key subtlety:** `maxFreq` is never decreased when shrinking. A stale (too large) `maxFreq` can only make the window look *valid* when it's not, but the window size then doesn't grow beyond a previously achieved valid size, so the *answer* is still correct because answer grows only when `maxFreq` truly rises.

**F. Algorithm**
1. `cnt[26]`, `left=0`, `maxFreq=0`, `best=0`.
2. For `right`: `maxFreq = max(maxFreq, ++cnt[s[right]-'A'])`.
3. If `(right-left+1) - maxFreq > k`: `--cnt[s[left]-'A']`, `left++`.
4. `best = max(best, right-left+1)`.

**G. C++17 solution**

```cpp
#include <string>
#include <array>
#include <algorithm>
using namespace std;

class Solution {
public:
    int characterReplacement(string s, int k) {
        array<int, 26> cnt{};
        int left = 0, maxFreq = 0, best = 0;
        for (int right = 0; right < (int)s.size(); ++right) {
            maxFreq = max(maxFreq, ++cnt[s[right] - 'A']);
            if ((right - left + 1) - maxFreq > k) {     // needs more than k replacements
                --cnt[s[left] - 'A'];
                ++left;                                  // slide: window length stays the same
            }
            best = max(best, right - left + 1);
        }
        return best;
    }
};
```

Dry run: `"AABABBA"`, k=1: windows grow to length 4 (`AABA` → needs 1). When `right` extends to a state needing 2, `left` slides by one, keeping length 4; best = 4.

**H. Correctness and complexity:** *Idea:* `best` only increases when a window of larger size is valid, which requires a true `maxFreq` ≥ `size − k`. A stale `maxFreq` can't create a larger *valid* window because `maxFreq` only increases when a count really reaches that value in some window. Time O(n); space O(26).

**I. Reusable template:** *Frequency-map window with dominant count.* Init: counts, `left`, `maxFreq`. Loop: add, check `len - maxFreq > k`, slide/shrink. **What generalizes:** "make window uniform with ≤ k edits", binary variants (Max Consecutive Ones III). **What changes:** the "cost" formula (`len - maxFreq`), alphabet.

**J. Common mistakes:** recomputing `maxFreq` each shrink (works but O(26n)); decrementing `maxFreq` incorrectly; using `while` and stale `maxFreq` confusion; not understanding why the answer stays correct.

**K. Variation:** *Lowercase + uppercase or Unicode.* Replace the array with a map; the stale-`maxFreq` argument still holds. If cost isn't `len - maxFreq` (e.g., weighted changes), the dominant-count trick fails and you need a different cost model.

**L. Recall questions**
- **Q25.1:** What is the replacement cost of a window of length `L` with dominant frequency `F`?
- **Q25.2:** Why doesn't a stale `maxFreq` produce a wrong final answer?

---

### Problem 26: Permutation in String

**A. Identification:** LeetCode **567** · Medium · Primary: fixed-size window with frequency compare · Secondary: matching-count optimization

**B. Statement:** Return `true` if `s2` contains a substring that is a permutation of `s1` (lowercase letters).

**C. Pattern recognition**
- Clues: "permutation of s1 appears as a substring" = a window of length `|s1|` with identical counts.
- First question: "What is the fixed window length?"
- **Rule:** *Substring is a permutation/anagram ⇒ fixed window of `|p|` + count comparison.*

**D. Brute force:** For each substring of length `|s1|`, sort and compare: O(n·m log m).

**E. Optimization reasoning:** The window length is fixed at `m = |s1|`. Maintain `windowCount[26]`; compare with `need[26]` (O(26) per step), or keep a `matches` counter. We use direct array equality: O(26n), simple and safe.

**F. Algorithm**
1. If `|s1| > |s2|` return false.
2. Build `need` from `s1`, `window` from first `m` chars of `s2`.
3. If equal → true. Slide: add `s2[i]`, remove `s2[i-m]`, compare.

**G. C++17 solution**

```cpp
#include <string>
#include <array>
using namespace std;

class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        int m = (int)s1.size(), n = (int)s2.size();
        if (m > n) return false;
        array<int, 26> need{}, window{};
        for (int i = 0; i < m; ++i) {
            ++need[s1[i] - 'a'];
            ++window[s2[i] - 'a'];
        }
        if (need == window) return true;
        for (int i = m; i < n; ++i) {
            ++window[s2[i] - 'a'];
            --window[s2[i - m] - 'a'];
            if (need == window) return true;
        }
        return false;
    }
};
```

Dry run: `s1="ab"`, `s2="eidbaooo"`: windows `ei, id, db, ba` → `ba` has counts `a:1,b:1` equals need → true.

**H. Correctness and complexity:** Two strings are permutations ⇔ equal counts. Every window of length `m` is tested. Time O(26·n) = O(n); space O(26).

**I. Reusable template:** *Fixed window + count equality.* Init: need/window arrays. Update: add/remove. Check: equality (or a `matches` counter for O(1)). **What generalizes:** Problem 27, any anagram-window search. **What changes:** what you do on a match (return vs collect indices).

**J. Common mistakes:** variable window instead of fixed; not handling `m > n`; remembering to remove `s2[i-m]`; assuming only lowercase.

**K. Variation:** *Large alphabet (Unicode).* Array equality over a big alphabet is slow; keep `matches` (number of letters whose counts agree) updated in O(1) per slide. Assumption: small fixed alphabet makes O(26) comparison negligible.

**L. Recall questions**
- **Q26.1:** Why is the window length fixed here but variable in Minimum Window Substring?
- **Q26.2:** How do you avoid the O(26) comparison per step?

---

### Problem 27: Find All Anagrams in a String

**A. Identification:** LeetCode **438** · Medium · Primary: fixed-size window with frequency compare · Secondary: result collection

**B. Statement:** Given strings `s` and `p`, return all start indices of substrings of `s` that are anagrams of `p`.

**C. Pattern recognition**
- Clues: same as Problem 26 but *all* positions.
- First question: "Is this Permutation in String with a vector of results?" Yes.
- **Rule:** *Find all anagram windows ⇒ same fixed window, push the start index on each match.*

**D. Brute force:** Sort/compare each substring: O(n·m log m).

**E. Optimization reasoning:** Identical to Problem 26. When `window == need` after processing index `i`, the window starts at `i - m + 1`. Overlapping matches are allowed.

**F. Algorithm**
1. If `|p| > |s|` return `{}`.
2. Build `need`, `window` for the first `m` chars; test.
3. For `i = m..n-1`: slide and test; push `i-m+1` on match.

**G. C++17 solution**

```cpp
#include <string>
#include <vector>
#include <array>
using namespace std;

class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        int n = (int)s.size(), m = (int)p.size();
        vector<int> result;
        if (m > n) return result;
        array<int, 26> need{}, window{};
        for (int i = 0; i < m; ++i) {
            ++need[p[i] - 'a'];
            ++window[s[i] - 'a'];
        }
        if (need == window) result.push_back(0);
        for (int i = m; i < n; ++i) {
            ++window[s[i] - 'a'];
            --window[s[i - m] - 'a'];
            if (need == window) result.push_back(i - m + 1);
        }
        return result;
    }
};
```

Dry run: `s="cbaebabacd"`, `p="abc"` → matches at 0 (`cba`) and 6 (`bac`) → `[0,6]`.

**H. Correctness and complexity:** Every length-`m` window is evaluated; equality of counts ⇔ anagram. Time O(26n); space O(26) + output.

**I. Reusable template:** same as Problem 26 (fixed window + count equality). **What changes:** return collected indices instead of a boolean.

**J. Common mistakes:** pushing `i-m` instead of `i-m+1`; skipping the first window; early `break` after the first match.

**K. Variation:** *Case-sensitive/Unicode or "at most one differing character".* The array comparison becomes a difference counter (number of mismatching letters) updated in O(1). Assumption: exact equality of multisets; relaxing it needs a mismatch counter.

**L. Recall questions**
- **Q27.1:** Which index do you push when the window ending at `i` matches?
- **Q27.2:** How would you allow a match with at most one differing character?

---

### Problem 28: Minimum Size Subarray Sum

**A. Identification:** LeetCode **209** · Medium · Primary: variable sliding window (shortest) · Secondary: prefix sums + binary search

**B. Statement:** Given an array of **positive** integers and `target`, return the minimal length of a contiguous subarray with sum ≥ `target`, or `0` if none.

**C. Pattern recognition**
- Clues: "minimal length", "sum ≥ target", **positive** integers.
- Positive values make the window sum monotone in both directions.
- First question: "Do negatives exist?" (No. This is why the window works.)
- **Rule:** *Shortest subarray with sum ≥ T, non-negative values ⇒ expand right, shrink left while still valid.*

**D. Brute force:** All subarrays: O(n²) (O(n³) without running sums).

**E. Optimization reasoning:** Expand `right` accumulating `sum`. Once `sum ≥ target`, try to shrink from the left as long as it stays valid, recording the minimal length at each valid state. Because values are positive, removing from the left strictly decreases the sum and adding on the right strictly increases it, so `left` never needs to move backward.

**F. Algorithm**
1. `left=0, sum=0, best=INF`.
2. For each `right`: `sum += a[right]`.
3. While `sum ≥ target`: `best = min(best, right-left+1)`; `sum -= a[left++]`.
4. Return `best == INF ? 0 : best`.

**G. C++17 solution**

```cpp
#include <vector>
#include <algorithm>
#include <climits>
using namespace std;

class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int left = 0, best = INT_MAX;
        long long sum = 0;
        for (int right = 0; right < (int)nums.size(); ++right) {
            sum += nums[right];
            while (sum >= target) {                      // valid: try to shorten
                best = min(best, right - left + 1);
                sum -= nums[left++];
            }
        }
        return best == INT_MAX ? 0 : best;
    }
};
```

Dry run: target 7, `[2,3,1,2,4,3]`: right=3 sum=8 → len 4, shrink → sum 6; right=4 sum=10 → len 4 → shrink to 7 → len 3 → shrink to 4; right=5 sum=7 → len 2 → answer 2.

**H. Correctness and complexity:** *Invariant:* after the `while`, `[left, right]` has sum < target, so no window ending at `right` and starting at `left` or later is valid; windows starting before `left` that ended at `right` were recorded while the loop shrank. Positivity gives monotonicity: a valid window can only be shortened from the left while it stays valid, and `left` never needs to move backwards. Time O(n) (each index enters and leaves once); space O(1).

**I. Reusable template:** *Shortest valid window* (Part 2.9). Init: `left=0`. Loop: add, `while valid` record+shrink. **What generalizes:** shortest window satisfying a monotone "enough" predicate. **What changes:** state and validity (sum ≥ T, covers all chars, etc.).

**J. Common mistakes:** using `if` instead of `while`; applying this with negatives; recording length after shrinking; returning `INT_MAX`.

**K. Variation:** *Array may contain negatives (LC 862).* Window fails because adding an element may *decrease* the sum, breaking monotonicity. Use prefix sums + a monotonic deque of indices (O(n)). Assumption making the original valid: all values are positive (or non-negative). Alternative for the original: prefix sums + binary search, O(n log n).

**L. Recall questions**
- **Q28.1:** Which property of the input makes `left` safe to move only forward?
- **Q28.2:** What fails for negatives and what replaces the window?

---

### Problem 29: Max Consecutive Ones III

**A. Identification:** LeetCode **1004** · Medium · Primary: variable sliding window (longest) · Secondary: "at most k bad elements"

**B. Statement:** Given binary array `nums` and `k`, return the longest subarray containing only `1`s after flipping at most `k` zeros.

**C. Pattern recognition**
- Clues: "flip at most k zeros" ⇒ "longest window with at most k zeros".
- First question: "Rephrase the flip operation as a constraint on the window."
- **Rule:** *"Allowed k modifications" ⇒ longest window with ≤ k violations.*

**D. Brute force:** For every subarray count zeros: O(n²).

**E. Optimization reasoning:** A window is valid iff it contains at most `k` zeros. Valid-ness is monotone under shrinking. Expand `right`; if `a[right]==0`, `zeros++`. While `zeros > k`, shrink from the left (`zeros--` when removing a zero). Window length is the candidate answer.

**F. Algorithm**
1. `left=0, zeros=0, best=0`.
2. For `right`: if zero → `zeros++`.
3. While `zeros > k`: if `a[left]==0` → `zeros--`; `left++`.
4. `best = max(best, right-left+1)`.

**G. C++17 solution**

```cpp
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    int longestOnes(vector<int>& nums, int k) {
        int left = 0, zeros = 0, best = 0;
        for (int right = 0; right < (int)nums.size(); ++right) {
            if (nums[right] == 0) ++zeros;
            while (zeros > k) {
                if (nums[left] == 0) --zeros;
                ++left;
            }
            best = max(best, right - left + 1);
        }
        return best;
    }
};
```

Dry run: `[1,1,1,0,0,0,1,1,1,1,0]`, k=2: the window at indices 5..10 is `[0,1,1,1,1,0]` (two zeros) → best = 6.

**H. Correctness and complexity:** *Invariant:* after the `while`, `[left, right]` is the longest valid window ending at `right`. Time O(n); space O(1).

**I. Reusable template:** *Longest window with ≤ k violations.* Init: `left=0`, violation count. Loop: add, shrink while violations > k, record. **What generalizes:** Problem 25 (violations = `len - maxFreq`), longest substring with ≤ k distinct. **What changes:** the violation measure.

**J. Common mistakes:** shrinking only once (use `while`); not decrementing when removing a zero; resetting the window on `0`.

**K. Variation:** *Binary array, delete exactly one element and find the longest all-1 subarray (LC 1493).* Same pattern with `k=1` and result `len - 1`. Assumption: "violations" are countable incrementally. If violations depend on the *relative arrangement* (not a count), a plain counter can't track it.

**L. Recall questions**
- **Q29.1:** How does flipping reduce to a window constraint?
- **Q29.2:** What is the answer shape for LC 1493 using this pattern?

---

### Problem 30: Minimum Window Substring

**A. Identification:** LeetCode **76** · Hard · Primary: frequency-map sliding window · Secondary: shortest valid window

**B. Statement:** Given `s` and `t`, return the smallest substring of `s` that contains every character of `t` with multiplicity; return `""` if none exists.

**C. Pattern recognition**
- Clues: "minimum window containing all", multiplicities.
- Validity is monotone: a superset window is also valid.
- First question: "How can I know the window is valid in O(1)?" → `missing` counter.
- **Rule:** *Shortest window covering a multiset ⇒ expand until covered, then shrink while still covered.*

**D. Brute force:** For every substring check coverage: O(n²·Σ) or worse.

**E. Optimization reasoning:** Track `need[c]` (required minus current count): positive means still lacking. `missing` = total number of required characters not yet covered. Adding `s[right]`: if `need[c] > 0` before decrement, `missing--`. When `missing == 0`, the window covers `t`; record and shrink: removing `s[left]` increments `need`; if it becomes positive, `missing++` (window invalid again).

**F. Algorithm**
1. Edge cases: empty `t` or `|s| < |t|` → `""`.
2. Fill `need` from `t`; `missing = |t|`.
3. For each `right`: update `need`, `missing`.
4. While `missing == 0`: record best; shrink from left updating `need`, `missing`.

**G. C++17 solution**

```cpp
#include <string>
#include <vector>
#include <climits>
using namespace std;

class Solution {
public:
    string minWindow(string s, string t) {
        if (t.empty() || s.size() < t.size()) return "";
        vector<int> need(256, 0);
        for (unsigned char c : t) ++need[c];
        int missing = (int)t.size();
        int bestLen = INT_MAX, bestStart = 0, left = 0;
        for (int right = 0; right < (int)s.size(); ++right) {
            if (need[(unsigned char)s[right]]-- > 0) --missing;   // was still required
            while (missing == 0) {
                if (right - left + 1 < bestLen) { bestLen = right - left + 1; bestStart = left; }
                if (++need[(unsigned char)s[left]] > 0) ++missing; // now required again
                ++left;
            }
        }
        return bestLen == INT_MAX ? "" : s.substr(bestStart, bestLen);
    }
};
```

Dry run: `s="ADOBECODEBANC"`, `t="ABC"`: first cover at `ADOBEC` (len 6). Shrinking and moving finds `CODEBA` (6), then `BANC` (4) → answer `"BANC"`.

**H. Correctness and complexity:** *Invariant:* `need[c] > 0` ⇔ the window lacks copies of `c`; `missing` = Σ positive `need`. For each `right`, the `while` considers the shortest valid window ending at `right` (shrinking until invalid). Every valid window is dominated by one of those. Time O(|s| + |t|); space O(256) = O(1).

**I. Reusable template:** *Coverage window* (Part 2.10). Init: `need`, `missing`. Loop: add right; `while missing == 0` record and remove left. **What generalizes:** Substring with Concatenation, Smallest Range, shortest window with all K types. **What changes:** the key type and validity counter.

**J. Common mistakes:** ignoring multiplicities of `t`; updating `missing` on every character instead of only needed ones; checking `need` against 0 incorrectly (use the "was positive" test); not storing best start/length.

**K. Variation:** *Windows must contain `t` as a subsequence (LC 727, premium).* Counting windows is no longer valid because order matters; use DP or a two-pointer forward/backward scan. Assumption: coverage depends only on multiset counts. If characters can be Unicode, switch to a map.

**L. Recall questions**
- **Q30.1:** What do `need[c] > 0` and `missing` mean?
- **Q30.2:** Why does the counting window fail for "t as a subsequence"?

---
## Section 4: Prefix Sums and Subarray Problems

### Problem 31: Range Sum Query: Immutable

**A. Identification:** LeetCode **303** · Easy · Primary: prefix sums · Secondary: class design (preprocessing + queries)

**B. Statement:** Design `NumArray(nums)` and `sumRange(left, right)` returning the sum of `nums[left..right]` (inclusive). Many queries are expected; the array never changes.

**C. Pattern recognition**
- Clues: many range-sum queries on a static array.
- First question: "Can I pay O(n) once so every query is O(1)?"
- **Rule:** *Static array + range sums ⇒ prefix array with a leading zero.*

**D. Brute force:** Sum the range for each query: O(n) per query, O(nq) total.

**E. Optimization reasoning:** `P[i]` = sum of the first `i` elements, `P[0] = 0`. Then `sum(l..r) = P[r+1] - P[l]`. The leading zero removes special cases for `l = 0`.

**F. Algorithm**
1. Constructor: `P[0]=0`; `P[i+1]=P[i]+nums[i]`.
2. Query: return `P[r+1] - P[l]`.

**G. C++17 solution**

```cpp
#include <vector>
using namespace std;

class NumArray {
    vector<int> prefix;                       // prefix[i] = sum of nums[0..i-1]
public:
    NumArray(vector<int>& nums) : prefix(nums.size() + 1, 0) {
        for (size_t i = 0; i < nums.size(); ++i) prefix[i + 1] = prefix[i] + nums[i];
    }
    int sumRange(int left, int right) {
        return prefix[right + 1] - prefix[left];
    }
};
```

Dry run: `[-2,0,3,-5,2,-1]` → `P=[0,-2,-2,1,-4,-2,-3]`; `sumRange(0,2) = P[3]-P[0] = 1`; `sumRange(2,5) = P[6]-P[2] = -1`.

**H. Correctness and complexity:** `P[r+1] - P[l] = (a[0..r]) − (a[0..l-1]) = a[l..r]`. Build O(n); query O(1); space O(n). (Under the problem's constraints sums fit in `int`; use `long long` if they might not.)

**I. Reusable template:** *Prefix array* (Part 2.11). Init: `P[0]=0`. Loop: accumulate. Query: difference. **What generalizes:** 2D prefix sums, prefix XOR, prefix counts. **What changes:** operation (must be invertible: sum, xor, count).

**J. Common mistakes:** missing the leading zero; off-by-one (`P[r]-P[l]`); using the array for point updates (needs Fenwick tree); overflow.

**K. Variation:** *Updates between queries (LC 307).* Prefix sums break (O(n) per update). Use Fenwick/segment tree for O(log n) both. Assumption: array immutable.

**L. Recall questions**
- **Q31.1:** Why is the prefix array of length `n+1`?
- **Q31.2:** What replaces prefix sums when point updates are interleaved with queries?

---

### Problem 32: Find Pivot Index

**A. Identification:** LeetCode **724** · Easy · Primary: prefix sums (running left sum) · Secondary: total-minus-left reasoning

**B. Statement:** Return the leftmost index where the sum of elements strictly left equals the sum strictly right (empty side = 0). Return `-1` if none.

**C. Pattern recognition**
- Clues: "left sum = right sum" around an index.
- First question: "Can the right sum be derived from the total and the left sum?"
- **Rule:** *Split condition ⇒ `left == total − left − a[i]`.*

**D. Brute force:** For each `i` compute both sides: O(n²).

**E. Optimization reasoning:** Compute `total`. Scan with running `leftSum`. At index `i`, `rightSum = total - leftSum - nums[i]`. Compare; then `leftSum += nums[i]`. Return the first match, which is the leftmost.

**F. Algorithm**
1. `total = sum(nums)`, `left = 0`.
2. For each `i`: if `left == total - left - nums[i]` return `i`; `left += nums[i]`.
3. Return `-1`.

**G. C++17 solution**

```cpp
#include <vector>
#include <numeric>
using namespace std;

class Solution {
public:
    int pivotIndex(vector<int>& nums) {
        long long total = accumulate(nums.begin(), nums.end(), 0LL);
        long long leftSum = 0;
        for (int i = 0; i < (int)nums.size(); ++i) {
            if (leftSum == total - leftSum - nums[i]) return i;
            leftSum += nums[i];
        }
        return -1;
    }
};
```

Dry run: `[1,7,3,6,5,6]`, total 28: i=3: left=11, right=28−11−6=11 → pivot 3.

**H. Correctness and complexity:** At index `i`, `leftSum = sum(a[0..i-1])` and `total - leftSum - a[i] = sum(a[i+1..])`. Time O(n), space O(1).

**I. Reusable template:** *Running prefix + total.* Init: total, `left=0`. Loop: compare, then update. **What generalizes:** Number of Ways to Split Array (Problem 39), equilibrium index. **What changes:** the comparison condition.

**J. Common mistakes:** updating `leftSum` before comparing; assuming the pivot is excluded from both sides incorrectly; returning an arbitrary match when leftmost is required; missing the edge case where index 0 (left sum 0) is the pivot.

**K. Variation:** *Find the index where left sum equals *twice* right sum.* Same structure with changed condition `left == 2 * right`. Assumption: only an O(1) check per index. With negatives it still works (equality, not monotonic structure).

**L. Recall questions**
- **Q32.1:** Express `rightSum` at index `i` without a second pass.
- **Q32.2:** Does the algorithm still work when elements are negative?

---

### Problem 33: Running Sum of 1d Array

**A. Identification:** LeetCode **1480** · Easy · Primary: prefix sums · Secondary: in-place transform

**B. Statement:** Return `runningSum` where `runningSum[i] = nums[0] + ... + nums[i]`.

**C. Pattern recognition**
- Clue: the answer *is* the inclusive prefix sum.
- First question: "Can I overwrite the input safely?" (If allowed, yes: each cell only needs the previous cell.)
- **Rule:** *Cumulative total ⇒ `a[i] += a[i-1]`.*

**D. Brute force:** For each `i` sum `0..i`: O(n²).

**E. Optimization reasoning:** `rs[i] = rs[i-1] + nums[i]`. Do it in place for O(1) auxiliary space (or into a new vector if the input must be preserved).

**F. Algorithm**
1. For `i = 1..n-1`: `nums[i] += nums[i-1]`.
2. Return `nums`.

**G. C++17 solution**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    vector<int> runningSum(vector<int>& nums) {
        for (size_t i = 1; i < nums.size(); ++i) nums[i] += nums[i - 1];
        return nums;
    }
};
```

Dry run: `[1,2,3,4]` → `[1,3,6,10]`.

**H. Correctness and complexity:** *Invariant:* before step `i`, `nums[i-1]` holds `a[0..i-1]` total. Time O(n); auxiliary space O(1) (returning by value copies; the algorithm itself uses none).

**I. Reusable template:** *Inclusive prefix in place.* Update: `a[i] = a[i-1] ⊕ a[i]`. **What generalizes:** prefix max/min, prefix XOR (`std::partial_sum` does this). **What changes:** the operator. **Note:** the inclusive prefix here differs from the exclusive form (`P[i]` excludes `a[i]`) used in Problem 31.

**J. Common mistakes:** starting at 0 (`nums[-1]`); using the original after overwriting; overflow for large inputs (not here).

**K. Variation:** *Return running sum modulo `m`.* Apply `% m` at each step; still O(n). Assumption: `+` is associative and has no hidden dependency on future values. If the operation isn't invertible, you can still compute the running aggregate, but you *cannot* answer arbitrary range queries from it.

**L. Recall questions**
- **Q33.1:** What's the difference between inclusive and exclusive prefix sums?
- **Q33.2:** Which standard-library function computes this?

---

### Problem 34: Subarray Sum Equals K

**A. Identification:** LeetCode **560** · Medium · Primary: prefix sum + frequency hash map · Secondary: complement lookup

**B. Statement:** Count contiguous subarrays whose sum equals `k`. Values may be **negative or zero**.

**C. Pattern recognition**
- Clues: "count subarrays", "sum equals k", negatives allowed ⇒ sliding window is invalid.
- First question: "Subarray sum is a difference of two prefix sums; what pair am I looking for?"
- **Rule:** *Count subarrays with sum K, any signs ⇒ prefix + map of prefix frequencies.*

**D. Brute force:** For each start, extend with a running sum: O(n²) time, O(1) space.

**E. Optimization reasoning:** `sum(l..r) = P[r+1] - P[l] = k` ⇒ `P[l] = P[r+1] - k`. While scanning, count how many earlier prefix values equal `current - k`. Seed the map with `{0: 1}` for the empty prefix (so subarrays starting at index 0 count). Query *before* inserting the current prefix (k = 0 would otherwise count an empty subarray).

**F. Algorithm**
1. `freq = {0:1}`, `pre = 0`, `ans = 0`.
2. For each `x`: `pre += x`; `ans += freq[pre - k]`; `freq[pre]++`.

**G. C++17 solution**

```cpp
#include <vector>
#include <unordered_map>
using namespace std;

class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<long long, int> freq;
        freq[0] = 1;                               // empty prefix
        long long pre = 0;
        int count = 0;
        for (int x : nums) {
            pre += x;
            auto it = freq.find(pre - k);
            if (it != freq.end()) count += it->second;
            ++freq[pre];
        }
        return count;
    }
};
```

Dry run: `[1,1,1]`, k=2: pre=1 (need −1: 0); pre=2 (need 0: 1) → count 1; pre=3 (need 1: 1) → count 2.

**H. Correctness and complexity:** *Invariant:* before processing index `i`, `freq` counts every prefix `P[0..i]` (empty + first `i` elements). Each earlier prefix `P[l]` equal to `pre - k` corresponds to exactly one subarray `l..i` summing to `k`. Time O(n) expected; space O(n).

**I. Reusable template:** *Prefix frequency map* (Part 2.12). Init: `{0:1}`. Loop: update prefix, query `pre - target`, then insert. **What generalizes:** any "count subarrays with property determined by prefix equality". **What changes:** the key (prefix sum, balance, remainder) and the lookup target.

**J. Common mistakes:** using a sliding window with negatives; forgetting `{0:1}`; inserting before querying; using `int` for prefix; thinking `freq` stores indices (it stores counts here).

**K. Variation:** *Only positive numbers.* Sliding window becomes valid (sum is monotone) and removes the hash map, O(1) space. Assumption of the prefix/hash approach: arbitrary signs. Also: *count subarrays with sum in `[L, R]`* requires an ordered structure (BIT / merge sort) because the lookup isn't an equality.

**L. Recall questions**
- **Q34.1:** Why is the map seeded with `{0: 1}`?
- **Q34.2:** For positive-only numbers, what simpler method counts subarrays with sum ≤ K?

---

### Problem 35: Contiguous Array

**A. Identification:** LeetCode **525** · Medium · Primary: prefix sum (balance) + earliest-index map · Secondary: value transformation

**B. Statement:** Given a binary array, return the length of the longest subarray with equal numbers of `0`s and `1`s.

**C. Pattern recognition**
- Clues: "equal number of 0s and 1s", longest contiguous.
- First question: "Can I transform values so 'equal counts' becomes 'sum is zero'?" → map `0 → −1`.
- **Rule:** *Equal counts ⇒ ±1 transform; longest zero-sum subarray ⇒ earliest index of each prefix.*

**D. Brute force:** All subarrays with counts: O(n²).

**E. Optimization reasoning:** Let `balance += (x==1 ? +1 : −1)`. A subarray `l..r` is balanced iff `balance` is equal before `l` and after `r`. To *maximize length*, record the **earliest** index at which each balance value occurs. Seed `first[0] = −1`.

**F. Algorithm**
1. `first = {0: -1}`, `balance = 0`, `best = 0`.
2. For each `i`: update `balance`; if seen → `best = max(best, i - first[balance])`; else `first[balance] = i`.

**G. C++17 solution**

```cpp
#include <vector>
#include <unordered_map>
#include <algorithm>
using namespace std;

class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        unordered_map<int, int> firstIndex;
        firstIndex[0] = -1;                         // balance 0 before the array starts
        int balance = 0, best = 0;
        for (int i = 0; i < (int)nums.size(); ++i) {
            balance += (nums[i] == 1) ? 1 : -1;
            auto it = firstIndex.find(balance);
            if (it != firstIndex.end()) best = max(best, i - it->second);
            else firstIndex[balance] = i;           // keep only the earliest index
        }
        return best;
    }
};
```

Dry run: `[0,1,0]`: bal −1 (store 0), 0 (seen at −1 → len 2), −1 (seen at 0 → len 2). Answer 2.

**H. Correctness and complexity:** Equal balance at prefix positions `l-1` and `r` ⇒ subarray `l..r` sums to 0 ⇒ equal 0s and 1s. Earliest stored index maximizes length. Time O(n); space O(n) (balance range is ≤ 2n+1, so an offset array also works).

**I. Reusable template:** *Prefix earliest-index map* (Part 2.12). Init: `{0:-1}`. Loop: update, if found compute length, else store. **What generalizes:** longest subarray with sum K, equal counts of two types, balanced parentheses variants. **What changes:** the transformation and the target difference.

**J. Common mistakes:** overwriting the first index (shrinks answers); forgetting `{0:-1}`; not converting 0 to −1; using `i - first + 1`.

**K. Variation:** *Longest subarray with equal numbers of 0s, 1s, and 2s.* Use a two-dimensional balance key (e.g., `(c1 − c0, c2 − c1)`). Assumption: one scalar can capture the invariant; with more classes you need a tuple key and hashing for pairs.

**L. Recall questions**
- **Q35.1:** Why store the earliest index, and why seed `-1`?
- **Q35.2:** How do you extend to three categories with equal counts?

---

### Problem 36: Subarray Sums Divisible by K

**A. Identification:** LeetCode **974** · Medium · Primary: prefix sum remainders + frequency · Secondary: modular arithmetic

**B. Statement:** Count non-empty contiguous subarrays whose sum is divisible by `k`. Values may be negative.

**C. Pattern recognition**
- Clues: divisible by `k` ⇒ equal remainders; counting.
- First question: "When is `P[r+1] − P[l]` divisible by `k`?" → when both prefixes have the same remainder mod `k`.
- **Rule:** *Divisible-by-K subarray ⇒ equal prefix remainders; normalise negative remainders.*

**D. Brute force:** All subarrays with running sums: O(n²).

**E. Optimization reasoning:** Track `rem = P mod k` in `[0, k-1]`. Count pairs of equal remainders: each time the current remainder was seen `c` times before, we add `c`. Seed `cnt[0] = 1`. In C++, `%` can be negative: normalise with `((x % k) + k) % k`.

**F. Algorithm**
1. `cnt[0]=1`, `rem=0`, `ans=0`.
2. For each `x`: `rem = ((rem + x) % k + k) % k`; `ans += cnt[rem]`; `cnt[rem]++`.

**G. C++17 solution**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    int subarraysDivByK(vector<int>& nums, int k) {
        vector<int> remainderCount(k, 0);
        remainderCount[0] = 1;                       // empty prefix
        int rem = 0, answer = 0;
        for (int x : nums) {
            rem = ((rem + x) % k + k) % k;           // keep remainder in [0, k-1]
            answer += remainderCount[rem];
            ++remainderCount[rem];
        }
        return answer;
    }
};
```

Dry run: `[4,5,0,-2,-3,1]`, k=5: remainders of prefixes: 4, 4, 4, 2, 4, 0 → counts of pairs give 7.

**H. Correctness and complexity:** Subarray `l..r` has sum divisible by `k` ⇔ `P[r+1] ≡ P[l] (mod k)`. Counting earlier equal remainders counts exactly those subarrays ending at `r`. Time O(n + k) (O(n) after allocating the array); space O(k). (`answer` fits in `int`: at most `n(n+1)/2` ≤ ~5·10⁸ under LeetCode's `n ≤ 3·10⁴`.)

**I. Reusable template:** *Prefix remainder frequency.* Init: `cnt[0]=1`. Loop: normalise remainder, add count, increment. **What generalizes:** "sum ≡ r mod k", "XOR equals X". **What changes:** key = prefix mod k; target remainder (here equal remainders).

**J. Common mistakes:** negative remainders in C++ (`-2 % 5 == -2`); forgetting `cnt[0] = 1`; using a map of sums instead of remainders; `int` overflow for the raw prefix (this solution never stores it).

**K. Variation:** *Subarray sum with remainder exactly `r`.* Look up `(rem − r + k) % k` before inserting. Assumption: the condition is on residue classes of the prefix. With very large `k` (beyond array allocation), use `unordered_map<long long,int>` instead of a vector.

**L. Recall questions**
- **Q36.1:** Why do equal prefix remainders imply a divisible subarray?
- **Q36.2:** How do you adapt the loop for "sum % k == r"?

---

### Problem 37: Continuous Subarray Sum

**A. Identification:** LeetCode **523** · Medium · Primary: prefix remainders + earliest-index map · Secondary: length ≥ 2 constraint

**B. Statement:** Given non-negative `nums` and integer `k`, return `true` if some **contiguous subarray of length ≥ 2** has a sum that is a multiple of `k` (0 counts as a multiple).

**C. Pattern recognition**
- Clues: "multiple of k", subarray, length constraint.
- First question: "Same as Problem 36, but now I need *existence* with a minimum length."
- **Rule:** *Existence + length ≥ 2 ⇒ earliest index per remainder; require `i - first ≥ 2`.*

**D. Brute force:** All subarrays of length ≥ 2: O(n²).

**E. Optimization reasoning:** Store the *earliest* index at which each prefix remainder appears (`first[0] = -1`). At index `i` with remainder `r`: if `r` was seen at `j` and `i - j ≥ 2`, the subarray `j+1..i` has length ≥ 2 and a multiple-of-`k` sum. If `i - j == 1` (length 1), ignore it and **do not overwrite** the earliest index.

**F. Algorithm**
1. `first = {0: -1}`, `rem = 0`.
2. For each `i`: `rem = (rem + nums[i]) % k`.
3. If found with `i - first[rem] ≥ 2` → true; if not found → `first[rem] = i`.
4. Return false.

**G. C++17 solution**

```cpp
#include <vector>
#include <unordered_map>
using namespace std;

class Solution {
public:
    bool checkSubarraySum(vector<int>& nums, int k) {
        unordered_map<int, int> firstIndex;
        firstIndex[0] = -1;
        long long rem = 0;
        for (int i = 0; i < (int)nums.size(); ++i) {
            rem = ((rem + nums[i]) % k + k) % k;
            auto it = firstIndex.find((int)rem);
            if (it == firstIndex.end()) firstIndex[(int)rem] = i;       // earliest only
            else if (i - it->second >= 2) return true;                  // length at least 2
        }
        return false;
    }
};
```

Dry run: `[23,2,4,6,7]`, k=6: rem: 5 (idx0), 1 (idx1), 5 (idx2: seen at 0, len 2) → true.

**H. Correctness and complexity:** Equal remainders at prefix positions `j` and `i` ⇒ `sum(j+1..i)` multiple of `k`. Keeping the earliest `j` maximizes the length `i-j`, so if any valid `j` exists, the earliest one satisfies the length ≥ 2 test. Time O(n); space O(min(n,k)).

**I. Reusable template:** *Prefix earliest-index map with length filter.* Init: `{0:-1}`. Loop: compute key; if absent store `i`; else test length condition. **What generalizes:** existence queries with a minimum/maximum length. **What changes:** remainder arithmetic and the length predicate.

**J. Common mistakes:** overwriting the earliest index; allowing length 1 (`i - j ≥ 1`); forgetting `first[0] = -1` (breaks subarrays starting at 0); mishandling `k` when negative or zero in variants.

**K. Variation:** *`k` may be 0.* A multiple of 0 means sum equal 0: key by the raw prefix sum instead of remainder (and require two zeros in a row or equal prefix sums). Assumption: `k ≥ 1`, so modulus is defined. For negative numbers use the normalised remainder.

**L. Recall questions**
- **Q37.1:** Why must we not update the stored index when a remainder repeats?
- **Q37.2:** What changes if `k` can be 0?

---

### Problem 38: Maximum Length Subarray With Sum K

**A. Identification:** **Not an official LeetCode title.** The standard interview problem is also known as *Longest Subarray with Sum K* (GeeksforGeeks naming) and corresponds closely to LeetCode **325. Maximum Size Subarray Sum Equals k**, which is a *Premium* problem to my knowledge (verify before relying on the number). Difficulty: Medium (standard). Primary: prefix sum + earliest-index map · Secondary: complement lookup on prefix values.

**B. Statement:** Given an integer array (negatives allowed) and `k`, return the length of the longest contiguous subarray whose sum is exactly `k`; return `0` if none exists.

**C. Pattern recognition**
- Clues: "longest", "sum exactly k", negatives allowed.
- First question: "Can shrinking a window be trusted when negatives exist?" No ⇒ prefix + map.
- **Rule:** *Longest subarray with sum K (any signs) ⇒ earliest index of each prefix; look up `pre − K`.*

**D. Brute force:** Enumerate all `(l, r)` with a running sum: O(n²) time, O(1) space.

**E. Optimization reasoning:** Same relation as Problem 34, but we maximize length instead of counting, so store only the **first** occurrence of each prefix. At index `i`: if `pre − K` was first seen at `j`, candidate length is `i − j`.

**F. Algorithm**
1. `first = {0: -1}`, `pre = 0`, `best = 0`.
2. For each `i`: `pre += a[i]`; if `first` contains `pre − K` → `best = max(best, i − first[pre−K])`; insert `pre` only if absent.

**G. C++17 solution**

```cpp
#include <vector>
#include <unordered_map>
#include <algorithm>
using namespace std;

class Solution {
public:
    // Returns the length of the longest subarray with sum exactly k (0 if none).
    int maxSubArrayLen(const vector<int>& nums, int k) {
        unordered_map<long long, int> firstIndex;
        firstIndex[0] = -1;
        long long prefix = 0;
        int best = 0;
        for (int i = 0; i < (int)nums.size(); ++i) {
            prefix += nums[i];
            auto it = firstIndex.find(prefix - k);
            if (it != firstIndex.end()) best = max(best, i - it->second);
            firstIndex.emplace(prefix, i);        // emplace does nothing if the key exists => keeps earliest
        }
        return best;
    }
};
```

Dry run: `[1,-1,5,-2,3]`, K=3: prefixes 1, 0, 5, 3, 6. At i=3, `pre=3` → need `0`, first seen at index −1 (empty prefix) → length 4 (`1-1+5-2 = 3` ✓). At i=4, `pre=6` → need `3`, first seen at index 3 → length 1. Answer 4.

**H. Correctness and complexity:** *Invariant:* `firstIndex[v]` is the smallest `j` with `P[j+1] = v` (with `-1` for the empty prefix). Any subarray ending at `i` with sum `K` begins right after some `j` with `P = pre − K`; the earliest `j` gives the longest. Time O(n) expected; space O(n).

**I. Reusable template:** *Prefix earliest-index map.* Init: `{0:-1}`. Loop: update prefix, lookup `pre − K`, `emplace` current. **What generalizes:** Problem 35 (balance), 37 (remainders). **What changes:** the lookup key formula and the stored quantity.

**J. Common mistakes:** using `operator[]` assignment which overwrites the first index; sliding window with negatives; forgetting `{0:-1}`; returning `i - j + 1`.

**K. Variation:** *Longest subarray with sum ≥ K, or ≤ K, with negatives.* Equality lookup no longer works. For non-negative values a window works; with negatives you need ordered prefix structures (monotonic stack/binary search over prefix minima). Assumption: exact equality allows O(1) lookup via hashing.

**L. Recall questions**
- **Q38.1:** Why use `emplace` (or "insert if absent") instead of assigning?
- **Q38.2:** Does this approach handle "sum ≥ K" with negatives? Why or why not?

---

### Problem 39: Number of Ways to Split Array

**A. Identification:** LeetCode **2270** · Medium · Primary: prefix sums (running left sum + total) · Secondary: counting valid split points

**B. Statement:** Count indices `i` in `[0, n-2]` such that `sum(nums[0..i]) ≥ sum(nums[i+1..n-1])`. Both sides must be non-empty.

**C. Pattern recognition**
- Clues: split point comparing left and right sums.
- First question: "Is the right sum `total − left`?" Yes.
- **Rule:** *Left vs right at every split ⇒ prefix + total.*

**D. Brute force:** Compute both sums for each split: O(n²).

**E. Optimization reasoning:** `total` once; running `left`; condition `left ≥ total − left`. Restrict `i` to `n-2` so the right side is non-empty. Use `long long` since values reach 10⁵ over up to 10⁵ elements (10¹⁰).

**F. Algorithm**
1. `total = sum`, `left = 0`, `ways = 0`.
2. For `i = 0..n-2`: `left += a[i]`; if `left ≥ total − left` → `ways++`.

**G. C++17 solution**

```cpp
#include <vector>
#include <numeric>
using namespace std;

class Solution {
public:
    int waysToSplitArray(vector<int>& nums) {
        long long total = accumulate(nums.begin(), nums.end(), 0LL);
        long long left = 0;
        int ways = 0;
        for (int i = 0; i + 1 < (int)nums.size(); ++i) {   // i <= n-2: right part non-empty
            left += nums[i];
            if (left >= total - left) ++ways;
        }
        return ways;
    }
};
```

Dry run: `[10,4,-8,7]`, total 13: i=0: 10 ≥ 3 ✓; i=1: 14 ≥ −1 ✓; i=2: 6 ≥ 7 ✗ → 2.

**H. Correctness and complexity:** For each valid split, `total − left` is exactly the right sum. Time O(n); space O(1).

**I. Reusable template:** *Prefix + total split scan* (same as Problem 32). **What changes:** the comparison (`≥` here, `==` in pivot) and the allowed range of split points.

**J. Common mistakes:** looping to `n-1` (empty right side); `int` overflow; using `>` instead of `≥`; recomputing right sum each time.

**K. Variation:** *Split into three parts with equal sums (LC 1013 partition array into three parts with equal sum).* Prefix sums + count when running sum hits `total/3` and `2·total/3`; handles negatives if you count carefully. Assumption: only one split point and an O(1) comparison; multiple splits require counting/hash of prefix values.

**L. Recall questions**
- **Q39.1:** Why must the loop stop at `n-2`?
- **Q39.2:** Why use `long long` here?

---

### Problem 40: Product of Array Except Self: Prefix/Suffix Perspective

**A. Identification:** **Deliberate revision of Problem 6** (LeetCode **238** · Medium). Primary: prefix/suffix arrays · Secondary: space compression from O(n) to O(1)

**B. Statement:** Same as Problem 6: `out[i]` = product of all elements except `nums[i]`, no division, O(n) time.

**C. Pattern recognition (new insight)**
- In Problem 6 you saw "two passes". Here, see the *structure*: excluding one index splits the array into a **prefix** `[0, i)` and a **suffix** `(i, n)`. The "everything except a point" query is a *union of a prefix query and a suffix query*, each answered in O(1) from precomputed tables.
- First question: "Which two precomputed tables make every answer a single multiplication?"
- **Rule:** *Complement of a single point = prefix ⊕ suffix; precompute both.*

**D. Brute force:** O(n²) recomputation. The *explicit-table* version below is the intermediate step: it is O(n) time and O(n) extra space and is the conceptual form of the answer.

**E. Optimization reasoning:** Define padded arrays with `prefix[0] = 1` and `suffix[n] = 1`:
- `prefix[i]` = product of `nums[0..i-1]` (the **exclusive** prefix, same indexing convention as `P` in Problem 31).
- `suffix[i]` = product of `nums[i..n-1]`.
- `out[i] = prefix[i] * suffix[i+1]`.

Compare with prefix sums: `sum(l..r) = P[r+1] − P[l]` uses *inversion* (subtraction). Products *without* division can't invert, so you can't cover a middle range from one prefix; but "all except `i`" doesn't need inversion because it is a prefix and a suffix, which are accessible directly. The space compression of Problem 6 comes from noticing `prefix[i]` is only needed *at index `i`* in a left-to-right pass, and `suffix[i+1]` only in a right-to-left pass: so each can be a rolling variable instead of an array.

**F. Step-by-step algorithm**
1. `prefix[0] = 1`; `prefix[i+1] = prefix[i] * nums[i]`.
2. `suffix[n] = 1`; `suffix[i] = suffix[i+1] * nums[i]`.
3. `out[i] = prefix[i] * suffix[i+1]`.
4. (Compression) Replace `prefix` by the running variable `left` in a first pass writing into `out`; replace `suffix` by `right` in a second pass (Problem 6).

**G. C++17 solution (explicit-table form, with the zero-count alternative in K)**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = (int)nums.size();
        vector<int> prefix(n + 1, 1);       // prefix[i] = product of nums[0..i-1]
        vector<int> suffix(n + 1, 1);       // suffix[i] = product of nums[i..n-1]
        for (int i = 0; i < n; ++i)     prefix[i + 1] = prefix[i] * nums[i];
        for (int i = n - 1; i >= 0; --i) suffix[i] = suffix[i + 1] * nums[i];
        vector<int> out(n);
        for (int i = 0; i < n; ++i) out[i] = prefix[i] * suffix[i + 1];
        return out;
    }
};
```

Dry run: `[2,3,4]`. `prefix=[1,2,6,24]`, `suffix=[24,12,4,1]`. `out[0]=prefix[0]*suffix[1]=12`, `out[1]=2*4=8`, `out[2]=6*1=6`. (`prefix[n]` is the total product, itself a valid prefix, so the problem's overflow guarantee covers it.)

**H. Correctness and complexity:** `prefix[i]·suffix[i+1]` multiplies exactly the elements with index `< i` and `> i`. Time O(n); auxiliary space O(n) (two tables), reducible to O(1) as in Problem 6.

**I. Reusable template:** *Padded prefix/suffix tables.* Init: identity-padded arrays of length `n+1`. Loops: one forward, one backward. Query: combine `prefix[i]` and `suffix[i+1]`. **What generalizes:** any associative combine: sum, max, gcd, bitwise AND/OR. **What changes:** identity element and combine operator. For *non-invertible* operators prefix/suffix tables are the only O(1)-query option.

**J. Common mistakes:** off-by-one between `suffix[i]` and `suffix[i+1]`; forgetting identity padding; inclusive vs exclusive prefix confusion; using total/`nums[i]` (division, zeros).

**K. Variation:** *Division is allowed and zeros exist.* Zero-count method: let `zeros` = count of zeros, `prodNonZero` = product of the non-zero elements. If `zeros ≥ 2` → all zeros; if `zeros == 1` → `out[i] = prodNonZero` only where `nums[i]==0`, else 0; if `zeros == 0` → `out[i] = total / nums[i]`. Assumption of prefix/suffix: no inverse needed. If overflow is possible, tables of `long long` or modular arithmetic with care (modular inverse doesn't exist for non-invertible elements, so prefix/suffix is again the safe route).

**L. Recall questions**
- **Q40.1:** Why can't "product of a middle range" be derived from a single prefix product table the way sums can?
- **Q40.2:** For `max-except-self`, what are the identity element and combine operator?

---
## Section 5: Mixed Patterns and Transfer

### Problem 41: Maximum Subarray

**A. Identification:** LeetCode **53** · Medium · Primary: Kadane's algorithm · Secondary: prefix-sum reasoning

**B. Statement:** Return the largest sum of a non-empty contiguous subarray of `nums`.

**C. Pattern recognition**
- Clues: "maximum sum", contiguous, signs mixed.
- First question: "What is the best subarray *ending at* index `i`, in terms of the one ending at `i-1`?"
- **Rule:** *Max contiguous sum ⇒ `cur = max(x, cur + x)`.*

**D. Brute force:** All subarrays with a running sum: O(n²).

**E. Optimization reasoning:** A subarray ending at `i` either starts at `i` or extends the best one ending at `i-1`. Extending is useful only if the previous best is positive. So `cur = max(a[i], cur + a[i])`; answer = max over all `cur`. Equivalent prefix view: `max_r (P[r+1] − min_{l ≤ r} P[l])`.

**F. Algorithm**
1. `cur = best = nums[0]`.
2. For `i ≥ 1`: `cur = max(nums[i], cur + nums[i])`; `best = max(best, cur)`.

**G. C++17 solution**

```cpp
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int cur = nums[0], best = nums[0];       // non-empty required: start from the first element
        for (size_t i = 1; i < nums.size(); ++i) {
            cur = max(nums[i], cur + nums[i]);   // restart here, or extend the previous best
            best = max(best, cur);
        }
        return best;
    }
};
```

Dry run: `[-2,1,-3,4,-1,2,1,-5,4]`: cur: −2, 1, −2, 4, 3, 5, 6, 1, 5 → best 6 (`[4,-1,2,1]`).

**H. Correctness and complexity:** *Invariant:* `cur` = max sum of a subarray ending exactly at `i`. Any optimal subarray ends at some `i`, so `best` captures it. Time O(n); space O(1).

**I. Reusable template:** *Kadane* (Part 2.14). Init: first element. Loop: extend/restart. Termination: end of array. **What generalizes:** best subarray under any "extend or restart" decomposition (max product needs two states, circular needs a second pass). **What changes:** the state kept per index.

**J. Common mistakes:** initializing `cur = best = 0` (wrong for all-negative arrays); resetting `cur` to 0 instead of `a[i]`; returning the sum of positives.

**K. Variation:** *Also return the subarray's indices.* Track `start` when restarting and update `bestL, bestR` on improvement. *Circular array (LC 918):* answer = `max(kadaneMax, total − kadaneMin)` with an all-negative special case. Assumption: contiguous, non-circular. If a length limit exists (`≤ L`), Kadane fails; use prefix sums + monotonic deque.

**L. Recall questions**
- **Q41.1:** What does `cur` represent after processing index `i`?
- **Q41.2:** How does Kadane change for a circular array?

---

### Problem 42: Maximum Product Subarray

**A. Identification:** LeetCode **152** · Medium · Primary: Kadane-style DP with max and min · Secondary: sign reasoning

**B. Statement:** Return the largest product of a non-empty contiguous subarray. Values can be negative or zero. The answer fits in 32-bit.

**C. Pattern recognition**
- Clues: product (not sum), negatives flip signs, zeros reset.
- First question: "What if the current element is negative: what previous product do I want?" → the *smallest* (most negative).
- **Rule:** *Product with negatives ⇒ track both max and min ending here.*

**D. Brute force:** All subarrays with running products: O(n²).

**E. Optimization reasoning:** Let `mx`, `mn` = max/min product of a subarray ending at `i`. A negative `x` swaps their roles (a large negative becomes a large positive). So: if `x < 0`, swap `mx, mn`; then `mx = max(x, mx·x)`, `mn = min(x, mn·x)`. A zero collapses both to 0, which restarts naturally (`max(x, mx·x)` with `x=0` gives 0, and the next element starts from `x` itself).

**F. Algorithm**
1. `mx = mn = best = nums[0]`.
2. For `i ≥ 1`: if `nums[i] < 0` swap(`mx`, `mn`); update both; `best = max(best, mx)`.

**G. C++17 solution**

```cpp
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    int maxProduct(vector<int>& nums) {
        long long mx = nums[0], mn = nums[0], best = nums[0];
        for (size_t i = 1; i < nums.size(); ++i) {
            long long x = nums[i];
            if (x < 0) swap(mx, mn);                 // negative flips max <-> min
            mx = max(x, mx * x);                     // extend or restart
            mn = min(x, mn * x);
            best = max(best, mx);
        }
        return (int)best;
    }
};
```

Dry run: `[2,3,-2,4]`: (mx,mn): (2,2) → (6,3) → x=−2: swap → (3,6) → mx=max(−2,−6)=−2, mn=min(−2,−12)=−12 → x=4: mx=max(4,−8)=4, mn=min(4,−48)=−48. best = 6.

**H. Correctness and complexity:** *Invariant:* `mx`/`mn` are the true max/min product of subarrays ending at `i`. The candidate products ending at `i` come from `{x, x·mx_prev, x·mn_prev}`; sign analysis shows the max/min are among them. Time O(n); space O(1). The `long long` is a safety margin for intermediate products; LeetCode guarantees the *answer* fits in 32 bits.

**I. Reusable template:** *Kadane with two states.* Init: `mx = mn = a[0]`. Update: swap on negative, extend/restart. **What generalizes:** DP where an operation's sign can flip extremes. **What changes:** the pair of tracked extremes; with `+` only one state is needed (Problem 41).

**J. Common mistakes:** tracking only the max; forgetting to swap before updating; resetting incorrectly on zero; using `int` when intermediate products may overflow (long chains of values ≥ 2).

**K. Variation:** *Maximum product of a subarray in doubles with values in (0, 2).* Use log-sums and Kadane. If the answer may exceed 64-bit, you need big integers or logarithms (with precision issues). Assumption: products stay within range. The prefix/suffix alternative: the best product is either a prefix product or a suffix product of a zero-delimited segment.

**L. Recall questions**
- **Q42.1:** Why do we track the minimum product as well?
- **Q42.2:** What happens to `mx` and `mn` at a zero, and why does the algorithm still work?

---

### Problem 43: Subarray Product Less Than K

**A. Identification:** LeetCode **713** · Medium · Primary: variable sliding window (counting) · Secondary: product window

**B. Statement:** Given positive integers `nums` and `k`, count contiguous subarrays whose product is **strictly less than** `k`.

**C. Pattern recognition**
- Clues: product threshold, all values ≥ 1, *counting* subarrays.
- Positivity ⇒ products grow when extending, shrink when removing: monotone.
- First question: "For a fixed right end, how many valid left ends exist?" → `right − left + 1` (all starts from `left` to `right`).
- **Rule:** *Count subarrays with monotone bound ⇒ window; add `right − left + 1` per right end.*

**D. Brute force:** All subarrays with running products: O(n²).

**E. Optimization reasoning:** Maintain `prod` of the window. After adding `nums[right]`, shrink `left` while `prod ≥ k` (divide exactly by `nums[left]`; exact because values are integers that were multiplied in). Then every subarray ending at `right` and starting in `[left, right]` is valid: add `right − left + 1`. If `k ≤ 1`, no product of positive integers can be `< k`: return 0 (also avoids shrinking past an empty window).

**F. Algorithm**
1. If `k ≤ 1` return 0.
2. `prod = 1`, `left = 0`, `count = 0`.
3. For each `right`: `prod *= a[right]`; while `prod ≥ k`: `prod /= a[left++]`; `count += right − left + 1`.

**G. C++17 solution**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    int numSubarrayProductLessThanK(vector<int>& nums, int k) {
        if (k <= 1) return 0;                        // products of positive ints are >= 1
        long long prod = 1;
        int left = 0, count = 0;
        for (int right = 0; right < (int)nums.size(); ++right) {
            prod *= nums[right];
            while (prod >= k) prod /= nums[left++];  // shrink until product < k
            count += right - left + 1;               // all starts in [left, right]
        }
        return count;
    }
};
```

Dry run: `[10,5,2,6]`, k=100: r=0: prod 10 → +1; r=1: 50 → +2; r=2: 100 ≥ 100 → divide by 10 → 10 → window `[5,2]` → +2; r=3: 60 → +3. Total 8.

**H. Correctness and complexity:** *Invariant:* after the `while`, `[left, right]` is the longest window ending at `right` with product `< k`, and all subarrays ending at `right` with start ≥ `left` have smaller product (values ≥ 1). Time O(n) (each pointer moves ≤ n times); space O(1). `prod < k·max(nums)` before the shrink, so `long long` suffices.

**I. Reusable template:** *Variable window counting* (`count += right − left + 1`). Init: neutral state. Loop: add, shrink until valid, count. **What generalizes:** Subarrays with at most K distinct, sum ≤ K (positives). **What changes:** the state and the validity test (product, sum, distinct).

**J. Common mistakes:** not handling `k ≤ 1` (infinite/negative-index shrink); using `if` instead of `while`; `≤` instead of `<`; counting only the window length once instead of `right-left+1`.

**K. Variation:** *Zeros allowed.* A zero makes the product 0 (< k whenever k > 0) and division by it is invalid; the window with product tracking breaks. Assumption: all values ≥ 1 (so product is monotone and division is exact). With zeros, count subarrays containing a zero separately (contribute `right + 1`) and reset the window after each zero, or use log-sums for non-negative reals.

**L. Recall questions**
- **Q43.1:** Why do we add `right − left + 1` per step?
- **Q43.2:** Why does the algorithm break if zeros are present?

---

### Problem 44: Merge Sorted Array

**A. Identification:** LeetCode **88** · Easy · Primary: merge from the back (three pointers) · Secondary: in-place merge

**B. Statement:** `nums1` has length `m + n` with its first `m` entries sorted and the last `n` as zero padding; `nums2` has `n` sorted entries. Merge `nums2` into `nums1` in place, sorted.

**C. Pattern recognition**
- Clues: in-place merge into an array with trailing free space.
- First question: "Where is the free space?" → at the end, so write the *largest* elements first.
- **Rule:** *In-place merge with trailing buffer ⇒ fill from the back.*

**D. Brute force:** Copy `nums2` into the tail and sort: O((m+n) log(m+n)). Or merge into a temporary array: O(m+n) space.

**E. Optimization reasoning:** Merging from the front would overwrite unread `nums1` data. From the back, the write index `k = m+n-1` is always ≥ the read indices, so writes never destroy unread elements. Compare `nums1[i]` and `nums2[j]`, write the larger at `k`.

**F. Algorithm**
1. `i = m-1`, `j = n-1`, `k = m+n-1`.
2. While `j ≥ 0`: if `i ≥ 0 && nums1[i] > nums2[j]` → `nums1[k--] = nums1[i--]`; else `nums1[k--] = nums2[j--]`.

**G. C++17 solution**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = m - 1, j = n - 1, k = m + n - 1;
        while (j >= 0) {                                   // remaining nums1 elements are already in place
            if (i >= 0 && nums1[i] > nums2[j]) nums1[k--] = nums1[i--];
            else                               nums1[k--] = nums2[j--];
        }
    }
};
```

Dry run: `nums1=[1,2,3,0,0,0]`, `nums2=[2,5,6]`: write 6, 5, then 3 (3>2), then 2 (tie → take from nums2), 2, 1... result `[1,2,2,3,5,6]`.

**H. Correctness and complexity:** *Invariant:* `nums1[k+1..]` holds the largest `m+n-k-1` elements in final order, and `k ≥ i` always so no unread element is overwritten. Once `nums2` is exhausted, the remaining `nums1[0..i]` are already in their final positions. Time O(m+n); space O(1).

**I. Reusable template:** *Merge from the back.* Init: three pointers at the ends. Loop: place the larger. Termination: the source whose elements would be overwritten is exhausted. **What generalizes:** any in-place merge/expansion where the target has free tail space (also "replace spaces with %20"). **What changes:** comparison and loop condition.

**J. Common mistakes:** merging from the front; looping `while (i >= 0 && j >= 0)` and forgetting to copy leftover `nums2`; using `≥` vs `>` doesn't matter for correctness but is a stability choice.

**K. Variation:** *Merge without free space at the end (two separate sorted vectors into a new one).* Forward merge with two pointers is natural. Assumption: free space is at the *end* of the destination; if it's at the front you'd merge from the front. For merging k sorted arrays use a min-heap.

**L. Recall questions**
- **Q44.1:** Why can't you merge from the front without extra space?
- **Q44.2:** Why don't we need to copy leftover `nums1` elements after `nums2` is exhausted?

---

### Problem 45: Rotate Array

**A. Identification:** LeetCode **189** · Medium · Primary: three-reversal trick · Secondary: modular index mapping

**B. Statement:** Rotate `nums` to the right by `k` steps in place (`k` may exceed `n`).

**C. Pattern recognition**
- Clues: rotate in place, O(1) extra space.
- First question: "What does rotation do to the order of the two blocks `[0, n-k)` and `[n-k, n)`?" → swaps them, preserving internal order.
- **Rule:** *Block swap in place ⇒ reverse everything, then reverse each block.*

**D. Brute force:** Rotate by one `k` times: O(nk). Or a copy with `out[(i+k)%n] = a[i]`: O(n) time, O(n) space.

**E. Optimization reasoning:** Reversing the entire array puts the last `k` elements first (reversed) and the first `n−k` last (reversed). Reversing each block restores their internal order. Normalise `k %= n` (and handle `n ≤ 1`).

**F. Algorithm**
1. `k %= n`; if `k == 0` return.
2. `reverse(a, a+n)`; `reverse(a, a+k)`; `reverse(a+k, a+n)`.

**G. C++17 solution**

```cpp
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = (int)nums.size();
        if (n == 0) return;
        k %= n;                                        // rotating by n is a no-op
        reverse(nums.begin(), nums.end());             // [7,6,5,4,3,2,1]
        reverse(nums.begin(), nums.begin() + k);       // first k back in order
        reverse(nums.begin() + k, nums.end());         // remaining n-k back in order
    }
};
```

Dry run: `[1,2,3,4,5,6,7]`, k=3 → reverse all: `[7,6,5,4,3,2,1]` → first 3: `[5,6,7,4,3,2,1]` → last 4: `[5,6,7,1,2,3,4]`.

**H. Correctness and complexity:** The first reversal sends element at index `i` to `n-1-i`. After reversing each block, the combined effect maps `i → (i + k) mod n`. Time O(n) (≈ 2n swaps); space O(1).

**I. Reusable template:** *Reverse-based block swap.* Init: normalise `k`. Steps: reverse whole, reverse blocks. **What generalizes:** left rotation (reverse blocks first, then whole), "reverse words in a string in place". **What changes:** which blocks and in what order.

**J. Common mistakes:** not reducing `k % n`; reversing the wrong block boundaries (`n-k` vs `k`) for left rotation; `n == 0` with `% 0` (undefined); using `k > n` directly with iterators.

**K. Variation:** *Left rotation by `k`.* Reverse `[0,k)`, reverse `[k,n)`, then reverse all. Alternative O(1)-space method: cycle-leader (`gcd(n,k)` cycles). Assumption: you may swap in place; if only O(1) *writes* are allowed, none of these apply.

**L. Recall questions**
- **Q45.1:** Why must `k` be reduced modulo `n`?
- **Q45.2:** What reversal order produces a *left* rotation by `k`?

---

### Problem 46: Find the Duplicate Number

**A. Identification:** LeetCode **287** · Medium · Primary: Floyd's cycle detection · Secondary: pigeonhole/binary search on value

**B. Statement:** `nums` has `n + 1` integers in `[1, n]`; exactly one value repeats (possibly many times). Find it **without modifying** the array and with O(1) extra space.

**C. Pattern recognition**
- Clues: values in `[1,n]` with `n+1` entries ⇒ pigeonhole ⇒ a repeat; read-only + O(1) space rules out sorting, sets, and index marking.
- First question: "Can I view `i → nums[i]` as a linked list?" Values are valid indices (≥ 1, ≤ n) so every step is defined; a duplicated value means two nodes point to the same node ⇒ a cycle whose entrance is the duplicate.
- **Rule:** *Range [1..n] in n+1 slots, read-only ⇒ treat as function graph and use Floyd.*

**D. Brute force:** Compare all pairs: O(n²), O(1) space. Set: O(n) time/space. Sort: O(n log n) and mutates (or O(n) copy).

**E. Optimization reasoning:** Starting from index 0, repeatedly apply `x ← nums[x]`. Since `nums[i] ∈ [1,n]`, index 0 is never pointed to, so the walk starts outside any cycle and must eventually cycle (finite nodes). Two nodes with the same value point into the same successor, so the cycle's entrance node is the duplicated value. Floyd phase 1 finds a meeting point inside the cycle with `slow` (1 step) and `fast` (2 steps). Phase 2 resets `slow` to the start; moving both one step at a time, they meet at the cycle entrance.

**F. Algorithm**
1. Phase 1 (do-while): `slow = nums[slow]`, `fast = nums[nums[fast]]`, starting both at index 0, until `slow == fast`.
2. `slow = 0`; while `slow != fast`: `slow = nums[slow]`, `fast = nums[fast]`.
3. Return `slow`.

**G. C++17 solution**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int slow = 0, fast = 0;
        do {                                  // phase 1: find a meeting point inside the cycle
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);
        slow = 0;                             // phase 2: find the cycle entrance
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;                          // entrance index == duplicated value
    }
};
```

Dry run: `[1,3,4,2,2]` as graph `0→1→3→2→4→2`: cycle `2→4→2`. Phase 1 meets inside the cycle; phase 2 from 0 and the meeting point reach `2` together → answer 2.

**H. Correctness and complexity:** *Standard Floyd argument:* let `a` = distance from the start to the cycle entrance, `c` = cycle length. At the meeting point, `slow` has moved `a + x` and `fast` `2(a + x)`; the difference `a + x` is a multiple of `c`, so after `a` more steps from the meeting point you land on the entrance, exactly where a pointer from the start arrives after `a` steps. Time O(n); space O(1); array untouched.

**I. Reusable template:** *Floyd tortoise-and-hare* on an implicit function graph. Init: both at the head. Phase 1: 1 vs 2 steps until equal. Phase 2: reset one pointer, step both by 1. **What generalizes:** linked list cycle II, happy number. **What changes:** the successor function `next(x)`.

**J. Common mistakes:** starting `slow`/`fast` at `nums[0]` but resetting to `0` inconsistently; using a `while` instead of `do-while` for phase 1 (they start equal); assuming it works when value `0` or values > n exist; modifying the array.

**K. Variation:** *Array is read-only but you can use O(log n) extra: binary search on the value range.* Count elements ≤ mid; if count > mid the duplicate lies in `[1, mid]`. O(n log n) time, O(1) space, also read-only. Assumption for Floyd: all values in `[1, n]`, so successors are valid indices and index 0 is not in the cycle. If values may be 0 or ≥ n+1, the graph walk fails.

**L. Recall questions**
- **Q46.1:** Why is index 0 guaranteed not to be on the cycle?
- **Q46.2:** What read-only alternative also uses O(1) space, and what's its time?

---

### Problem 47: Set Matrix Zeroes

**A. Identification:** LeetCode **73** · Medium · Primary: in-place marking using the matrix border · Secondary: auxiliary arrays (simpler variant)

**B. Statement:** If an element of an `m × n` matrix is `0`, set its entire row and column to `0` **in place**.

**C. Pattern recognition**
- Clues: "in place", O(1) space follow-up.
- Core difficulty: zeros you write look like original zeros.
- First question: "Where can I store 'this row/column must be zeroed' without extra memory?" → row 0 and column 0.
- **Rule:** *In-place matrix flags ⇒ use the first row/column as markers and remember separately whether they themselves must be zeroed.*

**D. Brute force:** Copy the matrix and zero rows/columns of every original zero: O(mn) space. Or use `rowZero[m]`, `colZero[n]` arrays: O(m+n) space.

**E. Optimization reasoning:** Use `matrix[i][0]` as the flag for row `i` and `matrix[0][j]` for column `j`. Two extra booleans record whether the *original* first row/column contained zeros (since their cells are overwritten as flags). Process the interior first (rows/cols ≥ 1), then zero the first row/column if their booleans are set.

**F. Algorithm**
1. `firstRowZero`, `firstColZero` ← scan row 0, column 0.
2. For `i, j ≥ 1`: if `a[i][j]==0`: `a[i][0]=0`, `a[0][j]=0`.
3. For `i, j ≥ 1`: if `a[i][0]==0 || a[0][j]==0`: `a[i][j]=0`.
4. If `firstRowZero` zero row 0; if `firstColZero` zero column 0.

**G. C++17 solution**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int m = (int)matrix.size(), n = (int)matrix[0].size();
        bool firstRowZero = false, firstColZero = false;
        for (int j = 0; j < n; ++j) if (matrix[0][j] == 0) firstRowZero = true;
        for (int i = 0; i < m; ++i) if (matrix[i][0] == 0) firstColZero = true;

        for (int i = 1; i < m; ++i)                    // mark using row 0 / column 0
            for (int j = 1; j < n; ++j)
                if (matrix[i][j] == 0) { matrix[i][0] = 0; matrix[0][j] = 0; }

        for (int i = 1; i < m; ++i)                    // apply marks to the interior
            for (int j = 1; j < n; ++j)
                if (matrix[i][0] == 0 || matrix[0][j] == 0) matrix[i][j] = 0;

        if (firstRowZero) for (int j = 0; j < n; ++j) matrix[0][j] = 0;   // finally the border
        if (firstColZero) for (int i = 0; i < m; ++i) matrix[i][0] = 0;
    }
};
```

Dry run: `[[1,1,1],[1,0,1],[1,1,1]]`: border has no zeros. Interior zero at (1,1) → `a[1][0]=0`, `a[0][1]=0`. Apply: cells with `a[i][0]==0` (row 1) or `a[0][j]==0` (col 1) are zeroed → `[[1,0,1],[0,0,0],[1,0,1]]`.

**H. Correctness and complexity:** *Invariant:* during marking, `a[i][0]`/`a[0][j]` (for `i, j ≥ 1`) are only changed to record zeros, and the original border state is preserved in the two booleans. The interior is decided from the flags *before* the border is zeroed. Time O(mn); space O(1).

**I. Reusable template:** *In-place marker scheme.* Init: capture border status. Pass 1: mark. Pass 2: apply. Pass 3: finish the border. **What generalizes:** any "flags in the data structure itself" technique (Game of Life with state encodings). **What changes:** what the marks mean and the ordering (apply before clobbering markers).

**J. Common mistakes:** zeroing the border before applying marks; losing the original border zero status; zeroing immediately while scanning (cascades); empty matrix guard (`matrix[0]` on empty).

**K. Variation:** *Elements can be any integer and zero is the trigger value (already the case).* If a sentinel value is needed and the data range is unrestricted, use the border-marker approach rather than a magic constant. Assumption: the first row/column may be used as scratch. If the matrix is read-only, you need O(m+n) auxiliary flags (or O(1) with bit tricks if m,n small).

**L. Recall questions**
- **Q47.1:** Why must the two border booleans be computed before the marking pass?
- **Q47.2:** Why is it wrong to zero rows/columns immediately when you encounter a zero?

---

### Problem 48: Spiral Matrix

**A. Identification:** LeetCode **54** · Medium · Primary: boundary-shrinking matrix traversal · Secondary: simulation

**B. Statement:** Return all elements of an `m × n` matrix in clockwise spiral order, starting at the top-left.

**C. Pattern recognition**
- Clues: traversal order defined geometrically.
- First question: "What are the unvisited cells after each side is traversed?" → a smaller rectangle.
- **Rule:** *Spiral ⇒ maintain `top, bottom, left, right`; traverse a side, shrink that boundary, guard the return legs.*

**D. Brute force:** Simulate with a visited matrix and direction changes: O(mn) time, O(mn) space.

**E. Optimization reasoning:** The unvisited region is always `[top..bottom] × [left..right]`. Traverse top row L→R then `top++`; right column T→B then `right--`; if rows remain, bottom row R→L then `bottom--`; if columns remain, left column B→T then `left++`. The two guards prevent re-reading a single row/column.

**F. Algorithm**
1. Initialise boundaries.
2. While `top ≤ bottom && left ≤ right`: traverse four sides with guards on the last two.

**G. C++17 solution**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> out;
        if (matrix.empty() || matrix[0].empty()) return out;
        int top = 0, bottom = (int)matrix.size() - 1;
        int left = 0, right = (int)matrix[0].size() - 1;
        while (top <= bottom && left <= right) {
            for (int c = left; c <= right; ++c) out.push_back(matrix[top][c]);
            ++top;
            for (int r = top; r <= bottom; ++r) out.push_back(matrix[r][right]);
            --right;
            if (top <= bottom) {                                   // a bottom row still exists
                for (int c = right; c >= left; --c) out.push_back(matrix[bottom][c]);
                --bottom;
            }
            if (left <= right) {                                   // a left column still exists
                for (int r = bottom; r >= top; --r) out.push_back(matrix[r][left]);
                ++left;
            }
        }
        return out;
    }
};
```

Dry run: `[[1,2,3],[4,5,6],[7,8,9]]` → top row `1,2,3`; right column `6,9`; bottom row `8,7`; left column `4`; then inner `5` → `[1,2,3,6,9,8,7,4,5]`.

**H. Correctness and complexity:** *Invariant:* the unvisited cells form `[top..bottom] × [left..right]`; each iteration removes the outer ring (or its remaining part) exactly once. The guards stop duplicates for a single remaining row/column. Time O(mn); auxiliary space O(1) (output excluded).

**I. Reusable template:** *Matrix boundaries* (Part 2.16). Init: four bounds. Loop: four directional sweeps, shrink after each. **What generalizes:** Spiral Matrix II (fill), rotate image (ring by ring). **What changes:** the per-cell action.

**J. Common mistakes:** missing one or both guards (duplicates for single row/col); wrong inclusive bounds; failing to update `right` before the bottom row.

**K. Variation:** *Generate an `n × n` spiral (LC 59).* Same traversal, assign `counter++` instead of reading. Assumption: rectangular matrix. For non-rectangular/jagged rows the boundary model fails; simulate with direction vectors and a visited set.

**L. Recall questions**
- **Q48.1:** Which two `if` guards are needed, and what bug do they prevent?
- **Q48.2:** How do you fill an `n × n` matrix with `1..n²` in spiral order?

---

### Problem 49: Insert Interval

**A. Identification:** LeetCode **57** · Medium · Primary: linear interval sweep (three phases) · Secondary: merging overlaps

**B. Statement:** `intervals` is sorted by start and non-overlapping. Insert `newInterval`, merging overlaps, and return the sorted non-overlapping result.

**C. Pattern recognition**
- Clues: input already sorted & disjoint ⇒ no sort needed.
- First question: "Which intervals end before the new one starts, which overlap it, and which start after it ends?" → three contiguous groups.
- **Rule:** *Sorted disjoint + insert ⇒ copy left group, merge overlap group, copy right group (O(n)).*

**D. Brute force:** Append, sort, then merge (Problem 50): O(n log n).

**E. Optimization reasoning:** Because the list is sorted and disjoint, the intervals divide into: (1) `end < new.start` (entirely left), (2) overlap (`start ≤ new.end` and `end ≥ new.start`), (3) `start > new.end` (entirely right). Merge group 2 into `new` by taking `min` of starts and `max` of ends. Touching intervals (`end == new.start`) count as overlapping here (the standard problem merges them).

**F. Algorithm**
1. Copy all intervals with `end < new.start`.
2. While `start ≤ new.end`: `new.start = min(...)`, `new.end = max(...)`.
3. Push `new`.
4. Copy the rest.

**G. C++17 solution**

```cpp
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        vector<vector<int>> result;
        size_t i = 0, n = intervals.size();
        while (i < n && intervals[i][1] < newInterval[0])            // strictly before
            result.push_back(intervals[i++]);
        while (i < n && intervals[i][0] <= newInterval[1]) {         // overlapping (touching counts)
            newInterval[0] = min(newInterval[0], intervals[i][0]);
            newInterval[1] = max(newInterval[1], intervals[i][1]);
            ++i;
        }
        result.push_back(newInterval);
        while (i < n) result.push_back(intervals[i++]);              // strictly after
        return result;
    }
};
```

Dry run: `[[1,3],[6,9]]`, new `[2,5]`: `[1,3]` ends at 3 ≥ 2 → not "before"; overlap: start 1 ≤ 5 → new=`[1,5]`; `[6,9]`: 6 > 5 → stop; push `[1,5]`, then `[6,9]` → `[[1,5],[6,9]]`.

**H. Correctness and complexity:** *Invariant:* `result` holds the finished prefix of the answer; phase 2 maintains `newInterval` as the union of itself with all overlapped intervals. Sortedness guarantees overlapping intervals are contiguous. Time O(n); space O(1) auxiliary besides output.

**I. Reusable template:** *Three-phase sweep.* Init: index `i`. Phase A copy; phase B merge; phase C copy. **What generalizes:** removing an interval range, adding a calendar booking if no overlap. **What changes:** overlap test and merge function.

**J. Common mistakes:** using `<=` vs `<` wrongly at the boundaries (touching intervals); forgetting to push the merged interval; appending the new interval at the wrong position; modifying while iterating by index.

**K. Variation:** *Intervals are unsorted or overlapping.* The three-group argument fails; sort first (O(n log n)), then use the merge algorithm (Problem 50). Assumption for the O(n) method: sorted + disjoint. If "touching intervals don't merge" (e.g., half-open `[a,b)` semantics treated differently), adjust `<=` to `<`.

**L. Recall questions**
- **Q49.1:** Why is no sorting needed here?
- **Q49.2:** Which comparison decides whether an interval is entirely before the new one?

---

### Problem 50: Merge Intervals

**A. Identification:** LeetCode **56** · Medium · Primary: sort by start + linear sweep · Secondary: greedy interval union

**B. Statement:** Given a list of intervals `[start, end]`, merge all overlapping intervals and return the non-overlapping set that covers the same points. Intervals that touch (`[1,4]`, `[4,5]`) are considered overlapping and merge.

**C. Pattern recognition**
- Clues: "overlap", "merge", unsorted intervals.
- First question: "After sorting by start, can overlap with the *current group* be decided by comparing to its end only?" Yes.
- **Rule:** *Interval union ⇒ sort by start; extend the last merged interval or start a new one.*

**D. Brute force:** Repeatedly scan pairs and merge until nothing changes: O(n²) or worse.

**E. Optimization reasoning:** After sorting by start, any interval overlapping the current merged block must start at or before that block's end (all earlier starts are already absorbed). If `cur.start ≤ last.end` → `last.end = max(last.end, cur.end)`; otherwise it's a new block. Taking `max` for the end is essential (a long interval may contain later short ones).

**F. Algorithm**
1. Sort by start.
2. For each interval: if result empty or `result.back().end < cur.start` → push; else `back.end = max(back.end, cur.end)`.

**G. C++17 solution**

```cpp
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());                  // by start, then end
        vector<vector<int>> merged;
        for (const auto& cur : intervals) {
            if (merged.empty() || merged.back()[1] < cur[0])       // gap: start a new block
                merged.push_back(cur);
            else                                                   // overlap or touch: extend
                merged.back()[1] = max(merged.back()[1], cur[1]);
        }
        return merged;
    }
};
```

Dry run: `[[1,3],[2,6],[8,10],[15,18]]` → `[1,3]`; `[2,6]` overlaps → `[1,6]`; `[8,10]` gap → new; `[15,18]` gap → new → `[[1,6],[8,10],[15,18]]`.

**H. Correctness and complexity:** *Invariant:* `merged` is sorted and disjoint and covers exactly the union of the processed intervals. Sorting ensures no later interval starts before the last block's start, so only `merged.back()` can overlap. Time O(n log n); auxiliary space O(1) besides output (and the sort).

**I. Reusable template:** *Interval merge skeleton* (Part 2.17). Init: sort. Loop: gap → push; overlap → extend with `max`. **What generalizes:** Meeting Rooms, Non-overlapping Intervals, Interval List Intersections. **What changes:** the overlap predicate (`<` vs `<=`) and the action on overlap.

**J. Common mistakes:** not sorting; setting `end = cur.end` instead of `max`; `<` vs `<=` mistakes at touching boundaries; modifying `intervals` while iterating without need.

**K. Variation:** *Touching intervals must stay separate (`[1,4]`,`[4,5]` do not merge).* Change the test to `merged.back()[1] <= cur[0]` for a new block (treat equality as a gap). Assumption: sorting by start makes overlap checks local. For huge coordinate ranges with few intervals this still works; for repeated insertions use an ordered set/interval tree.

**L. Recall questions**
- **Q50.1:** Why is `max(last.end, cur.end)` required and not just `cur.end`?
- **Q50.2:** What single comparison change keeps touching intervals separate?

---
## Part 4: Pattern Comparison Tables

### 4.1 Hash map versus sorting-based solutions

| Aspect | Hash map / set | Sorting + scan |
|---|---|---|
| Time | O(n) expected; O(n²) worst case under adversarial hashing | O(n log n) guaranteed |
| Extra space | O(n) | O(1) to O(log n) if sorting in place |
| Preserves original indices? | Yes (store index as value) | No, unless you sort `(value, index)` pairs |
| Output uniqueness (triplets, pairs) | Awkward (needs dedup structure) | Natural: duplicates are adjacent, skip them |
| Streaming / online input | Works | Doesn't (needs the whole array) |
| **Choose hash when** | Need indices, single pair/lookup, input unsorted, extra memory fine, or online queries | |
| **Choose sorting when** | Need all unique tuples (3Sum, 4Sum), memory is tight, worst-case guarantees matter, or input is already sorted | |
| Example | Two Sum (1), Longest Consecutive (7) | 3Sum (13), Merge Intervals (50) |

### 4.2 Two pointers versus sliding window

| Aspect | Opposite-end two pointers | Sliding window (same direction) |
|---|---|---|
| Pointer motion | Move toward each other | Both move left→right, `left ≤ right` |
| What defines the object | A *pair* `(l, r)` | A *contiguous range* `[l, r]` |
| Why pointers can move | Sorted/dominance order proves one end is useless | Monotone validity: shrinking never breaks (longest) or growing never breaks (shortest) |
| Typical input | Sorted array, palindromes, container-like objectives | Subarray/substring with a property |
| State | Usually none or a running max | Sum, counts, frequency table, last-seen map |
| **Use two pointers when** | The answer involves two *endpoints* and a monotone comparison | |
| **Use a window when** | The answer involves everything *between* two endpoints and the property can be updated incrementally | |
| Examples | 11, 12, 13, 14, 18, 19 | 24, 25, 28, 29, 30, 43 |

### 4.3 Fixed-size versus variable-size windows

| Aspect | Fixed-size | Variable-size |
|---|---|---|
| Window length | Given (`k`) | Discovered (longest/shortest/count) |
| Pointer rule | Right advances; left = `right - k + 1` | Right advances; left advances while condition demands |
| Loop type | One loop, add and remove each step | Outer `for` + inner `while` |
| Complexity argument | Each element added and removed once | Each pointer moves ≤ n times, so amortized O(n) |
| Needs monotone validity? | No | **Yes** |
| Examples | 22, 23, 26, 27 | 24, 25, 28, 29, 30, 43 |
| **Warning** | Needs an invertible aggregate (or deque for max/min) | Predicate must be monotone; "exactly K" must be done as `atMost(K) − atMost(K−1)` |

### 4.4 Prefix sums versus sliding windows

| Aspect | Prefix sums (+ structures) | Sliding window |
|---|---|---|
| Handles negatives | **Yes** | **Generally no** (sum not monotone) |
| Handles "exactly K" counting | Yes (with hash map) | Only for non-negative via `atMost(K) − atMost(K−1)` |
| Space | O(n) (array or map) | O(1) for sums; O(Σ) for counts |
| Multiple range queries | Natural (O(1) each) | Not applicable (one pass answers one question) |
| Streaming single pass | Yes with running prefix + map | Yes |
| **Prefer prefix sums when** | Arbitrary signs, many queries, or the condition is an *equality* on a difference of prefixes | |
| **Prefer window when** | All values non-negative (or monotone predicate) and you want O(1) extra space | |

### 4.5 Prefix sum alone versus prefix sum plus hash map

| Question type | Prefix alone | Prefix + hash map |
|---|---|---|
| "Sum of range `[l, r]`" (queries given) | ✔ O(1) per query | Not needed |
| "Is there a split point where left = right" | ✔ Running prefix + total | Not needed |
| "How many subarrays have sum K" | ✘ O(n²) pairs | ✔ Count of `pre − K` |
| "Longest subarray with sum K" | ✘ | ✔ Earliest index of `pre − K` |
| "Subarray sum divisible by K" | ✘ | ✔ Remainder frequency/earliest index |
| Rule | The *query endpoints are known* | The endpoints are *unknown* and must be found by a value relation |

### 4.6 Kadane's algorithm versus prefix-sum reasoning

| Aspect | Kadane | Prefix-sum reasoning |
|---|---|---|
| Question | Max sum of contiguous subarray (no length/value constraint) | Sum equals K, divisible by K, split points, constrained length |
| Core idea | Best ending at `i` = extend or restart | `sum(l..r) = P[r+1] − P[l]` |
| Equivalent view | `max_r (P[r+1] − min_{l≤r} P[l])` | Same, with a running minimum |
| Space | O(1) | O(1) to O(n) |
| **Choose Kadane when** | Objective is "max sum", decomposition is local | |
| **Choose prefix when** | Condition ties two prefixes by *equality*, remainder, or a bounded length | |
| Fails when | Length bound `≤ L` (use prefix + monotonic deque) | Objective is a product (prefix/suffix products or max/min tracking, Problem 42) |

### 4.7 In-place modification versus auxiliary storage

| Aspect | In-place | Auxiliary storage |
|---|---|---|
| Space | O(1) | O(n) or O(mn) |
| Simplicity | Tricky (ordering of writes, markers) | Simple, easier to prove |
| Risk | Overwriting unread data; destroying markers | Memory limits |
| Input must be mutable? | Yes | No |
| **Use in-place when** | The problem demands O(1) space and allows mutation | |
| **Use auxiliary when** | Input is read-only, correctness is more valuable than space, or n is small | |
| Examples | 9, 10, 15, 16, 17, 44, 45, 47 | 1, 2, 5, 7, 40 (explicit tables), 46 (Floyd avoids even this) |

### 4.8 Interval insertion versus interval merging

| Aspect | Insert Interval (49) | Merge Intervals (50) |
|---|---|---|
| Input | Sorted, disjoint + one new | Arbitrary list, may overlap |
| Sort needed | No | **Yes** (by start) |
| Time | O(n) | O(n log n) |
| Technique | Three phases (before / overlap / after) | Sweep with the last merged interval |
| Boundary rule | `end < new.start` is "before"; `start ≤ new.end` is "overlap" | `last.end < cur.start` starts a new block |
| **Use insertion logic when** | The list is guaranteed sorted & disjoint | |
| **Use merging when** | Anything else (or many intervals to combine) | |

### 4.9 Why ordinary sum-based sliding windows are unsafe with negatives

A sum window works because of **monotonicity**: extending the window never decreases the sum, and shrinking it never increases the sum. That lets you say "sum too big ⇒ drop left; sum too small ⇒ add right" and never need to revisit a discarded left end.

With negative numbers that fails: extending can *decrease* the sum, and dropping the left element can *increase* it. A decision to move `left` (or not) can discard the optimal answer.

**Concrete failure (shortest subarray with sum ≥ K):** `nums = [84, -37, 32, 40, 95]`, `K = 167`. The true answer is `[32, 40, 95]` (length 3). A "grow, then shrink while valid" window reaches `sum = 214` at the end, records length 5, removes `84` to get `130 < 167`, and stops. It never drops `-37`, because dropping it requires shrinking while the sum is *below* K, which the window rule forbids. It returns 5.

**Concrete failure (count subarrays with sum = 1):** `nums = [1, -1, 1]`, `K = 1`. The valid subarrays are `[1]`, `[1,-1,1]`, `[1]` (3 total). A one-`left` window can only report one valid left end per `right`, but when the sum returns to the same value there can be *several* valid left ends.

**What works instead:** prefix sums with a hash map. The condition `P[r+1] − P[l] = K` is an *equality* between two prefix values, so you look up `P[r+1] − K` among earlier prefixes regardless of signs: count (Problem 34), earliest index (Problems 35, 37, 38), or remainder (36). For inequalities like "sum ≥ K" with negatives, use a monotonic deque over prefix sums (not required Day 1).

---

## Part 5: C++17 Template Library

> **Read before copying.** These are *skeletons*, not universal solutions. Each lists what to customize and the assumptions that make it correct. If an assumption fails (for example, sliding window with negatives), switch templates rather than patching.

```cpp
#include <bits/stdc++.h>
using namespace std;

// =====================================================================
// T1. FREQUENCY MAP
// Customize: key type T; use array<int,26> instead for small alphabets.
// Assumes: T hashable. Worst-case hashing is O(n) per op; expected O(1).
// =====================================================================
template <class T>
unordered_map<T, int> frequencyMap(const vector<T>& a) {
    unordered_map<T, int> freq;
    for (const T& x : a) ++freq[x];
    return freq;
}

// =====================================================================
// T2. COMPLEMENT LOOKUP (unsorted Two Sum)
// Customize: the formula for `need`; what to store (index/count).
// Assumes: one pass, insert AFTER query so an element can't pair with itself.
// =====================================================================
pair<int, int> complementLookup(const vector<int>& a, long long target) {
    unordered_map<long long, int> seen;                // value -> index
    for (int i = 0; i < (int)a.size(); ++i) {
        long long need = target - a[i];                // CUSTOMIZE
        auto it = seen.find(need);
        if (it != seen.end()) return {it->second, i};
        seen[a[i]] = i;
    }
    return {-1, -1};
}

// =====================================================================
// T3. SORT + TWO POINTERS (unique value pairs with given sum)
// Customize: target relation; which pointer to move; duplicate skipping.
// Assumes: sorting is allowed; indices are not needed.
// =====================================================================
vector<pair<int, int>> uniquePairsWithSum(vector<int> a, long long target) {
    sort(a.begin(), a.end());
    vector<pair<int, int>> out;
    int l = 0, r = (int)a.size() - 1;
    while (l < r) {
        long long s = (long long)a[l] + a[r];
        if (s < target) ++l;
        else if (s > target) --r;
        else {
            out.push_back({a[l], a[r]});
            ++l; --r;
            while (l < r && a[l] == a[l - 1]) ++l;     // skip duplicates on both ends
            while (l < r && a[r] == a[r + 1]) --r;
        }
    }
    return out;
}

// =====================================================================
// T4. SLOW/FAST IN-PLACE (keep at most K copies in a SORTED array)
// Customize: K (1 for LC 26, 2 for LC 80) or replace the keep-test.
// Assumes: sorted input so equal values are adjacent; compare with
//          a[write - K] (already finalized), never with a[read - 1].
// =====================================================================
int dedupeSortedAtMost(vector<int>& a, int K) {
    int write = 0;
    for (int x : a)
        if (write < K || x != a[write - K]) a[write++] = x;
    return write;
}

// =====================================================================
// T5. FIXED-SIZE SLIDING WINDOW
// Customize: add(i), remove(i), and how `result` is read from state.
// Assumes: 1 <= k <= n and add/remove are exact inverses.
// =====================================================================
template <class Add, class Remove, class Read>
auto fixedWindow(int n, int k, Add add, Remove rem, Read read) {
    for (int i = 0; i < k; ++i) add(i);
    auto best = read();
    for (int i = k; i < n; ++i) {
        add(i);
        rem(i - k);
        best = max(best, read());                       // CUSTOMIZE: max/min/any/collect
    }
    return best;
}
// Example: long long sum = 0; auto best = fixedWindow((int)a.size(), k,
//   [&](int i){ sum += a[i]; }, [&](int i){ sum -= a[i]; }, [&]{ return sum; });

// =====================================================================
// T6. VARIABLE-SIZE SLIDING WINDOW (longest valid / shortest valid)
// Customize: add, remove, valid.
// Assumes: valid() is MONOTONE (shrinking keeps a valid window valid for
//          "longest"; growing keeps it valid for "shortest").
//          NOT safe for sum predicates when negatives are present.
// =====================================================================
template <class Add, class Remove, class Valid>
int longestValidWindow(int n, Add add, Remove rem, Valid valid) {
    int left = 0, best = 0;
    for (int right = 0; right < n; ++right) {
        add(right);
        while (!valid()) rem(left++);
        best = max(best, right - left + 1);
    }
    return best;
}

template <class Add, class Remove, class Valid>
int shortestValidWindow(int n, Add add, Remove rem, Valid valid) {
    int left = 0, best = INT_MAX;
    for (int right = 0; right < n; ++right) {
        add(right);
        while (left <= right && valid()) {
            best = min(best, right - left + 1);
            rem(left++);
        }
    }
    return best == INT_MAX ? 0 : best;
}

// =====================================================================
// T7. FREQUENCY-MAP WINDOW (smallest window covering all of t)
// Customize: alphabet size (256 here), what "covered" means.
// Assumes: coverage is monotone and depends only on counts (not order).
// =====================================================================
string coverWindow(const string& s, const string& t) {
    if (t.empty() || s.size() < t.size()) return "";
    vector<int> need(256, 0);
    for (unsigned char c : t) ++need[c];
    int missing = (int)t.size(), bestLen = INT_MAX, bestStart = 0, left = 0;
    for (int right = 0; right < (int)s.size(); ++right) {
        if (need[(unsigned char)s[right]]-- > 0) --missing;
        while (missing == 0) {
            if (right - left + 1 < bestLen) { bestLen = right - left + 1; bestStart = left; }
            if (++need[(unsigned char)s[left]] > 0) ++missing;
            ++left;
        }
    }
    return bestLen == INT_MAX ? "" : s.substr(bestStart, bestLen);
}

// =====================================================================
// T8. PREFIX-SUM FREQUENCY MAP (count subarrays with sum == K)
// Customize: key (prefix, balance, remainder) and the lookup (pre - K).
// Assumes: condition is an EQUALITY between two prefix keys. Signs are arbitrary.
// For remainder keys normalise with ((x % k) + k) % k.
// =====================================================================
long long countSubarraysWithSum(const vector<int>& a, long long K) {
    unordered_map<long long, long long> freq{{0, 1}};   // empty prefix
    long long pre = 0, count = 0;
    for (int x : a) {
        pre += x;
        auto it = freq.find(pre - K);                   // CUSTOMIZE
        if (it != freq.end()) count += it->second;
        ++freq[pre];                                    // insert AFTER the query
    }
    return count;
}

// =====================================================================
// T9. PREFIX-SUM EARLIEST-INDEX MAP (longest subarray with sum == K)
// Customize: key and lookup as in T8; add a length filter if required.
// Assumes: equality condition. Use emplace so the EARLIEST index is kept.
// =====================================================================
int longestSubarrayWithSum(const vector<int>& a, long long K) {
    unordered_map<long long, int> first{{0, -1}};
    long long pre = 0;
    int best = 0;
    for (int i = 0; i < (int)a.size(); ++i) {
        pre += a[i];
        auto it = first.find(pre - K);
        if (it != first.end()) best = max(best, i - it->second);
        first.emplace(pre, i);                          // does not overwrite
    }
    return best;
}

// =====================================================================
// T10. PREFIX / SUFFIX "EXCEPT SELF" (generic operator)
// Customize: identity and op (multiply -> 1, max -> INT_MIN, gcd -> 0).
// Assumes: op is associative. Needs no inverse. Beware overflow.
// =====================================================================
template <class T, class Op>
vector<T> exceptSelf(const vector<T>& a, T identity, Op op) {
    int n = (int)a.size();
    vector<T> pre(n + 1, identity), suf(n + 1, identity), out(n);
    for (int i = 0; i < n; ++i)     pre[i + 1] = op(pre[i], a[i]);
    for (int i = n - 1; i >= 0; --i) suf[i] = op(a[i], suf[i + 1]);
    for (int i = 0; i < n; ++i)     out[i] = op(pre[i], suf[i + 1]);
    return out;
}

// =====================================================================
// T11. KADANE (max subarray sum, with indices)
// Customize: the "restart" rule. Product needs two states (see LC 152).
// Assumes: subarray must be non-empty; n >= 1; no length constraint.
// =====================================================================
struct KadaneResult { long long sum; int left, right; };
KadaneResult kadaneWithIndices(const vector<int>& a) {
    long long cur = a[0], best = a[0];
    int curStart = 0, bestL = 0, bestR = 0;
    for (int i = 1; i < (int)a.size(); ++i) {
        if (cur < 0) { cur = a[i]; curStart = i; }      // restart (equivalent to max(a[i], cur+a[i]))
        else cur += a[i];
        if (cur > best) { best = cur; bestL = curStart; bestR = i; }
    }
    return {best, bestL, bestR};
}

// =====================================================================
// T12. MATRIX BOUNDARY TRAVERSAL (spiral with a visitor)
// Customize: visit(r, c). Keep both guards to avoid double visits.
// Assumes: rectangular matrix.
// =====================================================================
template <class Visit>
void spiralTraverse(int rows, int cols, Visit visit) {
    int top = 0, bottom = rows - 1, left = 0, right = cols - 1;
    while (top <= bottom && left <= right) {
        for (int c = left; c <= right; ++c) visit(top, c);
        ++top;
        for (int r = top; r <= bottom; ++r) visit(r, right);
        --right;
        if (top <= bottom) { for (int c = right; c >= left; --c) visit(bottom, c); --bottom; }
        if (left <= right) { for (int r = bottom; r >= top; --r) visit(r, left); ++left; }
    }
}

// =====================================================================
// T13. INTERVAL MERGE SKELETON
// Customize: the overlap test. `<` merges touching intervals ([1,4],[4,5]);
//            `<=` keeps touching intervals separate.
// Assumes: sorting by start is allowed. If already sorted and disjoint
//          plus one insertion, use the O(n) three-phase scan instead.
// =====================================================================
vector<vector<int>> mergeIntervals(vector<vector<int>> iv) {
    sort(iv.begin(), iv.end());
    vector<vector<int>> out;
    for (auto& cur : iv) {
        if (out.empty() || out.back()[1] < cur[0]) out.push_back(cur);   // CUSTOMIZE: < or <=
        else out.back()[1] = max(out.back()[1], cur[1]);
    }
    return out;
}
```

---

## Part 6: How to Start Coding Any DSA Problem

### 6.1 The pre-coding checklist

Spend 3 to 5 minutes on this before typing code. Write answers as comments.

| # | Question | What you're really deciding |
|---|---|---|
| 1 | What exactly is being asked? | Output type (count/boolean/indices/values), uniqueness, order, empty inputs |
| 2 | What are the constraints? | `n` size ⇒ allowed complexity (n ≤ 10⁵ ⇒ O(n log n); n ≤ 10³ ⇒ O(n²)); value ranges ⇒ overflow, counting arrays |
| 3 | What would brute force look like? | A correct baseline and its cost |
| 4 | What repeated work can be removed? | The redundant inner loop is the optimization target |
| 5 | Is the input sorted or structured? | Sorted ⇒ pointers/binary search; values in `[1..n]` ⇒ index tricks; small alphabet ⇒ arrays |
| 6 | Is it about contiguous ranges? | Yes ⇒ window / prefix sums / Kadane. No ⇒ hashing / sorting / pointers |
| 7 | Can the answer be expressed with previous states or cumulative information? | Prefix aggregates, running min/max, DP-like state |
| 8 | Which data structure preserves the information I need? | Set (membership), map (counts/first index), array (small domain), deque/heap (later) |
| 9 | What invariant stays true? | The sentence that justifies each pointer/state update |
| 10 | What is the complexity? | Time/space, worst vs expected |
| 11 | Which edge cases can break it? | Empty/size-1, all equal, all negative, zeros, duplicates, overflow, off-by-one |

### 6.2 The checklist applied to four Day 1 problems

**3Sum (Problem 13), pattern: sort + pointers**
1. Return all unique value triplets summing to 0.
2. `n ≤ 3000` ⇒ O(n²) is fine; values small enough for `int`, but use `long long` for sums if unsure.
3. Brute force: three loops, O(n³), dedupe with a set.
4. Repeated work: for a fixed first element, the pair search repeats; that is Two Sum.
5. Not sorted, but order of output doesn't matter ⇒ sort.
6. Not about contiguity.
7. After sorting, for a fixed anchor the pair sum is monotone in pointer positions.
8. No extra structure; the sorted array itself.
9. Invariant: any solution for this anchor lies within `[l, r]`; duplicates skipped by value.
10. O(n²) time, O(1) extra.
11. Duplicates (`[0,0,0,0]`), fewer than 3 elements, all positives, `INT` overflow.

**Subarray Sum Equals K (Problem 34), pattern: prefix + hash map**
1. Count subarrays with sum exactly `k`.
2. `n ≤ 2·10⁴`; values can be negative ⇒ windows invalid; sums fit but use `long long` for safety.
3. Brute force: O(n²) running sums.
4. Repeated work: re-summing ranges; a range sum is a difference of two prefixes.
5. Not sorted; negatives present.
6. Yes, contiguous ⇒ prefix sums.
7. `sum(l..r) = P[r+1] − P[l]` ⇒ need `P[l] = P[r+1] − k`.
8. Hash map from prefix value → count of occurrences.
9. Invariant: map holds all prefixes strictly before the current one (plus the empty prefix).
10. O(n) expected time, O(n) space.
11. `k = 0`, zeros and negatives (repeated prefixes), forgetting `{0:1}`.

**Longest Repeating Character Replacement (Problem 25), pattern: variable window with frequency**
1. Longest substring that can become uniform with ≤ `k` replacements.
2. `n ≤ 10⁵` ⇒ O(n) or O(n log n); uppercase only (26 letters).
3. Brute force: all substrings, count dominant letter: O(n²).
4. Repeated work: recounting letters for overlapping substrings.
5. Not sorted, small alphabet.
6. Contiguous ⇒ window.
7. Cost of a window = `len − maxFreq`; validity is monotone (a sub-window of a valid window is valid).
8. `count[26]`, `maxFreq`.
9. Invariant: window size never decreases, and a larger window is recorded only when a true dominant count supports it.
10. O(n) time, O(26) space.
11. `k = 0`, all same letters, `k ≥ n`, why stale `maxFreq` is harmless.

**Set Matrix Zeroes (Problem 47), pattern: in-place marking**
1. Zero out rows and columns of original zeros.
2. `m, n ≤ 200`; in place; O(1) space follow-up.
3. Brute force: copy matrix, scan, zero rows/cols: O(mn) space.
4. Repeated work/waste: copying; or O(m+n) flag arrays.
5. No order, but there is *free storage already in the matrix* (row 0, column 0).
6. Not about ranges.
7. Flags for rows/columns can be stored in the first row/column.
8. Border cells as flags + two booleans for the border itself.
9. Invariant: the interior decisions depend only on flags written before any zeroing of the border.
10. O(mn) time, O(1) space.
11. Zero in row 0 or column 0 only, matrix of one row/column, all zeros.

---
## Part 7: Day 1 Revision Sheet

Use this for a final 15 to 20 minute review: **(1)** read the table (5 min), **(2)** cover the right-hand columns and recite them from the pattern name (5 min), **(3)** write two templates from memory (5 min), **(4)** pick one blind re-solve from the list below.

### 7.1 Patterns, clues, rules, templates, complexity, traps

| Pattern | Recognition clue | One-line rule | Template | Time / aux. space | Common trap |
|---|---|---|---|---|---|
| Hash set | "seen before?", duplicates, consecutive values | Membership in O(1) replaces an inner scan | Seen-set scan | O(n) exp. / O(n) | Worst-case hashing is O(n) per op |
| Frequency counting | anagram, permutation, majority | Order-independent signature = counts | `array<int,26>` / map | O(n) / O(Σ) | Length check; alphabet assumptions |
| Complement lookup | pair equals target, unsorted | Look up `target − x`, insert after | T2 | O(n) exp. / O(n) | Self-pairing; indices vs values |
| Sort + pointers | pairs/triplets, unique results | Sort, anchor, squeeze, skip duplicates | T3 / 3Sum | O(n log n + n^(k−1)) / O(1) | Skipping duplicates *before* recording |
| Opposite-end pointers | sorted pair, palindrome, container | Discard the end that cannot improve | `pairSumSorted` | O(n) / O(1) | Moving the wrong pointer |
| Slow/fast compaction | in place, remove/move | Write only what survives; compare with last *kept* | `compactInPlace`, T4 | O(n) / O(1) | Comparing with an overwritten cell |
| Dutch flag | 3 classes, in place | `low/mid/high`; don't advance `mid` after swap with `high` | Dutch flag | O(n) / O(1) | Advancing `mid` incorrectly |
| Index-as-hash | values in `[1..n]`, O(1) space | Put `v` at `v−1` with `while` swaps | Cyclic placement | O(n) / O(1) | `if` instead of `while`; duplicates loop |
| Fixed window | length exactly `k` | Add right, remove `i−k` | T5 | O(n) / O(1)–O(Σ) | Non-invertible aggregate (max) |
| Variable window | longest/shortest/count, monotone validity | Expand always; shrink only to restore/record | T6 | O(n) / state | Negatives; non-monotone predicates |
| Frequency window | covers all of `t`, ≤ k edits, anagram window | Maintain a derived counter (`missing`, `maxFreq`) | T7 | O(n) / O(Σ) | Multiplicities; stale-state subtleties |
| Prefix sums | many range sums, split points | `sum(l..r)=P[r+1]−P[l]` | `buildPrefix` | O(n) / O(n) | Missing leading 0; off-by-one |
| Prefix + counts map | count subarrays with sum/remainder/balance | Look up `pre − K` before inserting; seed `{0:1}` | T8 | O(n) exp. / O(n) | Windows with negatives; seed missing |
| Prefix + earliest map | longest subarray with sum/balance/remainder | Keep the first index only; seed `{0:−1}` | T9 | O(n) exp. / O(n) | Overwriting the first index |
| Prefix/suffix products | "except self" | `out[i] = prefix[i]·suffix[i+1]` | T10 | O(n) / O(1) extra | Division with zeros; overflow |
| Kadane | max subarray sum | `cur = max(x, cur + x)` | T11 | O(n) / O(1) | Init to 0 with all-negative input |
| Max/min product | product with negatives | Track both; swap on negative | `maxProduct` | O(n) / O(1) | Tracking only max |
| Boundary two-pointer | trapped water, area | Process the side with the smaller boundary | Trap pattern | O(n) / O(1) | Wrong side processed |
| Merge from back | in-place merge with tail space | Write largest first from the end | `merge` | O(m+n) / O(1) | Front merge overwrites |
| Reverse trick | rotate array | Reverse all, then both blocks; `k %= n` | `rotate` | O(n) / O(1) | Not reducing `k` |
| Floyd cycle | `n+1` values in `[1..n]`, read-only | Array as linked list; find cycle entrance | Floyd | O(n) / O(1) | Phase 2 reset point |
| Matrix markers | in place, zero rows/cols | Border as flags + 2 booleans | `setZeroes` | O(mn) / O(1) | Zeroing the border early |
| Matrix boundaries | spiral | Four bounds + two guards | T12 | O(mn) / O(1) | Missing guards |
| Interval merge | overlaps | Sort by start, extend with `max` end | T13 | O(n log n) / O(1) extra | `<` vs `<=`; using `cur.end` |
| Interval insert | sorted disjoint + insert | Before / overlap / after | three-phase | O(n) / O(1) extra | Boundary comparison |

### 7.2 Complexity reminders

- Sliding windows are O(n) because **each pointer moves at most n times**, not because the loops aren't nested.
- Hash-based O(n) means *expected*; sorting is the guaranteed alternative at O(n log n).
- Auxiliary space excludes the output array; say so explicitly in interviews.
- k-Sum with sorting: 2Sum O(n), 3Sum O(n²), 4Sum O(n³).
- Prefix sums: O(n) build, O(1) query, but no updates.

### 7.3 Top ten traps of the day

1. Sliding window on arrays with negatives (28, 34 to 38).
2. Forgetting seeds `{0:1}` (counts) and `{0:−1}` (earliest index).
3. Overwriting the earliest index in the map (35, 37, 38).
4. Inserting into the lookup structure *before* querying (1, 34).
5. Skipping duplicates before recording (13, 20).
6. `if` instead of `while` (10, 28, 29, 43).
7. `left = last + 1` without `max` (24).
8. Negative remainders in C++ `%` (36, 37).
9. Integer overflow in sums/products (12, 20, 22, 39, 42).
10. Off-by-one at the final index (39 uses `n−2`; 48 guards; 31 needs `n+1`).

### 7.4 Problems that deserve blind re-solving

| Priority | Problems | Why |
|---|---|---|
| Must (same day or next morning) | 10, 13, 19, 24, 25, 30, 34, 35, 42, 46, 47, 50 | Highest insight density; each exposes a distinct template |
| Should | 4, 5, 7, 9, 14, 28, 36, 37, 38, 49 | Common interview themes with subtle boundaries |
| Quick drill only | 2, 3, 8, 11, 15, 16, 17, 21, 31, 32, 33, 41, 44 | Mostly mechanical; 3-minute drills suffice |

---

## Part 8: Self-Assessment and Answer Key

### 8.1 Recall questions

Each problem in Part 3 ends with two recall questions (L). Their answers are in **8.6**, organized by section. Answer in writing before checking.

### 8.2 Pattern-classification quiz (unlabeled)

Name the pattern/template and a one-line justification. Answers in 8.7.

1. Given positive integers and a threshold `S`, find the length of the shortest contiguous block with sum at least `S`.
2. Given integers (possibly negative), count contiguous blocks whose sum is divisible by 7.
3. A sorted array and a target; find two numbers adding to the target using O(1) extra space.
4. Group words that become identical after rearranging letters.
5. An array of `n + 1` integers in `[1, n]` is read-only; find the repeated number using O(1) space.
6. Find the longest substring containing at most two distinct characters.
7. Given a list of integers, find the maximum sum of a contiguous block.
8. Given meeting intervals in random order, produce the combined busy periods.
9. In a binary array, find the longest block with equal numbers of 0s and 1s.
10. Move all odd numbers to the end of an array while preserving relative order, in place.
11. For every window of length `k` in a bit string, report the best (largest) count of 1s.
12. Compute the total water trapped between elevation bars.
13. Find the value occurring more than half the time, using O(1) space, when one is guaranteed.
14. For each position, output the product of all other positions without division.

### 8.3 "A familiar approach fails because a constraint changed"

Name why the original approach fails and what to use instead. Answers in 8.8.

- **C1.** Minimum Size Subarray Sum (28), but the array now contains negative numbers. Why does the window fail?
- **C2.** Longest Substring Without Repeating Characters (24) becomes "at most 2 distinct characters". Why does the `last[]`-jump trick fail, and what replaces it?
- **C3.** Two Sum II (12) with an **unsorted** array and O(1) extra space not required. Why do pointers fail?
- **C4.** Majority Element (8) when a majority is **not guaranteed**. What must be added?
- **C5.** Subarray Product Less Than K (43) when zeros are allowed. What breaks?
- **C6.** Insert Interval (49) when the existing list is **unsorted** and may overlap. What changes?

### 8.4 Blank template-recall exercise

Fill the blanks (`____`) from memory, then compare with Part 5. These are intentionally incomplete and not compilable.

```cpp
// (a) Complement lookup
unordered_map<long long, int> seen;
for (int i = 0; i < (int)a.size(); ++i) {
    auto it = seen.find(____);
    if (it != seen.end()) return {____, i};
    ____;
}

// (b) Shortest valid window
int left = 0, best = INT_MAX;
for (int right = 0; right < n; ++right) {
    ____;                                  // add right
    while (____) {                         // while valid
        best = min(best, ____);
        ____;                              // remove left
    }
}

// (c) Count subarrays with sum K (prefix + map)
unordered_map<long long, int> freq{{____, ____}};
long long pre = 0; int count = 0;
for (int x : a) {
    pre += x;
    count += freq[____];
    ++freq[____];
}

// (d) Dutch national flag
int low = 0, mid = 0, high = n - 1;
while (____) {
    if (a[mid] == 0)      swap(a[low++], a[____++]);
    else if (a[mid] == 1) ____;
    else                  swap(a[mid], a[____--]);
}

// (e) Interval merge
sort(iv.begin(), iv.end());
for (auto& cur : iv) {
    if (out.empty() || out.back()[1] ____ cur[0]) out.push_back(cur);
    else out.back()[1] = ____(out.back()[1], cur[1]);
}
```

### 8.5 Checklist: do I genuinely understand this pattern?

Tick all boxes for a pattern before calling it learned.

- [ ] I can write its template from a blank file in under 5 minutes.
- [ ] I can state its invariant in one sentence.
- [ ] I can name the clue words that trigger it.
- [ ] I can state its time and auxiliary-space complexity and justify them.
- [ ] I can describe the brute force it replaces and the exact repeated work it removes.
- [ ] I can give a counterexample where the pattern fails (negatives, unsorted, non-monotone).
- [ ] I can solve one variation (different constraint) without looking.
- [ ] I can explain *why* each pointer/state update is safe, not just *that* it works.
- [ ] I tested it on empty, size-1, duplicates, negatives, and an overflow case.
- [ ] I solved a problem using it **the next day** without help.

### 8.6 Answer key: per-problem recall questions

Answer Qx.1 and Qx.2 for problem x. **Cover this section until you have written your own answers.**

**Section 1 (Problems 1 to 10)**

| Q | Answer |
|---|---|
| 1.1 | Otherwise `nums[i]` could match itself (e.g., target 6 and value 3 would pair index `i` with `i`). Inserting after the lookup guarantees the match is an earlier index. |
| 1.2 | Opposite-end two pointers on the sorted array: O(n) after sorting (O(n log n) if you must sort first), O(1) auxiliary. |
| 2.1 | `true` iff the element was newly inserted; `false` means it was already present. |
| 2.2 | Keep a set of the last `k` elements: after processing index `i`, erase `nums[i-k]`; a failed insert means a duplicate within distance k. |
| 3.1 | The loop only reads indices `< s.size()`. If `t` is longer, its extra characters are never counted (false positive); if `t` is shorter, `t[i]` reads out of bounds. Equal lengths make the zero-count test exact. |
| 3.2 | Use a map keyed by the (decoded) code point, e.g., `unordered_map<char32_t,int>`; or sort. The fixed 26-array no longer fits. |
| 4.1 | Without a delimiter, different count vectors can concatenate to the same string (e.g., counts 1,11 vs 11,1). |
| 4.2 | Canonical form of the rotation class: the lexicographically smallest rotation (e.g., via `min` of all rotations or Booth's algorithm). |
| 5.1 | A frequency can be as large as `n`, so valid indices are `1..n`, requiring `n+1` slots. The maximum useful index is `n` (when all elements are equal). |
| 5.2 | When `k` is much smaller than the number of distinct values or the data streams in: O(n log k) time and O(k) heap space; also when frequency range is not bounded by n. |
| 6.1 | Because `out[i]` is assigned `left` before `left` absorbs `nums[i]`, `left` at that moment covers indices `0..i-1` only. |
| 6.2 | Prefix max and suffix max, then `max(prefixMax[i-1], suffixMax[i+1])` with `-inf` sentinels at the ends. |
| 7.1 | Only sequence starts (elements whose predecessor `x-1` is absent) trigger an expansion, so each element is walked in exactly one expansion. |
| 7.2 | Keep a hash map from run endpoints to run length; on insertion, merge the neighbors' runs (or use union-find). |
| 8.1 | Boyer–Moore only guarantees that *if* a majority exists, it is the survivor; it does not prove that one exists, so a verification pass is needed otherwise. |
| 8.2 | Two candidates (Misra–Gries with k−1 = 2 counters), followed by a verification count. |
| 9.1 | The element swapped in from `high` has not been classified yet; advancing `mid` would skip it. |
| 9.2 | Counting sort in O(n + K), or a recursive/radix-style partition; Dutch flag handles only a constant number of classes. |
| 10.1 | With `n` elements, at most `n` distinct positives can be present, so among `1..n+1` at least one is missing; the smallest missing positive cannot exceed `n+1`. |
| 10.2 | The element swapped into position `i` might itself be misplaced; `if` would leave it unprocessed, giving wrong answers. |

**Section 2 (Problems 11 to 20)**

| Q | Answer |
|---|---|
| 11.1 | In an all-punctuation or trailing-punctuation string, the skip loops could run past the other pointer or out of bounds; `l < r` bounds them. |
| 11.2 | On the first mismatch, check whether `s[l+1..r]` or `s[l..r-1]` is a palindrome (plain check); return true if either is. Still O(n). |
| 12.1 | Every possible partner of `a[r]` is at least `a[l]`; since `a[l]+a[r] > target`, every sum involving `a[r]` also exceeds `target`. |
| 12.2 | If `a[l]+a[r] < target`, all pairs `(l, l+1..r)` work: add `r - l`, then `l++`; else `r--`. |
| 13.1 | Skipping first would drop valid triplets whose duplicate value is part of the answer (e.g., `[-1,-1,2]`). Skip only to avoid *repeating* the same recorded value. |
| 13.2 | O(n^(k−1)) for k ≥ 2 after sorting (2-Sum O(n), 3-Sum O(n²), 4-Sum O(n³)). |
| 14.1 | Moving the taller side shrinks width and cannot raise the limiting (shorter) height, so area can't improve. |
| 14.2 | If area depended on all bars between the endpoints (not just on the two endpoint heights), or if the objective weren't dominated by the shorter endpoint. |
| 15.1 | `nums[read-1]` may already be overwritten; `nums[write-1]` is the last kept value and is stable. |
| 15.2 | Keep `nums[read]` when `write < 2 || nums[read] != nums[write-2]`. |
| 16.1 | The value swapped in from the end hasn't been checked; it might also equal `val`. |
| 16.2 | The read/write (compaction) variant. |
| 17.1 | Self-swap (`write == read`) leaves the array unchanged; this only happens while no zero has been seen yet. |
| 17.2 | Scan from right to left with `write` starting at the last index, swapping non-zeros toward the back. |
| 18.1 | The largest unplaced square is always at one of the two ends; placing it at the last open slot is final. |
| 18.2 | `f` is concave: the extremes are the smallest outputs. Fill from the front (ascending order) with the end pointers, comparing `f` at the ends. |
| 19.1 | Because the right side contains a bar at least `h[r] > h[l]`, the right boundary is at least as high as `h[l]`'s needed level; the binding constraint is `leftMax`. |
| 19.2 | `water += max(0, min(leftMax[i], rightMax[i]) - h[i])` summed over `i`. |
| 20.1 | Four values can reach ±4·10⁹, exceeding `int` range. |
| 20.2 | Hash map of pair sums (`A[i]+B[j]` → count) and look up `-(C[k]+D[l])`: O(n²). |

**Section 3 (Problems 21 to 30)**

| Q | Answer |
|---|---|
| 21.1 | You can only buy before you sell, so the buy day for a sell on day `j` must come from `0..j-1`. |
| 21.2 | Sum all positive consecutive differences `max(0, p[i] - p[i-1])`. |
| 22.1 | `k` is fixed, so average = sum / `k`; maximizing one maximizes the other. |
| 22.2 | Non-invertible aggregates like max/min (no O(1) removal); use a monotonic deque. |
| 23.1 | `count` equals the number of vowels in the current length-`k` window. |
| 23.2 | Use a variable window: expand right, shrink left while consonants > m (longest valid). |
| 24.1 | `last[c]` may point to an index left of the current window; taking the plain value could move `left` backwards. |
| 24.2 | A count map (frequency table) plus a distinct counter; shrink while distinct > K. |
| 25.1 | `L - F` replacements (change everything that isn't the dominant letter). |
| 25.2 | The answer only grows when a window of larger size is valid; that requires a genuinely higher dominant frequency, and `maxFreq` only increases when a real count reaches it. |
| 26.1 | A permutation has exactly `|s1|` characters, so the window length is fixed. For minimum window, the valid length is unknown and varies. |
| 26.2 | Maintain a `matches` counter (number of letters with equal counts) updated in O(1) per add/remove. |
| 27.1 | `i - m + 1` (window start). |
| 27.2 | Maintain a mismatch counter (letters whose counts differ) and accept windows where mismatches ≤ the allowed number. |
| 28.1 | Positive values: adding on the right increases the sum, removing on the left decreases it, so window sums are monotone. |
| 28.2 | Negatives break monotonicity (adding may reduce the sum); use prefix sums with a monotonic deque of indices (LC 862). |
| 29.1 | Flipping ≤ k zeros ⇔ the window contains at most k zeros; find the longest such window. |
| 29.2 | Window with at most 1 zero, answer = `len - 1` (one element must be deleted). |
| 30.1 | `need[c] > 0`: the window still lacks `c`; `missing` is the total count of required characters still absent. Window valid iff `missing == 0`. |
| 30.2 | A multiset count ignores order, but a subsequence requires characters in order; coverage is no longer a function of counts. |

**Section 4 (Problems 31 to 40)**

| Q | Answer |
|---|---|
| 31.1 | The extra leading element `P[0] = 0` lets `sumRange(0, r)` use `P[r+1] - P[0]` with no special case. |
| 31.2 | A Fenwick tree (BIT) or segment tree: O(log n) update and query. |
| 32.1 | `rightSum = total - leftSum - nums[i]`. |
| 32.2 | Yes; it relies on equality of two sums, not on monotonicity. |
| 33.1 | Inclusive: `P[i] = a[0..i]`. Exclusive: `P[i] = a[0..i-1]` (prefix excluding index `i`). |
| 33.2 | `std::partial_sum` (in `<numeric>`). |
| 34.1 | It represents the empty prefix, so subarrays starting at index 0 (whose `pre - k = 0`) are counted. |
| 34.2 | Sliding window (sum is monotone with positive numbers): count windows ending at `r` as `r - left + 1` after shrinking while sum > K. |
| 35.1 | Earliest index gives the longest subarray; `-1` represents the balance before the first element so subarrays starting at 0 have the right length. |
| 35.2 | Key by the pair `(c1 - c0, c2 - c1)`, store the earliest index per pair in a hash map. |
| 36.1 | If `P[r+1] ≡ P[l] (mod k)` then `P[r+1] - P[l]` is a multiple of `k`, i.e. the subarray `l..r` has a sum divisible by `k`. |
| 36.2 | Look up `(rem - r + k) % k` in the count table before inserting the current remainder. |
| 37.1 | Keeping the earliest index maximizes `i - j`; overwriting with later indices could miss valid length ≥ 2 subarrays. |
| 37.2 | Multiples of 0 mean sum exactly 0; key by raw prefix sums, require two equal prefixes at distance ≥ 2. Remainder arithmetic is undefined. |
| 38.1 | Assigning overwrites the earliest index, shortening future lengths; we need the first occurrence for the longest subarray. |
| 38.2 | No. The `=` lookup relies on hashing exact prefix values; a `≥` condition requires ordered structures (e.g., monotonic prefix minima + binary search), and plain sliding windows fail with negatives. |
| 39.1 | The right part must be non-empty, so the last valid split index is `n-2`. |
| 39.2 | Sums can reach about 10¹⁰ (10⁵ elements of 10⁵), beyond 32-bit range. |
| 40.1 | Without division there's no inverse of a prefix product; a middle range `[l, r]` would need `P[r+1] / P[l]`. |
| 40.2 | Identity: `-∞` (`INT_MIN` sentinel); combine: `max`. `out[i] = max(prefixMax[i], suffixMax[i+1])` with padded sentinels. |

**Section 5 (Problems 41 to 50)**

| Q | Answer |
|---|---|
| 41.1 | `cur` = the maximum sum among subarrays that end exactly at index `i`. |
| 41.2 | Compute `max(kadaneMax, total - kadaneMin)`; if all elements are negative the second term corresponds to an empty subarray, so return `kadaneMax`. |
| 42.1 | A negative `x` turns the smallest (most negative) previous product into the largest new one. |
| 42.2 | A zero makes both `mx` and `mn` equal 0; the next element restarts from itself via `max(x, mx*x)`. No special case is required. |
| 43.1 | Every subarray ending at `right` with start in `[left, right]` is valid (values ≥ 1, so shorter subarrays have smaller products): that's `right - left + 1` subarrays. |
| 43.2 | A zero makes the product 0 and division by it is invalid; the window can't shrink back by dividing, and monotonicity (extending increases the product) fails. |
| 44.1 | Writing from the front overwrites `nums1` elements not yet merged; the free space is at the back, so the back is the safe place to write. |
| 44.2 | They're already sorted and in the correct relative position at the front of the array. |
| 45.1 | Rotating by `n` returns the original; `k > n` would otherwise index/iterate out of range. Reduce first (and handle `n = 0`). |
| 45.2 | Reverse `[0,k)`, reverse `[k,n)`, then reverse the whole array. |
| 46.1 | All values are in `[1, n]`, so no element points to index 0; index 0 has no incoming edge and cannot be in a cycle. |
| 46.2 | Binary search on the value range with counting (`count(x ≤ mid)`): O(n log n) time, O(1) space, read-only. |
| 47.1 | Marking overwrites the first row/column, destroying the information about whether they originally contained zeros. |
| 47.2 | The newly written zeros are indistinguishable from original zeros and would cascade, zeroing too many rows and columns. |
| 48.1 | `if (top <= bottom)` before the bottom row and `if (left <= right)` before the left column; they prevent re-traversing the same row/column when a single row/column remains. |
| 48.2 | Same boundary loop, but write `counter++` into each visited cell instead of pushing values. |
| 49.1 | The list is already sorted and disjoint, so overlapping intervals are contiguous and found by a single linear scan. |
| 49.2 | `intervals[i][1] < newInterval[0]` (interval ends strictly before the new one starts). |
| 50.1 | A long interval may fully contain later intervals; assigning `cur.end` could shrink the merged end. |
| 50.2 | Use `merged.back()[1] <= cur[0]` as the "new block" condition (equality = gap, no merge). |

### 8.7 Answer key: pattern-classification quiz

| # | Pattern / template | Justification |
|---|---|---|
| 1 | Variable window, *shortest* (T6) | Positive values ⇒ monotone sum; shrink while sum ≥ S |
| 2 | Prefix remainders + frequency map | Equal prefix remainders mod 7 ⇒ divisible subarray; handles negatives (normalise `%`) |
| 3 | Opposite-end two pointers | Sorted + pair sum + O(1) space |
| 4 | Hash map with canonical key (sorted string / counts) | Group by equivalence class |
| 5 | Floyd cycle detection | Read-only, values in `[1..n]`, O(1) space ⇒ array as linked list |
| 6 | Frequency-map variable window (longest) | "At most K distinct" is monotone; shrink while distinct > 2 |
| 7 | Kadane | `cur = max(x, cur + x)` |
| 8 | Interval merge (sort by start, extend with `max`) | Sorting localizes overlap checks |
| 9 | Prefix balance (±1) + earliest-index map | Longest zero-sum block after mapping 0 → −1 |
| 10 | Slow/fast stable partition | Stable compaction: keep evens forward, then move odds (or swap-based stable variant) |
| 11 | Fixed window with counter | Length `k` fixed; add/remove one bit per step |
| 12 | Boundary two pointers | Water depends on `min(leftMax, rightMax)`; process the smaller side |
| 13 | Boyer–Moore voting | Guaranteed strict majority, O(1) space |
| 14 | Prefix/suffix products | Exclusive prefix × suffix, no division |

### 8.8 Answer key: constraint-change questions

| # | Answer |
|---|---|
| C1 | Negatives break monotonicity: adding an element can *decrease* the sum, dropping one can *increase* it, so the shrink/expand decisions are no longer safe (see 4.9, `[84,-37,32,40,95]`, K=167). Use prefix sums with a monotonic deque of increasing prefix values (LC 862), O(n). |
| C2 | A repeated character no longer defines validity; the constraint is about *how many distinct* characters are inside. Use a count map with a distinct counter and shrink while distinct > 2. The window is still valid because "at most K distinct" is monotone. |
| C3 | Two pointers rely on sortedness to decide which end is useless. Unsorted ⇒ no elimination argument. Use a hash map (O(n), O(n) space) or sort `(value, index)` pairs first (O(n log n)). |
| C4 | A verification pass: count occurrences of the Boyer–Moore candidate; return it only if its count exceeds `n/2`, else report none. The survivor is only a *candidate* without the guarantee. |
| C5 | A zero makes the product 0 and division by zero is invalid; extending a window no longer increases the product (monotonicity lost). Handle zero-containing subarrays separately (all contain-zero subarrays have product 0 < k when k > 0) and run the window on zero-free segments. |
| C6 | The three-group (before/overlap/after) argument requires sorted disjoint input. Append the new interval, sort by start, and run the merge sweep (Problem 50): O(n log n). |

---

## Part 9: Personal Revision Tracker

**Status values:** `Independent` · `Hint used` · `Failed` · `Already mastered`. All attempt statuses are intentionally blank. Fill them in yourself.

### 9.1 Tracker (50 problems)

| Problem | Pattern | Attempt status | Main mistake | Template learned | Blind re-solve needed |
|---|---|---|---|---|---|
| 1. Two Sum | Complement lookup |  |  |  |  |
| 2. Contains Duplicate | Hash set |  |  |  |  |
| 3. Valid Anagram | Frequency counting |  |  |  |  |
| 4. Group Anagrams | Canonical-key hashing |  |  |  |  |
| 5. Top K Frequent Elements | Frequency + bucket sort |  |  |  |  |
| 6. Product of Array Except Self | Prefix/suffix products |  |  |  |  |
| 7. Longest Consecutive Sequence | Hash set, start detection |  |  |  |  |
| 8. Majority Element | Boyer-Moore voting |  |  |  |  |
| 9. Sort Colors | Dutch national flag |  |  |  |  |
| 10. First Missing Positive | Index-as-hash |  |  |  |  |
| 11. Valid Palindrome | Opposite-end pointers |  |  |  |  |
| 12. Two Sum II | Sorted two pointers |  |  |  |  |
| 13. 3Sum | Sort + pointers |  |  |  |  |
| 14. Container With Most Water | Opposite-end greedy |  |  |  |  |
| 15. Remove Duplicates from Sorted Array | Slow/fast |  |  |  |  |
| 16. Remove Element | Slow/fast |  |  |  |  |
| 17. Move Zeroes | Slow/fast stable partition |  |  |  |  |
| 18. Squares of a Sorted Array | Two ends, fill from back |  |  |  |  |
| 19. Trapping Rain Water | Boundary two pointers |  |  |  |  |
| 20. 4Sum | Sort + nested + pointers |  |  |  |  |
| 21. Best Time to Buy and Sell Stock | Running minimum |  |  |  |  |
| 22. Maximum Average Subarray I | Fixed window |  |  |  |  |
| 23. Max Vowels in Substring of Length K | Fixed window |  |  |  |  |
| 24. Longest Substring Without Repeating Characters | Variable window + last index |  |  |  |  |
| 25. Longest Repeating Character Replacement | Frequency window |  |  |  |  |
| 26. Permutation in String | Fixed window + counts |  |  |  |  |
| 27. Find All Anagrams in a String | Fixed window + counts |  |  |  |  |
| 28. Minimum Size Subarray Sum | Variable window (shortest) |  |  |  |  |
| 29. Max Consecutive Ones III | Variable window (≤ k violations) |  |  |  |  |
| 30. Minimum Window Substring | Frequency window (cover) |  |  |  |  |
| 31. Range Sum Query - Immutable | Prefix sums |  |  |  |  |
| 32. Find Pivot Index | Prefix sum + total |  |  |  |  |
| 33. Running Sum of 1d Array | Prefix sums |  |  |  |  |
| 34. Subarray Sum Equals K | Prefix + count map |  |  |  |  |
| 35. Contiguous Array | Prefix balance + earliest index |  |  |  |  |
| 36. Subarray Sums Divisible by K | Prefix remainders |  |  |  |  |
| 37. Continuous Subarray Sum | Prefix remainders + earliest index |  |  |  |  |
| 38. Maximum Length Subarray With Sum K (not an official LC title) | Prefix + earliest index |  |  |  |  |
| 39. Number of Ways to Split Array | Prefix sum + total |  |  |  |  |
| 40. Product Except Self (revisit of 6) | Prefix/suffix tables |  |  |  |  |
| 41. Maximum Subarray | Kadane |  |  |  |  |
| 42. Maximum Product Subarray | Kadane with max/min |  |  |  |  |
| 43. Subarray Product Less Than K | Variable window (counting) |  |  |  |  |
| 44. Merge Sorted Array | Merge from back |  |  |  |  |
| 45. Rotate Array | Reverse trick |  |  |  |  |
| 46. Find the Duplicate Number | Floyd cycle |  |  |  |  |
| 47. Set Matrix Zeroes | In-place markers |  |  |  |  |
| 48. Spiral Matrix | Matrix boundaries |  |  |  |  |
| 49. Insert Interval | Three-phase interval sweep |  |  |  |  |
| 50. Merge Intervals | Sort + merge |  |  |  |  |

### 9.2 Personal lists

**My weakest three patterns**

1. ______________________
2. ______________________
3. ______________________

**Problems to re-solve the same day**

| Problem # | Reason | Done (Y/N) |
|---|---|---|
|  |  |  |
|  |  |  |
|  |  |  |
|  |  |  |

**Problems to re-solve the following morning**

| Problem # | Reason | Done (Y/N) |
|---|---|---|
|  |  |  |
|  |  |  |
|  |  |  |
|  |  |  |

**Templates I cannot recall without help**

| Template | Which part I forget (init / update / invariant / termination) | Recalled on re-test (Y/N) |
|---|---|---|
|  |  |  |
|  |  |  |
|  |  |  |
|  |  |  |

---

*End of Day 1. Days 2 to 10 are intentionally not included in this document.*
