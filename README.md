# Ruth Bader GinsBot & the Supremes  
With the recent confusion in the United States Supreme Court, due to the ongoing health concerns of Justice [Ruth Bader Ginsburg](https://en.wikipedia.org/wiki/Ruth_Bader_Ginsburg) and the sudden death of Justice [Antonin Scalia](https://en.wikipedia.org/wiki/Antonin_Scalia) in 2016, I explored the idea of immortalizing their unique perspectives through data science and text generation.  

Using a semantic-based word-level long short term memory ('LSTM') deep recurrent neural network ('RNN'), based on Gensim's Word2Vec library and documents written by each of the two justices, I generated brief simulated [opinions](https://en.wikipedia.org/wiki/Judicial_opinion). The goal was to generate sentence-like structures while tying pre-trained Word2Vec semantics to each of the words.  

For an overview, please see the [presentation](https://docs.google.com/presentation/d/1puuGy_bqB3j-175qPZ7TONECoViH4B311gBVEzOBCUA/edit?usp=sharing) for this model.    

## Tools
* Amazon Web Services  
* BeautifulSoup  
* MongoDB  
* Pymongo  
* Keras  
* Gensim  
* HDF5  

## Data  
Data was scraped using BeautifulSoup from [Legal Information Institute](https://www.law.cornell.edu/) hosted by Cornell Law School, specifically court opinions written by Justice [Ruth Bader Ginsburg](https://www.law.cornell.edu/supct/justices/ginsburg.dec.html) and Justice [Antonin Scalia](https://www.law.cornell.edu/supct/justices/scalia.dec.html). Further details of all cases were acquired from Washington University Law's [Supreme Court Database](http://supremecourtdatabase.org/data.php).   

See [Scraping_Cornell.ipynb](https://github.com/janniec/GinsBot/blob/master/notebooks/Scraping_Cornell.ipynb).  

## Pipeline  
1. Clean and Explore the data.    
  * Cleaned using Pymongo in Jupyter Notebook.  
  * Explored and selected areas of law that I wanted to focus on.  
  * Exported selection of case opinions into text files.   
2. Pre-processed text files using Python's regular expressions.  
  * Tokenized the sentences into individual words.  
  * Vectorized the words mapping each word to vectors in Gensim's Word2Vec library.  
3. In a GPU on an AWS instance, built and trained a 4 stacked LSTM RNN model. I do not recommend  running this model on a CPU.   

See [Mongo_Cleaner.ipynb](https://github.com/janniec/GinsBot/blob/master/notebooks/Mongo_Cleaner.ipynb).  

## Model  
The LSTM RNN model is particularly useful to process sequences of words. The model is trained through iterations. Each iteration, the model trains on my text data for a set number of epochs. After each epoch, if the loss improves, the model overwrites and saves a newer version of itself. At the end of each iteration, the model takes in a seed sentence and generates text.  All text generations are saved to a text file. And the iteration will restart.  

See [SUPREMES_BOT.ipynb](https://github.com/janniec/GinsBot/blob/master/notebooks/SUPREMES_BOT.ipynb).

## Text Generations
The generations require some interpretation.  Using Gensim's most_similar() function, I explored words that were close proximity within the vectorspace of each word generated. For example, the word 'justice', was in the proximity to 'Constitution', 'rule', and 'law'.  
##### Example Seed Sentence  
I fed the following sentence to the Ruth Bader GinsBot model:  
"use of race discrimination in university admissions policy is lawful to achieve critical mass student body diversity"  
##### Example Generation  
"the utterly_contemptuous this Predatory_lending_practices Prudence_dictates Prudence_dictates intend Knee_Jerk_reactions"  
##### Interpretation  
Predatory lending practices have been long linked by the U.S. Supreme Court to the discrimination of minorities, for example, through the Fair Housing Act. "intend Knee_Jerk_reactions" implies a need for action.  Ruth Bader Ginsburg has repeatedly voted in favor of raced-based government action to combat the effects of discrimination against minorities.   
