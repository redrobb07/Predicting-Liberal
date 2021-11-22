# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 3: Web APIs & NLP


### Problem Statement

In the current politically polarized state of our nation, it is getting .. As more and more people move to social media and other forms of online communication, (something here about how the ways in which we communicate online is driving deeper political divides. This is essentially your first report..) it is getting more difficult to 

In this project I set out to determine if it is possible to determine whether a reddit poster was a Liberal or Conservative from the language used in the title of the post or in what is called the selftext. 

### Data Description

I located two different subreddits: one that was titled "Liberal" and another that was titled "Conservative".  Using [Pushshift's](https://github.com/pushshift/api) API, I then scrapped 10,000 posts from each subreddit, totaling 20,000 combined.  

My first exploration was to check the columns for post titles and selftext. First, looking at the self text columns I discovered that there were many empty selftexts. I went and checked other subreddits and it seems that most of them have posts with selftext. For me, that indicated that the Liberal and Conservative were simply subreddits where people use only titles for the most part. In order to overcome this deficit in the data, I engineered a feature that was based on the intersection of the title and selftext columns. 

I then wanted to find out what words were used across all of the subreddit posts to see if there was information I could glean from that. It was then that I discovered the most interesting thing about these two subreddits. Of the posts that had selftext, a majority of them were either removed (probably by moderators) or deleted (probably by the user). This suggests that there were offensive or otherwise inappropriate language in those selftexts. The other most common words seemed appropriate for the current political times. Biden was the next most, followed by Trump. Other prominent words in the top 10 were Rittenhouse (the young man on trial in Wisconsin) and Covid.

This was the end of my initial exploration. In the next section I will explain the process I used to build a model.


### Process and findings

I started the process of building a model by testing a count vectorizer and then using a logistic regression model to test for accuracy. I also attempted to tune the parameters of the count vectorizer, but discovered that I got my best score with the default settings. This combination produced a cross-val score of 0.79, successfully predicting 4,002 posts out of 5000 in the test sample.  The confusion matrix showed that there were 461 type 1 errors, predicting Liberal but the actual post was Conservative, and 537 type 2 errors, predicting Conservative when the post was Liberal. This gave the count vectorizer and logistic regression a precision of 0.78 and a sensitivity, or recall, of 0.81.

Next, I tried a Term Frequency-Inverse Document Frequency vectorizer (TF-IDF) along with the logistic regression model. The TF-IDF performed worse than the count vectorizer on this dataset. The cross val score was 0.75 and the amount of both type 1 and type 2 errors were higher with the TF-IDF.

After determining that the count vectorizer was the better vectorizer to use for this corpus of subreddit posts, I then wanted to figure out if logistic regression was also the best model to use. 

In order to determine this I set up a voting classifier with 4 different models: 1) an ADA boost classifier, 2) A gradient booster, 3) a random forrest classifier, and 4) a KNN model. The voting regressor returned that the logistic regressor was the best model for this project. The cross val score for each model was as follows: the KNN score was 0.64, the adabooster was 0.68, the boosted trees was 0.72, and the final logistic with fine tuned parameters increased the accuracy against the test data slightly but still remained at 0.79. 

For my last piece of analysis I wanted to look at the ROC AUC curve to test how separated the distributions are. The final model with the count vectorizer and the logistic regression model had a ROC AUC curve of .88. This score means that the positive (Liberal) and negative (Conservative) classes were highly separated by the model, as the closer to 1.0 the score is, the more the classes are evenly separated. 


### Data Explanation and future research

The model was able to show that it could do a better job at predicting whether a particular post was from the Liberal subreddit or the Conservative subreddit than the null model, which was an even 50-50 split. This suggests that there are key words or phrases that Liberal redditors use more than Conservative redditors. For future study, I would like to test this same trained model on other political subreddits to see whether the model maintains it’s accuracy. 