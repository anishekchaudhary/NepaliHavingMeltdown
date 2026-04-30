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

8) joined two stocks Snow Cover and Glacier Volume with the flow Glacial Deposit

9) Changed the equation of Meltwater Inflow 
from: x
to: MIN(...., x)

10) Corrected the value of GLOF Threshold from 50 to 5



Changes:

# 9) Base Tempertaure to be change from 14 to lets say -15 ??

# 10) Change Human Emissions equation

# 11) Change Change GLOF Threshold

12) Why are we not considering temperature in Glacial Deposit

13) We can make Base Deforestation Rate as a Policy

14) Will there be an effect of Glacial Lakes on Glacial melt Rate?

15) Should we link Black Carbon Emission with Human Emission. We can make it policy dependent?

16) Fix GLOF Risk Index as it is causing trouble in Deforestation.

17) How do we ensure that Melt Water Inflow a=is always less than Glacier Melt Rate?

18) Make Greenhouse Emission a policy.

19) What is Ice Fraction?

20) Check equations of Ice Fraction and Surface Albedo 




# Inputs to the "Snow Cover & Glacier Volume" System are Temperature & Black Carbon

# 


Policy :

1) Human Emission: We will try to limit year on year Human Emission Growth Rate from 1.8% per year to 1% per year
from: 36 * EXP(0.018 * (Time - 1980))
to: IF THEN ELSE ( Time < 2030 , 36 * EXP(0.018 * (Time - 1980)) , 36 * EXP(0.01 * (Time - 1980)) )



2) Black Carbon Emission: We will Try to Reduce the year on year Black Carbon Emission growth rate from 1.2% per year to -0.6% per year
from: 60 * EXP(0.012 * (Time - 1980))
to: IF THEN ELSE ( Time < 2030 , 60 * EXP(0.012 * (Time - 1980)) , EXP(0.012 * 50) *  60 * EXP(-0.03 * (Time - 2030)) )



3) Chnage Melt Sensitivity: 
from:  0.86
to: IF THEN ELSE ( Time < 2030 , 0.86 , 0.86 * (1 - 0.3) ) 

# Screenshots are taken from the Policy 2 model's folder and model




3) CCS -> but is very aspirational

Main :- Glacier Volume, GHG, Black Carbon, Human Emission

A graph showing Base, Policy 1, Policy 2, Policy 3 , Policy 1+2+3 for Glacier Volume





