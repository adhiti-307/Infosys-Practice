# Largest Number Formation Problem

## Problem Statement

Given an array of non-negative integers, arrange them such that they form the largest possible number.

### Example

#### Input
```java
[1, 34, 3, 98, 9, 76, 45, 4]
```

#### Output
```java
998764543431
```

---

## Approach

1. Convert all integers into strings.
2. Sort the strings using a custom comparator:
   - Compare `(a + b)` and `(b + a)`
   - Place the larger combination first.
3. Concatenate all sorted strings.
4. Handle edge case where all elements are `0`.

---

## Java Solution

```java
import java.util.*;

class Main {

    public static String compared(int[] arr) {
        int n = arr.length;
        String[] strNums = new String[n];
        for (int i = 0; i < n; i++) {
            strNums[i] = String.valueOf(arr[i]);
        }

        //Arrays.sort(str, (s1,s2)->(s2+s1).compareTo(s1+s2));
        Arrays.sort(strNums, new Comparator<String>() {

            @Override
            public int compare(String s1, String s2) {
                String order1 = s1 + s2;
                String order2 = s2 + s1;
                return order2.compareTo(order1);
            }
        });

        if (strNums[0].equals("0")) {
            return "0";
        }

        StringBuilder result = new StringBuilder();
        for (String str : strNums) {
            result.append(str);
        }
        return result.toString();
    }

    public static void main(String[] args) {
        int[] arr = {1, 34, 3, 98, 9, 76, 45, 4};
        String result = compared(arr);
        System.out.println(result);
    }
}
```

---

## Time Complexity

- Sorting takes:
  
```text
O(n log n)
```

where `n` is the number of elements.

---

## Space Complexity

```text
O(n)
```

for storing string representations of numbers.

---

## Key Learning

The main trick is using a custom comparator to decide which concatenation order produces a larger number.

Example:

```text
"9" + "34" = "934"
"34" + "9" = "349"
```

Since `934 > 349`, `9` should come before `34`.

---
