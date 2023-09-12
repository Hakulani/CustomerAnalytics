<!DOCTYPE html>
<html>
<head>Voice of Customer Topic Modeling from review </head>
<body>

<h1>Voice of Customer Topic Modeling from review using PyThaiNLP, Gensim and pyLDAvis</h1>

<p>This repository contains a project that performs topic modeling on Thai text. The main libraries used are PyThaiNLP for Thai language processing, Gensim for topic modeling, and pyLDAvis for visualization.</p>

<h2>Installation</h2>

<p>Before running the code, ensure you have the necessary libraries installed. You can install them using pip:</p>

<code>
!pip install --upgrade --pre pythainlp<br>
!pip install pyLDAvis<br>
!pip install gensim<br>
!pip install numpy
</code>

<p><strong>Note:</strong> After installing the libraries, it's recommended to restart the runtime.</p>

<h2>Usage</h2>

<p>Once the libraries are installed, you can proceed with the main code:</p>

<code>
import pandas as pd<br>
import pythainlp<br>
import gensim<br>
import numpy as np<br>
import pyLDAvis.gensim<br>
pyLDAvis.enable_notebook()<br>
import warnings<br>
warnings.filterwarnings("ignore", category=DeprecationWarning)<br>

df = pd.read_csv('Sukee.csv')
</code>

<p>This code initializes the necessary libraries, suppresses deprecation warnings, and loads your dataset from 'Sukee.csv'.</p>

<h2>Further Steps</h2>
<p>Continue by tokenizing the Thai text, building a dictionary and corpus, and running the LDA model. Afterwards, use pyLDAvis to visualize the topics in the dataset.</p>
<h2>Tokenization</h2>

<p>Thai text tokenization is achieved using the <code>pythainlp</code> library. The following code segment showcases the tokenization process:</p>

<code>
#Tokenize Words<br>
stopwords = list(pythainlp.corpus.thai_stopwords())<br>
removed_words = ['',' ','  ','\n','ค่ะ','คะ','ก้อ','อ่า','ครับ','เค้า','ร้าน','\u200b','นะคะ','ๆ','('')','(',')','(' ,')','(' , ')','(', ')','โอเค','"','"', 'กก' , 'Got' , 'To' , 'Grill']<br>
screening_words = stopwords + removed_words<br>
<br>
def tokenize_with_space(sentence):<br>
    merged = ''<br>
    words = pythainlp.word_tokenize(str(sentence), engine='newmm')<br>
    for word in words:<br>
        if word not in screening_words:<br>
            merged = merged + ',' + word<br>
    return merged[1:]<br>
<br>
df['Review_tokenized'] = df['Review'].apply(lambda x: tokenize_with_space(x))<br>
</code>

<h2>Topic Modeling</h2>

<p>For extracting topics from the reviews, the Gensim library is utilized. The following code illustrates the topic modeling process:</p>

<code>
#Create Dictionary<br>
documents = df['Review_tokenized'].to_list()<br>
texts = [[text for text in doc.split(',')] for doc in documents]<br>
dictionary = gensim.corpora.Dictionary(texts)<br>
<br>
gensim_corpus = [dictionary.doc2bow(text, allow_update=True) for text in texts]<br>
word_frequencies = [[(dictionary[id], frequence) for id, frequence in couple] for couple in gensim_corpus]<br>
<br>
#Topic Modeling<br>
num_topics = 4<br>
chunksize = 4000 # size of the doc looked at every pass<br>
passes = 20 # number of passes through documents<br>
iterations = 50<br>
eval_every =1 # Don't evaluate model perplexity, takes too much time<br>
<br>
#make a index to word dictionary<br>
temp = dictionary[0] #This is only to load the dictionary<br>
id2word = dictionary.id2token<br>
<br>
%time model = gensim.models.LdaModel(corpus=gensim_corpus, id2word=id2word, chunksize=chunksize, \
                                    alpha='auto', eta='auto', \
                                    iterations=iterations, num_topics=num_topics, \
                                    passes=passes, eval_every=eval_every)
</code>

<p>With this, you should have a topic model ready. You can then visualize the topics, inspect their keywords, or perform further analyses based on the topics.</p>

</body>
</html>

