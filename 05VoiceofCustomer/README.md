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

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/2890a2c1-6e86-4cff-b639-d065c360cf14)


<p>This code initializes the necessary libraries, suppresses deprecation warnings, and loads your dataset from 'Sukee.csv'.</p>


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

df.tail()
</code>
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/8ca6c6d5-4e0b-472e-a52b-f1b53f77b311)

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

<h2>Visualizing Topics with pyLDAvis</h2>

<p>After building the topic model, you can use <code>pyLDAvis</code> to visualize the topics and their relevance with terms:</p>

<code>
pyLDAvis.gensim.prepare(model, gensim_corpus, dictionary)
</code>

<p>This will provide an interactive visualization where you can explore the topics, view the terms that make up each topic, and see the distribution of topics across the corpus.</p>
![0](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/b61af453-7bea-4cef-8574-aeb11b2fbefc)

![1](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/344ebb22-b1a1-452f-9f4f-909080d7ba49)


![2](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/1a7d3a45-7fe0-4f71-8dc4-b25f9db88b5b)

![3](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/299131c9-ec58-46f6-94aa-7d31560fc300)


<h2>Assigning Dominant Topic to Each Review</h2>

<p>For analysis purposes, you might want to know which topic is the most dominant for each review:</p>

<code>
df['topics'] = df['Review_tokenized'].apply(lambda x: model.get_document_topics(dictionary.doc2bow(x.split(',')))[0][0])
df['score'] = df['Review_tokenized'].apply(lambda x: model.get_document_topics(dictionary.doc2bow(x.split(',')))[0][1])
</code>

<p>This will add two new columns to the DataFrame:</p>
<ul>
    <li><code>topics</code> - the most dominant topic for each review</li>
    <li><code>score</code> - the score of the dominant topic for each review</li>
</ul>

<h3>Sorting by Topic</h3>

<p>You can also sort the DataFrame based on the topics for a better view:</p>

<code>
sorted_df = df.sort_values(by='topics')
</code>

<p>This will produce a DataFrame where reviews are grouped by their dominant topic, making it easier to analyze.</p>
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/4d09ff96-0c7f-4a1e-af91-f2ff234a8e81)

<h2>Visualizing Top Keywords per Topic using a Heatmap</h2>

<p>To understand which keywords are the most significant for each topic, a heatmap visualization can be helpful.</p>

<h3>Setting up the Thai Font</h3>
<p>For the correct display of Thai characters, you need to set up a Thai font:</p>
<code>
!wget -q https://github.com/google/fonts/raw/main/ofl/sarabun/Sarabun-Regular.ttf
import matplotlib as mpl
mpl.font_manager.fontManager.addfont('Sarabun-Regular.ttf')
mpl.rc('font', family='Sarabun')
</code>

<h3>Extracting Keywords from Topics</h3>
<p>Extract the top keywords from the model:</p>
<code>
n_keywords = 20
topics = model.show_topics(num_topics=num_topics, num_words=n_keywords, formatted=False)
topic_keywords = {i: {t[0]: t[1] for t in topics[i][1]} for i in range(num_topics)}
df_topic_keywords = pd.DataFrame(topic_keywords).T.fillna(0)
</code>

<h3>Visualizing with Heatmap</h3>
<p>Using Seaborn, you can easily plot this data:</p>
<code>
plt.figure(figsize=(15, 20))
sns.heatmap(df_topic_keywords, cmap="YlGnBu", annot=True, cbar=True)
plt.title('Top Keywords per Topic')
plt.xlabel('Keywords')
plt.ylabel('Topics')
plt.show()
</code>

<h3>Refining the Visualization</h3>
<p>To enhance readability, it's a good practice to omit near-zero values and adjust the annotation's rotation:</p>
<code>
annotation_mask = np.vectorize(lambda x: '' if x < 0.001 else '{:.3f}'.format(x))(df_topic_keywords.values)
plt.figure(figsize=(15, 10))
sns.heatmap(df_topic_keywords, cmap="YlGnBu", annot=annotation_mask, fmt='', cbar=True, cbar_kws={'label': 'Keyword Importance'})
for text in plt.gca().texts:
    text.set_rotation(90)
plt.title('Top Keywords per Topic')
plt.xlabel('Keywords')
plt.ylabel('Topics')
plt.show()
</code>

<p>By following these steps, you'll obtain a comprehensive heatmap that clearly showcases the significance of each keyword for every topic.</p>
 

![Chart](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/1dcd7c6b-af01-4eda-a8f9-9a05db4d3c17)



</body>
</html>

