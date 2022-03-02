# Stackoverflow semantic-search

## What is semantic search?
Semantic search is a data searching technique in a which a search query aims to not only find keywords, but to determine the intent and contextual meaning of the the words a person is using for search


## Problem statement

Since stack overflow is one of the largest Q&A platforms there are lots of data for every query, this huge amount of information makes it difficult to search for the solution that one is looking for. someone who is new to programing, it is not easy to find optimal answers to their queries when search engine results are based only on words used in search query and not based on semantic similarity.

Therefore an semantic search engine with scalability is necessary to tackle the problem. Given any query our search engine should return questions and answers related to question that are semantically similar in a small window.


## Source/Useful Links

Stackoverflow has received over 21 million questions and 31 million answers as of 2021 with thousands of question asked daily. In order to be effectively crate a semantic search engine with large amounts of data, we need to be able to preprocess the data effectively and extract contextual information from them.

This dataset provided by Stack overflow contains about 37000 unique tags.

Source: https://www.kaggle.com/stackoverflow/stacksample


## Real-world/Business objectives and constraints.

   Encode contextual information.  
   Scalable implementation.  
   Search results should not take minutes to load. results should be displayed in seconds.
    
    

# Machine Learning Problem
##  Data
### Data Overview

  Source : https://www.kaggle.com/stackoverflow/stacksample/data
  
  There are 3 Data frames/n
  questions.csv - which contains questions and relavant details
  tags.csv - tags tagged to each question
  answers.csv - which contains answers and relavant details
  
Every question contains 

  Id – unique id assigned to every question when a question is posted
  Title – summary of the question asked
  Body - description of the question
  Tags – categories that questions that belong to
  Answers – accepted answer to the posted question
  Score – no of upvotes
  Question created date - date when question is posted
  Answer created date - date when the answer is posted
  owner id - User id of each user


Total dataset consist of 1263995 questions and 37000 unique tags with 2million questions anwsered.


###  Machine Learing Objectives and Constraints

Objective: return similar questions for each question given by user using simiarity measures.

Constraints:

    embeddings for every data point is required.
    compute constrains and scalability
    Some Latency constraints.

## Pre-processing text data

Concatenating tags tagged to each question and merging the feature with question's data frame.
using regular expression to remove tags and clean the data.
Tried spell checkers on data, unfortunately none of them performed well

## Exploratory Data Analysis

####  Distribution of tags
####  Value counts of tags with each question
####  CDF of question scores
####  count plot of score
####  Word Cloud for title
#### Distribution of number of words in body
####  PDF of answer scores

### Multivariate Analysis

#### Distribution of score with respect to number of words in body
#### Distribution of score with respect to number of words in Title and if the question is cloeed or open
#### Distribution of number of words in title with respect to number of words in body



# Embedding techniques
### Doc2Vec embeddings with only the title of the question
    question: how to concatenate two string in java
    answers (['winpcap how can i get protocol within tcp packet http fields',
       'bufferedwriter not writing all the information',
       'implement stripe payment gateway in cordova phonegap application',
       'panda dataframe conditional mean depending on values in certain column',
       'error saving a file on server
       
    Results are poor
### Doc2Vec embeddings with title and body of the question
    question: how to concatenate two string in java
    answers: ['is it possible to load dynamically a phase listener with jsf one two',
       'getting mouse position with javascript within canvas',
       'fastest way to escape a string in javame',
       'why would my java program send multicast packets with a ttl of one',
       'tokenize text into type string pairs']
       
      Results are poor
       
 ### Avg Tfidf-Word2Vec with only the Title of the question 
     question: how to concatenate two string in java
     answers: ['concatenate string in swift',
       'concatenate string in java arraylist',
       'how to concatenate strings in java',
       'concatenate to string in rails form',
       'how do i concatenate a string literal and a string variable in java']
      
     Results are better
 
 ### Avg Tfidf-Word2Vec with Title and body of the question (with 400000 data points)
     question: how to concatenate two string in java
     answers: ['how do you concatenate two strings',
       'concatenate all list content in one string in c',
       'concatenating string in classic asp',
       'what is an efficient way to concatenate all strings in an array separating with a space',
       'what is the fastest way to concatenate two strings in java']
       
     Results are better
     
 ### BERT pretrained on stack overflow with only title embeddings
     question: how to concatenate two string in java
     answer: ['java concatenate two strings error',
       'what is the fastest way to concatenate two strings in java',
       'how do you concatenate two strings',
       'how do i concatenate a string literal and a string variable in java',
       'how to append two string']
       
      Result : bert embeddings produce the best result
 
 ### BERT pretrained on stack overflow with only title+body embeddings (with 400000 data points)
     question: how to concatenate two string in java
     answer : (['java concatenate two strings error', 'how to append two string',
       'how do i concatenate a string literal and a string variable in java',
       'concatenate integer values separated by pipe and using java',
       'in android how to concatenate basesixfour encoded strings']
       
     Result : bert embeddings produce the best result
     
     

## Similarity measure
   Cosine similarity is used to retrive similar questions.
    

## Model Deployment
   Due to compute constrains only title of the question is used during deployment.
   Flask api is used to host the applicaton in local machine. 
   ngrok is used to tunnel local host in to public network.
