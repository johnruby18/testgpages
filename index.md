---
title: Spotify Playlist Data
---

## Project Statement and Motivation

Spotify often creates playlists for the benefit of its users. Users can show interest in and support for a playlist by "following" the playlist, ensuring easy access to the playlist in the User Interface, as well as updates of playlist developments. Determining the sucess of a playlist (based on number of followers) is certainly of interest to Spotify, as they strive to deliver a quality product to thier users, as well as an interesting modeling project.

We noticed in our EDA that playlists tend to fall into two buckets at around the 25th percentile of followers. Therefore, we decided to treat this as a classification problem of determining successful and unsuccesful playlists based on this ad-hoc cutoff.

## Intro and Data Description

All of our data comes from the Spotify API.

We acquired 1,595 official Spotify-generated non-user playlists and extracted 41 features (including some Spotify generated scores) as follows:

* 'id' A unique ID for the playlist in the API
* 'name' The Name of the playlist
* 'followers' The Number of Spotify users who follow the playlist, our response
* 'collab' Whether or not the playlist is collaborative
* 'num_tracks' The length of the playlist
* 'mean_time' The average length of each song 
* 'std_time' The standard deviation of the length of each song
* 'total_explicit' The number of explicit tracks on the playlist
* 'max_popularity' The popularity score of the most popular track on the playlist
* 'mean_popularity' The average popularity score for tracks on the playlist
* 'std_popularity' The standard deviation of popularity scores for tracks on the playlist
* 'mean_artists_count' The average number of artists per track
* 'std_artists_count' The standard deviation number of artists per track
* 'user' Either Spotify or a Spotify affiliate
* 'mean_\[AUDIO_FEATURE\]' The average Spotify Audio Feature score across the playlist tracks, including:
    * acousticness
    * danceability
    * energy
    * instrumentalness
    * liveness
    * loudness
    * mode
    * speechiness
    * tempo
    * valence
* 'std_\[AUDIO_FEATURE\]'The standard deviation of the above Spotify Audio Feature score across the playlist tracks
* 'mean_artistfollowers' Average followers for the artists on the playlist
* 'std_artistfollowers' Standard deviation of followers for the artists on the playlist
* 'mean_artistpopularity' Average popularity score for artists on the playlist
* 'std_artistpopularity' Standard Deviation of popularity score for artists on the playlist
* 'genre1' The most common genre appearing on the playlist
* 'genre2' The second most common genre appearing on the playlist
* 'genre3' The third most common genre appearing on the playlist

Track and Artist scores, such as popularity scores, are determined outside the scope of the playlist.

Certain features, such as'id' and 'name' are of no use to us. 'collab' had so few TRUE observations that we decided to drop it entirely. 

Genre existed across 500 Spotify genres, so we grouped these into a smaller set of genres comprised of 'rock', 'jazz', 'pop', 'rap', 'indie', 'country', 'house', 'tropical', 'christmas', 'soul', 'folk', 'classical.' We chose these genres to be representive of the playlists Spotify has been producing. If 'rock' appeared in either 'genre1,' 'genre2,' or 'genre3,' we set the binary 'rock' feature to TRUE.

We also introduced several genre-based interactions that we figured made sense, including
* 'rocklive' rock x mean_liveness
* 'rockloud' rock x mean_loudness
* 'poptempo' pop x mean_tempo
* 'jazztempo' jazz x mean_tempo
* 'rapvulgar' rap x total_explicit
* 'classicalinstru' classical x mean_instrumentalness
* 'popenergy' pop x mean_energy

After this feature engineering, we have 52 features along with our response variable.

We chose to transform followers to log(followers + 1) and normailze our other features to get them on similar scale. 

## Literature Review / Related Works

