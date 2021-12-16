## Investigating Factors Influencing the Quality of Books

This analysis attempts to define a few rule-of-thumb measures to see if they can help to assess the quality of a book. 

Factors considered are how far the author is into their career (by years and number of books published), the price and length of the book, the genre, and whether it has won an award. There is then a quick look at the ratings of books compared to their adapted movies.

Summary of contents:

   1. Introduction
   2. Summary
   3. Data Inspection and Cleaning
   4. Analysis
   5. Conclusions and Discussion
   6. Future Analysis

### 1. Introduction

So many books, so little time. Can publicly available data provide new insight on how to pick a winner? Websites such as Amazon and Goodreads provide plenty of reviews from the general public, along with other details such as when the book was published, awards it has received, cost etc. The motivation for this analysis was to see if this data could be used to develop some rules-of-thumb to guide book selection. On a personal level, there are a number of authors whose work I think I would like, so it would be useful to have a method to help pinpoint books that are better examples of an authors work. Finally, some people prefer movies to books, which leads to the question of whether a highly rated book will make for a highly rated movie.

### 2. Summary

It might be expected that the more time someone dedicates to something the better they get at it - so authors would write increasingly better books, with their best work coming towards the end of their career. There is something that can be thought of as "The Simpsons Effect" - the best seasons of The Simpsons are generally regarded as being around Seasons 3 - 7, with a slow decrease in quality after. Similarly, some authors tend to produce their best work towards the beginning of their career.

There is no clear relationship between ratings, price and length, or between ratings and different genres. The number of awards a book received explained none of the change in ratings (Rsq=0, p=0.301), however, there are significant differences when considering the type of award. Notably books that received The Man Booker prize on average have a rating 0.19 *less* than books that received no awards (p<2e-16), the Pulitzer Prize is not significantly different from no award (p=0.06) and the best performer, The Goodreads Choice Awards, are rated only 0.06 higher than no award (p<2e-16). There is a difference in the distributions for both genres and awards, pointing to a risk/reward payoff that might be useful in some situations.

There was also a significant relationship between good books and good movie adaptions, with around 15% of the variation in movie ratings being explained by the book ratings (Rsq=0.155, p=1.29e-11).

### 3. Data Inspection and Cleaning

#### Data Files

There are four datafiles used, three relating to books and one to movies.

__full.csv__ Source: https://zenodo.org/record/4265096
This dataset was sourced from the GoodReads Best Books Ever list. It has information about ratings, genres, awards, price, length and the format (paperback, hardcover etc.). It has 41,756 entries.


__books.csv__ Source: https://www.kaggle.com/davidpitts2bds/30k-books-tagged-if-made-to-movie-or-not-and-more
This data was also collected from Goodreads. It contains fields for ratings, length and whether it was made into a movie. The creator notes that there are some false negatives in the "made into movie" column. It has 29,630 entries.

__booksummaries.txt__ Source: https://www.cs.cmu.edu/~dbamman/booksummaries.html
This is the CMU Book Summary Dataset, extracted from Wikipedia along with aligned metadata from Freebase. The publication dates in the other two sets was unusable (the first only has two digits for year and the second has dates for reprints rather than the original publication date), so this set is used for the publication dates. It has 10,949 entries with the dates.

__movies.csv__ Source: https://www.kaggle.com/rounakbanik/the-movies-dataset
The data is from the Full MovieLens Dataset. Only the movie titles and ratings were needed, so only the movies_metadata.csv file was used. It has 2,049 entries.

The datasets are all fairly clean, however, only one of the book datasets have the ISBN, so they need to be joined on author and title. Both author and title have been stored differently in each set so those fields were homogenised before joining. 
Collections have been included as a single row which creates a lot of noise. These were filtered out using keywords, although some still were unable to be removed. 
The false negatives in the "made into movie" column were easily removed.
Some dates are yyyy-mm-dd and some are yyyy. Only the years were used, so they were converted to yyyy.
Ratings from both data sets were combined, weighted by number of ratings. The number of pages should be the same, so where there were two values the Kaggle pages data was chosen. Books that had less than 100 total votes were removed.
The movie dataset has some duplicate rows and duplicate title entries, some of which are due to new release dates and some that have low vote counts. The duplicates and those with a low vote count are removed.

### 4. Analysis

#### Q1)a) How does an author's book ratings change over their career? 

To get the best example of someone's work, should you go towards the end, middle or beginning of their career? To answer this, authors who have written a few books over time are needed, let's say at least 5 books and over at least 10 years. For consistency only fiction books are included.

