# STA380: Homework #2
Billy Yuan, Lindsay Tober  
August 16, 2016  




# Flights at ABIA



There comes a time in a person's life when he or she has to travel to a place that is too far to reach by car, and there is no choice but to resort to air travel. Unfortunately, air travel comes with a litany of issues, some of which include overpriced food, long security lines, and crying babies. Among all of the possible issues associated with air travel, a delayed flight may be the most frustrating for customers. Fortunately, by using flight data from all flights that either landed at or departed from Austin-Bergstrom International Airport (APIA), we could pick flights that have the least probability of getting delayed.

This analysis will address the following three questions that relate to choosing flights that have the least likelihood of getting delayed:

* What are the best times of the year and week to book air travel?
* Which airlines have the most delays?
* How does your destination impact the answers to the first 2 questions?

For analysis purposes, we will assume any departure delay greater than 10 minutes to be a delayed flight regardless of what percentile this number may be bucketed to. Even if 10 minutes is considered below average for a flight during the Christmas season, a delay of 10 minutes is still infuriating for the customer. 

### Best Times to Travel (on Average)

To begin, let's understand the general trends of delayed flights. The chart below shows how the time of year impacts the number of delayed flights.

<img src="STA380_HW2_files/figure-html/delay_by_month_chart-1.png" style="display: block; margin: auto;" />

<img src="STA380_HW2_files/figure-html/dealy_by_weekday_chart-1.png" style="display: block; margin: auto;" />

According to both of these charts, the month and day with the lowest percentage of delayed flights is October and Saturday, respectively. However, these percentages aggregate all destinations and airlines together.

### Airlines to Avoid

Next, let's look at which airlines have the highest percentage of delayed flights. 

<img src="STA380_HW2_files/figure-html/delay_by_airline_chart-1.png" style="display: block; margin: auto;" />

<font size="0.5">***US Airways data only available from January-September 2008. October-December data is combined with American Airlines due to a merger.</font>

The two worst airlines for delays are Atlantic Southeast Airlines and Comair. In addition to those two, the percentage of flights with departure delays in 2008 from seven different airlines exceeded 20%. 

However, airlines don't have flights to every city and may specialize in certain regions. For example, in 2008, Southwest Airlines did not have any flights to New York City from Austin. If there are multiple airlines that fly to a city, how can a customer know which airline to take? And if that city has multiple airports, which airport should he or she choose?

### Tale of Cities with Two Airports: Choosing the Best Airlines and Months to Travel

While customers usually have limited flexibility on travel dates, times, and destinations, they often have most control over flight options when traveling to a city with multiple airports (e.g., Dallas/Ft. Worth metroplex is serviced by both DFW and Dallas Love Field).

Suppose customer 'Mary' has extended holiday time and is planning to visit three cities, each of which has two major airports (New York City, Dallas, and Chicago).  Mary will be flying out of Austin and is looking to minimize the chance of getting a delay for each of her trips. To support her decision on when and how to fly to each city, she has three questions:

* For what months should Mary book her flights?
* For each city, which airport should she fly to ?
* Which airlines should she choose?

Charts of % departure delays, by month and airline, for each city she plans to visit are shown below.

<center><h5>% Flights to NYC with >10 min Departure Delay by Airline</h5></center>



<img src="STA380_HW2_files/figure-html/NYC plot-1.png" style="display: block; margin: auto;" />

<center><h5>% Flights to DAL with >10 min Departure Delay by Airline</h5></center>



<img src="STA380_HW2_files/figure-html/Dallas plot-1.png" style="display: block; margin: auto;" />

<center><h5>% Flights to Chicago with >10 min Departure Delay by Airline</h5></center>



<img src="STA380_HW2_files/figure-html/CHI plot-1.png" style="display: block; margin: auto;" />

According to these 3 groups of charts, September and October appear to be the best times to fly, which is consistent with what the very first chart of this analysis showed. However, the best airlines for each city vary. Below is a sample itinerary that Mary could use:


Table: Sample Itinerary

                Airline and Airport      Month       Day of Week 
--------------  -----------------------  ----------  ------------
New York City   JetBlue Airways JFK      October     Wednesday   
Dallas          American Airlines DFW    November    Tuesday     
Chicago         Southwest Airlines MDW   September   Wednesday   

