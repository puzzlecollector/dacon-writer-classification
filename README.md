# Dacon Writer Classification

### Contest Information 
Similar to Kaggle's Spooky Author Classification Contest. We are given pieces from the text and we need to classify who wrote it. 
There are five possible authors who could have written the text. This is the [link to the competition](https://dacon.io/competitions/official/235670/overview/)

I have joined the contest together with Kevin Kim (University of Toronto, email: kimkevin2657@naver.com)   

### Data 
Over 54,000 training data, where we have a text snippet and a corresponding author label (0~4). 
Approximately 19,000 text snippet data for test data, and we need to predict the author for each of them. 

### Evaluation 
The contest scores participants based on logloss and the public LB is determined by calculating log loss for the random 30% of the data. 

### Code For the models we have used 
[1. Neural Net with no recurrent layer, backtranslation augmented](https://github.com/puzzlecollector/dacon-writer-classification/blob/main/no_recurrent_augmented.ipynb) - public LB Score 0.361. 
Model consists Embedding -> GlobalAveragePooling1D -> Dense Layer.  


[2. Neural Net with recurrent layer, backtranslation augmented](https://github.com/puzzlecollector/dacon-writer-classification/blob/main/with_recurrent_augmented.ipynb) - public LB Score 0.327. 
Model consists of Embedding -> Dropout -> bidirectional LSTM -> Conv1D -> GlobalMaxPooling1D -> Dense -> Dense. 


### Our Observations 
- We decided to choose backtranslation as our method of data augmentation, because it seemed to prevent overfitting as well as give more diversity to the dataset. googletrans api was initially used for backtranslation (to augment data), but making multiple api calls created error as it seemed to block our IP. We tried to switch to VPN and we also tried to put pauses of 3~10 seconds for each api calls, but our attempts were not successful. As a result, we used the pretrained NN neural machine translator called MarianNMT using pytorch. It was slow, but it did the job.  

- We decided to not remove stopwords, keep the punctuations and also not lower case all texts, because we believed that they tell us a lot about the stylistic aspects of the authors. Indeed, not removing stopwords,punctuations and not converting everything to lowercase outperformed no matter what model we used.  

- Using more number of words to keep when tokenizing improved performance.  

- Using pretrained embeddings (Glove, Word2Vec) did not help in our case. 

- We used a 10-fold ensemble for each of the models. At the end we did an average of the generated predictions. 


### References 
[Kaggle Spooy Author Identification Project by Robin Khatri](https://robinredx.github.io/docs/spooky_author_identification.pdf) 
