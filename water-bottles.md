# Water Bottles

## Problem description: 

There are `numBottles` water bottles that are initially full of water. You can exchange `numExchange` empty water bottles from the market with one full water bottle.

The operation of drinking a full water bottle turns it into an empty bottle.

Given the two integers `numBottles` and `numExchange`, return the _*maximum* number of water bottles you can drink._

## Examples

9 full bottles --> Drink --> 9 empty bottles --> Exchange --> 3 full bottles --> Drink --> 3 empty bottles --> Exchange --> 1 full bottle --> Drink --> 1 empty bottle --> Cannot exchange further, DONE

Input: `numBottles = 9`, `numExchange = 3`
Output: `19`
Explanation: You can exchange 3 empty bottles to get 1 full water bottle. Number of water bottles you can drink: `9` + `3` + `1` = `13`

## Constraints

- `1 <= numBottles <= 100`
- `2 <= numExchange <= 100`

## My solution

First, based on the constraints we can rule out one case. If numBottles is less than numExchanged when passed in to the function, then we can immediately return numBottles, as we will never have enough to exchange.

```java
Class Solution {
    public int numWaterBottles(int numBottles, int numExchange) {
        
        if (numBottles < numExchange) { return numBottles; }

    }
}
```

Next, we will need a `sum` to keep count of how many bottles of water we drink. After the bottles are drank, we need to figure out how many empty bottles we can exchange for another full bottle of water. This is simple integer division. For example, if we have `numBottles = 2` bottles, and the exchange rate is `numExchange = 2`, then we can exchange our 2 empty bottles for 1 full bottle of water, or `exchangedBottles = numBottles / numExchange` (division between two int's in Java is integer division by default). `exchangedBottles` can then be added to our `sum`, since we are tracking how many full bottles we can drink. 

Then we need to calculate the remaining bottles leftover. There are situations where we have more bottles than we can exchange, such as `numBottles = 10` and `numExchange = 4`. In this case, after exchanging, we will have 2 full bottles and 2 remaining bottles. Once the two full bottles are drank, we have 4 total empty bottles, which could be exchanged for 1 more full bottle. To calculate the remaining bottles, we can just use modulo division: `remainingBottles = numBottles % numExchange`

This needs to be implemented in a loop. We can use a `while(numBottles < numExchange)` loop condition, updating numBottles to be `numBottles = exchangedBottles + remainingBottles` until `numBottles < numExchange` return `true`, which means we no longer have enough bottles to exchange for a full bottle of water.

Now to implement it all:

```java
Class Solution {
    public int numWaterBottles(int numBottles, int numExchange) {
        
        // If we are unable to exchange, then numBottles is max num of bottles
        if (numBottles < numExchange) { return numBottles; }
        
        int sum = numBottles;
        while (numBottles < numExchange) { 
            
            // compute exchanged bottles
            int exchangedBottles = numBottles / numExchange;
            int remainingBottles = numBottles % numExchange;

            // Track the newly exchanged bottles
            sum += exchangedBottles;

            // Update numBottles to be the total of newly exchanged and remaining bottles
            numBottles = exchangedBottles + remainingBottles;
        }

        return sum;
    }
}
```

Analyzing the time complexity of the solution, since the iteration condition is being updated via division, this would be considered O(log[n]).