Segmenting data to match the scope of the problem is extremely important. In this case, the problem is figuring out which airlines to take to cities that have 2 airports. For example, although Southwest Airlines had the second-highest percentage of delayed flights when including all of its destinations, its flights to Midway Airport in Chicago are relatively punctual between June and September. However, because this analysis only contains data fron 2008, one-off events such as storms or emergencies may skew the data. Regardless of this limitation, the general trends seem to be consistent: flights during the holidays experience the most delays, and flights during the middle of the week have the least delays. Beyond this, generalizations are slightly harder to make.

</br>

# Author Attribution
Author attribution is one of the many applications of text analysis. Given a group of documents by various authors, how accurately can we predict the authorship of an out-of-sample document? Reuters, a well-known news source, is a prime subject for conducting author attribution analysis.



### Preparing the Data
The Reuters C50 corpus contains articles written by 50 different authors, where each author has 50 'training' articles and 50 'testing' articles.  The training articles were used to build a Document Term Matrix for predicting author attribution of the 50 testing articles.  This required some transformation of the data in pre-processing (e.g., changing case to all lowercase; removing numbers, punctuation, and excess white-space; and eliminating 'stop words' that are common enough in the English language to remove valuable insight for author attribution) to allow for effective text analysis.

Given the training and testing data sets are two separate corpuses, aligning the terms used in their Document Term Matrices to ensure matching dimensions is critical to accurately modeling author attribution. Establishing a single 'umbrella' term list for the two corpuses requred a couple of assumptions.  First, new terms existing in articles from the testing data set (i.e., terms present in the testing articles that are not present in the training articles) would not have any weight indicators to assist in attributing the articles to a given author, so these 'new' terms were excluded from the umbrella term list. Second, terms present in the training articles that were not present in the testing articles were kept in the umbrella term list as they were used in building the model on the training data (e.g., for Naive Bayes, the weight matrix is constructed using the bag of words model, which required usage of all words to accurately build weights).  This resulted in approximately 10,000 irrelevant words for the test data set, but it ensured that the weights of the common words were accurate for a given document and author.

### Model Summary
Two models were selected to predict author attribution of the testing articles: (1) Principal Component Multinomial Logistic Regression and (2) Naive Bayes.

#### Model 1: Principal Component Multinomial Logistic Regression

Conducting Principal Compenent Analysis (PCA) on the training articles provided contexts, or loadings, to apply to the Document Term Matrix.  Given each of the 50 authors had 50 articles included in the training set, PCA resulted in 2,500 principal components to capture variance across the 2,500 documents within the training corpus.  With limitations on computational power, the first 100 principal components were selected for running multinomial logistic regression.  This subset of 100 principal components accounted for 45% of the variance and still took a significant time to process.

A multinomial logistic regression model was trained to predict authors on the 100 principal components for all training articles, then applied to the testing articles to predict author identities in the out-of-sample data set. Multinomial logistic regression using PCA resulted in a test accuracy of 65.46%.

#### Model 2: Naive Bayes

For Naive Bayes, the Document Term Matrix was used to create a training table of multinomial probability vectors by author. Each multinomial probability vector was calculated by aggregating the scores by term across documents written by a given author, adding a smoothing factor (1/2500) to ensure non-zero totals, and taking the respective probability for that term against the author's full volume of terms.  The resulting multinomial probability vectors provide a 'bag of words' situational probability of selecting that word at random from within the host of articles written by that author.

Using the training table of multinomial probability vectors by author, log probabilities for each article within the testing data set were calculated under the Naive Bayes model.  Whichever author resulted in the highest sum of log probabilities was taken as the 'predicted' author. The Naive Bayes model resulted in a test accuracy of 55.28%.


### Model Evaluation
Taken at face value, the Naive Bayes model outperformed the Principal Component Multinomial Logistic Regression (PCR) model by almost 10% (64.56% vs. 55.28%, respectively).  However, model accuracy varied by author:


Table: Author Prediction Accuracy by Model

