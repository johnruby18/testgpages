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

Getting the data from went fairly well for our group. One of us had some experience working with APIs before, and the spotipy package was pretty good. The code did take all night to run, since many calls were needed to get information on the playlists, the component tracks, the audio features for the component tracks, and the artist info. We may have not had the most efficient code to make multiple repeated requests to the API, but given the one-time-only nature of getting the data, it was good enough. We're pretty happy with the amount of features we got from the API, given we went beyond playlist level data by extracting track-by-track metrics and audio features.

#### Modeling Approach
holder

## Results, Conclusions, and Future work

holder
