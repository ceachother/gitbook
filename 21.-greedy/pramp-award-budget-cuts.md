# Pramp: Award Budget Cuts

> The awards committee of your alma mater \(i.e. your college/university\) asked for your assistance with a budget allocation problem they’re facing. Originally, the committee planned to give N research grants this year. However, due to spending cutbacks, the budget was reduced to newBudget dollars and now they need to reallocate the grants. The committee made a decision that they’d like to impact as few grant recipients as possible by applying a maximum cap on all grants. Every grant initially planned to be higher than cap will now be exactly cap dollars. Grants less or equal to cap, obviously, won’t be impacted.
>
> Given an array grantsArray of the original grants and the reduced budget newBudget, write a function `findGrantsCap` that finds in the most efficient manner a cap such that the least number of recipients is impacted and that the new budget constraint is met \(i.e. sum of the N reallocated grants equals to newBudget\).
>
> **Example**:
>
> ```text
> input:  grantsArray = [2, 100, 50, 120, 1000], newBudget = 190
>
> output: 47 # and given this cap the new grants array would be
>            # [2, 47, 47, 47, 47]. Notice that the sum of the
>            # new grants is indeed 190
> ```

## Solution: Greedy

It's very hard to think in this way during interview!!!!!!

```python
    def find_grants_cap(self, grantsArray, newBudget):
        if grantsArray is None or newBudget <= 0:
            return 0

        length = len(grantsArray)

        grantsArray.sort()
        for index in range(length):
            current = grantsArray[index]
            numbersLeft = length - index
            if current > newBudget / numbersLeft:
                return newBudget / numbersLeft
            newBudget -= current

        return grantsArray[length - 1]
```

Or the way from Pramp: sorting in descending order:

```python
    def find_grants_cap(self, grantsArray, newBudget):
        #  [2, 50, 100, 120, 1000]
        # 190/5 = 38
        # [2, 38, 38, 38, 38] <190
        # 38 - 190 ->(190-38)/2 = 76
        # [2, 76, 50 ,76, 76] > 190
        # 38 - 76
        n = len(grantsArray)

        grantsArray.sort(reverse=True)
        grantsArray.append(0)
        surplus = sum(grantsArray) - newBudget
        if surplus <= 0:
            return grantsArray[0]
        for i in range(n):
            surplus -= (i + 1) * (grantsArray[i] - grantsArray[i + 1])
            if surplus <= 0:
                break
        return grantsArray[i + 1] + (-surplus / float(i + 1))
```