Author              NB Accuracy   PCR Accuracy 
------------------  ------------  -------------
AaronPressman       90.00 %       88.57 %      
AlanCrosby          96.00 %       75.00 %      
AlexanderSmith      43.90 %       58.73 %      
BenjaminKangLim     44.83 %       38.24 %      
BernardHickey       78.95 %       65.52 %      
BradDorfman         61.76 %       69.84 %      
DarrenSchuettler    52.38 %       46.88 %      
DavidLawder         34.62 %       27.27 %      
EdnaFernandes       89.29 %       70.97 %      
EricAuchard         44.62 %       67.27 %      
FumikoFujisaki      98.00 %       95.74 %      
GrahamEarnshaw      70.91 %       85.37 %      
HeatherScoffield    32.79 %       34.62 %      
JanLopatka          58.33 %       57.50 %      
JaneMacartney       17.19 %       26.44 %      
JimGilchrist        86.21 %       89.09 %      
JoWinterbottom      86.96 %       91.67 %      
JoeOrtiz            50.68 %       70.18 %      
JohnMastrini        46.34 %       45.21 %      
JonathanBirt        80.39 %       72.86 %      
KarlPenhaul         90.91 %       83.87 %      
KeithWeir           69.70 %       65.62 %      
KevinDrawbaugh      62.50 %       68.89 %      
KevinMorrison       60.42 %       53.73 %      
KirstinRidley       80.49 %       74.07 %      
KouroshKarimkhany   91.49 %       86.79 %      
LydiaZajc           91.89 %       77.27 %      
LynneO'Donnell      90.91 %       81.82 %      
LynnleyBrowning     87.72 %       89.13 %      
MarcelMichelson     69.57 %       78.95 %      
MarkBendeich        72.34 %       71.88 %      
MartinWolk          91.18 %       65.38 %      
MatthewBunce        97.78 %       91.30 %      
MichaelConnor       84.62 %       81.58 %      
MureDickie          33.33 %       40.82 %      
NickLouth           80.39 %       85.00 %      
PatriciaCommins     73.47 %       70.83 %      
PeterHumphrey       44.58 %       52.94 %      
PierreTran          64.71 %       61.54 %      
RobinSidel          88.10 %       78.33 %      
RogerFillion        90.91 %       85.37 %      
SamuelPerry         64.15 %       54.90 %      
SarahDavison        44.44 %       69.70 %      
ScottHillis         16.90 %       21.74 %      
SimonCowell         60.00 %       51.79 %      
TanEeLyn            39.02 %       38.18 %      
TheresePoletti      67.27 %       86.96 %      
TimFarrand          69.09 %       65.71 %      
ToddNissen          37.74 %       29.63 %      
WilliamKazer        39.58 %       48.78 %      

As the chart shows, Naive Bayes showed the most benefit over PCR for author Martin Wolk, with a test accuracy of 91.18% for NB and 65.39% for PCR. On the flip side, PCR performed best over Naive Bayes for author Sarah Davison, with a test accuracy of 69.70% for PCR and 44.44% for NB. Aside from the biggest gaps in accuracy between the models, the chart comparing model accuracy reveals that some authors  had high success on both models (e.g., Aaron Pressman, Fumiko Fujisaki) or low success on both models (e.g., David Lawder, Jane Macartney). What might be driving these issues?

We can start to assess this question by looking at the plot of authors against the top two Principal Components:
<img src="STA380_HW2_files/figure-html/PCR_plot-1.png" style="display: block; margin: auto;" />



Authors 38 (Peter Humphrey) and 46 (Tan EeLyn) stand out as two authors  that are closer together in the plane of P1 x P2 but further away from most of the other authors.  Going back to the summary table, we see that Tan EeLyn (author 46) had test accuracies of 39.02% for NB and 38.18% for PCR.  Given the close distance to 38, we can check the portion of errors where test articles for author 46 were incorrectly attributed to author 38.  Out of 30 incorrect attributions for author 46, 15 documents (or 50.00%) were attributed to author 38.

How does this interaction play out within the Naive Bayes model?  For Naive Bayes, 55.88% of the incorrect attributions for author 46 were to author 38.  Similar to the closer loadings of PC1 and PC2 under the PCR model, we can confirm whether authors 46 and 38 have similar weights for the Naive Bayes model.  This is done easily by comparing the top 10 terms from both of authors' multinomial probability vectors:


Table: Top 10 Terms of Multinomial Probability Vector

Author 46 (Tan EeLyn)   Authors 38 (Peter Humphrey) 
----------------------  ----------------------------
beijing                 beijing                     
british                 british                     
china                   china                       
chinas                  chinese                     
chinese                 hong                        
hong                    kong                        
kong                    kongs                       
kongs                   legislature                 
legislature             people                      
tungchan                tungchan                    

Both lists of terms are very similar, which may explain why many of the test articles for author 46 are attributed to author 38.  Take, for example, test article #2261, which had the following term frequencies:


Table: Top 10 Terms by Frequency for Test Article #2261

               Frequency in Document
------------  ----------------------
hong                              16
kong                              14
legislature                        9
july                               6
abode                              5
beijing                            5
handover                           5
provisional                        5
china                              4
june                               4

