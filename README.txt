All the code is written in python notebooks.
There are 6 files in total:
	-process.ipynb
	-train.ipynb
	-graph.ipynb
	-Posfeatures.ipynb
	-customFeatures.ipynb
	-UnigramFeatures.ipynb
	

1)	first all the block have to be run in process.ipynb. This file has to be run twice once for the train file and seconf for the test file
The files names are in the second block and you just need to comment one out and comment the other in to get the processed train and test file 
which are called "TopTenTrain.csv" and "TopTenTest.csv"

2)	After this,run the UnigramFeatures.ipynb completely and then the Posfeatures.ipynb and then the customFeatures. Each f them will produce two csv file named
"unigramFeatures.csv", "unigramFeaturesTest.csv", "posFeatures.csv", "posFeaturesTest.csv", and "customFeatures.csv", "customFeaturesTest.csv"

3)	Then completely run the train.ipynb, this will print out the accuracies for all ten classes for all six combinations of feature sets. First witll be LIWC
Second will Unigram feature set, third will POS feature set, fourth will be custom feature set, fifth will be all three feature sets and the last will all features + LIWC feature set. Additionally since the last gave the best score, it also prints out the confusion matrices.

4) If you want to see the graphs that are used in the report then run the grap.ipynb and it will give the coreect graphs. 


#code is well documented and commented in the notebooks and also expalined in the report