![YrRatingDists](https://user-images.githubusercontent.com/92626980/146294581-0428eeef-b132-47ea-977f-af75aefc9215.png)


![yrsFristPub](https://user-images.githubusercontent.com/92626980/146300552-01f64c9a-dd30-4ae2-9001-d4b54b5aaed6.png)

The mean line is fairly flat, beginning a decline at 30 years. There is an increase from 40 years onwards, but also fewer data points as time goes on so the data becomes less reliable. 

#### Q1)b) Which book is the highest rated for each author? 1st? 3rd? 7th?

![BestPubAll](https://user-images.githubusercontent.com/92626980/146300717-bc54d752-69ba-4556-a716-4b86718bf184.png)

There is a clear peak at book number 5. Below is the same graph but broken down by how many books an author has published.

![BestPubStatic](https://user-images.githubusercontent.com/92626980/146300771-e3045063-a3b6-457d-b6b4-b72670fb722c.png)

For many authors, their highest rated book was published towards the start of their career. For those who published 10 or less books, the third and fifth seem to be the best. The distribution is flatter for over 10 books published, but even the over 20 group still have some of their highest ratings early on. Note that there are only 4 authors who have more than 20 books published in this dataset.

#### Q2) Is price more influenced by the rating or the length of a book? Will paying more mean a better read?

The cover type of a book can have a big influence on price, so only paperback books are considered. The distributions for price and length are both heavily right-skewed, making it harder to see the patterns. For this reason, the dataset was restricted to books with less than 1000 pages and price under $50.

![CostLengthAll](https://user-images.githubusercontent.com/92626980/146300893-9ba17d24-4a78-4b36-8966-daf60975cc03.png)

It is hard to see what is going on here, so the data is divided into "cheap" books (under $15) and more expensive ones.

![CostLengthSplit](https://user-images.githubusercontent.com/92626980/146300916-fdbd8a11-7487-4083-96c8-30f1ac7e8f65.png)

The scatter plots indicate there's no clear effect here and a quick look at correlations below supports that. Cheaper books do seem to have slightly lower ratings, but longer books don't look to be more expensive. Longer books also seem to rate higher which could be noise from collections that were missed by the filter, since a collection is more likely to have a good rating. 

<html>
   <table>
      <tr><th></th><th>Rating</th><th>Price</th><th>Pages</th></tr>
      <tr><td>Rating</td><td>1.000000</td><td>0.144354</td><td>0.115402</td></tr>  
      <tr><td>Price</td><td>0.144354</td><td>1.000000</td><td>-0.013663</td></tr>
      <tr><td>Pages</td><td>0.115402</td><td>-0.013663</td><td>1.000000</td></tr>
   </table>
</html>

#### Q3) Are some genres more likely to have higher rated books?

![genreFreq](https://user-images.githubusercontent.com/92626980/146301513-2e907143-ca6f-4c6d-8207-21270f97dbd6.png)

![genreDist](https://user-images.githubusercontent.com/92626980/146301515-bbba5905-147d-4f13-8c8a-b65c287f7e89.png)

The medians are fairly consistent, but there some variation in the distributions - historical and adventure are more tightly grouped where fantasy and science fiction have a slightly flatter distribution and longer tails.

These observations aren't independent. Most books have more than one genre and there is considerable overlap between groups, so no further analysis is done here.

#### Q4) Do awards indicate better books?

First the ratings of the book are plotted against the number of awards to see if there is a correlation. Then a comparision is made between books that have won specific awards. Awards selected are based on those featured at https://www.goodreads.com/award and https://gobookmart.com/10-most-prestigious-award-for-authors-and-writers-in-the-world/. Awards with less than 20 books were excluded.

![awardRatings](https://user-images.githubusercontent.com/92626980/146301603-0fc520e3-15ce-48e8-a7e7-734113b86d0d.png)

<img width="317" alt="awardsOLS" src="https://user-images.githubusercontent.com/92626980/146301771-7bd9661b-7f35-412b-97e7-0ac1d84c72bc.png">

The number of awards has no relationship to the ratings (Rsq=0, p=0.301)

![awardsBox](https://user-images.githubusercontent.com/92626980/146301827-a8487ca0-cf85-40a8-8a64-6e721ca9797b.png)

To compare the prizes to books with no awards, a Welch ANOVA is done followed by Dunnett's Test. The Miles Franklin and Costa awards have too few samples so are excluded.
Tests for normality and equal variances below.

<img width="415" alt="awardsSum" src="https://user-images.githubusercontent.com/92626980/146301972-213bfc9e-b649-4b80-b1ba-0a542d96c548.png">

Man Booker Prize:  NormaltestResult(statistic=5.107901052365614, pvalue=0.0777738108305568) </br>
Pulitzer Prize:  NormaltestResult(statistic=2.1736454444546185, pvalue=0.33728644576634276) </br>
The Goodreads Choice Award:  NormaltestResult(statistic=56.87954880748067, pvalue=4.4541302205934914e-13) *** </br>
The Edgar Award:  NormaltestResult(statistic=2.359730014850332, pvalue=0.3073202217491422) </br>
RITA Award:  NormaltestResult(statistic=0.37876843912625086, pvalue=0.8274685160178017) </br>
No awards:  NormaltestResult(statistic=1117.052423000484, pvalue=2.7236314923093435e-243) *** </br>

![normDist](https://user-images.githubusercontent.com/92626980/146302035-19f33e8d-796d-4bc7-96a2-c0a98a2bd9a6.png)

Test for equal variances:  LeveneResult(statistic=22.10988349601877, pvalue=3.456598775568692e-22) *** </br>
Ratio of largest to smallest variance:  2.058440007988861

ANOVA also requires independent samples. One book can have many awards which could violate the independence assumption. To check the similarity, this is a lower matrix showing how many times different awards are given to the same book. Naturally, books with no award never have an award. Otherwise there is minimal overlap, mainly with The Goodreads Choice Award. As the comparison is between made between books with no awards and the awards individually, it shouldn't be a problem.

<img width="350" alt="dissimMatrix" src="https://user-images.githubusercontent.com/92626980/146302174-6e9c28a2-142c-4113-adc0-70f5c7cf32cc.png">

statistic = 52.98084321664306 </br>
pvalue = 2.5180982652975606e-46 </br>
df = (5.0, 667.7997813052062) </br>
df_num = 5.0 </br>
df_denom = 667.7997813052062 </br>
nobs_t = 29660.0 </br>
n_groups = 6 </br>
means = array([3.79096136, 4.03115871, 4.04848271, 3.9242717 , 4.03637166,
           3.98454849]) </br>
nobs = array([  214.,   228.,  1783.,   204.,   295., 26936.]) </br>
vars_ = array([0.070358  , 0.04584779, 0.06553732, 0.04602772, 0.03991293,
           0.08215836]) </br>
use_var = 'unequal' </br>
welch_correction = True </br>
tuple = (52.98084321664306, 2.5180982652975606e-46) </br>

#### R output

<img width="472" alt="dunnetsOutput" src="https://user-images.githubusercontent.com/92626980/146303990-cf0ec9cf-a8f0-4e8d-9ff7-587638fc091b.png">

![dunnetts](https://user-images.githubusercontent.com/92626980/146302262-eb7ac6b1-ba32-4ba2-8b87-8861b9ab7e75.png)

The Goodreads Choice Award (p<2e-16) and the RITA Award (by Romance Writers of America, p=0.0088) rate significantly higher than no award. The Pulitzer Prize shows no significant difference (p=0.0647). The Man Booker Prize (p<2e-16) and the Edgar Award (p=0.0122) rate significantly worse than books that haven't received any awards. The differences in means are very small (-0.19 to +0.06), suggesting that there is not much practical information about the rating of a book given by its award status.

#### Q5) Will a good book make a good movie? Is there a correlation between book ratings and ratings of the movie adapted from the book?

There are some titles with more than one movie rating. As seen before, this is likely due to more than one film being made from a book, so this data should be kept. An average of the film ratings can be taken.

![bookMovie](https://user-images.githubusercontent.com/92626980/146302333-71e04d9a-ae25-4c4c-979c-ae2916f70fce.png)

<img width="307" alt="movieReg" src="https://user-images.githubusercontent.com/92626980/146302416-323a7adc-5a55-4b36-9c1b-fbe8fa5146a3.png">

The model indicates a significant effect (p=1.29e-11) which accounts for a small part of the movie ratings (Rsq=0.155).

### 5. Conclusions and Discussion

There were a couple of clear and useful results here. Firstly, that many authors produce their best work towards the beginning of their career. This pattern can be seen in other artistic mediums - TV shows, music, even mathematics. There were some authors that produced their best works later, so this isn't a hard and fast rule. Also this analysis does not make an attempt to understand why some peak early and some later. It does fit the requirement of being a simple, easy to implement guide.

Second, awards seem to have no objective bearing on ratings - the difference in means between all award groups was less than 0.25 and there was no effect of the number of awards on the ratings (Rsq=0, p=0.301). What was noticible was the difference in distributions. Remembering that these results are a collection of reviews by the general population, it is interesting that books with awards tended to have fewer very bad reviews, but also fewer very good reviews. This points to a risk/reward situation. If a book was, for example, intended as a present, it would be advisable to get one with an award as there is less chance of getting a bad book. On the other hand, if the intention was to get an exceptionally good book, it would be better to take a risk and get one that hasn't won any awards.

There was a relationship between good books and better rated adpated movies, but it only explained a small amount of the variance (p=1.29e-11, Rsq=0.155). On its own it might not be useful in selecting a good film, but it could be useful when considered along with other factors.

The length of time someone has been writing for is not a good indicator of rating in this data set. With more information, particularly about those who have been writing for over 20 years, it may be possible to get a more certain answer. Different genres were also fairly consistent. Again, there is considerable overlap between them making it hard to tease apart an effect. Genre is also very subjective - people have their favourites.

Ultimately there are two key things this analysis points to - 1) An author's 3rd - 5th book has a good chance of being one of their best and 2) an award, no matter how prestigious, doesn't mean a better book.

### 6. Further Research

The data used to look at the best rated books by publication order is relatively small (~1,000 entries after cleaning and joining). It should be possible to get a much bigger data set by scraping the Goodreads or Amazon sites directly. This would give more clarity around the trends.

These trends could also be compared to other mediums - TV, movies, art, games - to investigate the patterns of peak quality. It is interesting that a similar trend is seen with TV shows and books, as TV shows are generally a collaborative effort with a writing team where books are generally written by one person. Being able to understand why quality declines in some scenarios could lead to methods that allow people to avoid the decline and continue to produce higher quality works.

NB: This project was initially a programming assessment and as such a literature review was not undertaken and no references are provided.