Many of these terms appear in the top 10 lists for authors 46 and 38. In addition, the higher frequency of legislature in test article #2261 may have been one of the key drivers for attributing that document to author 46 instead of 38, as 'legislature' has a higher relative multinomial probability for author 46.

While both models significantly improved the chances of correctly attributing the author - up to 64.56% for NB and 55.28% for PCR from 1/50, or 2% at random - Naive Bayes did have a slightly higher test accuracy.  In addition, the results of the Naive Bayes model were easier to interpret, given the 'bag of words' probabilities by term are more intuitive than the contextual representation of Principal Components.  As such, we prefer to use the Naive Bayes model for author attribution.  Given there were only 50 articles from each author in the training data set, we believe that the Naive Bayes model could be significantly improved by including additional training articles to further differentiate multinomial probability vectors between different authors.

</br>



# Practice with Association Rule Mining
Applying association rule mining to market baskets is a helpful method to understand and predict market basket associations.  For example, pretend that 'Paul' is marketing director of a local grocery store. Paul is interested in understanding the purchasing habits of the store's customers to inform the new layout management is rolling out in the next month.  Specifically:

* Which items should be located to the front of the store?
* Which items should be grouped together (i.e., which items are customers more likely to buy together)?  

These questions can be addressed using association rule mining, which helps to predict additional items within a customer's basket (consequents) given an initial set of market basket items (antecedents).


Taking transactional data on grocery purchases, we can run an 'apriori' algorithm to give Paul the information he needs. To focus in on key relationships, we will set thresholds as follows:

* Support threshold > 1%, to include only antecedents that show up in over 1% of all baskets (casting a wide net)
* Confidence threshold > 10%, to include only antecedents that have the associated consequents over 10% of the time 
* Max predictor items = 3, to include only antecedents with 3 or less items, as there is not sufficient shelf space to group a higher number of items


### Subset 1: Frequent Purchases
After running the algorithm, the first thing we notice is that eight items have no antecedents:




Table: Market Basket Rules with No Antecedents

Rules                         Support   Confidence   Lift
-------------------------  ----------  -----------  -----
{} => {bottled water}       0.1105236    0.1105236      1
{} => {tropical fruit}      0.1049314    0.1049314      1
{} => {root vegetables}     0.1089985    0.1089985      1
{} => {soda}                0.1743772    0.1743772      1
{} => {yogurt}              0.1395018    0.1395018      1
{} => {rolls/buns}          0.1839349    0.1839349      1
{} => {other vegetables}    0.1934926    0.1934926      1
{} => {whole milk}          0.2555160    0.2555160      1

These eight items are purchased on a fairly regular basis, so Paul should recommend that management place these items in easy to find places (e.g., eye level for refrigerated items) and put them towards the front of the store to the extent possible.

### Subset 2: Strong Indicators
To address Paul's second question, we can focus on associations where antecedents imply consequents with a lift greater than or equal to 2, meaning that customers are at least twice as likely to buy the consequent goods with the antecedent goods in their basket, and 50% confidence, meaning that at least half of the customers with the antecedent goods have the consequent goods in their baskets:


Table: Market Basket Rules with Lift >= 2 & Confidence >= 0.5

      Rules                                                       Support   Confidence       Lift
----  -------------------------------------------------------  ----------  -----------  ---------
340   {curd,yogurt} => {whole milk}                             0.0100661    0.5823529   2.279125
346   {butter,other vegetables} => {whole milk}                 0.0114896    0.5736041   2.244885
349   {domestic eggs,other vegetables} => {whole milk}          0.0123030    0.5525114   2.162336
358   {whipped/sour cream,yogurt} => {whole milk}               0.0108795    0.5245098   2.052747
364   {other vegetables,pip fruit} => {whole milk}              0.0135231    0.5175097   2.025351
370   {citrus fruit,root vegetables} => {other vegetables}      0.0103711    0.5862069   3.029608
385   {root vegetables,tropical fruit} => {other vegetables}    0.0123030    0.5845411   3.020999
388   {root vegetables,tropical fruit} => {whole milk}          0.0119980    0.5700483   2.230969
394   {tropical fruit,yogurt} => {whole milk}                   0.0151500    0.5173611   2.024770
403   {root vegetables,yogurt} => {other vegetables}            0.0129131    0.5000000   2.584078
406   {root vegetables,yogurt} => {whole milk}                  0.0145399    0.5629921   2.203354
409   {rolls/buns,root vegetables} => {other vegetables}        0.0122013    0.5020921   2.594890
412   {rolls/buns,root vegetables} => {whole milk}              0.0127097    0.5230126   2.046888
430   {other vegetables,yogurt} => {whole milk}                 0.0222674    0.5128806   2.007235

