# Notes: Conditional Probability and Sequential Modeling

## 1. Conditional Probability
Conditional probability is used to reason about the likelihood of an experiment's outcome based on partial information. 

* **Mathematical Definition:** For an event A given that event B has occurred, the conditional probability is denoted as P(A|B). Assuming that P(B) > 0, it is defined by the formula P(A|B) = P(A ∩ B) / P(B).
* **A Valid Probability Law:** Conditional probabilities satisfy all standard probability axioms (non-negativity, normalization, and additivity) . All of the conditional probability is concentrated on the given event B, meaning it can be treated as a legitimate probability law operating on a new universe, B.
* **Equally Likely Outcomes:** If an experiment has a finite number of equally likely outcomes, the conditional probability is the number of elements shared by both A and B, divided by the total number of elements in B.

## 2. Sequential Modeling and the Multiplication Rule
Many probabilistic models have a sequential, step-by-step nature, which are conveniently modeled using tree diagrams.

* **Tree Diagrams:** Experiments can be visualized as trees where each leaf represents a possible outcome. To find the probability of a specific outcome (a leaf), you multiply the conditional probabilities recorded along the branches of the path leading to that leaf.
* **The Multiplication Rule:** This rule mathematically formalizes the tree diagram approach for an intersection of multiple events. Assuming all conditioning events have positive probabilities, the rule is P(A₁ ∩ A₂ ∩ ... ∩ Aₙ) = P(A₁) * P(A₂|A₁) * P(A₃|A₁ ∩ A₂) * ... * P(Aₙ|A₁ ∩ ... ∩ Aₙ₋₁) .

### Deep Dive: Example 1.11 (Student Groups)
**Scenario:** A class of 4 graduate and 12 undergraduate students is randomly divided into 4 groups of 4.
**Events:** * A₁ = {students 1 and 2 are in different groups} 
* A₂ = {students 1, 2, and 3 are in different groups} 
* A₃ = {students 1, 2, 3, and 4 are in different groups} 

**Why P(A₁) = 12/15?**
1. **Place Student 1 (The Anchor):** There are 16 total chairs. Place Student 1 in any chair.
2. **Count Remaining Chairs:** There are exactly 15 empty chairs left in the room.
3. **Identify "Safe" Chairs for Student 2:** Student 1's group originally had 4 chairs, leaving 3 chairs in that specific group. This means 3 of the 15 remaining chairs will put Student 2 in the *same* group. The other 12 chairs (15 total - 3 in the same group) will put them in a *different* group. 
4. **Result:** Since Student 2 is equally likely to be assigned to any of the 15 remaining chairs, the probability of landing in a different group is 12/15.

**Why P(A₃) = P(A₁ ∩ A₂ ∩ A₃)?**
1. **Logical Nesting (Subsets):** If event A₃ happens (all four are separate), it is logically guaranteed that A₂ also happened (three are separate), and A₁ also happened (two are separate). Because A₃ cannot happen without A₁ and A₂ happening, A₃ is a subset of A₁ and A₂. The intersection (∩) represents the event where *all* conditions are met, which resolves to the most restrictive set: A₃.
2. **Setting up the Multiplication Rule:** The textbook expands it into an intersection to deliberately set up the multiplication rule, breaking the math down into bite-sized, sequential steps.

## 3. The Total Probability Theorem
This theorem provides a "divide-and-conquer" approach for calculating the probabilities of various events.

* **Partitioning the Sample Space:** The theorem applies when you have a set of disjoint events (A₁, ..., Aₙ) that form a complete partition of the sample space (every possible outcome falls into exactly one of these events).
* **The Formula:** For any given event B, its overall probability is found by adding up its intersections with each part of the partitioned space: P(B) = P(A₁)P(B|A₁) + ... + P(Aₙ)P(B|Aₙ) .
* **Intuitive Meaning:** The theorem views the sample space as divided into multiple scenarios. The total probability of event B occurring is calculated as a weighted average of its conditional probabilities under each specific scenario, with the weights being the unconditional probabilities of the scenarios themselves.