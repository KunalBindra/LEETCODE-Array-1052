# LEETCODE-Array-1052
Let's walk through the given `maxSatisfied` method step by step to understand how it works.

### Problem Description

The function aims to maximize the number of satisfied customers by using a technique to keep the shop owner's mood calm for a maximum of `X` minutes. During these `X` minutes, the shop owner will not be grumpy and all customers will be satisfied. Outside of these `X` minutes, if the owner is grumpy, some customers will not be satisfied.

### Function Details

1. **Input Parameters**:
   - `customers`: an array where `customers[i]` represents the number of customers at the `i-th` minute.
   - `grumpy`: an array where `grumpy[i]` is `1` if the owner is grumpy at the `i-th` minute, otherwise `0`.
   - `X`: an integer representing the maximum number of consecutive minutes during which the owner can be made not grumpy.

2. **Output**:
   - The function returns the maximum number of satisfied customers that can be achieved by calming the owner for `X` minutes.

### Walkthrough

Let's break down the code:

1. **Initialization**:
   ```java
   int satisfied = 0;
   int madeSatisfied = 0;
   int windowSatisfied = 0;
   ```

   - `satisfied`: Total customers satisfied when the owner is not grumpy.
   - `madeSatisfied`: The maximum number of customers we can additionally satisfy by calming the owner for `X` minutes.
   - `windowSatisfied`: The number of customers that can be satisfied in the current sliding window of `X` minutes.

2. **Main Loop**:
   ```java
   for (int i = 0; i < customers.length; ++i) {
     if (grumpy[i] == 0)
       satisfied += customers[i];
     else
       windowSatisfied += customers[i];
     if (i >= X && grumpy[i - X] == 1)
       windowSatisfied -= customers[i - X];
     madeSatisfied = Math.max(madeSatisfied, windowSatisfied);
   }
   ```

   - For each minute `i`:
     - If the owner is not grumpy (`grumpy[i] == 0`), add `customers[i]` to `satisfied`.
     - If the owner is grumpy (`grumpy[i] == 1`), add `customers[i]` to `windowSatisfied`.
     - If we have passed the first `X` minutes (`i >= X`), subtract the customers at the beginning of the window (if that minute was grumpy) from `windowSatisfied`.
     - Update `madeSatisfied` to be the maximum value between the current `madeSatisfied` and `windowSatisfied`.

3. **Final Calculation**:
   ```java
   return satisfied + madeSatisfied;
   ```
   - The result is the sum of `satisfied` (customers satisfied without any special treatment) and `madeSatisfied` (the best result from calming the owner for `X` minutes).

### Example Dry Run

Let's do a dry run with an example:

- `customers = [1, 0, 1, 2, 1, 1, 7, 5]`
- `grumpy = [0, 1, 0, 1, 0, 1, 0, 1]`
- `X = 3`

#### Step-by-Step Execution:

1. Initialize `satisfied = 0`, `madeSatisfied = 0`, `windowSatisfied = 0`.

2. Loop through each minute:

   - **i = 0**: `grumpy[0] = 0`
     - `satisfied = 1` (1 customer satisfied)
   - **i = 1**: `grumpy[1] = 1`
     - `windowSatisfied = 0 + 0 = 0` (0 customers affected)
   - **i = 2**: `grumpy[2] = 0`
     - `satisfied = 2` (1 more customer satisfied)
   - **i = 3**: `grumpy[3] = 1`
     - `windowSatisfied = 0 + 2 = 2` (2 customers affected)
     - `madeSatisfied = 2` (update madeSatisfied)
   - **i = 4**: `grumpy[4] = 0`
     - `satisfied = 3` (1 more customer satisfied)
     - `windowSatisfied = 2 + 0 = 2`
     - `madeSatisfied = 2`
   - **i = 5**: `grumpy[5] = 1`
     - `windowSatisfied = 2 + 1 = 3` (1 customer affected)
     - `madeSatisfied = 3`
   - **i = 6**: `grumpy[6] = 0`
     - `satisfied = 10` (7 more customers satisfied)
     - `windowSatisfied = 3 - 2 + 7 = 8` (subtracting the affected customers 3 minutes ago)
     - `madeSatisfied = 8`
   - **i = 7**: `grumpy[7] = 1`
     - `windowSatisfied = 8 + 5 = 13` (5 more customers affected)
     - `windowSatisfied = 13 - 2 = 11` (subtracting affected customers 3 minutes ago)
     - `madeSatisfied = 11`

3. Final calculation: `satisfied + madeSatisfied = 10 + 11 = 21`

The method works correctly, and the output for the example is `21`.
