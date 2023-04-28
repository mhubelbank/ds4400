![head](header.png)

# Predicting Song Popularity with the Spotify API
### Mara Hubelbank — DS4400 Course Project

To view the Jupyter Notebook, please see src.ipynb. If running the source code, uncomment the `!pip install` lines.

## Abstract
This project explores the Spotify API’s [track audio features endpoint](https://developer.spotify.com/documentation/web-api/reference/get-audio-features), which yields song information such as tempo, danceability, and popularity. Pre-scraped data is sourced from a similar project on GitHub by user [@DorAzaria](https://raw.githubusercontent.com/DorAzaria/Spotify-Machine-Learning-Project/main/data.csv), with ~170k samples scraped in January 2022. Exploratory data analysis is conducted over the audio features, visualizing each features’ distribution and time series. Then, approaches within binary classification and multi-class classification are used to attempt to predict song popularity. For this task, we obtain a maximum accuracy of 0.83 and F1 score of 0.71 with a Random Forest binary classification model.

## Introduction
Streaming services such as Spotify and Apple Music have disrupted the music industry, providing an ever-expanding collection of the world’s music available on users’ smartphones for a relatively low cost. One of the most unique and compelling features of such platforms is personalized recommendation algorithms, which provide users with cultivated music suggestions based on the songs, albums, and artists they engage with. Applying the free, easy-to-use [Spotify API](https://developer.spotify.com/), many data scientists have created their own machine learning-powered music recommendation platforms.

According to Spotify, its database contains over 100 million songs -- how does it select from such an expansive pool which songs to suggest to users? In exploring Spotify’s developer documentation, we found that one metric Spotify uses to organize and suggest tracks is [popularity](https://developer.spotify.com/documentation/web-api/reference/get-an-artists-top-tracks#:~:text=The%20value%20will%20be%20between,how%20recent%20those%20plays%20are.), which budding musicians can [leverage](https://www.loudlab.org/blog/spotify-popularity-leverage-algorithm/) to gain visibility for their streaming career. Having the knowledge of what makes a song popular, and consequently being able to predict the likelihood of a song’s popularity, is crucial for Spotify to remain as competitive of a player in the music industry as it has been for [ten years](https://www.fastcompany.com/90205527/the-definitive-timeline-of-spotifys-critic-defying-journey-to-rule-music). 

Spotify’s popularity feature is a continuous value in the range of [0-100]; we could use regression, binary classification, or multi-classification to predict this target. This dataset is well-structured, has 15 features, and ~170k samples. Our target variable has an IQR of 36, and is very imbalanced due to 16% of songs having a popularity score of 0%. This makes sense, because Spotify allows anyone to release music, and thus the vast majority of songs will be put out by very small creators. Considering our dataset as well as time and resource constraints, we elect to use a shallow learning approach. 

We found several similar projects online, which supports our selection of popularity as an interesting target variable: [GitHub](https://github.com/DorAzaria/Spotify-Machine-Learning-Project), [Kaggle](https://www.kaggle.com/code/amansorout/spotify-song-popularity-classification), [Medium](https://gabbyamparo.medium.com/using-classification-models-to-predict-song-popularity-ace733c12e48). All of these sources tackle the problem from a binary classification perspective. We try this first, but also attempt to use regression and multi-classification in order to better represent the continuous popularity scale used by Spotify’s algorithm. In our modeling experimentation, we find it especially difficult to predict when a song will be extremely popular (defined as having a popularity score >80%), as this comprises just 0.3% of the dataset. The binary approaches linked previously don’t account whatsoever for the upper echelon of popular songs, and we propose that future extensions of this project explore that subset of songs in particular to determine what features extremely popular tracks share in common.

## Setup

Our dataset has 169,909 rows and 18 columns. There are no null values in any column, thus it is a complete, dense dataset; there are also no duplicate songs. We first conduct exploratory data analysis. This is the largest section of the project, because we didn’t get a chance to do too much of this throughout the course, and it happens to be our favorite aspect of data science. We visualize the distribution and time series of each audio feature. We conduct feature engineering; the total list of approaches we tried are: target variable binarization, cost-sensitivity, one-hot encoding, scaling, and class balancing. For modeling, we first try regression, but abandon this approach as we cannot get performance above an accuracy score of 25%. We instead use binary classification first, and then attempt multi-classification. We use the models of logistic regression, decision tree (native and with AdaBoost), k-nearest neighbors, and random forest. In multi-classification, we conduct grid search over each model to determine the best hyperparameters. We attempted to use SVC, but ran out of time to experiment with this model, because we ran the code on a 6-core CPU on a Dell G5 laptop.

## Results and Discussion

In exploratory data analysis, we find that year has a strong linear correlation with popularity, because release date and recent plays are components of Spotify’s popularity algorithm. Thus, we rule out year as a feature to use, since this would confound our experimental results. Interestingly, the authors of the similar project posted to GitHub mention this, claim that they will remove “year” from the feature set, but don’t actually remove the feature -- they, of course, get excellent performance (acc > 0.9); this is one example of how data science reports can be intentionally or unintentionally misleading.

We also find in EDA that song acousticness and instrumentalness decreased sharply after 1950; around the same time, song tempo, energy, loudness, and danceability sharply increased. As mentioned previously, we select popularity as our target variable, and find that (excluding the confounded year variable), energy, loudness, and danceability correlate most strongly with a song’s popularity. These features are consistent with our hypotheses, as these features are prevalent in pop music.

We set a binary threshold of 0.5 for popularity: songs with >50% popularity are “popular”, and otherwise “unpopular”. As expected, the binary models perform much better than the multi-classification models. In both cases, the Random Forest classifier obtains the best performance, with an accuracy of 0.81 and 0.56 respectively. Accousticness consistently ranks among the most important features with a strong negative effect on predicted popularity. This is consistent with our basic concept of pop music, which rarely features strong acoustic elements.

## Limitations
In this project, much more time was devoted to EDA than to modeling; this is because we wanted to supplement what we’d learned about data science in DS4400, because we most enjoy the visualization task, and because we didn’t allot enough time to modeling and tuning. Although this report uses “we” for formality, there’s only one author; working with a team would have allowed for a more balanced distribution of efforts amongst the project components. Lastly, having more data (especially for the extremely popular class) and more computing power would benefit this experiment greatly.

## Conclusion and Next Steps
In conclusion, we find that audio features can be used to reliably estimate the popularity scores produced by the Spotify algorithm. We believe that the models’ performance could be even further improved with additional time, data, and computing power. It isn’t included in the report/code for the sake of brevity, but I spent a lot of time experimenting with the Spotify API, and discovered many new inquiry features to experiment with in regard to Spotify tracks, albums, artists, and playlists. This has been a very enriching project, and I plan to continue building on it this summer through refining the current models, as well as exploring and predicting release decade and genre (there are 30k genres with no predefined binning mechanism, so it wasn’t a reasonable target variable for this project). For example, I am curious about "viral" songs, which are popular tracks released by typically unpopular artists -- how often have they happened over time, and what do they have in common? How does virality relate to the rise of social media platforms?

## References
[GitHub](https://github.com/DorAzaria/Spotify-Machine-Learning-Project), [Kaggle](https://www.kaggle.com/code/amansorout/spotify-song-popularity-classification), [Medium](https://gabbyamparo.medium.com/using-classification-models-to-predict-song-popularity-ace733c12e48)
