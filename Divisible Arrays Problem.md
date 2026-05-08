# Divisible Arrays Problem

## Problem Statement

Alex selected two integers `N` and `K`.

He wrote down all possible integer arrays of length `K`:

```text id="65g2rx"
a[1], a[2], ..., a[K]
```

such that:

* Each element is in the range:

```text id="b6hnzt"
1 ≤ a[i] ≤ N
```

* For every valid index:

```text id="tnj1jr"
a[i + 1] is divisible by a[i]
```

That means:

```text id="m9yejl"
a[i + 1] % a[i] == 0
```

Your task is to determine the number of different valid arrays.

Since the answer can be very large, print it modulo:

```text id="3jqb2u"
10000
```

---

# Input Format

* The first line contains an integer `N`

  * Maximum possible value in the array
* The second line contains an integer `K`

  * Length of the array

---

# Output Format

Print a single integer representing the number of valid arrays modulo `10000`.

---

# Example 1

## Input

```text id="4uofb5"
2
1
```

## Output

```text id="bgx7rq"
2
```

## Explanation

Array length is `1`.

Possible arrays:

```text id="fml3y7"
[1]
[2]
```

Total arrays = `2`

---

# Example 2

## Input

```text id="qrm0v7"
2
2
```

## Output

```text id="rw58pm"
3
```

## Explanation

Possible arrays:

```text id="g9m56c"
[1, 1]
[1, 2]
[2, 2]
```

Invalid array:

```text id="v1ktkr"
[2, 1]
```

because:

```text id="yaz7v2"
1 is not divisible by 2
```

Total arrays = `3`

---

# Example 3

## Input

```text id="gsc7if"
3
2
```

## Output

```text id="qgydph"
5
```

## Explanation

Possible arrays:

```text id="b3p4ko"
[1, 1]
[1, 2]
[1, 3]
[2, 2]
[3, 3]
```

Total arrays = `5`

---

# Approach

## Dynamic Programming

Let:

```text id="vk2f1l"
dp[len][num]
```

represent:

* Number of valid arrays of length `len`
* Ending with number `num`

---

## Base Case

For arrays of length `1`:

```text id="67s8uo"
dp[1][i] = 1
```

because every single number from `1` to `N` forms a valid array.

---

## Transition

If current number is `x`, then next number can be any multiple of `x`.

So:

```text id="r7qgk2"
dp[len + 1][multiple] += dp[len][x]
```

where:

```text id="yxgjgu"
multiple % x == 0
```

---

# Algorithm

1. Initialize DP table
2. Set base case:

   * `dp[1][i] = 1`
3. For each length:

   * For every number:

     * Traverse all multiples
4. Sum all values for arrays of length `K`
5. Print answer modulo `10000`

---

# Java Solution

```java id="xjlwm8"
import java.util.*;

class Main {

    static final int MOD = 10000;

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        int K = sc.nextInt();

        int[][] dp = new int[K + 1][N + 1];

        // Base case
        for (int i = 1; i <= N; i++) {
            dp[1][i] = 1;
        }

        // Build DP
        for (int len = 1; len < K; len++) {

            for (int num = 1; num <= N; num++) {

                // Traverse multiples of num
                for (int multiple = num; multiple <= N; multiple += num) {

                    dp[len + 1][multiple] =
                        (dp[len + 1][multiple] + dp[len][num]) % MOD;
                }
            }
        }

        int answer = 0;

        for (int i = 1; i <= N; i++) {
            answer = (answer + dp[K][i]) % MOD;
        }

        System.out.println(answer);
    }
}
```

---

# Dry Run

## Input

```text id="tf38u4"
N = 2
K = 2
```

---

## Step 1: Base Case

```text id="9kshlw"
dp[1][1] = 1
dp[1][2] = 1
```

---

## Step 2: Build Arrays of Length 2

### From 1

Multiples of `1`:

```text id="9k6nr8"
1, 2
```

So:

```text id="96wvlv"
dp[2][1] += dp[1][1]
dp[2][2] += dp[1][1]
```

---

### From 2

Multiples of `2`:

```text id="u57a3q"
2
```

So:

```text id="fc7j3j"
dp[2][2] += dp[1][2]
```

---

## Final DP

```text id="1u4vdf"
dp[2][1] = 1
dp[2][2] = 2
```

Total:

```text id="l95rqz"
1 + 2 = 3
```

---

# Time Complexity

For every number, we iterate through its multiples.

## Complexity

```text id="9kt63d"
O(K × N × log N)
```

approximately.

---

# Space Complexity

```text id="w5bb42"
O(K × N)
```

---

# Optimization

Space can be optimized to:

```text id="9h8o5n"
O(N)
```

using rolling arrays because only the previous row is required.

---

# Key Insight

If a number `x` appears in the array, the next number must be a multiple of `x`.

This naturally forms a Dynamic Programming transition over multiples.
