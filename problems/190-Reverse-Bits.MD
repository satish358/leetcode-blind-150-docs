# 🎓 Expert Mentoring: Reverse Bits

In this problem, we aren't just reversing a list of numbers; we are diving deep into a single 32-bit integer and flipping its "switches" from left to right.

---

### 1. Problem Understanding

Computers store a 32-bit integer as a sequence of 32 zeros and ones. Imagine this sequence is like a row of 32 lightbulbs.

- **The Task:** Take the row of bulbs and flip the order. The bulb at position 0 moves to position 31, the bulb at position 1 moves to 30, and so on.
- **Example (8-bit for simplicity):**
- **Input:** `00001011` (This is the number 11)
- **Output:** `11010000` (This is the number 208)

**Important Note:** In Java, integers are signed, but this problem asks us to treat the input as **unsigned**. This means we don't care about positive or negative signs; we just care about the bits themselves.

---

### 2. Pattern Recognition: Bit Shifting & The "Result Reservoir"

To solve this, we use the **Bitwise Extraction** pattern.

Think of it like a conveyor belt:

1. We look at the bit at the very end (the rightmost bit) of our input.
2. We take that bit and put it into the first slot of our "Result" container.
3. We shift our input to the right to see the next bit.
4. We shift our "Result" to the left to make room for the next incoming bit.

**When to use this:**

- When you need to manipulate the internal structure of a number.
- When the problem involves "reversing," "flipping," or "rotating" binary data.

---

### 3. Visual Explanation

```mermaid
graph TD
    Start[Result = 0, Loop 32 times] --> Extract[Extract rightmost bit of n: n & 1]
    Extract --> ShiftResult[Shift Result left: res << 1]
    ShiftResult --> PutBit[Add extracted bit to Result: res | bit]
    PutBit --> ShiftN[Shift n right: n >>> 1]
    ShiftN --> Loop{Finished 32 rounds?}
    Loop -- No --> Extract
    Loop -- Yes --> End[Return Result]

```

---

### 4. Step-by-Step Solution

Let's use an 4-bit example: `n = 1011` (11)

1. **Round 1:**

- Extract last bit of `n`: **1**.
- Shift `Result` (0) left and add bit: `Result` = **1**.
- Shift `n` right: `n` = `101`.

2. **Round 2:**

- Extract last bit of `n`: **1**.
- Shift `Result` (1) left: `10`. Add bit: `Result` = **11**.
- Shift `n` right: `n` = `10`.

3. **Round 3:**

- Extract last bit of `n`: **0**.
- Shift `Result` (11) left: `110`. Add bit: `Result` = **110**.
- Shift `n` right: `n` = `1`.

4. **Round 4:**

- Extract last bit of `n`: **1**.
- Shift `Result` (110) left: `1100`. Add bit: `Result` = **1101**.
- Shift `n` right: `n` = `0`.

5. **Finish:** Result is `1101`.

---

### 5. Code Implementation (Java)

```java
/**
 * Time Complexity: O(1) - We always loop 32 times regardless of input size.
 * Space Complexity: O(1) - We only use one integer variable for the result.
 */
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int result = 0;

        // Since it's a 32-bit integer, we iterate exactly 32 times
        for (int i = 0; i < 32; i++) {
            // 1. Shift result to the left to make space for the next bit
            // The very first bit added will end up at the far left after 31 shifts
            result = result << 1;

            // 2. Isolate the last bit of n using the AND operator (&)
            int lastBit = n & 1;

            // 3. Place that bit into the empty spot we just made in result
            // We use the OR operator (|) to set the bit
            result = result | lastBit;

            // 4. Shift n to the right to bring the next bit into the "last" position
            // We use >>> (unsigned shift) so we don't copy the sign bit
            n = n >>> 1;
        }

        return result;
    }
}

```

---

### 6. Complexity Analysis

- **Time Complexity: **
- Wait, isn't there a loop? Yes, but it always runs exactly 32 times. Because the number of steps does not grow as the input gets "larger," we consider this constant time.

- **Space Complexity: **
- We only use one variable, `result`, to build our answer.

---

### 7. Similar Problems

1. **LeetCode 191: Number of 1 Bits** (Another problem about counting specific bits).
2. **LeetCode 7: Reverse Integer** (Reversing a decimal number instead of a binary one—it uses very similar "Shift and Add" logic!).
3. **LeetCode 231: Power of Two** (Checking bit patterns to identify powers of 2).

---

### 8. Key Takeaways

- **Conveyor Belt Thinking:** When reversing bits, think of one number "giving" its bits one by one and the other number "receiving" them.
- **Shift Operators:** `<<` (Left shift) multiplies by 2, and `>>>` (Unsigned right shift) divides by 2 without worrying about negative signs.
- **Bitwise Masks:** `n & 1` is the standard way to "peek" at the very last bit of any number.
