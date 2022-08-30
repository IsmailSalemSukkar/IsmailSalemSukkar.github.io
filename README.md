# Ismail Sukkar's Portfolio

## [Project 1: Clustering of Insect Genera to determine the appropriate selection of Indicies of Biotic Integrity](https://gitlab.com/sugar_stats/pmi-cpmi-striation)
### Introduction

On November 10th, 1978, the National Parks and Recreation Act was passed
by Congress, and established the Pinelands National Reserve and called
for the state of New Jersey to set up a management plan to preserve this
space . In June of 1979, the state of New Jersey supplemented this law
by passing the Pinelands Protection Act in order to require that all
local land use ordinances must comply with the management plan that
would beset up by the federal statute, as well as establishing the
Pinelands commission .

It was the job of the Pinelands Commission to draft this management plan
and delineate what areas should be considered part of this national
reserve. This was done via running extensive surveys of plants, mammals,
fish, reptiles, amphibians, and even people to assist with delineating
the Pinelands from the surrounding area. The commission ended up
creating two distinct regions of the Pinelands, the Preservation Area
and the Protection Area. The Preservation Area encompasses the unbroken
forests and berry farms where development is essentially banned, while
the Protection area is the surrounding regions that are generally more
inhabited and much more nuanced in its restrictions. In 1981, the
Commission completed their task and the Pinelands political boundary was
established .

One important survey that was overlooked was insect surveys,
particularly aquatic insect surveys. This is especially problematic
because the Environmental Protection Agency (EPA) recommends each state
to collect aquatic insects and apply them to locally calibrated Indices
of Biological Integrity (IBIs) to determine biological health of streams
and rivers. The general idea of IBIs is that as taxonomic diversity
increases, the IBI score increases, which should signify decreasing
degradation of the stream. The presence of pollution-sensitive species,
such as mayflies and stoneflies, is weighted higher than pollution
tolerant species, such as midges and leeches. For example, if a leech is
present in a stream, this will not increase the score as dramatically as
if a mayfly was present.

Before 2005, there were two separate IBIs used within New Jersey: The
Coastal Plain Macroinvertebrate Index (CPMI) and the High Gradient
Macroinvertebreate Index (HGMI). The border between these two separate
IBIs is the Atlantic Seaboard Fall Line. Everything north of the fall
line was considered to be under the purview of the HGMI, while
everything below as considered to be under the CPMI. However, sites
within the Pinelands seemed to score very poorly on the CPMI due to it
being penalized by the CPMI’s use of mayflies as biological indicators.
Mayflies are intolerant to low pH, which is the normal condition for
Pinelands streams . This led to calls to create a new IBI for the
region.

The IBI developed for the Pinelands region is called the Pinelands
Macroinvertebrate Index (PMI). It was decided that every site within the
Pinelands political boundary, and an additional 5 kilometer buffer,
would be analyzed with this newly created index. This buffer was chosen
arbitrarily to ensure streams that had similar character with Pinelands
streams would not be excluded from the PMI. The PMI does not use
mayflies as biological indicators, instead relying on caddisflies and
stoneflies .

One major outcome of this situation was that the CPMI lost many of their
reference sites when the region was split. There are only 6 reference
sites remaining in the CPMI . While this is fine and good for the PMI,
the CPMI is now stuck in a position where if it ever needs to be
recalibrated there will be serious issues in doing so. This is
especially problematic because that issue is currently being researched
and studied .

One possible solution to this is delineate these two regions not on an
arbitrary buffer and a political boundary, but via the data that has
been collected for the past 20 years. The state of New Jersey analyzes
their waters at the resolution of sub-watershed, also known as the 14
digit Hydrologic Unit Map (HUC14), and chooses sampling locations in
order to summarize the condition of the specific watershed in question.
While some sub-watersheds have more than one site, this is the
exception, not the rule. It follows logically that the waters of a
watershed should be draining into the same basin, yet the current border
has numerous sub-watershed regions split in half. There is no reason to
split a watershed in half when it comes to delineating the PMI and CPMI
regions. These new boundaries could follow the borders of these
sub-watershed regions, giving a far more ecological and hydrological
rational to the positioning of the border instead of just a 5 kilometer
buffer region.

