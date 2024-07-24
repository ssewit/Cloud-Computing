# Collaborative Filtering

# Introduction 

The projcet aims to show how collaborative filtering could be used to select or recommend movies for viewers. The movie rating of the viewers is collected in a .data format and we will showcase two implementations approaches for this soltion.

Collaborative filtering is used in this projcet as it is a technique used in recommendation systems to predict user preferences based on the behavior of similar users. In this project's first implementation, the ALS algorithm is used to approximate the ratings matrix as the product of two lower-rank matrices, which are then used to make predictions.

This project has two implementations: The first one is developing a movie recommendation system using movie rating data collected form users on PySpark GCP. The second one is useing alternating least squares (ALS) algorithm to filter through viewer rating to create a movie recommendation system.

## Implementation 1: 

### Overview

Briefly we will read the data and train it into a matrix factorization model using ALS to predict rating. Then we will evaluate our model by considering the Mean Squared Error (MSE) which will help us determine how close the predicted ratings are from the actual ratings. 

By harnessing the power of collaborative filtering and ALS in PySpark, this project aims to develop a robust and scalable movie recommendation system, capable of handling large datasets and providing accurate personalized recommendations for users.

The goal of this implementation is to create a movie recommendation with machine learing library using collaborative filtering.

Brief overview:

 MovieLens dataset contains user ratings for movies (e.g., user ID, movie ID, rating, timestamp)
 
 Goal: develop a system that recommends movies to users based on their past ratings
 
 MLlib's Collaborative Filtering algorithm will be used for its scalability and performance
 
 This project demonstrates a practical application of machine learning in recommendation systems.

### Design 
Data Preprocessing:

  Convert MovieLens data from (UserID, MovieID, rating, Timestamp) to (UserID, MovieID, rating)
  
  Use shell scripts to process the data (Shell script 1 and 2)
  
  Shell script 1: uses while loop to read each line and echo the desired format
  
  Shell script 2: uses tr and cut commands to remove unnecessary spaces and extract the first three fields

System Architecture:

  Apache Spark for distributed processing
  
  MLlib's Collaborative Filtering algorithm for recommendation generation
  
  Spark provides scalability and fault-tolerance, while MLlib provides efficient matrix factorization

Key Components:

  Data Ingestion: reading and processing the MovieLens data
  
  Model Training: training the Collaborative Filtering model on the processed data
  
  Recommendation Generation: generating movie recommendations for users

### Implementation
Technical Details:
  
  Programming Language: Python
  
  Libraries: Apache Spark, MLlib
  
  Code Structure:
  
  Data loading and preprocessing
  
  Model training and evaluation
  
  Recommendation generation

Create a Bucket

Create the python file and save as recommendation_example.py and upload it to the bucket.

```
from pyspark import SparkContext
from pyspark.mllib.recommendation import ALS, MatrixFactorizationModel, Rating

if __name__ == "__main__":
    sc = SparkContext(appName="PythonCollaborativeFilteringExample")

    # Load and parse the data
    data = sc.textFile("formatted_data.csv")
    ratings = data.map(lambda l: l.split(','))\
                  .map(lambda l: Rating(int(l[0]), int(l[1]), float(l[2])))

    # Build the recommendation model using ALS
    rank = 10
    numIterations = 10
    model = ALS.train(ratings, rank, numIterations)

    # Evaluate the model on training data
    testdata = ratings.map(lambda p: (p[0], p[1]))
    predictions = model.predictAll(testdata).map(lambda r: ((r[0], r[1]), r[2]))

    ratesAndPreds = ratings.map(lambda r: ((r[0], r[1]), r[2])).join(predictions)
    MSE = ratesAndPreds.map(lambda r: (r[1][0] - r[1][1])**2).mean()
    print("Mean Squared Error = " + str(MSE))

    # Save and load model
    model.save(sc, "target/tmp/myCollaborativeFilter")
    sameModel = MatrixFactorizationModel.load(sc, "target/tmp/myCollaborativeFilter")
```
Directly upload the python file or use the following code to upload to bucket 
```
gsutil cp recommendation_example.py
```

Convert the u.data file to cvs file of a name you want to use
```
cat u.data | tr -s ' ' | cut -d' ' -f1-3 | tr ' ' ',' > formatted_data.csv
```

Upload your cvs data to your bucket
```
gsutil cp formatted_data.csv gs://mv_recommendation_sys/
```

<img width="544" alt="image" src="https://github.com/user-attachments/assets/34ff5054-1fa9-4be7-8f4e-dd3abc553155">

Create a cluster with the same location setting as the bucket. Mine was east1 but make sure to adjust it to your setting. To create a cluster you can run this code in the shell or go to dataproc option from the GCP menu bar and select create cluster to visually set up the cluster. 

