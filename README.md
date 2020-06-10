# Amazon Product Feature Analysis

## Abstract

Developing a new product or service requires knowing what features concern your customer’s attention. You also need to know your customer’s prioritization for these features. However, gaining feedback on a potential product becomes a challenge when you have nothing to put in front of a customer. I believe this challenge can be resolved by looking at real world feedback from reviews left on Amazon.

Feature identification and prioritization will be helpful for product managers, aspiring entrepreneurs, marketing teams, business leaders, and even customers wanting better insight in comparing purchase options. The gained business intelligence could reshuffle a product development teams’ product development roadmap, product manager’s budget in developing certain features, and marketing’s narrative selling the product.

Challenge:

  - A product manager needs a map of their customer's needs. 

Goal:

  - Use natural language processing to identify product features using Amazon reviews for a given product.
  - Prioritize the importance for these product features by calculating the relationship that each review has with each category.
 

## Results


  - I implemented topic modeling on Amazon product reviews with Scikit Learn's Non-negative Matrix Factorization (NMF) to identify the features customers cared about most based on their feedback in Amazon reviews.

  - I prioritized these features in relationship to one another to provide a hierarchy of importance for the identified product features. 


## Subject Matter

I chose to focus my analysis on a microwave after being inspired by Jack Donaghy, a business leader from the 2000's hit TV show '30 Rock'. The product in question was less relevant then the process implemented to demonstrate my goal.

I selected the Panasonic 2.2 Cu. Ft. 1250 W/ Genius Sensor Countertop/Built-In Microwave Oven with Inverter Technology because it had the most reviews of an microwave in my chosen dataset.

![image](https://github.com/GrilledJeez/amazon_product_feature_analysis/blob/master/images/panasonic_microwave.jpg)

Panasonic Microwave
   <br><br><br>

## Data

I sourced my data from Amazon's Reviews Dataset hosted on their AWS site. Instructions to find and load the data can be found on the [Amazon Customer Reviews Dataset](https://s3.amazonaws.com/amazon-reviews-pds/readme.html) website. The data selected came from Amazon's 'Kitchen' category

This data covers Amazon reviews from 1995-2015. The data is five years old at the youngest, and is an excellent analog to demonstrate product feature identification and prioritization using customer reviews as feedback.

![image](https://github.com/GrilledJeez/amazon_product_feature_analysis/blob/master/images/data_eda.png)
Data Highlights
   <br><br><br>

## Workflow

![image](https://github.com/GrilledJeez/amazon_product_feature_analysis/blob/master/images/technology_stack.png)

Technology Stack
   <br><br><br>

## Coherence Score

I used GENSIM's NMF model and Coherence Score function to determine the best hyper-parameter combination and total topics to use. The final hyper-parameters I settled at after trialing multiple iterations was:

 - min_df = 2
 - max_df = .95
 - n_grams = 2
 - features = 3000
 - topics = 5

Some of the outputted coherence scores scored higher than these combinations. However, I went with these inplace of other combinations with higher scores as this combination provided the most inferences while holding the highest similarity amongst associated keywords within my different topics.

![image](https://github.com/GrilledJeez/amazon_product_feature_analysis/blob/master/images/coherence_score.png)

Coherence Scoring
   <br><br><br>

## Model

I used Scikit-Learn's Non-negative Matrix Factorization for my model because in addition to performing topic-modeling on the dataset I wanted to reveal any latent relationships existing within the topics if they were there.

Within my model I decided to initialize with 'nndsvda', setting 'mu' as my solver and 'kullback-leibler' as my beta_loss. I settled on these parameters for the sake of gaining greater inference into the data. The distinction when choosing to use these versus choosing not to use these comes when sparsity is desired within the dataset. As far as datasets go this is a relatively small one (1875 reviews), and although there are many dimensions created once we vectorize our corpus of text, we do not gain much by sparsing the data. Scikit-Learn recommends using 'nndsvda' to initialize when data sparsity is not desired. 'Mu' and 'kullback-leibler' were settled on after realizing better results when matching reviews to our topics' categories.

Once I ran the model and clustered our words into our five different topics, I saw the following result. I inferred the topic labels for my categories. There are some words (highlighted in red) that do not match well with the theme for the category, but this is the nature and the problem when using unsupervised learning.

![image](https://github.com/GrilledJeez/amazon_product_feature_analysis/blob/master/images/keywords.png)

Top Keywords and Labeled Topics
   <br><br><br>


## Output

Upon completing my model I used softmax (a heuristic that converts arbitrary positive values into probabilities) to source arrays within my NMF model's W matrix then assign a probability for an existing relationship between each of my reviews and each of my topics. 

![image](https://github.com/GrilledJeez/amazon_product_feature_analysis/blob/master/images/categorized_review.png)

Technology Stack
   <br><br><br>

Once I had a relationship probability for every review with each topic, I summed the probabilities for every category and divided the sum by the total reviews in my dataframe. This resulted in my final ranking and prioritization for my topics (product features). 

![image](https://github.com/GrilledJeez/amazon_product_feature_analysis/blob/master/images/product_features.png)

Product Featurs Ranked & Prioritized
   <br><br><br>

Additionally I performed some spot checks on randomly selected reviews to determine how my model was performing overall. There are clear examples where my model performs excellently and others where it performs poorly. 

Below are two. In the first the customer complained about a malfunctioning door which was a common complaint as revealed by our model. In the second we see the review categorized in the 'Size' topic, but there is no mention of the size of the microwave. Additionally in the second review we see the customer reference the lifetime and power of the microwave, but neither of these registered with the categorization. 

![image](https://github.com/GrilledJeez/amazon_product_feature_analysis/blob/master/images/good_categorization.png)

Model Categorizing Well
   <br><br><br>

![image](https://github.com/GrilledJeez/amazon_product_feature_analysis/blob/master/images/bad_categorization.png)

Model Categorizing Poorly
   <br><br><br>


## Future Work

In the future I would like to integrate the star ratings for each of the reviews to provide more informative insights into the customer's relationship with the given product. Additionally I would like to implement GENSIM's NMF model to compare its performance with Scikit-Learn's and to perhaps realize better results. Once I have determined the best pipeline for building my model, I would like to apply the same methodology on a dataset of app-reviews webscraped from Google's Playstore or Apple's App Store. 