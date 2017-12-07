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

...* acousticness
...* danceability
...* energy
...* instrumentalness
...* liveness
...* loudness
...* mode
...* speechiness
...* tempo
...* valence

* 'std_\[AUDIO_FEATURE\]'The standard deviation of the above Spotify Audio Feature score across the playlist tracks
* 'mean_artistfollowers' Average followers for the artists on the playlist
* 'std_artistfollowers' Standard deviation of followers for the artists on the playlist
* 'mean_artistpopularity' Average popularity score for artists on the playlist
* 'std_artistpopularity' Standard Deviation of popularity score for artists on the playlist
* 'genre1' The most common genre appearing on the playlist
* 'genre2' The second most common genre appearing on the playlist
* 'genre3' The third most common genre appearing on the playlist

## Literature Review / Related Works

holder

## Modeling Approach and Project Trajectory

holder

## Results, Conclusions, and Future work

holder

This is the home page

## Lets have fun!!!

>here is a quote

Here is *emph* and **bold**.

Here is some inline math $\alpha = \frac{\beta}{\gamma}$ and, of-course, E rules:

$$ G_{\mu\nu} + \Lambda g_{\mu\nu}  = 8 \pi T_{\mu\nu} . $$
