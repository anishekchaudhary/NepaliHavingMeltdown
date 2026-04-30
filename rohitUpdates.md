The following changes have been made from the context to the current model provided:

1) corrected the equation of Ocean Sink 
from 10.6 * LN(MAX(GreenHouseGas / Initial GHG, 1) + 1) 
to 10.6 * LN(GreenHouseGas / Initial GHG + 1)

2) changes equation of Snow Probability
from MAX(0, MIN(1, 1 / (1 + EXP(1.2 * (Temperature - 3)))))
to 1 / (1 + EXP ( 1.2 * (Temperature - 3) ) )

3) Changed unit of Surface Melt Rate
from km*km*km/Year
to km*km*km/(Year*Celsius)

4) Added arrow from GLOF Risk Index to Flood Pressure 

5) Added dependencies of GLOF Risk Index

6) Removed the variable named Duraition

7) Removed the shadow varioable Snowfall Precipitation

8) joined two stocks Snow Cover and Glacier Volume with the flow Glacial DepositPermafrost Emission 

9) Changed equations of Permafrost Emission 
from IF THEN ELSE(Delta Temperature > Permafrost Thaw Threshold,
	0.037 * (Delta Temperature - Permafrost Thaw Threshold), 0)

to IF THEN ELSE(Delta Temperature > Permafrost Thaw Threshold,
	0.0154 * (Delta Temperature - Permafrost Thaw Threshold), 0)





Changes to be made and thought about:

# 9) change INITIAL value of Base Tempertaure. Currently the value is 14, however this value does not seem appropriate as the base temperature at an approximate altitude of 45000 feet in nepal where glaciers are present. 

# 10) Change is required in the equation of Human Emissions 

# 11) Change is required in the equation of GLOF Threshold

12) Why are we not considering temperature in Glacial Deposit

13) We can make Base Deforestation Rate as a Policy

14) Will there be an effect of Glacial Lakes on Glacial melt Rate?

15) Should we link Black Carbon Emission with Human Emission. We can make it policy dependent?

16) Fix GLOF Risk Index as it is causing trouble in Deforestation.

17) How do we ensure that Melt Water Inflow is always less than Glacier Melt Rate?

18) We can make Greenhouse Emission as a policy.

19) What is the meaning of the variable Ice Fraction?

20) Check equations of Ice Fraction and Surface Albedo 

21) Make a dependency graph in text form.

22) Generate a very breif one line explanation of the meaning of every veriable/flow/stock.