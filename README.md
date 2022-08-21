# Ismail Sukkar's Portfolio

## [Project 1: Financial Analysis using R](https://gitlab.com/sugar_stats/financial-analysis)

A simple project that collects stock data and gives ratios of interest per stock. There are 4 scripts in this project that are meant to be run in tandem.

dataobtain.R captures data for ALL stocks on the NYSE, and stockdata.R calculates the Price/Book Ratio, the Current Ratio, the Net/Current Ratio and the P/E Ratio.

dataObtainSP500.R and stockdata500.R do the same, except they only use companies in the S&P 500.

Good luck!

Possible future updates:
1. Set the two scripts to run in tandem
2. Clean up the working directory

## [Project 2: Serverless RShiny App: Automodeler](https://gitlab.com/sugar_stats/automodeler-shiny-electron)

This work uses the technique to export shiny apps as .exe files in <https://github.com/COVAIL/electron-quick-start>.

The intentions of this project is to allow those with little to no R experience use a standalone app to create generalized linear models.

As of the current version, the only format the program takes is a csv file with 2 columns named 'x' and 'y', with y being the dependent variable and x being the independent variable. The program will check your assumptions, place it into a gaussian (normal) generalized linear model, give the R-squared and plot the data. 

## [Project 3: Clustering of Insect Genera to determine the appropriate selection of Indicies of Biotic Integrity](https://gitlab.com/sugar_stats/pmi-cpmi-striation)
![image](https://user-images.githubusercontent.com/111706007/185808401-22423c98-8828-4140-81e2-15847135255d.png)
A project performed to determine where different macroinvertebrate index of biotic integrity should be used in southern New Jersey. Currently a political boundary with a buffer zone is being used, and this project attempts to find a biological border. More information in the link above.


