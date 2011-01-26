frameseq
========

This is the Frame Bias Model for selection of gene finder predictions (stops).

It is a probabilistic model which selects/ranks a sequence of gene predictions
from some gene finder based on the score given by the gene finder and the general
tendency that genes are picky about the reading frames of the neighbor genes.

Setting up the software
-----------------------

First, a few things must be done to setup the system. 

1. Download and install [PRISM](http://sato-www.cs.titech.ac.jp/prism/). The Frame Bias Model was tested to work with version 2.0 of PRISM.
2. Setup paths in lost.pl

There are three facts which you will probably need to change:

	lost_config(prism_command,'prism').

This one is OK if you have set up PRISM to be in your binary path. Otherwise you should 
provide the extended, full path to the prism binary.

	lost_config(lost_base_directory,'/change/to/your/local/lost/directory').

This should just be updated to the directory this README file resides.

	lost_config(platform,'to specify unix or windows').

This should be either 'unix' (which covers most unix derivates such as BSD, Linux or OSX) or 'windows'. 

Running the software
--------------------

The program can be started through the PRISM system. When you start up the PRISM system you will 
be greeted by a message followed by an interactive prompt, that looks like this:

	| ?-

At this prompt, you can load the Frame Bias Model by typing (disregarding the prompt characters): 

	| ?- [framebias].

After this you can run a particular inference using the model. 

### Filtering using the model

For instance, you mau want to filter predictions from a gene finder to remove potential 
false positives. To do this, you type:

	| ?- filter(PredictionsFile,TrainingDataFile,FilteredPredictionsFile).

where,

- *PredictionsFile* points to a file in Prolog format, which contains all of the predictions from the 
  gene finder. In scripts, you will found genemark_to_prolog which converts a report in the format
  producted by genemark to the expected Prolog format. 
- *TrainingDataFile* points to a file with training data. Typically, this is derived from a GenBank PTT file or similar.
  In the scripts directory there is a Prolog file, ptt_to_prolog.pl, which can be used to convert a PTT file to a 
  Prolog file of the right format.
- *FilteredPredictionsFile* points to a filename where the results of running the model (a subset of the originally
  predicted genes) will be written to.

For instance, to run the filter on the supplied sample data (E-coli K12, MG1665), you would type:

	| ?- filter('data/genemark.report.pl','data/U00095.ptt.pl','data/filter_results.pl').


### Evaluating the results:

The program allows you to evaluate how good a set of predictions is: 

	| ?- evaluate(GoldenStandardFile, PredictionsFile).

where,

- *GoldenStandardFile* is a file containing a list of verified genes in the Prolog format.
- *PredictionsFile* Is a file containing a list of predicted genes in the Prolog format.

Running this query will output a various evaluation metrics such as sensitivity and specificity.

Note that  _all file names should be inclosed in single quotes_.