We certainly relied on the Spotify Web API documentation (https://developer.spotify.com/web-api/user-guide/), as well as Spotify's proprietary audio feature scores. We were unable to find any information about how Spotify extracts these features.

We also used SKlearn as well as CS109a course materials.

The following websites were also helpful along the way: https://burakhimmetoglu.com/2016/12/01/stacking-models-for-improved-predictions/ for a refresher on the concept of stacking and https://en.wikipedia.org/wiki/Support_vector_machine for Support vector machines.

## Modeling Approach and Project Trajectory

#### API Work

Getting the data from Spotify API went fairly well for our group. One of us had some experience working with APIs before, and the spotipy package was pretty good. The code did take all night to run, since many calls were needed to get information on the playlists, the component tracks, the audio features for the component tracks, and the artist info. We may have not had the most efficient code to make multiple repeated requests to the API, but given the one-time-only nature of getting the data, it was good enough. We're pretty happy with the amount of features we got from the API, given we went beyond playlist level data by extracting track-by-track metrics and audio features.

We've made our .csv file available at https://www.dropbox.com/s/rxfp3ye12r5p0ot/cs109a_final_data_Milestone3.csv?dl=0

#### Modeling Approach

We decided to opt for more of a model selection project than a feature selection project. We certainly didn't ignore feature selection, but given our group of 2, we originally split model types into pairs of (Random Forest, SVM) and (Logistic Regression, KNN). We later threw Discriminant Analysis into the mix. We figured we could both try to make the best models for our assigned types, and then come together and compare results. We also saw an opportunity to implement stacking across our individual models.

#### Baseline Model

To have a score to compare our models against, we fit a single unregularized logistic regression model using the max popularity score of a track to appear on the playlist. This offered an accuracy score of 75.5%, barely above the expected score of 75% that we would get by predicting all 1’s. We also saw that the baseline AUC was quite low, at 58%.

#### L1 Regularized Logistic Regression

We chose L1 regularization over L2 regularization for the benefit of gaining some sense of feature selection as uninformative features have their coefficients driven to zero. After a routine cross validation to select our penalty factor, we find that a strongly penalized model (with C=1)  offers the best CV score of 79.5% and reduced the relevant feature space by half. It seems a lot of audio features (such as loudness) aren’t influential enough to overcome the regularization penalty. Still, taking advantage of regularization and the additional features we got from the API, this first model attempt manages to improve above the baseline model by about 4.5% to a test accuracy of 81%.

The AUC for the regularized model substantially improves to 85%. Given the expected split of 75/25 1/0 in our response variable, this offers an alternative perspective on how much we’ve improved the model.

Looking at broad trends across the coefficients, it seems that (as intuition would support) playlists that include songs and artists that are standalone popular tend to be more successful. 

Many of the features that are standard deviations of metrics have negative coefficients, suggesting that very consistent/homogenous playlists tend to be successful. 

Surprisingly, classical and jazz have moderate positive coefficients, suggesting these perform slightly better holding all else constant. However, it may be that collinearity among genre and popularity scores make this a less-than-actionable finding in practice.

### LDA

LDA performed slightly weaker than our regularized logit model (test accuracy of 80%), and without hyperparameters to play around with or an effective means to visualize the decision boundary in such high dimensional space, our momentum on the model somewhat stalled. We could’ve possibly done better by adding/removing certain features, but as it’s just one model out of five, we opted to move on to Random Forests. 

### Random Forests

We produced a reasonable amount of hyperparameter pairs to cross validate over, utilizing out-of-bag scores to make our decision. We found that building a forest of 800 trees, sampling 80% of our feature space performed the best. We found that the sophisticated model of the RF delivered a marginal improvement over our previous best logit model, increasing our best test accuracy to 83%. 

Looking at feature importance, we found that playlist length and component popularity scores were very influential, while a lot of audio features and genre/audio feature interactions were sort-of tier 2 features, and a lot of genre features didn’t serve as very important. This is interesting relative to the features that L1 regularization eliminated, as the models work in different ways. L1 regularization tended to eliminate the genre/feature interactions before the lone genre features, but the Random Forest favors the opposite. However, the most powerful features were fairly consistent across both. 

### Support Vector Machines

We compared SVMs with linear, poly, and rbf kernels across a variety of hyper parameter values. After routine cross validation, we find that the poly kernel performs best (with C=200). This C value make sense, as we have far from separable data. However, the SVM model performance is somewhat disappointing, with a test accuracy of 78%. 

### K Nearest Neighbor

With a routine cross validation to choose K on the complete feature space, we achieved poor test performance of  74%. This was surprising, given that we could easily achieve better performance by predicting exclusively 1’s.

Thinking about the KNN algorithm, it can be very sensitive to the feature space. Proximity across uninformative dimensions can overpower proximity across influential ones. We already knew from our previous models that about half of our feature space was somewhat uninformative, and for KNN, we couldn’t ignore this. 

We leveraged our feature importance scores from our Random Forest model to reduce to size of our feature space, cross validating across both K and the number of most important features. After the CV, it seemed that using the 9 most important features and a K of 30 would be our best bet for a KNN algorithm. This drastically improved test performance from 74% to 81%, showing that KNN could be useful on this data if strategically used. 

### Stacked Model

To bring our entire project together, we tried to stack our models through a random forest, which achieved a test score of 81%. Though respectably above our baseline, this is worse performance than our best solo model. However, it was interesting to see how the relative importance of our models compared in the stack. They seemed to generally correspond to model performance, but given the information contained in the other models, KNN seems to be less influential than the solo-worse performing Logit and LDA models. Stacking may offer opportunities with a different choice of meta model.


## Results, Conclusions, and Future work

While we put a lot of effort into increasing the sophistication of our models, our best models could not exceed an accuracy of around 0.83. If anything, this seems to show that this is not the sort of data that can ever be modeled perfectly, and that it is not a perfect science. As could have been expected, the popularity of online material depends a lot of times on trends, visibility and accessibility, and other factors that it is very hard for us to capture in this context. 

	However, we have managed to identify, through the analysis of our coefficients with our logistic model, and the feature importance in our random forest model, a few variables that factor into our models more than others, and that give us some insight into what makes a playlist popular. The most important feature in the random forest model is apparently the number of tracks contained by the playlist, which we suspect is just a very efficient way of weeding out the degenerate playlists that have only a few songs on them and are not necessarily intended to be widely followed. In both the logistic regression and the random forest, a lot of the top predictors turn out to be related to the popularity either of the artists or of the track. This result, indeed, was highly predictable and seems to suggest that there is no big secret in making a successful playlist: people like to hear what they know and what they like, and love a playlist that combines a lot of popular songs. 

	We observed that no genre or none of the interaction variables that we created using the genre dummy variables were particularly successful in either model, which suggests that there is no go-to genre that will make a popular playlists. There are popular and unpopular playlists in each genre, and the popularity of the genre can be measured via the number of playlists of that genre more than with the popularity of these said playlists. 

	It is important not to read too much into the different variables we see appearing as significant in the two models, especially when the two models analyze variables so differently. While we decided to focus much more on model selection and performance than on interpretability, we could definitely have gone down a different path. 

First, our decision to view this as a classification problem rather than a continuous variable prediction problem was a crucial choice. We could definitely have imagined a different project in which we left the dependent variable in its continuous form, and dealt with it through linear regression or k-NN. Linear regression results might have been more easily interpretable than these. We could also easily have used polynomial features and more interaction variables, potentially using PCA to deal with the subsequent high dimensionality of our predictors. Such a model might have achieved better results, although the interpretability would again be diminished.

Our decision to use classification was motivated by the histogram of the log-transformed ‘followers’ variable (shown in the EDA) which indicates a visible bimodality in the variable, which we felt could not be captured too well using linear regression alone. A model which we considered was one that combines a classification model and linear regression problem, first determining which of the two modes an observation belongs to, and then using two different linear regression models with different parameters based on that prediction. We found, however, that as things are, our model did not perform well enough on the classification end to achieve satisfying results with such a model.

There are indeed many paths that we could have taken, and were only able to explore a few. But overall, our findings quite lined up with our expectations. It made sense that Random Forest was among our best performing models because of the non-linear, quite complicated relationships that go into something as intangible as playlist popularity. More than exploring other models, we wish we’d had access to more data specifically on the visibility of each playlist, the words that are most often researched on Spotify (in order to cross it with playlist names) demographic information on the playlists’ followers and other features that might have allowed us to dig much deeper into the matter. 

