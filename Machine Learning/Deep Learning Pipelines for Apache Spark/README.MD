# Deep Learning Pipelines for Apache

## Introduction

Deep learning is a subfield of machine learning that involves the use of artificial neural networks to analyze and interpret data. Deep learning is characterized by: Multiple layers, Large datasets, Automatic feature extraction, Improved accuracy.

Pipelines are a crucial concept in deep learning, especially when working with large-scale data and complex models. 
Apache Spark MLlib is a scalable machine learning library built on top of Apache Spark, a unified analytics engine for large-scale data processing. MLlib provides a wide range of machine learning algorithms and utilities.Common use cases for Apache Spark MLlib include: 
Predictive maintenance, 
Customer segmentation, 
Recommendation systems, 
Image classification, 
Natural language processing.

## Design

Here we will be using Deep Learning Pipelines for Apache Spark.
Technologies used are: 
Deep Learning Pipelines by Databricks, 
Apache Spark, 
Keras, 
TensorFlow,  
Google Cloud Platform (GCP).

Cluster Environment Setup

    - Install spark-deep-learning via Maven and attach to cluster
    
    - Ensure Spark version 2.0+ 
    
    - CPU/GPU instances
    
    - Install TensorFlow, Keras, h5py via PyPI


## Implementation

Open Databricks and subscribe for free trial. Connect it with the given py notebook from databrick site.

<img width="712" alt="image" src="https://github.com/user-attachments/assets/5daeb7aa-539b-4cd5-b681-a03749c80a0e">

Extract Dataset. 

<img width="980" alt="Screenshot 2024-08-04 at 5 16 43 AM" src="https://github.com/user-attachments/assets/3198e6c2-95ac-43f9-83f6-317edba3809e">

Train and Test data.

<img width="608" alt="image" src="https://github.com/user-attachments/assets/2b39124e-8655-4fce-93eb-c009d3d4c68c">


Train and Evaluate the model

<img width="1000" alt="Screenshot 2024-08-04 at 5 11 39 AM" src="https://github.com/user-attachments/assets/74a6e395-2fe2-48e1-9330-1e043c2216a3">


<img width="824" alt="image" src="https://github.com/user-attachments/assets/e9d94747-7eb6-4073-a2ba-54098d15e257">

## Testing 

Image Processing and Prediction with InceptionV3

Load and preprocess images in a DataFrame

Generate predictions using the pre-trained model

Scalable architecture with Spark DataFrames and transformers


<img width="554" alt="image" src="https://github.com/user-attachments/assets/acb55498-8517-4db5-987d-acbebfeaf50c">


Keras Model Results

URI: Image file paths

Predictions: Model predictions with class probabilities


<img width="786" alt="image" src="https://github.com/user-attachments/assets/d106bb1a-ca2b-46ba-a88c-ccce46731a9c">


## Conclusion

Successfully integrated Deep Learning Pipelines with Apache Spark, enabling scalable deep learning. 

Efficiently handled large-scale image datasets using Spark DataFrames. 

Demonstrated transfer learning, trained logistic regression models, and showcased scalability benefits. 

Created a streamlined workflow for deep learning on large datasets.



