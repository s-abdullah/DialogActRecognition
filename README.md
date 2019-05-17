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

![alt text](https://github.com/s-abdullah/DialogActRecognition/blob/master/images/table.png)

Overall the best performance (0.957129) was given by the classifier with all the four feature sets.
Without using the LIWC features, the combination of all the 3 features gave the best performance of
0.955923. Out of my feature sets alone, the one that used the unigram features was the best with a
performance of 0.947023.


## Error Analysis
### Easiest Classes
The easiest classes to predict were “fc” (Conventional-closing) “x” (Non-verbal) and “qy” (Yes-No-
Question), all with an accuracy of 0.99.
In case of “x” since it is very easy to identify because it will always have non-verbals and because the
custom features included non-verbals as a feature therefore it was easy to discriminate against all the
other classes.
In case of “qy” since it is easy to identify because yes and no unigrams together with custom features
like having “?” symbols really separate it from other classes and hence easy to classify.
Incase of “fc”, I believe it was easy to classify because I believe that this class will mostly have the
unigram “you” in it which will help distinguish from other class which do not necessitate the inclusion of
this world. It will probably have “you” because one addresses the other person when closing the
conversation (unless it’s a hang up).
### Hardest Classes
The hardest classes to predict were “sd” (Statement-non-opinion) and “sv” (Statement-opinion), both of
an accuracy below 0.9.
In both these cases they are statements that may involve a lot of words. Furthermore, it is really hard to
distinguish if something said is opinionated or not especially if the people talking are subtle. Hence, I
believe the low accuracy of these classes are because of the difficulty of telling them part from each
other and not other classes.
Common Errors
Judging form the confusion matrices I believe that the classifier had difficulty in the following classes: sd,
sv, b, aa. As stated above, opinionated statements are hard to detect hence there might be confusion
between sd and sv classes. Furthermore, I believe there was confusion between b and aa class because
the appreciation can sometimes be hard to distinguish from agreement (in real life and by machines as
well)
Otherwise all the rest of the classes had fairly low true false negative and false positives.
### Improvements
To improve “sd” and “sv” classification, we can further use bigram feature because I hypothesize that “I
am ” would occur more in “sd” and “I think” would occur more in “sv”, as it is an easy way to seeing if
something is opinionated or not.
Another improvement should be the placement of certain words in the text. Example if “Yes” is in the
start then that might indicate “qy” (yes-no-question) but if it is somewhere else then it might indicate
“aa” (affirmative agreement). So, parse trees and word location will further improve the accuracies.
