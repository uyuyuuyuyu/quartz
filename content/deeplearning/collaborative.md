# Lecture Notes: Deep Learning Foundations - Probabilities, Loss, and Collaborative Filtering

## 1. Sigmoid and Probabilities
* **The Problem:** Neural networks output raw, unconstrained numbers (logits) that can range from negative to positive infinity. 
* **The Solution (Sigmoid/Softmax):** The Sigmoid function ($\sigma(x) = \frac{1}{1 + e^{-x}}$) acts as a mathematical "trash compactor," squishing any input into a strict $0 \rightarrow 1$ scale.
* **Interpretation:** While we call the output a "probability" because it fits the mathematical rules (between 0 and 1), it is practically just a **confidence score**. Modern neural networks are often poorly calibrated, meaning a 99% confidence score does not guarantee a 99% real-world accuracy rate.

## 2. Cross-Entropy Loss
* **Concept:** A grading system based on Information Theory that penalizes a model for confident but incorrect predictions.
* **The Math:** Multiplies the true probability by the logarithm of the predicted probability. 
* **The Collapse:** Because the true label is usually One-Hot Encoded (e.g., $1.0$ for the correct class, $0.0$ for all others), the math for the wrong answers gets multiplied by zero and vanishes. The formula simplifies to: $Loss = - \log(P_{\text{correct}})$.
* **Why Logarithm?** It creates a non-linear, exponential penalty. If the model assigns a very low probability to the correct answer, the loss skyrockets.

## 3. PyTorch Loss Functions: Class vs. Function
Almost every loss function in PyTorch exists in two formats:
* **`nn.CrossEntropyLoss` (The Class):** * Instantiated as an object in your model's `__init__`.
  * **Stateful:** It remembers parameters (like class weights for imbalanced data).
  * **Benefit:** PyTorch automatically handles moving its weights to the GPU along with the model. Cleaner for production code.
* **`F.cross_entropy` (The Function):** * Called directly in the training loop.
  * **Stateless:** Has no memory; you must pass all parameters manually every time you call it.
  * **Benefit:** Great for quick experiments or vanilla math with no special rules.

## 4. Multi-Target Models
* **Concept:** Training a single neural network to predict multiple distinct targets (e.g., Rice Disease AND Rice Variety) simultaneously.
* **The Architecture:** Expand the final layer to output the combined total of both targets (e.g., 20 outputs instead of 10).
* **The Loss Trick:** Write a custom loss function that slices the output array in half, calculates Cross-Entropy for Target A, calculates Cross-Entropy for Target B, and adds the two losses together. Backpropagation forces the model to optimize for both.

## 5. Collaborative Filtering (Recommendation Systems)
* **Goal:** Matrix Completion. Predicting missing values in a dataset (e.g., guessing user movie ratings).
* **Latent Factors (Embeddings):** Hidden traits represented by a list of numbers assigned to both Users and Movies. 
* **The Dot Product:** `User_Factors * Movie_Factors = Predicted_Rating`.
* **The Excel Proof:** Deep learning isn't magic. You can initialize random factors in a spreadsheet, calculate Mean Squared Error (MSE) against known ratings, and use Excel's Solver tool to tweak the factors until the error minimizes.

## 6. One-Hot Encoding and Embeddings
* **One-Hot Vector:** Translates a category into math. It's a list of $0$s with a single $1$ at the index of the specific category (e.g., `[0, 1, 0]`).
* **Embeddings:** A computational shortcut (a lookup table). 
* **The Lookup Trick:** Multiplying a One-Hot Vector by a massive parameter matrix mathematically erases all rows except the one corresponding to the $1$. Therefore, an embedding layer skips the expensive matrix multiplication and simply does an array lookup by index. 
  * *Code Example:* `user_factors.t() @ one_hot_3` (Retrieves the exact latent factors for User #3).

## 7. Backpropagation vs. Gradient Descent
They are two halves of the same process:
* **Backpropagation (The GPS):** The calculus. Uses the Chain Rule to calculate the gradients (slopes) backwards through the network to see how much each weight contributed to the error. It changes no numbers.
* **Gradient Descent (The Driver):** The optimization. Takes the gradients provided by backprop and actually updates the weights by stepping them down the slope.

## 8. L2 Regularization & Weight Decay
* **Goal:** Prevent overfitting (memorizing noisy data) by restricting model complexity.
* **L2 Regularization (The Loss):** Adds a penalty to the loss function based on the squared size of the weights ($\sum w^2$). It forces the model to use a large number of very small weights, rather than a few massive ones.
  * *Vs. L1 (Absolute Value):* L1 applies a constant pull and forces most weights to exactly zero (sparse). L2 applies an elastic pull (smooth derivative) and distributes the workload.
* **Weight Decay (The Action):** The mathematical consequence of L2 during Gradient Descent. Because the derivative of $w^2$ is $2w$, the update rule mathematically forces the weight to shrink (decay) by a small percentage on every single training step *before* applying the normal gradient.