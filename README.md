# Ismail Sukkar's Portfolio

## [Project 1: Analysis of the effect of Demographic group vs Vaccination Rate](https://gitlab.com/sugar_stats/demovsvaxrate)

There have been many anectodal claims in the media that certain demographic groups are less likely to be vaccinated than others. I wanted to see if this was supported by data.

In order to do this, I decided to use the makeup of currently vaccinated people, and compare that to the makeup of this country. If the demographic percentages of vaccinated people were comparable to the demographic percentages of the census, than this would be considered false. The alternative hypothesis would be if the percentages of vaccinated people were different than the percentages in the census, then the claims the media makes may have some merit.

I first checked to see if the vaccinated % were different than census data. Using a one-way t-test, I found that all groups differed significantly from their census population percentages.

|                                    | Hispanic | AIAN/Native American | Asian | Black | NHOPI/Pacific Islander | White |
|-----------|-----------|-----------|-----------|-----------|-----------|-----------|
| Census %                           | 18.9%    | 1.3%                 | 6.1%  | 13.6% | 0.3%                   | 59.3% |
| Average Vaccine PopulationMakeup % | 18.5%    | 1.0%                 | 6.6%  | 9.3%% | 0.29%                  | 57.8% |
| Performance                        | -2%      | -23%                 | 8%    | -32%  | -2%                    | -3%   |

Asian Americans were one of the few groups that consistantly made up a larger percentage of the vaccination groups than the census data would show, with an 8% overperformance.

Most other groups underperformed their census data, but since its only by 2-3% there may not be a real world isue. However, Black Americans and Native Americans underperformed by over 3%, with 32% and 23% underperformance respectively.

