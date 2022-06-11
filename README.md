# Flexible GIC

A flexible GIC is an investment with an embedded option to redeem the principal and accrued interest at any time after 30 days from the date of purchase. In other words, the holder of GIC has an option to redeem the principal and accrued interest at any time after 30 days of from the date of purchase. No interest is paid if the investment is redeemed within first 30 days from the purchase date.

We price the option of a flexible GIC with a one factor Hull-White model via a trinomial tree. 
The Hull-White model assumes a normal distribution for the rates. Our solution constructs a Hull-White tree. The calibration procedures take an interest rate curve as input (ignoring volatility surfaces) and assume volatility and mean reversion parameters as constants. 

The width of the tree increases as a square root of time rather than linearly as in the Hull-White algorithm. The tree wings that are away from the mean by more than  t1/2 are cut off. The resulting error is of the order of 10-8, which is much less than the error resulting from discretization of the random walk component of the interest rate. This modification gives a noticeable acceleration of the calculations at a large number of time slices (the calculation time scales then as n2.5 rather than n3 of the Hull-White tree, where n stands for the number of the tree time slices).


The initial approximation of the short rate in the middle node of a time slice is taken as a forward rate for the time interval between the given and next slices, without using the Hull-White analytical approximation, The initial value is subsequently improved by the Newton-Raphson formula.

To facilitate the theta calculations, the time slice immediately following the root is separated from the root by a much smaller time interval than the intervals between the other time slices (typically it was taken as 10-4 of a regular time interval). The transition probabilities from the nodes of this, irregular time slice is calculated by the perturbation method.

The convergence of the tree based pricing method is slow. Generally, the numerical error of the tree methods scales as the inverse number of the time slices of the tree, while for the Hull-White methodology the time required for the tree calibration scales as the cube of the number of time slices. 

Therefore, the cost of a modest improvement of accuracy very quickly becomes prohibitive. This is especially true for the calculations of derivative based values, such as theta or duration. While the GA benchmark employed a modification of the original Hull-White method that makes the calibration time scale as the power 2.5 of the time slice number, that modification only slightly alleviated the problem.

Users should be aware of these complications when using the tree methods in general. The derivative based values (theta, vega, duration and individual key rate sensitivities) should be used with the understanding that their accuracy is inherently lower than that of the option price. The Hull-White model should not be used for calculations of duration values and the key rate sensitivities for volatilities larger than 30%, and should be used with caution for lower volatilities

References:

https://finpricing.com/lib/EqCliquet.html

https://osf.io/w97gv/wiki/home/

https://osf.io/q6ut7/download
