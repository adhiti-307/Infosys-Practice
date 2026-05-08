# Monster Quest Problem

## Problem Statement

While playing an RPG game, you are assigned one of the hardest quests.
There are `n` monsters that need to be defeated.

Each monster is described by two integers:

* `power[i]` → Minimum experience required to defeat the monster
* `bonus[i]` → Experience gained after defeating the monster

You start with an initial experience value `e`.

### Rules

* A monster can only be defeated if your current experience is **greater than or equal to** its power.
* After defeating a monster, your experience increases by its bonus value.
* Monsters can be defeated in **any order**.
* If you attempt to fight a monster without enough experience, you lose immediately.

Your task is to determine the **maximum number of monsters** that can be defeated.

---

# Input Format

* The first line contains an integer `n` — number of monsters.
* The second line contains an integer `e` — initial experience.
* The next `n` lines contain the values of `power[i]`.
* The next `n` lines contain the values of `bonus[i]`.

---

# Output Format

Print a single integer representing the maximum number of monsters that can be defeated.

---

# Example 1

## Input

```text
2
123
78
130
10
0
```

## Output

```text
2
```

## Explanation

Initial experience = `123`

* Defeat monster with power `78`

  * New experience = `123 + 10 = 133`
* Defeat monster with power `130`

  * New experience = `133 + 0 = 133`

Total monsters defeated = `2`

---

# Example 2

## Input

```text
3
100
101
100
304
100
1
524
```

## Output

```text
2
```

## Explanation

Initial experience = `100`

Monsters:

| Power | Bonus |
| ----- | ----- |
| 101   | 100   |
| 100   | 1     |
| 304   | 524   |

Optimal order:

1. Defeat monster with power `100`

   * Experience becomes `101`
2. Defeat monster with power `101`

   * Experience becomes `201`

Monster with power `304` cannot be defeated.

Total monsters defeated = `2`

---

# Approach

## Greedy Strategy

To maximize the number of monsters defeated:

* Always fight the monster with the **smallest power requirement first**
* This helps gain bonus experience early
* Increased experience makes it possible to defeat stronger monsters later

---

# Algorithm

1. Store each monster as a pair `(power, bonus)`
2. Sort all monsters by `power` in ascending order
3. Traverse the sorted list:

   * If current experience is enough:

     * Defeat the monster
     * Add bonus to experience
     * Increase count
   * Otherwise stop

---

# Java Solution

```java
import java.util.*;

class Main {

    static class Monster {
        int power;
        int bonus;

        Monster(int power, int bonus) {
            this.power = power;
            this.bonus = bonus;
        }
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        int exp = sc.nextInt();

        int[] power = new int[n];
        int[] bonus = new int[n];

        // Input powers
        for (int i = 0; i < n; i++) {
            power[i] = sc.nextInt();
        }

        // Input bonuses
        for (int i = 0; i < n; i++) {
            bonus[i] = sc.nextInt();
        }

        // Create monster objects
        Monster[] monsters = new Monster[n];

        for (int i = 0; i < n; i++) {
            monsters[i] = new Monster(power[i], bonus[i]);
        }

        // Sort monsters by power
        Arrays.sort(monsters, (a, b) -> a.power - b.power);

        int count = 0;

        // Defeat monsters greedily
        for (Monster m : monsters) {

            if (exp >= m.power) {
                exp += m.bonus;
                count++;
            } else {
                break;
            }
        }

        System.out.println(count);
    }
}
```

---

# Dry Run

## Input

```text
3
100
101
100
304
100
1
524
```

## Sorted Monsters

| Power | Bonus |
| ----- | ----- |
| 100   | 1     |
| 101   | 100   |
| 304   | 524   |

### Step 1

Experience = `100`

Defeat monster `(100, 1)`

New experience = `101`

Count = `1`

---

### Step 2

Experience = `101`

Defeat monster `(101, 100)`

New experience = `201`

Count = `2`

---

### Step 3

Experience = `201`

Cannot defeat monster `(304, 524)`

Final answer = `2`

---

# Time Complexity

## Sorting

```text
O(n log n)
```

## Traversal

```text
O(n)
```

## Overall Complexity

```text
O(n log n)
```

---

# Space Complexity

```text
O(n)
```

Used for storing monster objects.

---

# Key Insight

Defeating weaker monsters first maximizes experience gain early, which increases the chances of defeating stronger monsters later. Hence, sorting by power and processing greedily gives the optimal solution.
