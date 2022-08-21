# Ismail Sukkar's Portfolio

## [Project 1: Financial Analysis using R](https://gitlab.com/sugar_stats/financial-analysis)

A simple project that collects stock data and gives ratios of interest per stock. There are 4 scripts in this project that are meant to be run in tandem.

dataobtain.R captures data for ALL stocks on the NYSE, and stockdata.R calculates the Price/Book Ratio, the Current Ratio, the Net/Current Ratio and the P/E Ratio.

dataObtainSP500.R and stockdata500.R do the same, except they only use companies in the S&P 500.

Good luck!

Possible future updates:

Set the two scripts to run in tandem

Clean up the working directory

## [Project 2: Serverless Shiny App: Automodeler](https://gitlab.com/sugar_stats/automodeler-shiny-electron)

This work uses the technique to export shiny apps as .exe files in the <https://github.com/COVAIL/electron-quick-start>.

The intentions of this project is to allow those with little to no R experience use a standalone app to create generalized linear models.

As of the current version, the only format the program takes is a csv file with 2 columns named 'x' and 'y', with y being the dependent variable and x being the independent variable. The program will check your assumptions, place it into a gaussian generalized linear model, give the R-squared and plot the data.
