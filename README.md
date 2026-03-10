# DSA — Palindrome String Checker

A clean C++ implementation of one of the most fundamental string problems in DSA — checking whether a given string is a **palindrome**. The solution uses a **two-pointer approach**, comparing characters from both ends of the string moving inward, with an early exit on the first mismatch. Simple in appearance, but rich in the foundational patterns it introduces: mirrored traversal, optimistic flag initialization, and short-circuit termination.

---

## Problem Statement

Given a string, determine whether it reads the same forwards and backwards.

**Example Input:**
```
Enter a string: racecar
```

**Example Output:**
```
The string is a palindrome.
```

---

**Example Input:**
```
Enter a string: hello
```

**Example Output:**
```
The string is not a palindrome.
```

---

## The Code

```cpp
#include <iostream>
#include <string>
using namespace std;

int main()
{
    string str;
    cout << "Enter a string: ";
    cin >> str;

    int len = str.length();
    bool isPalindrome = true;

    for (int i = 0; i < len / 2; i++) {
        if (str[i] != str[len - i - 1]) {
            isPalindrome = false;
            break;
        }
    }

    if (isPalindrome) {
        cout << "The string is a palindrome." << endl;
    } else {
        cout << "The string is not a palindrome." << endl;
    }

    return 0;
}
```

---

## How It Works

The algorithm uses a classic **two-pointer strategy** — one pointer starting at the left (`i = 0`) and one mirrored from the right (`len - i - 1`), both moving toward the center simultaneously.

1. **Read input** — The string is read from standard input.
2. **Optimistic initialization** — `isPalindrome` is set to `true` by default. The loop only looks for a reason to disprove it.
3. **Half-length traversal** — The loop runs only from `i = 0` to `i < len / 2`. The middle character of an odd-length string never needs comparison — it always matches itself.
4. **Early exit** — The moment a mismatch is found (`str[i] != str[len - i - 1]`), `isPalindrome` is set to `false` and the loop breaks immediately. No unnecessary comparisons are made.
5. **Output** — The result is printed based on the final value of `isPalindrome`.

---

## The Mirror Formula — `str[len - i - 1]`

The expression `str[len - i - 1]` is the mirror index of position `i`:

| `i` | Mirror Index (`len - i - 1`) | For `"racecar"` (len=7) |
|-----|------------------------------|--------------------------|
| 0   | 6                            | `r` ↔ `r` ✅             |
| 1   | 5                            | `a` ↔ `a` ✅             |
| 2   | 4                            | `c` ↔ `c` ✅             |
| 3   | 3 (middle — never reached)   | `e` — skipped ✅         |

The `-1` corrects for 0-based indexing. This formula is a building block used across dozens of string and array problems — worth internalizing.

---

## Why Only `len / 2` Iterations?

For a string of length `n`, only the first `n/2` character pairs need to be verified.

- **Even length** (`"abba"`, len=4): pairs are (0,3) and (1,2) — exactly 2 = 4/2 iterations
- **Odd length** (`"racecar"`, len=7): pairs are (0,6), (1,5), (2,4) — exactly 3 = 7/2 iterations (integer division drops the remainder)

The middle character of an odd-length string (`'e'` in `"racecar"`) is its own mirror. Comparing it to itself would always pass and is therefore unnecessary — the code correctly skips it.

---

## Algorithm (Pseudocode)

```
read str
len ← str.length()
isPalindrome ← true

for i from 0 to (len/2 - 1):
    if str[i] != str[len - i - 1]:
        isPalindrome ← false
        break

print result based on isPalindrome
```

---

## Dry Run

**Input:** `str = "madam"`, `len = 5`

| `i` | `str[i]` | Mirror Index | `str[mirror]` | Match? |
|-----|----------|--------------|---------------|--------|
| 0   | `m`      | 4            | `m`           | ✅     |
| 1   | `a`      | 3            | `a`           | ✅     |
| 2   | (middle) | —            | —             | Skipped|

**`isPalindrome` remains `true`**
**Output:** `The string is a palindrome.`

---

**Input:** `str = "hello"`, `len = 5`

| `i` | `str[i]` | Mirror Index | `str[mirror]` | Match? |
|-----|----------|--------------|---------------|--------|
| 0   | `h`      | 4            | `o`           | ❌ → break |

**`isPalindrome` set to `false` immediately**
**Output:** `The string is not a palindrome.`

---

## Complexity Analysis

