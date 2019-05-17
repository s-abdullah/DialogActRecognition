# Dialog Act Recognition
## Feature Description
### Data Preprocessing
Before the features could be extracted, the data was preprocessed. The 11 most frequent tags were
extracted using Pandas and then the “+” tag was removed because it did not have any explanation. The
final tags included in the data were:

['sd', 'b', 'sv', '%', 'aa', 'ba', 'qy', 'x', 'ny', 'fc']

Furthermore, the raw data was also preprocessed to remove fillers and non-verbals. New column of
Clean text with only words was created and a filler column with the filler types and an action column
with the nonverbals as well. This code is in process.ipynb and the file created is TopTenTrain.csv with
147510 rows.
The following here feature sets were extracted from the data
1. Unigram Features
2. Parts of Speech Tags
3. Custom Features

#### Unigram Features
These features were extracted using sklearn and NLTK. First the corpus was created by extracted all of
the clean text. Then ngrams function from NLTK was used to create the unigrams and their frequency. A
threshold of 100 was set and any word with frequency below 100 was dismissed. Next CountVectorizer
from sklearn was used to get a vocabulary of unigrams in lowercase and then this vocabulary was used
to generate vectors of each text in the processed trained data. Overall there were 19541 unigrams and
after thresholding we got 778 and after lowercasing the final count of feature was 702. The file is saved
as unigramFeatures.csv and unigramFeaturesTest.csv and the code is in unigramFeatures.ipynb.
The motivation for this feature set was that there would be certain words that have a higher frequency
in certain dialog acts. For example, the “ny” act which yes-no-question would have high frequency of yes
or no words as compared to “sd” which is statement-non-opinion tag which might not have these
words.

#### Part of Speech Features
These features were also extracted using sklearn and NLTK. Again, the corpusof clean text was extracted
but tokenized using NLTK token function and then pos_tag method was used to get the POS tags. Then
CountVectorizer was used to create a vocabulary of these POS tags. Then this vocabulary was used to
create a vector for POS tags for each text in the data. These were a set of 33 features saved in
posFeatures.csv and posFeaturesTest.csv and the code is in posFeatures.ipynb
The motivation behind this was that these features will differentiate a lot between acts such as “ny” and
“aa” that are yes-no question and affirmative action and acts such as “sd” and “sv” which are statement-
non-opinion and statement-opinion. The motivation being that the latter would contain a lot more
nouns than the former.
#### Custom features
These features are handmade features for this specific data. Only sklearn and NLTK was used for this.
Sklearn learn was used to create a vocabulary of non-verbals that were extracted in data preprocessing,
which gave a set of 58 features. Along with this, simple python was used to create a further 16 features.
5 of these corresponded to the 5 fillers in the raw data [A F D E C] and them 6 of these were symbols in
the raw data [? - + -- # !] and the last 5 were length buckets from 0, 5, 15, 25 and greater than that.
These features are stored in customFeatures.csv and customFeaturesTest.csv with corresponding code in
custom.ipynb.
The motivation behind these features was to capture non lexical properties of the conversation. Some
acts such as “x” were only nonverbals with no text. Furthermore fillers are indicative of difficult question
or that the person is thinking (filled pause) which might correspond to “sv” statement-opinion tags.



## Classification
For the purpose of classification, I used logistic regression from sklearn, the solver was liblinear and the
classification was binary. For training of the classifier with all the 4 feature sets, the iterations were
increased from 1000 to 5000 due to warnings, otherwise it was same for all the rest.
Following are the results:

Overall the best performance (0.957129) was given by the classifier with all the four feature sets.
Without using the LIWC features, the combination of all the 3 features gave the best performance of
0.955923. Out of my feature sets alone, the one that used the unigram features was the best with a
performance of 0.947023.
