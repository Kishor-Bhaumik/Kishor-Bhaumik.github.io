Initial characters (sorted): a[1]=1, a[2]=3, a[3]=7, a[4]=8, a[5]=15
```

**Why do we ALWAYS combine a[1] and a[2] first?**

Think about it:
- a[1] = 1 is the **smallest**
- a[2] = 3 is the **second smallest**
- Huffman always picks the 2 smallest → So first step MUST be combining a[1] and a[2]

After combining: We get a **synthetic character** with frequency `1+3=4`

Now we have:
- **Leaf characters**: `a[3]=7, a[4]=8, a[5]=15`
- **Synthetic characters**: `synthetic1 = 4`

**The question asks**: When will the NEXT combination be **two more leaf characters** (a[3] and a[4])?

**Answer**: Only if `a[3] + a[4] < 2 × synthetic1`

In other words: `7 + 8 < 2 × 4`? → `15 < 8`? **NO!**

So usually, we'd combine `synthetic1=4` with `a[3]=7` next.

But IF the next two leaves were very close in value, like:
```
a[3]=4, a[4]=5, and synthetic1=4
```
Then a[3]=4 and a[4]=5 might be the next smallest pair!

---

### **Question 3.2: Synthetic Characters Are Also Sorted!**

This is the **KEY INSIGHT**:

When you create synthetic characters by combining, they come out in **increasing order** too!

**Why?**

Let's trace through an example:
```
Initial (sorted): 1, 3, 7, 8, 15

Step 1: Combine 1+3 = 4 (synthetic1)
Step 2: Combine 4+7 = 11 (synthetic2)  
Step 3: Combine 8+15 = 23 (synthetic3)
Step 4: Combine 11+23 = 34 (synthetic4)
```

Notice: `4 < 11 < 23 < 34` - the synthetic characters are also sorted!

**Intuition**: Each new synthetic is made from some of the smallest elements. Later synthetics include more/larger elements, so they're bigger.

**From the slides** (slide 26): Remember the cost formula?
```
cost = Σ (pile_size × depth)
```

Smaller piles go deeper (combined first), so they create smaller synthetic values first.

---

### **Question 3.3: Design O(n) Algorithm**

Now the brilliant part! Since BOTH lists are sorted:
- **Leaf characters**: sorted in a[1..n]
- **Synthetic characters**: also sorted as we create them

**Use TWO QUEUES!**
```
Queue 1 (Leaves): [1, 3, 7, 8, 15]  ← starts full
Queue 2 (Synthetic): []              ← starts empty
```

**Algorithm**:
```
While we have more than 1 node total:
    1. Look at the fronts of BOTH queues
    2. Pick the 2 SMALLEST (might be from Q1, Q2, or one from each)
    3. Combine them → new synthetic character
    4. Add to BACK of Queue 2
```

**Example**:
```
Initial:
Q1: [1, 3, 7, 8, 15]
Q2: []

Step 1: Take 1 and 3 from Q1 → create synthetic 4
Q1: [7, 8, 15]
Q2: [4]

Step 2: Compare front of Q1(7) vs front of Q2(4) → take 4 from Q2 and 7 from Q1
Q1: [8, 15]
Q2: [11]  ← added 4+7=11 to back

Step 3: Take 8 and 15 from Q1 → create synthetic 23
Q1: []
Q2: [11, 23]

Step 4: Take 11 and 23 from Q2 → create synthetic 34
Q1: []
Q2: [34]

Done! Root = 34