```
gcloud dataproc clusters create cluster-9c40 \
 --region us-east1 \
 --zone us-east1-a \
 --single-node
```


Submit the dataproc job on the cluster
```
gcloud dataproc jobs submit pyspark
gs://mv_recommendation_sys/recommendation_example.py \
 --cluster cluster-9c40 \
 --region us-east1

```
### Result
We can view the result of the job as we run the job and view the mean square error to evaluate how good the predictions are. You will see something like this.

<img width="682" alt="image" src="https://github.com/user-attachments/assets/3fecd40c-8772-4a4e-a1d4-496936d094f9">




## Implementation 2: 

### Overview

This is a more complex implementation that used more complex data collections about the movie, user rating and movie tags (what type of movies they are). The implementation is going to be more meaningful since we are considering narrowed down information about the movies as well as the rating. By harnessing the power of collaborative filtering and ALS in PySpark we are planning to develop a robust and scalable movie recommendation system, capable of handling large datasets and providing accurate personalized recommendations for users.

The system's core functionality will be to accept user ratings as input and predict ratings for movies that a user hasn't rated, thereby providing personalized movie recommendations. To achieve this, the project will utilize the MovieLens dataset, a well-known collection of movie ratings and related data. The system will take user ratings as input and predict ratings for unseen movies, providing personalized recommendations. This project implementation utilizes the MovieLens dataset, consisting of movies.csv, ratings.csv, and tags.csv files.

### Dsign

The project design involves the following steps:

  Data Ingestion: The MovieLens dataset (movies.csv, ratings.csv, tags.csv) will be ingested into PySpark DataFrames for processing.
  
  Data Preprocessing: Handle missing values, data type conversions, and any necessary data transformations.
  
  ALS Algorithm: Utilize PySpark's implementation of the Alternating Least Squares (ALS) algorithm for collaborative filtering.

Model Training: Train the ALS model on the ratings data to generate user and item embeddings.

Model Evaluation: Evaluate the performance of the trained model using metrics such as Mean Absolute Error (MAE) or Root Mean Squared Error (RMSE).

### Implementation

PySpark Setup: Set up a PySpark environment in Google Colab, ensuring the necessary libraries and dependencies are installed.

Data Loading: Load the MovieLens dataset into PySpark DataFrames using the spark.read.csv() method.

Data Preparation: Preprocess the data using PySpark's DataFrame API, handling missing values and data type conversions as needed.

ALS Implementation: Utilize PySpark's ALS class to implement the collaborative filtering algorithm, specifying parameters such as ranking, regularization, and iterations.

Create GCP Bucket and take not of the location setting.

Upload to GCP Bucket: You can upload manually or through the terminal.

<img width="607" alt="image" src="https://github.com/user-attachments/assets/9479e8bc-e420-4876-9412-cda69396090a">

```
gsutil cp movies.csv gs:// movie_recommendation_mllib/
gsutil cp ratings.csv gs:// movie_recommendation_mllib/
gsutil cp Recommendation_Engine_MovieLens.py gs:// movie_recommendation_mllib

```

Set up Pyspark files in to read from the Google Cloud Storage. You can manually upload the file or run this code. 

```
gsutil cp Recommendation_Engine_MovieLens.py gs://movie_recommendation_mllib
```

Create a cluster with the same setting: You can manually create a cluster or run this code

```
gcloud dataproc clusters create cluster-9c40 \
 --region us-east1 \
 --zone us-east1-1 \
 --master-machine-type n1-standard-4 \
 --worker-machine-type n1-standard-4 \
 --num-workers 2

```

Submit the job on with the GCP path: Make sure to change the bucket name, cluster name and the rest of the setting to yours before implementing. 

```
gcloud dataproc jobs submit pyspark gs://movie_recommendation_mllib/Recommendation_Engine_MovieLens.py \
 --cluster=cluster-9c40 \
 --region=us-east1 \
 -- \
 --input_path_movies=gs://
movie_recommendation_mllib/movies.csv \
 --input_path_ratings=gs://
movie_recommendation_mllib/ratings.csv

```

### Result Preview

When we run the python Recommendation_Engine_MovieLens.pynb and Recommendation_Engine_MovieLens_checkpoint.pynb files attached to this project the result is shown there to help us see the mean absolute error (MAE) and root mean squared error (RMSE) to evaluate the accuracy of the predictions. When you run the job you will see something like this. The results match with the code we run in the ipnb file. 

<img width="664" alt="image" src="https://github.com/user-attachments/assets/139f1687-26ff-48a9-b9d7-22c571e3ac26">


<img width="717" alt="image" src="https://github.com/user-attachments/assets/6929b00d-47ed-40e6-a8ee-fce63eb5d373">


<img width="674" alt="image" src="https://github.com/user-attachments/assets/454893e2-5fbd-4816-a4b6-10cf1f1de86e">



