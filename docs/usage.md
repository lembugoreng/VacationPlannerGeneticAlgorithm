# Usage Guide

This page demonstrates how to run the genetic algorithm loops in the notebook and how to adapt them in your own scripts.

## Run inside the notebook
1. Open `Genetic Algorithm.ipynb`.
2. Run cells sequentially. Each method (2%, 4%, 5% mutation) will iterate through generations, track best fitness, and plot the progression using Seaborn.

## Minimal script example
The following standalone example mirrors the notebook’s structure and shows how to run one GA loop and plot fitness progression.

```python
import random
import pandas as pd
import seaborn as sns

# Public API from the notebook

def individual(vacationDuration, hotelStarRating, touristSpot, oneTouristSpot, transportFreq, transportFees, foodprice, foodFreq):
    return (
        (vacationDuration * hotelStarRating)
        + (touristSpot * oneTouristSpot)
        + (transportFreq * transportFees)
        + (foodprice * foodFreq)
    )


def fitness(moneyTarget, vacationDuration, hotelStarRating, touristSpot, oneTouristSpot, transportFreq, transportFees, foodprice, foodFreq):
    moneyused = individual(
        vacationDuration,
        hotelStarRating,
        touristSpot,
        oneTouristSpot,
        transportFreq,
        transportFees,
        foodprice,
        foodFreq,
    )
    total = moneyTarget - moneyused
    if total < 0:
        return -99999
    elif (total == 0) and (moneyused == 5000):
        return 99999
    else:
        return abs(1 / total)


# GA loop (Mutation ~2%)
random.seed(42)
solutions = []
for _ in range(1000):
    solutions.append(
        (
            random.uniform(1, 300),  # hotelStarRating ~ price
            random.uniform(1, 6),    # touristSpot
            random.uniform(1, 300),  # oneTouristSpot
            random.uniform(1, 10),   # transportFreq
            random.uniform(1, 100),  # transportFees
            random.uniform(1, 50),   # foodprice
            random.uniform(1, 20),   # foodFreq
        )
    )

generation_list = []
fitness_list = []

for generation_idx in range(10000):
    ranked_solutions = []
    for s in solutions:
        ranked_solutions.append(
            (
                fitness(5000, 5, s[0], s[1], s[2], s[3], s[4], s[5], s[6]),
                s,
            )
        )
    ranked_solutions.sort()
    ranked_solutions.reverse()

    generation_list.append(generation_idx)
    fitness_list.append(ranked_solutions[0][0])

    if ranked_solutions[0][0] > 999:
        break

    best_solutions = ranked_solutions[:100]

    # Extract the top candidates into separate lists
    hotelStarList, touristList, oneTouristList, transportFreqList, transportFeesList, foodPriceList, foodFreqList = (
        [] for _ in range(7)
    )
    for _, params in best_solutions:
        hotelStarList.append(params[0])
        touristList.append(params[1])
        oneTouristList.append(params[2])
        transportFreqList.append(params[3])
        transportFeesList.append(params[4])
        foodPriceList.append(params[5])
        foodFreqList.append(params[6])

    # Mutation by ~2%
    new_generation = []
    for _ in range(5000):
        e1 = random.choice(hotelStarList) * random.uniform(0.99, 1.01)
        e2 = random.choice(touristList) * random.uniform(0.99, 1.01)
        e3 = random.choice(oneTouristList) * random.uniform(0.99, 1.01)
        e4 = random.choice(transportFreqList) * random.uniform(0.99, 1.01)
        e5 = random.choice(transportFeesList) * random.uniform(0.99, 1.01)
        e6 = random.choice(foodPriceList) * random.uniform(0.99, 1.01)
        e7 = random.choice(foodFreqList) * random.uniform(0.99, 1.01)
        new_generation.append((e1, e2, e3, e4, e5, e6, e7))

    solutions = new_generation

# Plot
sns.lineplot(x=range(len(fitness_list)), y=fitness_list)
```

## Tweaking mutation intensity
- ~2% mutation: multiply each selected parameter by `random.uniform(0.99, 1.01)`
- ~4% mutation: use `random.uniform(0.99, 1.03)`
- ~5% mutation: use `random.uniform(0.98, 1.03)` or similar

Higher mutation rates typically increase exploration but may slow convergence.

## Tips
- Consider seeding the RNG for reproducibility: `random.seed(42)`.
- If converting the notebook into a module, move `individual` and `fitness` into a `.py` file and import them in your scripts and notebooks.