Since we are looking at Macroinvertebrate IBIs, one way to generate this
border is to analyze the insect community structures of the many sites
and create clusters based on the structure. Using genera would be most
logical, since that is what the IBIs are based off of. Since the
original borders of the Pinelands used animal surveys, it is fitting
that this new border in terms of insects should use aquatic insect
surveys . Since abiotic factors have also been found to have a distinct
character within the Pineland Region, namely pH and specific
conductivity , these factors should also be taken into consideration.

It should be clear that an arbitrary buffer or reliance on a boundary
that did not take insect communities into account should be updated now
that the data is available to do so. There are a few possible outcomes
of this study:

1.  This study will find that there are not any distinct regions in the
    area either biotically or abiotically. This may provide evidence
    that this entire region, in the aquatic sense, was once the same
    character, and degradation or other factors have causes there to be
    a separation.

2.  The study will determine that the current border between the CPMI
    and PMI region was too generous to the PMI region, and using the
    abiotic, biotic, or a combination of the two datasets will help in
    finding a true border between the regions.

3.  The study will find brand new borders biotically or abiotically that
    are not related to the current borders.

This is purely to determine which index should be used at each stream.
There is no attempt here to comment on the need to increase or decrease
the size of the Pineland Preserve.

### Methods

Data was obtained through the Water Quality Portal
(<https://www.waterqualitydata.us/>), a data repository hosted by the
United States Geological Survey (USGS), and the Environmental Protection
Agency (EPA). All data within the the Water Quality Portal is quality
assured and made publicly available.

The data was obtained by setting the organization ID as *NJDEP_BFBM*,
and downloading *Sample Results (Biological Metadata)* and *Biological
Habitat Metrics*. The last data set used in this analysis is simply a
table that displays the family, genus and possibly species of the
collected insects, included in the appendix.

The primary software used was RStudio. RStudio was used to access the
language R, which is instrumental in cleaning, visualizing, modeling and
mapping out the data. QGIS was also used to lower the resolution of the
data points to the sub-watershed level.

Two separate sets of data were used to cluster the data, the biotic data
and the abiotic data.

###### Biotic Cluster

A site by genera matrix was used to cluster the data biologically. The
clustering algorithm chosen was a spherical k-means algorithm, due to
genera matrices being especially suited to spherical k-means .

This data was generated by averaging the numbers of each genera found in
each site over the past 20+ years of sampling. Once the averages were
found in each site, they were scaled and centered using the generic
scale function found in base R package. They were then placed in to the
Spherical K-Means function (skmeans) function in the skmeans package to
generate the clusters. Since this purpose of this study was to find two
distinct clusters, only two clusters were entered into the function.
These two clusters were then formatted by the function SpatialPoints in
the package sp to prepare it to enter the Average Nearest Neighbor Index
function (nni) in the package spatialEco to find if there was any
spatial component to the clustering.

###### Abiotic Cluster

Abiotic parameters were also collected, with pH and specific
conductivity generally found to have a distinct character in less
disturbed Pinelands streams . These two factors shall be used to
generate an abiotic cluster using the same methods outlined in the
biotic cluster. Spherical k-means will also be used to generate clusters
for abiotic maps in order to stay consistent.

###### Combined Cluster

In order to see if these two cluster would compliment or detract from
each other, a final cluster was performed that used genera, pH and
specific conductivity. This cluster also used the same method outlined
in the biotic cluster, and still used the spherical k-means algorithm to
remain consistent. The only exception is that any site that lacked
either specific conductivity or pH was dropped, since it would be
impossible to cluster the dataset otherwise.

###### Lowering the Resolution of Data Points

To generate the final border, lowering the resolution to the HUC14 level
is necessary. Each cluster was brought into QGIS and overlaid on a map
with the HUC14s of New Jersey. An intersecting algorithm was performed,
where the HUC14 would change color depending on the cluster of the
datapoint. HUC14s that lacked any datapoint would be assumed to follow
the original borders of the 5km buffer.

### Results

###### Current Index Distribution Cluster

The current index distribution and sub-watershed resolution equivalent
appear to be a very high level of spatial separation for the two
regions, shown by the strong level of clustering for at least one
cluster (**Figure 1**). This initial step shows the extent of clustering
in the original maps to use as a baseline to any further generated
clusters. Only one clusters is spatially statistically significant
(Cluster 1 *p* \<0.001).

![image](https://user-images.githubusercontent.com/111706007/187327560-9939cade-19b9-4573-aa46-0039ac79f513.png)

![image](https://user-images.githubusercontent.com/111706007/187327572-e3e90646-b071-439a-ab6d-21f9bde8cc1b.png)

###### Biotic Cluster

While the clustering found via genera is not nearly as visually
clustered as the currently used maps, there does appear to have some
weaker significance in one of the clusters (Cluster 2’s *p* = 0.078).
There also appears to be some interference within the dataset when
expanding the data to the HUC14 resolution (**Figure 2**).

![image](https://user-images.githubusercontent.com/111706007/187327591-58fa5cd7-e419-4c9a-a508-ca5088aa3a1f.png)

![image](https://user-images.githubusercontent.com/111706007/187327604-1f2ec0d9-dfc8-4461-9a65-cbae5e51b4e4.png)

###### Abiotic Cluster

The clustering found via pH and specific conductivity can be found in
**Figure 3**. Clustering in the HUC14 level appears to have far less
interference than the biotic map. This is further solidified by the fact
that both clusters had some level of spatial clustering (Cluster 1 *p* =
0.09, Cluster 2 *p* = 0.011).

![image](https://user-images.githubusercontent.com/111706007/187327628-b75621d5-41fd-4fbe-9075-6d7b7ef92a73.png)

![image](https://user-images.githubusercontent.com/111706007/187327634-7b3e9818-1613-461c-9197-cc1bf82aceac.png)

###### Combined Cluster

The clustering found via pH, specific conductivity and genera can be
found in **Figure 4**. When combining both biotic and abiotic factors,
the map shows one spatially significant cluster and one insignificant
cluster. However, since genera is our factor of interest to input into
our IBIs, this may be the best we can do. The HUC14 resolution reveals a
distribution very similar to the genera cluster map shown in **Figure
2**, but with less interference.

![image](https://user-images.githubusercontent.com/111706007/187327645-0a5d9d93-733c-4637-83dc-47b9bf7fe965.png)

![image](https://user-images.githubusercontent.com/111706007/187327650-2302c4b4-0dc1-4626-a4a0-707da3d9ca69.png)

### Discussion

The current boundary of the PMI does not accurately reflect the current
aquatic insect assemblage or the water quality characteristics that are
specific to the Pinelands region. The spherical k-means algorithm
produced excellent clusters that show some real progress into solving
this dilemma of separating southern New Jersey by the appropriate index.

One interesting factor is that every single clustering method
essentially shows that the current border of the PMI is too lenient.
This is especially apparent in the northwest of the clusters within the
PMI region, where each cluster shows that the northwest is not similar
in character to the other sites. This does make sense, since this region
is flowing into the Delaware River, while the majority of the waterways
in the Pinelands flows into the Atlantic Ocean directly.

While each cluster has its purpose, the cluster with the most readily
usable data is likely the cluster showing the abiotic and biotic
factors. Since New Jersey’s macroinvertebrate IBIs are only using genera
as of now, using only abiotic factors is not appropriate. If these
clusters were for a more geological purposes, than the abiotic
separation would likely be more appropriate. However, abiotic factors
are useful since they appear to have a more stabilizing effect to the
genera data, where it lowers the amount of interference in the model.

The few outlier HUC14s in that are CPMI regions surrounded by PMI
regions could be due to agriculture or other degradations (**Figure
4b**). The current CPMI regions are more degraded than the PMI regions,
so a degraded Pineland sub-watershed would likely would have enough in
common to be grouped with the CPMI region in the spherical k-means
algorithm. Future analysis that incorporates land use data will likely
determine the cause of these outliers.

The EPA’s study in separating New Jersey into distinct eco-regions
appear to corroborate the results of this study , and contain the
majority of the datapoints within the proposed PMI region (**Figure
5**). When extrapolating to sub-watershed, there are more visual
differences, but the majority of these differences were in
sub-watersheds without data. If these sub-watersheds had data, they may
have also corroborated with the EPA ecoregion.

There are also a few outlier HUC14s near the border of southern NJ that
are PMI regions surrounded by CPMI regions (**Figure 4b**). This unique
pocket is also found in every other clustering method. These same
pockets were also found in the ecoregions . With two studies
corroborating the existence of this pocket, this pocket most likely also
has a Pineland-like character and should also be considered part of the
PMI.

Using the appropriate IBI for the appropriate region is key to keeping
the streams in NJ fishable, swimmable and drinkable. Data generated
using an index due to an arbitrary reason cannot be relied on to detect
trends or other analysis. A more accurate approach will allow the state
to detect any degradation occurring in streams with more confidence.

![image](https://user-images.githubusercontent.com/111706007/187327676-e4eaacd4-1634-4180-b987-b2ea6b555406.png)

![image](https://user-images.githubusercontent.com/111706007/187327686-f13c6ee1-a3ad-49e8-88c6-9bb848e07c5d.png)

### Conclusion

The borders proposed in **Figure 4b** have a far more ecologically sound
basis than the one currently used in **Figure 1a**. Stronger analysis
would have to be performed on the outlier HUC14s to see why exactly they
are showing opposite character of their sister HUC14s. Choosing the
correct IBI to use based on **Figure 4b**, or a future improvement of
4b, will allow the state of New Jersey to have a much stronger and more
logical separation of the PMI and CPMI regions instead of the current
use of a political boundary with an arbitrary buffer.

## [Project 2: Is it really more dangerous to drive in the holiday season?](https://gitlab.com/sugar_stats/holidaytraffic)

### Introduction

It is very common to hear the phrase, "Thanksgiving Eve is the most dangerous night to drive" and other warnings after attending a holiday party, but how accurate is it? This is what I intended to find out.

In order to see how accurate this statement was in New Jersey, a dataset with the traffic data from the nation was pulled from here: <https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents> (cited below). The variables of interest were Date, the amount of accidents per date, and month.

After summarizing and fixing the data to grant the frequency per date, I ran 2 models. The first model was to test if month was a significant factor, and the second was to see if increasing date was a significant factor.

### Month

``` r
freqVSmonth <- glmmTMB(frequency~(Month),  family=nbinom2(),data=trafDataNJ)
fitMonth <- r.squaredGLMM(freqVSmonth)
emmeans(freqVSmonth,pairwise~Month,type = "response")
fitMonth
```

![image](https://user-images.githubusercontent.com/111706007/187333842-88b2573f-3195-4147-92a8-5f1d1e961684.png)

The above showed that month was statistically significant (p \< 2.2e-16) but was a relatively weak correlation (Pseudo R² = 0.06). The above also showed that the Fall/Winter Months were all statistically similar, but were statistically different from the Spring/Summer months.

### Date

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

### Conclusion

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

The first model attempted to use the ordinal variables representing severity (1,2,3,4, with 4 being the most severe and 1 being the least) as integer values, and a poisson distribution was used. Unfortunately, the qqplots and other assumptions failed.

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

