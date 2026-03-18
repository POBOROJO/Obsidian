═══════════════════════════════════ TOPIC: Arrays — Basic Manipulation ═══════════════════════════════════

PATTERN 1 — Two Pointers / In-place Partition When to use: sorted array + remove/move elements in-place, maintain relative order, no extra space allowed Core idea: one pointer holds the "last valid" position, second pointer scans — swap or overwrite on condition

Q1. Remove Duplicates from Sorted Array Key insight: sorted = duplicates are adjacent, so i just needs to track last unique, j finds the next different value and overwrites Remember: i starts at 0, not -1 — return i+1 not i

Q2. Move Zeroes Key insight: don't move zeros, move non-zeros forward — zeros fall to end automatically, order preserved Remember: find first zero index first, then start swapping from j+1 — skipping this setup causes wrong j position

───────────────────────────────── PATTERN 2 — Array Rotation via Reversal When to use: rotate array by k steps in-place, no extra space, circular shift behavior Core idea: three reversals — full array, first k, remaining n-k — gives exact rotation

Q1. Rotate Array Key insight: reversing the whole array then reversing two parts is equivalent to shifting — no element needs to be moved more than once Remember: always do k = k % n first — forgetting this breaks when k > n

Q2. Check if Array is Sorted and Rotated Key insight: a sorted+rotated array has exactly 1 "break point" in circular traversal — count inversions including wrap-around nums[n-1] → nums[0] Remember: the circular check (nums[n-1] > nums[0]) is a separate count — missing it = wrong answer

───────────────────────────────── PATTERN 3 — Math / Range Trick When to use: array has numbers in a known range [0,n] or [1,n], one value missing or repeated, no extra space Core idea: expected formula minus actual gives the gap

Q1. Missing Number Key insight: sum of [0..n] is fixed — actual sum differs by exactly the missing number, no need to track anything Remember: formula is n*(n+1)/2, use XOR version if overflow is a concern

═══════════════════════════════════ TOPIC: Arrays — Hashing & Frequency ═══════════════════════════════════

PATTERN 1 — Complement / Lookup via HashMap When to use: find pair/group summing to target, need index not just existence, one-pass required Core idea: for each element store it, and check if its complement already exists in the map

Q1. Two Sum Key insight: instead of searching forward for the partner, look backward — store what you've seen, ask "does my complement exist already?" Remember: put element into map AFTER checking — else same index used twice

Q2. Count Subarrays with Given Sum (Subarray Sum = K) Key insight: if prefixSum[j] - prefixSum[i] = k, then subarray i+1..j has sum k — so look for (prefixSum - k) in the map Remember: initialize map with {0 → 1} before loop — handles subarrays starting at index 0

───────────────────────────────── PATTERN 2 — Prefix Sum + HashMap (Longest/Count Subarray) When to use: longest or count of subarrays with target sum/XOR, array has negatives (sliding window fails) Core idea: store first occurrence of prefix value — collision = valid subarray found between those indices

Q1. Largest Subarray with 0 Sum Key insight: two equal prefix sums = zero-sum subarray between them — store first index, compute length on revisit Remember: store FIRST occurrence only — overwriting shrinks the subarray length

Q2. Count Subarrays with XOR = K Key insight: XOR version of subarray sum — prefixXor[j] ^ prefixXor[i] = k means prefixXor[i] = prefixXor[j] ^ k, look that up in map Remember: initialize map with {0 → 1} — same as sum version, same reason

═══════════════════════════════════ TOPIC: Arrays — Greedy / Kadane-style ═══════════════════════════════════

PATTERN 1 — Running Optimal + Reset When to use: max/min subarray value, single buy-sell, carry forward only what helps Core idea: track current running value, reset when it becomes harmful, update global best at each step

