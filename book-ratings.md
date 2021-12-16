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

![YrRatingDists](https://user-images.githubusercontent.com/92626980/146294348-43398b84-c613-4ad2-b486-d6027ccc68a4.png)

