# Sugar's Portfolio

## [Project 1: Clustering of Insect Genera to determine the appropriate selection of Indicies of Biotic Integrity](https://gitlab.com/sugar_stats/pmi-cpmi-striation)

Currently, there are two seperate aquatic macroinvertebrate indicies of biotic integrity of use within southern NJ, the Coastal Plain Macroinvertebrate Index (CPMI), and the Pineland Macroinvertebreate Index (PMI). The current borders of use for these indicies are currently a 5km buffer on a political boundary. I decided that it would be best to use a more scientific approach to creating this boundary.  


![image](https://user-images.githubusercontent.com/111706007/187327676-e4eaacd4-1634-4180-b987-b2ea6b555406.png)
A spherical k-means on the insect genera found, pH and Specific Conductivity was ran, and two distincy clusters were found. What was interesting about this was that these clusters found appear to agree with the eco-regions proposed by the EPA and USGS. Further study will be needed before anything can be definitively concluded. 



## [Project 2: Is it really more dangerous to drive in the holiday season?](https://gitlab.com/sugar_stats/holidaytraffic)



It is very common to hear the phrase, "Thanksgiving Eve is the most dangerous night to drive" and other warnings after attending a holiday party, but how accurate is it? This is what I intended to find out.

In order to see how accurate this statement was in New Jersey, a dataset with the traffic data from the nation was pulled from here: <https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents> (cited below). The variables of interest were Date, the amount of accidents per date, and month.

After summarizing and fixing the data to grant the frequency per date, I ran 2 models. The first model was to test if month was a significant factor, and the second was to see if increasing date was a significant factor.


``` r
freqVSmonth <- glmmTMB(frequency~(Month),  family=nbinom2(),data=trafDataNJ)
fitMonth <- r.squaredGLMM(freqVSmonth)
emmeans(freqVSmonth,pairwise~Month,type = "response")
fitMonth
```

![image](https://user-images.githubusercontent.com/111706007/187333842-88b2573f-3195-4147-92a8-5f1d1e961684.png)

The above showed that month was statistically significant (p \< 2.2e-16) but was a relatively weak correlation (Pseudo R² = 0.06). The above also showed that the Fall/Winter Months were all statistically similar, but were statistically different from the Spring/Summer months.


``` r
freqVsDate <- glmmTMB(frequency~(Date),family="nbinom2",data=trafDataNjDate)
fitDate <- r.squaredGLMM(freqVsDate)
plot(simulateResiduals(freqVSmonth)) #QQ plot and residual plot look acceptable
check_overdispersion(freqVSmonth) #If unsure, use a poisson first and check if overdispersion exists
Anova(freqVsDate,type=2)
fitDate
```

![image](https://user-images.githubusercontent.com/111706007/187333793-34c27bc2-e696-4f49-baf7-e7dfc614d15a.png)


Using date gave a much stronger correlation, with a Pseudo R² of 0.38 and a p-value of \< 2.2e-16. This shows that as the date increases from January to December, accident rates appear to increase! Since the holiday season is generally at the end of the year, this does support the hypothesis that the holiday season is more dangerous to drive in.


This study is not the end-all, be-all on this topic. One major issue is that while this dataset had multiple years, accidents increased rapidly each year for NJ. I hightly doubt that there were only 500 accidents in the state in the year 2017, and this number quadrupled in 2021. The most obvious answer is that certain agencies began reporting in 2021, or certain sources were missed when this dataset was created. It is very possible that in 2021 this same issue was occuring, where in January it was lower due to the dataset having more accident data as it becomes more recent. 

For now, however, my curiousity is sated and I see that the holiday season should definitely be a time of high alert when driving. 


Moosavi, Sobhan, Mohammad Hossein Samavatian, Srinivasan Parthasarathy, and Rajiv Ramnath. "A Countrywide Traffic Accident Dataset.", 2019.

Moosavi, Sobhan, Mohammad Hossein Samavatian, Srinivasan Parthasarathy, Radu Teodorescu, and Rajiv Ramnath. "Accident Risk Prediction based on Heterogeneous Sparse Data: New Dataset and Insights." In proceedings of the 27th ACM SIGSPATIAL International Conference on Advances in Geographic Information Systems, ACM, 2019.


## [Project 3: Comparing New Jersey's traffic data to the nation](https://gitlab.com/sugar_stats/nj_usa_traffic_data_analysis)

I was just wondering how safe New Jersey's roads were compared to the rest of the nation, and performed a study to do this.

In order to see how New Jersey's road safety compares with the rest of the nation, a dataset from with the traffic data from the nation was pulled from here: <https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents> (cited below). The variables of interest were Severity and State.

The dataset is "big data" which R is not so good with, so was placed on a MariaDB server and the RMariaDB package was used to query the database and pull the fields of interest.

The dplyr function sample_n was used to pull a 1000 unit random subsample from each state in order to produce the dataset for this study. Unforunately, the raw dataset is far too large to use.

Since we are attempting to determine a relationship between an ordinal variable (Severity defined as 1,2,3 and 4) and a nominal variable (State), we should first determine if the nominal variable has a significant effect on the ordinal variable, and then use a post-hoc test to see the significant contrasts. The most appropriate test for this is the Kruskal-Wallis test.

```{r}
kruskal.test(Severity~State,data=trafDataSample)

```

The above model showed that State has a significant effect on the severity of accidents. We then had to determine effect size to see if this has a real world effect. The appropriate test would be the epsilon-squared test

```{r}
library(rcompanion)
epsilonSquared(trafDataSample$Severity, trafDataSample$State)
```

This showed a moderate effect, so we can confidently move forward with a post-hoc test. It was difficult to find a post-hoc test that was ok with a ordinal vs nominal test, but eventually the kruskalmc test was used.

```{r}
library(pgirmess)
datMC <- kruskalmc(Severity~State,data=trafDataSample)
```

The important results of this test, namely the significance of contrasts, we then saved and used for later models.

I decided to then compare two metrics, the average accident rating and the percent chance an accident will be severe. The average accident rating used a poisson regression and used the ordinal variables as numbers. The regression was likely not necassary, and I could have probably just taken the mean accident rating of each State.

The percent chance of severity was determined by taking turning severity into a binomial distribution. Severety levels 1 and 2 were counted as not severe and levels 3 and 4 were considered severe. A binomial generalized linear model was then used to determine significance and then the emmeans package was used to determine the odds ratio.

The final table I made has comparisons to New Jersey. What was interesting to me is that many regions in the mid-west have higher accident ratings and higher probabilities of a severe accident then NJ, despite NJ being the most densely populated state.

![image](https://user-images.githubusercontent.com/111706007/187337759-fa784560-d898-492b-b295-72c266a7ea24.png)
Alternavively, instead of looking at the table, the above plot can be used to visualize the above, where we can see that a far few states have average accident ratings above NJ. 

-   Moosavi, Sobhan, Mohammad Hossein Samavatian, Srinivasan Parthasarathy, and Rajiv Ramnath. “[A Countrywide Traffic Accident Dataset](https://arxiv.org/abs/1906.05409).”, 2019.

-   Moosavi, Sobhan, Mohammad Hossein Samavatian, Srinivasan Parthasarathy, Radu Teodorescu, and Rajiv Ramnath. ["Accident Risk Prediction based on Heterogeneous Sparse Data: New Dataset and Insights."](https://arxiv.org/abs/1909.09638) In proceedings of the 27th ACM SIGSPATIAL International Conference on Advances in Geographic Information Systems, ACM, 2019.

## [Project 4: Financial Analysis using R](https://gitlab.com/sugar_stats/financial-analysis)

A simple project that collects stock data and gives ratios of interest per stock. There are 4 scripts in this project that are meant to be run in tandem.

dataobtain.R captures data for ALL stocks on the NYSE, and stockdata.R calculates the Price/Book Ratio, the Current Ratio, the Net/Current Ratio and the P/E Ratio.

dataObtainSP500.R and stockdata500.R do the same, except they only use companies in the S&P 500.

Good luck!

Possible future updates:
1. Set the two scripts to run in tandem
2. Clean up the working directory

## [Project 5: Serverless RShiny App: Automodeler](https://gitlab.com/sugar_stats/automodeler-shiny-electron)

This work uses the technique to export shiny apps as .exe files in <https://github.com/COVAIL/electron-quick-start>.

The intentions of this project is to allow those with little to no R experience use a standalone app to create generalized linear models.

As of the current version, the only format the program takes is a csv file with 2 columns named 'x' and 'y', with y being the dependent variable and x being the independent variable. The program will check your assumptions, place it into a gaussian (normal) generalized linear model, give the R-squared and plot the data. 

