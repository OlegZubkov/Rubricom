# Rubricom
Analysis and clustering of comments from the marketplace project.

**Team project done with https://github.com/SvetlanaY and https://github.com/dpanina**

The main project repository is here: https://github.com/SvetlanaY/Wildberries_bot/tree/master
## Description
There are a lot of knowledge about products and their features in comments in marketplaces. But sometimes there are too many of them and it becomes impossible for a human being to find something useful among them. 

In order to help people find relevant information in the comments, our team decided to create the telegram bot named Rubricom. Our bot does the folowing job:
* Receives from the user a link to the product in the marketplace
* **Parsing comments** on this product in real time
* Identifies the most popular words in comments and displays them as a word cloud
* Clusters all comments and gives the user a choice of **5 most popular topics**
* Gets back from the user the topic from the given one or any other word
* Asks what comments the user wants to receive (**positive, negative or all**)
* Returns 10 comments that are most relevant to the user's request
## Example of bot using
![Rubricom](https://user-images.githubusercontent.com/79414527/119517171-0dce0080-bd80-11eb-8596-2132779e31c0.gif)
## Technical detail
### Parsing
Parsing were made using Scrapy mainly by https://github.com/SvetlanaY and https://github.com/dpanina
### Preprocessing
As part of the preprocessing, the following was done:
* Removing standard stop words
* Custom stop word list for each product
* Cleaning text from unnecessary characters
* Removing the rarest and most frequent words by IDF
* MyStem lemmatization
### Topic modeling
As a baseline, we used a simple inference of the most popular words by IDF, but we didn’t really like the result because we saw that many real topics from the comments were not brought up. So we've decided to try LDA as more advanced tool, but again we didn't like the result as it was absolutely not interpretable for humans. 

Finally we made clasterization of comments using K-means method. After clasterization we defined most important word using TF-IDF for each of 5 cluster that we displayed to users as topics. 

Comments were displayed to users based of distance to cluster centroid.
### Sentiment analysis
Since marketplace users often make mistakes when rating products and as a result ratings do not correspond to the real text, we decided to independently determine the tone of comments and give it to our users. 

To do this, we took a ready-made dataset with reviews from Kaggle and a comparable set of comments from the marketplace (cumulatively, we had over 500,000 comments) and trained a **neural network on Keras** that predicts sentiment on them. We used **LSTM** because in our case the word order was very important. 

As main metric we used F1 score since it was not affected by class imbalance, but in order have metric for people who are far away from statistics we also used Accuracy, as it easier in explanation. **The final F1 score on validation was 94% and accuracy was 91%**.

## The repository structure
### telegram.py
The bot with models
### wildberries.py
Parsing
### Stopwords_and_Sentiment.ipynb
Preprocessing, clusterization model and inference of sentiment model
### Sentiment_model_full_extended.ipynb
Sentiment model training
