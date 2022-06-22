## Overview

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;For project 3, I utilized webscraping, APIs, Natural Language Processing (NLP), and classifcation modelling to predict whether a specific Reddit post comes from one of two subreddits. The two subreddits chosen for the project this time were r/ProRevenge and r/MaliciousCompliance; two similar text-based story subreddits detailing user's real life experiences. To gather data, user /u/stuck_in_the_matrix's pushshift.io Reddit API was utilized. 


## Data Dictionary

Three types of data were utilized to create my classification model. They are listed below with a short description of each.


|Feature|Type|Dataset|Description|
|---|---|---|---|
|subtext|object|MaliciousCompliance/ProRevenge subreddits|The text underneath the title of the post; the story the user concocts| 
|title|object|MaliciousCompliance/ProRevenge subreddits|The title of the subreddit post| 
|subreddit|object|MaliciousCompliance/ProRevenge subreddits|The subreddit| 


## General Procedure
### Data Acquisition
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;As mentioned in the overview, the posts and relevant data were taken directly from their respective subreddits using user /u/stuck_in_the_matrix's pushshift.io Reddit API. The data was scraped by chronological order and after storing the data, only the three relevant columns: subtext, title and subreddit, were kept. At this point, around 20,000 observations were collected.

### Data Cleaning
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Upon initial count vectorization of the data (turning the data into singe word tokens then counting the frequency), it was revealed that an alarmingly large number of the data observations contained the word 'removed'. Exploring the data revealed that pushshift.io collected not only actualy posts, but those that were also removed by the moderators due to not fitting in the subreddit. For reasons such as spam, advertising, or just simply not within subreddit guidelines. The data was then stripped of any texts with the 'removed' tagline resulting in a final observation count of around 12,000. Null values were not present within the chosen data columns.

### Modelling
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Various modelling techiques and pipelines were used to train and adjust our maching learning endeavor. As mentioned in the Data Cleaning section above, the data was count vectorized and tested on two vectorizers, CountVectorizer and TfidVectorizer (Term frequency-inverse document Vectorizer). What we were trying to predict, the subreddit that the posts came from, was dummy encoded to 1's and 0's. 1 for r/ProRevenge and 0 for r/MaliciousCompliance. From there, the data was sent through piplines of various classification models (logistic regression and randomforest) and gridsearched to look for the parameters with the best outcomes. For scoring, it was decided to utilize the f1 score since that takes into account both recall(sensitivity) and precision.


## Conclusion

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Utilizing the CountVectorizer with unigrams and bigrams along with RandomForest on *only* the selft proved to be the best model I was able to explore. HOWEVER, due to machine restrictions, downstream visualization procedures were heavily impacted by the sheer volume of vectors created by this model making it unfeasible to visualize. As such, the next best model, RandomForest with Countvectorizer on the text data was utilized. It beat out our baseline accuracy of 0.55 with 0.813, the model finalized with a f1 score of 0.846. The feature importance shows that some of the most impactful predictive vectors were single vectors including revenge, want, bully, ok, and okay. Surprisingly the first instance of a bigram appearing is 'malicious compliance', however, it is still within the top 20 important features. It should be noted that the feature importances only tell us of how much of a predictive effect those variables (words) have on our model. They do not tell us the direction. However, when we use this information along with some visualization (found in the data folder), we can see that the majority of 'revenge' appears in ProRevenge titles and 'want' in MaliciousCompliance titles. From this we may be able to infer that 'revenge' in the title may more likely indicate ProRevenge posts and 'want' for Malicious compliance. Other features may also be able to be explained in this way. In conclusion, our RandomForest model is fairly accurate, with an accuracy of 81.3%, in predicting whether or not a post may belong to r/ProRevenge or r/MaliciousCompliance.