Q1. Maximum Subarray (Kadane's) Key insight: a negative running sum is poison — it only drags future sums down, so drop it and start fresh from current element Remember: reset sum AFTER updating maxi — if you reset before, all-negative arrays return 0 instead of the least negative

Q2. Best Time to Buy and Sell Stock Key insight: profit = current price - minimum seen so far — you never need to track days, just the running minimum prefix Remember: update maxProfit before updating mini — order matters to avoid buying and selling on same day conceptually

ALGO TO REMEMBER — Kadane's Algorithm What it does: finds maximum sum contiguous subarray in O(n) When you forget it, think of: "carry a bag — if the bag becomes a burden (negative), drop it and start a new one" Core steps: 1. sum = 0, maxi = -∞ 2. add current element to sum 3. update maxi = max(maxi, sum) 4. if sum < 0, reset sum = 0

───────────────────────────────── PATTERN 2 — Index Mapping / Direct Placement When to use: rearrange elements by a property (sign, color), count limited (0/1/2 or equal +/-), order must be preserved Core idea: compute the exact target index for each element category, place directly — no swapping

Q1. Rearrange Array by Sign Key insight: positives → even indices, negatives → odd indices — place directly, equal count guaranteed so no overflow Remember: pos starts at 0, neg starts at 1, both increment by 2 — starting values determine which sign goes first

Q2. Sort Colors (Dutch National Flag) Key insight: partition into 3 regions with 3 pointers — low tracks 0s, high tracks 2s, mid is the explorer Remember: when swapping with high, do NOT increment mid — the swapped element from high is unseen and needs to be checked

═══════════════════════════════════ TOPIC: Arrays — Sorting-based Patterns ═══════════════════════════════════

PATTERN 1 — Sort + Two Pointers (Multi-sum) When to use: find pairs/triplets/quads summing to target, duplicates must be avoided in result, O(n²) acceptable Core idea: sort first, fix outer element(s), shrink window from both ends, skip duplicates at every level

Q1. 3Sum Key insight: fix one element, reduce to two-sum on sorted array — sorting enables pointer movement AND duplicate skipping in one pass Remember: skip duplicates for i (outer) AND for j/k after finding a valid triplet — missing any one level gives duplicate results

Q2. 4Sum Key insight: same as 3Sum but one more fixed loop — use long for sum to avoid overflow, duplicate logic identical at all 4 levels Remember: cast to long before addition — int overflow silently gives wrong answers

───────────────────────────────── PATTERN 2 — Greedy + Sorting (Intervals) When to use: overlapping intervals, merge/count ranges, interval scheduling Core idea: sort by start time — overlapping intervals become adjacent, merge greedily with last result

Q1. Merge Intervals Key insight: after sorting by start, you only ever need to compare current interval with the last merged one — if they overlap, extend end Remember: overlap condition is current.start ≤ last.end (not <) — adjacent intervals like [1,4][4,5] DO merge

───────────────────────────────── PATTERN 3 — Lexicographical / Permutation When to use: next/previous arrangement, generate permutations in order, "next greater" phrasing Core idea: find the rightmost dip (pivot), make smallest possible swap, reset suffix to lowest order

Q1. Next Permutation Key insight: the longest non-increasing suffix is already at max — break it at the pivot, swap with smallest element just greater than pivot, then reverse the suffix Remember: reverse the suffix, do NOT sort it — suffix is already sorted descending so reverse = O(n) not O(n log n)

═══════════════════════════════════ TOPIC: Arrays — Divide & Conquer ═══════════════════════════════════

PATTERN 1 — Merge Sort Modification (Count during sort) When to use: count pairs with a condition across subarrays (i < j and arr[i] > f(arr[j])), brute O(n²) too slow Core idea: during merge of two sorted halves, count valid pairs using a pointer — each element in left half accounts for all remaining right elements

Q1. Count Inversions Key insight: when arr[left] > arr[right] during merge, ALL remaining left elements are also greater — so count = mid - left + 1, not just 1 Remember: count BEFORE merging, not after — once merged the halves are mixed and the count is lost

Q2. Reverse Pairs Key insight: condition is arr[i] > 2_arr[j] — count in a separate pass BEFORE merge (sorted halves allow two-pointer), then merge separately Remember: use 2L not 2 — integer overflow makes arr[i] > 2_arr[j] silently wrong for large values

ALGO TO REMEMBER — Merge Sort Counting What it does: counts cross-half pairs satisfying a condition in O(n log n) instead of O(n²) When you forget it, think of: "two sorted lines at a concert — for each person in line A who is taller than someone in line B, everyone behind them in line A is also taller" Core steps: 1. divide array into halves recursively 2. BEFORE merging: use two pointers on sorted halves to count valid pairs 3. advance right pointer only forward (never reset) — O(n) total across all left elements 4. merge both halves normally after counting

═══════════════════════════════════ TOPIC: Arrays — Voting Algorithms ═══════════════════════════════════

PATTERN 1 — Boyer-Moore Voting (Majority Detection) When to use: find element appearing > n/2 or > n/3 times, O(1) space required, no sorting/hashing Core idea: cancel out different elements — the majority survives all cancellations because it outnumbers everything else

Q1. Majority Element (> n/2) Key insight: every non-majority element cancels one majority element — but majority has more than all others combined, so it's always left standing Remember: verification pass is optional when existence guaranteed, mandatory otherwise — skip it and you may return a wrong candidate

Q2. Majority Element II (> n/3) Key insight: more than n/3 threshold means at most 2 such elements can exist — so track exactly 2 candidates, cancel triplets instead of pairs Remember: check if current element matches el1 BEFORE checking if cnt1 == 0 — order of if-else determines correctness

ALGO TO REMEMBER — Boyer-Moore Voting What it does: finds majority candidate in O(n) time, O(1) space by pair cancellation When you forget it, think of: "election vote counting — every time two different people vote, cancel them out — whoever is left is the potential winner" Core steps: 1. maintain candidate + count (or 2 candidates for n/3 version) 2. if count = 0, assign current element as new candidate 3. if matches candidate, count++; else count-- 4. second pass to verify actual frequency

═══════════════════════════════════ TOPIC: Arrays — Matrix Problems ═══════════════════════════════════

PATTERN 1 — Matrix Transformation (In-place) When to use: rotate/transpose matrix in-place, no extra space, square matrix Core idea: decompose the transformation into simple reversible steps (transpose + reflect)

Q1. Rotate Image (90° clockwise) Key insight: 90° clockwise = transpose + reverse each row — two simple O(n²) steps, no temp matrix needed Remember: transpose loop uses j = i+1 — starting j from 0 causes double-swapping which undoes the work

Q2. Spiral Matrix Key insight: shrink 4 boundaries after each direction traversal — top++, right--, bottom--, left++ in order; boundary checks prevent double-counting in single row/col edge cases Remember: check if top ≤ bottom before left traversal, left ≤ right before upward traversal — non-square matrices need these guards

═══════════════════════════════════ TOPIC: Arrays — Combinatorics / Math ═══════════════════════════════════

PATTERN 1 — Math Formula / Equation System When to use: numbers in fixed range [1,n], find missing + repeating simultaneously, O(1) space Core idea: set up two equations (sum difference, square-sum difference), solve for two unknowns

Q1. Missing and Repeating Number Key insight: x-y = S-SN and x²-y² = S2-S2N — divide second by first to get x+y, then simple arithmetic gives both values Remember: use long for all computations — n²(n+1)²/4 overflows int for n > 46000

Q2. Pascal's Triangle Key insight: each value is nCr computed incrementally as C = C * (row-col) / col — no need to look at previous row, no factorial computation Remember: use long for intermediate val — the multiplication before division overflows int even if final answer fits