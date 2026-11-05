# Abigail Ray || ECON 3916 || OLS Diagnostics: When the Standard Error Is Lying

## Objective
I used simulated data with known violations and California Housing data to show how heteroskedasticity, outliers and omitted variables affect OLS estimates and standard errors, and what diagnostics can and cannot catch.

## Methodology
- Simulated errors whose spread grows with x and fitted one regression with classical and with robust (HC3) standard errors.
- Ran 400 simulated samples to check how often each 95% interval covered the true slope.
- Read four residual plots (well behaved, heteroskedastic, missing curvature, an outlier) beside Breusch-Pagan and Jarque-Bera tests.
- Measured leverage, standardised residual and Cook's distance for the outlier panel, then deleted the most influential row and refit.
- Fitted a California Housing regression with classical and HC3 standard errors and looked at the districts stuck at the 5.0 ceiling.
- Wrote a function, `hc3_se()`, that fits a regression and returns the HC3 standard error of one coefficient, and ran it on all districts and on the districts below the ceiling.
- For the take-home challenge, built a regression that passes every diagnostic and is still wrong because of an omitted confounder.

## Key Findings
- The slope was 2.0147 under both methods, against a true 2.0. The standard error was 0.0264 classical and 0.0290 robust, so heteroskedasticity changed the standard error and left the estimate alone.
- In 400 samples, the classical interval covered the true slope 91.8% of the time (Monte Carlo SE 1.4%) and the robust one 93.5% (Monte Carlo SE 1.2%), against a nominal 95%. Only the classical shortfall was clearly more than chance.
- Breusch-Pagan caught the heteroskedastic panel (p = 0.0000) but missed the missing-curvature panel (p = 0.7918). Only Jarque-Bera flagged the curvature (p = 0.0159), and the plot showed it plainly.
- Deleting one row out of 600 moved the slope 0.58 of its own standard error, but the standard error fell 33%, from 0.0317 to 0.0213. The outlier affected the standard error much more than the slope.
- On California Housing, the HC3 standard error on AveRooms was 0.0121 against a classical 0.0024, about 5.02 times larger. The coefficient was -0.0273.
- With `hc3_se()`, the HC3 standard error on AveRooms was 0.0121 with all districts and 0.0082 without the 965 districts at the 5.0 ceiling (4.68% of 20,640). I did not treat the smaller number as a reason to drop them, since that changes the sample.
- In the challenge, a regression with an omitted confounder passed every diagnostic (Breusch-Pagan p = 0.2602, Jarque-Bera p = 0.6404, largest Cook's distance 0.0132) and still gave a coefficient of 1.976 against a true effect of 1.0. Adding the confounder back gave 1.009. Diagnostics test the model's internal assumptions, but they cannot test whether x is correlated with the error.