The primary insight from this information is that a number of different items are good indicators that a customer will buy whole milk, though most of the antecedents are either dairy or produce products.  This is likely because of the high percentage of baskets that included whole milk (25%).  Regardless, these indicators may still warrant a suggestion to put the dairy products near produce.  

### Subset 3: Stronger Indicators with More Frequent Purchases
However, some of these rules do not have very high support levels.  We can view a different segment that has at least 3% support, meaning that the antecedent goods are in at least 3% of the baskets, lowering lift to greater than or equal to 1.5 and confidence to greater than or equal to 0.3:


Table: Market Basket Rules with Support >= 0.03, Confidence >= 0.3, & Lift >= 1.5

      Rules                                        Support   Confidence       Lift
----  ----------------------------------------  ----------  -----------  ---------
197   {whipped/sour cream} => {whole milk}       0.0322318    0.4496454   1.759754
216   {pip fruit} => {whole milk}                0.0300966    0.3978495   1.557043
278   {sausage} => {rolls/buns}                  0.0306050    0.3257576   1.771048
306   {tropical fruit} => {other vegetables}     0.0358922    0.3420543   1.767790
308   {tropical fruit} => {whole milk}           0.0422979    0.4031008   1.577595
316   {root vegetables} => {other vegetables}    0.0473818    0.4347015   2.246605
318   {root vegetables} => {whole milk}          0.0489070    0.4486940   1.756031
330   {yogurt} => {other vegetables}             0.0434164    0.3112245   1.608457
332   {yogurt} => {whole milk}                   0.0560244    0.4016035   1.571735
338   {other vegetables} => {whole milk}         0.0748348    0.3867578   1.513634

Whole milk is still a key item, further supporting the insights from the first subset of rules.  In addition, some new rules have surfaced given the revised parameters, prompting the following potential recommendations:

* Put rolls/buns near saugages in refrigerated section to increase sales of these breakfast combos (related to rule #278)
* Put tropical fruit in a prime location to encourage purchasing of additional vegetables (related to rule # 306)
* Put root vegetables in a prime location to encourage purchasing of additional vegetables (related to rule # 316)


### Subset 4: Highest Lift
One final subset might be to look at relationships with the largest lift values, indicating antecedents that are most likely to increase the odds of a consequent being in the same basket:


Table: Market Basket Rules with 10 Highest Lift Values

      Rules                                                       Support   Confidence       Lift
----  -------------------------------------------------------  ----------  -----------  ---------
342   {whole milk,yogurt} => {curd}                             0.0100661    0.1796733   3.372304
371   {citrus fruit,other vegetables} => {root vegetables}      0.0103711    0.3591549   3.295046
357   {other vegetables,yogurt} => {whipped/sour cream}         0.0101678    0.2341920   3.267062
386   {other vegetables,tropical fruit} => {root vegetables}    0.0123030    0.3427762   3.144780
60    {root vegetables} => {beef}                               0.0173869    0.1595149   3.040367
59    {beef} => {root vegetables}                               0.0173869    0.3313953   3.040367
370   {citrus fruit,root vegetables} => {other vegetables}      0.0103711    0.5862069   3.029608
385   {root vegetables,tropical fruit} => {other vegetables}    0.0123030    0.5845411   3.020999
417   {other vegetables,whole milk} => {root vegetables}        0.0231825    0.3097826   2.842082
348   {other vegetables,whole milk} => {butter}                 0.0114896    0.1535326   2.770630

This segment of rules with higher lift totals suggest a couple of additional ideas for store layout:

* Ensure curd is near the whole milk and yogurt (related to rule #342)
* Consider positioning some sour cream near vegetables and some near yogurt (related to rule #357)
* Promote 'meat and potatoes' (beef and root vegetable) pairings (related to rule #60 and rule #59)


### Conclusions
Overall, the discovered item sets make sense given the skew from items that had strong representation across all market baskets (e.g., whole milk, other vegetables). Additional market basket data could help to improve confidence in some of the rules, if they hold true. As Paul continues to collect market basket data, he should revise his recommendations on product placement. If nothing else, this analysis supports ensuring that the inventory of whole milk is stocked regularly so that customers don't have to ask, 'Got Milk?'.  


</br>
