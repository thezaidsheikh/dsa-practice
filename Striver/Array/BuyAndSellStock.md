Prob: https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/

Sol 1: Using linear Search by remembering the past.
```java
class Solution {
    public int maxProfit(int[] prices) {
       int profit = 0;
        int buying_price = prices[0];

        for(int i = 1; i < prices.length; i++) {
            if(prices[i] < buying_price) buying_price = prices[i];
            else if(prices[i] >= buying_price) {
                profit = Math.max(profit, prices[i] - buying_price);
            }
        }
        return profit;
    }
}
```
Time complexity - O(n),
Space complexity - O(1)

Sol 2: It is also a linear search
```java
public class Solution{
    public static int maximumProfit(ArrayList<Integer> prices){
        int profit =0;
        int min = prices.get(0);

        for(int i = 1; i < prices.size(); i++) {
            int price = prices.get(i) - min;
            profit = Math.max(profit,price);
            min = Math.min(min,prices.get(i));
        }
        return profit;

    }
}
```
Time complexity - O(n),
Space complexity - O(1)