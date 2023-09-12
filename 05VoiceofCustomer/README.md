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

</body>
</html>

