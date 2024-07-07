# Movie Pairing Recommender System -- Naël El Janati El Idrissi

## Objective

The objective of this project is to develop a recommender system that pairs two movies based on complementary genres, themes, or viewer preferences. The system is designed to recommend movies for a couple of users, using a hybrid approach that combines collaborative filtering and content-based filtering.

## Methodology

### Preprocessing

1. **Datasets**: 
   - **MovieLens Dataset**: Contains user ratings, user information, and movie informations.
   - **IMDb Dataset**: Provides detailed information about movies, including genres and cast.

2. **Data Merging**:
   - Extract the year from the movie titles in the MovieLens dataset.
   - Merge the MovieLens and IMDb datasets on movie titles and years.

3. **Feature Engineering**:
   - Explode the IMDb 'knownForTitles' column to map movie IDs to primary names (cast members).
   - Create a new column 'Cast' in the merged movie dataset.

### Collaborative Filtering

We will use FunkSVD for matrix factorization to perform collaborative filtering. FunkSVD is variant of SVD (single value decomposition) for sparse matrix.
The idea is to minimize the sum of the squared errors between the predicted user-item ratings and the real ratings in the training set in order to reconstruct the ranking matrix.

I chose this technique because it addresses the popularity bias and cold start issues. This allows for better generalization and improved prediction accuracy, especially for less popular items and new users or items with limited interactions.

Additionally, the FunkSVD model can be updated easily. We can implement new data to the model without a complete training of the latter. We use regularization, so that FunkSVD helps to prevent overfitting, ensuring that the model performs well on unknown data.

1. **Matrix Preparation**:
   - Pivot the ratings data to create a user-item interaction matrix.
   - Fill missing values with the mean rating for each movie.

2. **Model Training**:
   - Set the hyperparameters values: latent dimension, learning rate, and regularization rate.
   - Train the FunkSVD model on the training set.

3. **Model Evaluation**:
   - Split the data into training and testing sets.
   - Evaluate the model using Mean Squared Error (MSE) and Root Mean Squared Error (RMSE) on the test set.

### Hybrid Approach

For further improved performance, a selection of features such as the cast, the year and the genre can be used. We define a couple score which will be used to suggest a selection of movies to watch.

1. **Combine User Preferences**:
   - Combine the latent features of two users to represent their combined preferences.

2. **Predict Ratings**:
   - Use the combined latent features to predict ratings for each movie.

3. **Content-Based Filtering**:
   - Calculate genre similarity and normalize the year of the movies.
   - Compute a couple score based on predicted ratings, genre similarity, and year score.

4. **Recommendation**:
   - Recommend the top 10 movies with the highest couple score.

## Experiments

1. **Hyperparameter Tuning**:
   - Tested various latent dimensions, learning rates, and regularization parameters to find the optimal values. This part can be enhanced with a grid search.

2. **Model Evaluation**:
   - Evaluated the model on the test set and computed MSE and RMSE to measure the performance.

## Results

- The final model achieved an MSE of 0.401 on the training set. This is a really good result, with more training data we can expect the model to be one of the most performant.
- The final model achieved an MSE of 1.5723 and an RMSE of 1.2539 on the test set. Here we can see that on new data the model doesn't perform as expected. However with a rating difference of almost 1, this is still satisfying.

## Conclusions

- The FunkSVD model with regularization provided reasonable performance.
- The hybrid approach, combining collaborative filtering and content-based filtering, effectively leveraged user preferences and movie features to recommend movies for a couple.
- Further improvements could include more sophisticated regularization techniques, a better grid search for hyperparameters and incorporate more data in the training set.