![image](https://user-images.githubusercontent.com/111706007/206538044-15e3ea2d-6f7c-47ab-8513-38fc2a4874b7.png)


Unfortunately, it appears that the anecdotal evidence presented by the media did have some truth to it. It does appear that black and native americans both have poorer vaccination rates than the general public. Considering both groups are historically marginalized, more work needs to be done to assist these groups and educate about he safety of the vaccines.

One flaw with the dataset, and the study as a whole, is that 33% of americans did not report their demographic data. For all we know, this may be where the majority of black and native americans are located. Since both of these groups are marginalized and do not trust the government for various reasons (Tuskagee, etc ), this is not a far-fetched hypothesis.



## [Project 2: Tax Analysis on the 1%](https://gitlab.com/sugar_stats/taxanalysis)

This project initially started off as an attempt to see if different states had different federal effective tax rates (ETR) on the 1% of this country. Utilizing linear models, this was determined to be incorrect for the top 1% of this country, but accurate for other percentiles. This made me question why this was true.

I began a second analysis of determining the AGI makeup of the different AGI-percentiles, and it was interesting to note a few things:


![image](https://user-images.githubusercontent.com/111706007/206015056-c65ade0e-24f8-49a0-8455-3438b10b5bb3.png)


1.  The AGI of the 1% is the only bracket where salary was NOT the majority of their income (35% of AGI for the 1% vs 64% of AGI for the 5%)

    !![image](https://user-images.githubusercontent.com/111706007/206015146-56b8778b-9927-42d7-b343-10abc6d5d0fd.png)


2.  The ETR plateau at about 1 million AGI, around 35%. One issue here is that since we are only looking at average income per bracket, its not possible to really check the actual point of plateau.

    It appears that the 1% achieve this tax "plateau" via diversifying their AGI sources. Since salary is traditionally thought of as the most taxed AGI source, while sources like captial gains is capped at 15%, there are many ways for the 1% to avoid paying income taxes.

    | 1%    | 5%    | 10%   | 25%   | 50%   | 75%      |
    |-------|-------|-------|-------|-------|----------|
    | 0.161 | 0.282 | 0.165 | 0.204 | 0.116 | Not Sig. |

    : The slope of salary when compared to the ETR of each respective percentile

3.  Salary appears to be a large driver of taxes for every bracket except the 75%. What is interesting is that Salary is a larger drive of taxes for the 5% then the 1%, while the 10% and the 1% have salary being drivers of similar strength. This makes sense since while the 1% do pay more in salary/income taxes, its a smaller percentage of their income then the 5%.


## [Project 3: Clustering of Insect Genera to determine the appropriate selection of Indicies of Biotic Integrity](https://gitlab.com/sugar_stats/pmi-cpmi-striation)

Currently, there are two seperate aquatic macroinvertebrate indicies of biotic integrity of use within southern NJ, the Coastal Plain Macroinvertebrate Index (CPMI), and the Pineland Macroinvertebreate Index (PMI). The current borders of use for these indicies are currently a 5km buffer on a political boundary. I decided that it would be best to use a more scientific approach to creating this boundary.  


![image](https://user-images.githubusercontent.com/111706007/187327676-e4eaacd4-1634-4180-b987-b2ea6b555406.png)
A spherical k-means on the insect genera found, pH and Specific Conductivity was ran, and two distincy clusters were found. What was interesting about this was that these clusters found appear to agree with the eco-regions proposed by the EPA and USGS. Further study will be needed before anything can be definitively concluded. 



## [Project 4: Is it really more dangerous to drive in the holiday season?](https://gitlab.com/sugar_stats/holidaytraffic)



It is very common to hear the phrase, "Thanksgiving Eve is the most dangerous night to drive" and other warnings after attending a holiday party, but how accurate is it? This is what I intended to find out.

In order to see how accurate this statement was in New Jersey, a dataset with the traffic data from the nation was pulled from here: <https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents> (cited below). The variables of interest were Date, the amount of accidents per date, and month.

After summarizing and fixing the data to grant the frequency per date, I ran 2 models. The first model was to test if month was a significant factor, and the second was to see if increasing date was a significant factor.


``` r
freqVSmonth <- glmmTMB(frequency~(Month),  family=nbinom2(),data=trafDataNJ)
fitMonth <- r.squaredGLMM(freqVSmonth)
emmeans(freqVSmonth,pairwise~Month,type = "response")
fitMonth
```

![image](https://user-images.githubusercontent.com/111706007/188328024-367a6ac8-0071-437d-b3e2-79de9ea7e52c.png)

The above showed that month was statistically significant (p \< 2.2e-16) but was a relatively weak correlation (Pseudo R² = 0.06). The above also showed that the Fall/Winter Months were all statistically similar, but were statistically different from the Spring/Summer months.


``` r
freqVsDate <- glmmTMB(frequency~(Date),family="nbinom2",data=trafDataNjDate)
fitDate <- r.squaredGLMM(freqVsDate)
plot(simulateResiduals(freqVSmonth)) #QQ plot and residual plot look acceptable
check_overdispersion(freqVSmonth) #If unsure, use a poisson first and check if overdispersion exists
Anova(freqVsDate,type=2)
fitDate
```

![image](https://user-images.githubusercontent.com/111706007/188327866-af5e2b7c-8ad1-4839-b7b3-bb12f1becc9f.png)



Using date gave a much stronger correlation, with a Pseudo R² of 0.38 and a p-value of \< 2.2e-16. This shows that as the date increases from January to December, accident rates appear to increase! Since the holiday season is generally at the end of the year, this does support the hypothesis that the holiday season is more dangerous to drive in.


This study is not the end-all, be-all on this topic. One major issue is that while this dataset had multiple years, accidents increased rapidly each year for NJ. I hightly doubt that there were only 500 accidents in the state in the year 2017, and this number quadrupled in 2021. The most obvious answer is that certain agencies began reporting in 2021, or certain sources were missed when this dataset was created. It is very possible that in 2021 this same issue was occuring, where in January it was lower due to the dataset having more accident data as it becomes more recent. 

For now, however, my curiousity is sated and I see that the holiday season should definitely be a time of high alert when driving. 


Moosavi, Sobhan, Mohammad Hossein Samavatian, Srinivasan Parthasarathy, and Rajiv Ramnath. "A Countrywide Traffic Accident Dataset.", 2019.

Moosavi, Sobhan, Mohammad Hossein Samavatian, Srinivasan Parthasarathy, Radu Teodorescu, and Rajiv Ramnath. "Accident Risk Prediction based on Heterogeneous Sparse Data: New Dataset and Insights." In proceedings of the 27th ACM SIGSPATIAL International Conference on Advances in Geographic Information Systems, ACM, 2019.


## [Project 5: Comparing New Jersey's traffic data to the nation](https://gitlab.com/sugar_stats/nj_usa_traffic_data_analysis)

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

## [Project 6: Financial Analysis using R](https://gitlab.com/sugar_stats/financial-analysis)

A simple project that collects stock data and gives ratios of interest per stock. There are 4 scripts in this project that are meant to be run in tandem.

dataobtain.R captures data for ALL stocks on the NYSE, and stockdata.R calculates the Price/Book Ratio, the Current Ratio, the Net/Current Ratio and the P/E Ratio.

dataObtainSP500.R and stockdata500.R do the same, except they only use companies in the S&P 500.

Good luck!

Possible future updates:
1. Set the two scripts to run in tandem
2. Clean up the working directory

<script src="https://public.tableau.com/javascripts/api/tableau-2.js"></script>
<div id="tableauViz">
function initializeViz() {
  var placeholderDiv = document.getElementById("tableauViz");
  var url = "https://public.tableau.com/app/profile/ismail.salem.sukkar/viz/prototypeInvest/Dashboard1";
  var options = {
    width: '600px',
    height: '600px',
    hideTabs: true,
    hideToolbar: true,
  };
  viz = new tableau.Viz(placeholderDiv, url, options);
}


</div>
## [Project 7: Serverless RShiny App: Automodeler](https://gitlab.com/sugar_stats/automodeler-shiny-electron)

This work uses the technique to export shiny apps as .exe files in <https://github.com/COVAIL/electron-quick-start>.

The intentions of this project is to allow those with little to no R experience use a standalone app to create generalized linear models.

As of the current version, the only format the program takes is a csv file with 2 columns named 'x' and 'y', with y being the dependent variable and x being the independent variable. The program will check your assumptions, place it into a gaussian (normal) generalized linear model, give the R-squared and plot the data. 

