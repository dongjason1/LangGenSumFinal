# LangGenSumFinal
jwd2139

## LSTM.ipynb
Contains the data processing, model training, and sampling process for the LSTM method of the project. Notebook was run in Google Colaboratory

#### Data
The dataset was downloaded from kaggle. We convert the csv rows into a text file of username separated by newline characters before feeding it to the model. 

#### Model
The Model is an LSTM built in tensorflow. We first create a character to integer mapping to that we can convert the data into a readable format by the model and define our model as a single LSTM layer, a dense hidden layer, and a softmax output layer. 

#### Sampling
We implement two sampling methods, tampling with temperature and topK sampling. We observed better results with temperature sampling and use this as the results in the paper

## GPT2.ipynb
Contains scratch code used to test GPT2, praw (a reddit scraper) and Universal Sentence Encoder(USE) embeddings. We found little success using USE and so we decided to pursue FastText as a subword level embedding generator. We'd been using Google Colaboratory up to this point and had trouble getting FastText to work in this enviornment so we decided to do the rest of the project in a local jupyter instance. 

## LanguageGenSum.ipynb
Contains the data processing, model training, and sampling process for the finetuned GPT2 method of the project.

#### Data
The first part of the notebook experiments with FastText. We demonstrate that we can use this embedding as an appropriate approximation of subreddit similarity and username similarity. 

We then attempt to scrap Reddit usernames using praw. After a few attempts, we realized we would not be able to get enough data from this technique as Reddit has a built in maximum response limit of 1000 posts. Even by traversing comment trees, the time needed to generate enough data was too much. 

We pivoted to using a public BigQuery dataset called fh-bigquery.reddit_posts. We used queries directly in the BigQuery UI and saved the datasets locally (available upon request). We then process the data by only extracting data pairs with a high cosine similarity score between the username and the subreddit's fasttext embedding. We format this for GPT2 by writing the rows as a text file.

#### Model
We use gpt_2_simple as our starting point for fine tuning GPT2. It provides a nice interface for downloading, finetuning, and sampling from the trained model. We train two models, one on the dataset with a similarity score threshold of 0.2 and the other with a score threshold of 0.15.

#### Sampling
We use gpt_2_simple's generate() method to create samples. By including the prefix argument, we can change the starting point of the model's generation. We also have the option of tuning the generation temperature, and seed if neccessary. 
