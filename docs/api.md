# API Reference

This page documents the public functions used by the notebook.

## `individual(vacationDuration, hotelStarRating, touristSpot, oneTouristSpot, transportFreq, transportFees, foodprice, foodFreq)`

Computes the combined cost/score for a candidate solution.

### Parameters
- `vacationDuration` (float|int): Number of days of the vacation.
- `hotelStarRating` (float): Proxy for lodging price per day.
- `touristSpot` (float): Number or scale of tourist spots.
- `oneTouristSpot` (float): Per-spot cost or score multiplier.
- `transportFreq` (float): Number of transport uses.
- `transportFees` (float): Per-transport cost.
- `foodprice` (float): Average meal price.
- `foodFreq` (float): Number of meals.

### Returns
- `float`: Combined cost/score computed as:
  - `(vacationDuration*hotelStarRating) + (touristSpot*oneTouristSpot) + (transportFreq*transportFees) + (foodprice*foodFreq)`

### Example
```python
from math import isfinite

# Example values derived from the notebook
score = individual(
    vacationDuration=5,
    hotelStarRating=255.7384,
    touristSpot=4.9247,
    oneTouristSpot=272.4080,
    transportFreq=9.2073,
    transportFees=96.1070,
    foodprice=44.2664,
    foodFreq=10.0758,
)
assert isfinite(score)
print("Individual score:", score)
```

---

## `fitness(moneyTarget, vacationDuration, hotelStarRating, touristSpot, oneTouristSpot, transportFreq, transportFees, foodprice, foodFreq)`

Evaluates how good a candidate is relative to a target budget.

### Parameters
- `moneyTarget` (float|int): Target budget to stay at or under.
- `vacationDuration` (float|int)
- `hotelStarRating` (float)
- `touristSpot` (float)
- `oneTouristSpot` (float)
- `transportFreq` (float)
- `transportFees` (float)
- `foodprice` (float)
- `foodFreq` (float)

### Returns
- `float`: Fitness value computed from `total = moneyTarget - moneyused` where `moneyused` is the same expression as in `individual(...)`.
  - If `total < 0`: returns `-99999` (overspent).
  - If `total == 0` and `moneyused == 5000`: returns `99999` (perfect match at 5000).
  - Otherwise: returns `abs(1/total)`.

### Example
```python
# Budget target of 5000 as used in the notebook examples
fit = fitness(
    moneyTarget=5000,
    vacationDuration=5,
    hotelStarRating=255.7384,
    touristSpot=4.9247,
    oneTouristSpot=272.4080,
    transportFreq=9.2073,
    transportFees=96.1070,
    foodprice=44.2664,
    foodFreq=10.0758,
)
print("Fitness:", fit)
```

### Typical ranges (as used in the notebook)
These are the random initialization ranges used to generate candidate solutions:
- `hotelStarRating`: 1 to 300
- `touristSpot`: 1 to 6
- `oneTouristSpot`: 1 to 300
- `transportFreq`: 1 to 10
- `transportFees`: 1 to 100
- `foodprice`: 1 to 50
- `foodFreq`: 1 to 20

Adjust as needed for your domain.