| Metric | Complexity |
|--------|------------|
| Time   | **O(n/2) → O(n)** — at most half the characters are compared; in the worst case (palindrome), all `n/2` pairs are checked |
| Space  | **O(n)** — the string of length `n` is stored in memory; no auxiliary data structures are used |

> **Best case (early mismatch):** O(1) — if the first and last characters differ, the loop exits immediately after one comparison. A string like `"az..."` is rejected in a single step.

---

## Edge Cases

| Scenario | Behavior |
|----------|----------|
| Single character (`"a"`) | `len/2 = 0` — loop doesn't execute, `isPalindrome` stays `true`. Correct: every single character is a palindrome |
| Empty string (`""`) | `len = 0`, loop skipped, output: palindrome. Mathematically valid |
| Two identical characters (`"aa"`) | One comparison: `str[0] == str[1]` → palindrome |
| Two different characters (`"ab"`) | One comparison: mismatch → not a palindrome |
| All same characters (`"aaaa"`) | All comparisons pass → palindrome |

---

## An Important Limitation — `cin >> str`

The code uses `cin >> str`, which reads input **up to the first whitespace**. This means:

```
Input:  "a man a plan a canal panama"
Read:   "a"              ← only the first word
Result: palindrome       ← trivially true, not what was intended
```

For multi-word palindrome checking, `getline(cin, str)` should be used instead, along with preprocessing to remove spaces and handle case sensitivity:

```cpp
getline(cin, str);   // reads the full line including spaces
```

This is a known and deliberate scope boundary of the current implementation — it correctly handles **single-word palindromes**, which covers the vast majority of standard DSA problem inputs.

---

## Palindrome Examples

| String      | Palindrome? |
|-------------|-------------|
| `racecar`   | ✅ Yes      |
| `madam`     | ✅ Yes      |
| `level`     | ✅ Yes      |
| `noon`      | ✅ Yes      |
| `abcba`     | ✅ Yes      |
| `hello`     | ❌ No       |
| `world`     | ❌ No       |
| `abcd`      | ❌ No       |

---

## Repository Structure

```
DSA-Palindrome/
│
├── palindrome.cpp      # Main C++ implementation
└── README.md           # Project documentation
```

---

## How to Compile and Run

**Prerequisites:** GCC / G++

```bash
# Clone the repository
git clone https://github.com/rishita-ops/DSA-Palindrome.git
cd DSA-Palindrome

# Compile
g++ palindrome.cpp -o palindrome

# Run
./palindrome
```

**On Windows:**
```bash
g++ palindrome.cpp -o palindrome.exe
palindrome.exe
```

---

## Key Concepts Covered

- **Two-pointer technique** — simultaneous traversal from both ends of a sequence toward the center
- **Mirror index formula** — `str[len - i - 1]` as the symmetric counterpart of `str[i]`
- **Optimistic flag initialization** — assuming `true` and searching for a contradiction, rather than building toward `true`
- **Early exit / short-circuit termination** — `break` on first mismatch avoids redundant comparisons
- **Half-length traversal** — understanding why only `len/2` iterations suffice, for both odd and even lengths
- **`cin` vs `getline`** — scope awareness of single-word vs full-sentence input

---

## Why This Problem Matters in DSA

The palindrome check is a template problem — master it, and the same pattern scales directly into:

| Problem | Connection |
|---------|------------|
| Valid Palindrome II (LeetCode #680) | Allow one character deletion — two-pointer with a skip |
| Longest Palindromic Substring (#5) | Expand-around-center uses the same mirroring concept |
| Palindrome Linked List (#234) | Two-pointer adapted for a linked list structure |
| Palindrome Number (#9) | Same logic applied to digits of an integer |
| Reverse String (#344) | Direct application of the mirror index formula |
| Manacher's Algorithm | Industrial-strength palindrome detection — built on this foundation |

Every string problem involving symmetry traces back to the intuition built here.

---

## Contributing

Contributions are welcome. Consider adding:
- A version using `getline` for **multi-word palindrome** support
- A **case-insensitive** version (treating `"Racecar"` as a palindrome)
- A version that **ignores spaces and punctuation** (e.g., `"A man a plan a canal Panama"`)
- Implementations in Python, Java, or JavaScript

```bash
git checkout -b feature/your-feature
git commit -m "Add: your feature description"
git push origin feature/your-feature
# Then open a Pull Request
```

---

## License

This project is open-source and available under the [MIT License](LICENSE).

---

*Part of a structured DSA practice series — fundamentals, done right.*
