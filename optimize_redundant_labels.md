# INLS 613 Text Data Mining

# 1 Motivation

Predictive analysis of text requires labeled data for training and testing. Usually, labeled data
is produced by human annotators. Traditionally, human annotators have consisted of trained
experts who go through the process of putting together a coding manual, code data until they
achieve an acceptable level of inter-annotator agreement, and finally work independently to pro-
duce a large enough set of labeled data for training and testing. While this careful process tends
to produce high-quality data, it has two main drawbacks: it is time-consuming and expensive.
One alternative to using highly-trained assessors is to use less expensive assessors who are
given less instruction and little training. Services like Amazon’s Mechanical Turk provide access
to such a pool of assessors.^1 Of course, there is a catch! Using less-expensive, non-expert asses-
sors can lead to noisy data, for several reasons: an assessor may not completely understand the
task, may be biased in favor of a particular class value (e.g.,positiveornegativereview), or may
not do the task carefully in an attempt to maximize their revenue. However, because the work
can be done for less, it is oftentimes feasible (and advisable) to collectredundantjudgements (i.e.,
multiple judgements per instance). Redundant judgements can serve multiple purposes. They
can be used to distinguish between reliable/unreliable assessors and they can be used to dis-
tinguish between easy instances (those with a clear majority label) and difficult instances (those
where assessors largely disagreed). For example, if an assessor tends disagree with the majority
vote label, we could down-weight his/her assessments in deciding on a finaltruelabel for train-
ing and testing, or we could ignore his/her assessments altogether. Likewise, when training the
model, we could place more weight on instances with a higher level of agreement (i.e., clear-cut
cases).
The goal for this homework is to expose you to the task of training a model using noisy
redundantlabels from multiple assessors. This has been a popular research topic in recent years.
For a nice example of this type of work, see Shenget al.[2].^2

# 2 Assignment

This is a fairly open-ended assignment. The prediction task is classifying music album reviews
intopositiveandnegativesentiment. The data originates from the full dataset used in Blitzeret
al.[1]. Download the dataset from:http://ils.unc.edu/courses/2022_fall/inls613_
001/hw/hw3_data.zip.
After uncompressing hw3_data.zip, you will notice two files:music.train.annotators.csv
andmusic.test.csv. Both files are in comma-separated format, so they can be opened using
most spreadsheet applications (e.g., Microsoft Excel) and they can be opened in LightSIDE.

- music.train.annotators.csv: This file contains 1000 music album reviews that have
    beenredundantlylabeled by eight hypothetical, non-expert annotators: a 1 ,a 2 ,a 3 ,... , a 8.

(^1) If you are not familiar with Amazon’s Mechanical Turk, start herehttp://en.wikipedia.org/wiki/
Amazon_Mechanical_Turkand herehttps://www.mturk.com/mturk/welcome
(^2) The third author of this paper, Panos Ipeirotis, has a done a lot of work on methods for obtaining reliable data
from non-expert assessors.

Every annotator labeled every review. There is no gold standard. All you are given are
these eight sets of noisy annotations. While alla 1 − 8 annotators can be considered noisy
annotators, they are not equal. Some may be more reliable than others (in general), some
may be more reliable in predicting one class than the other, and some may be biased
towards a particular class. As explained in more detailed below, your goal is to somehow
use these annotations to maximize prediction accuracy on the test set. There are lots of
things you could do!

- music.test.csv: This file contains 1000 music album reviews with gold standard labels.
    This is the test set. You can assume that these labels were produced by the “expert” (yet
    expensive) annotator we wish we had for the training set. Your goal is be try a few different
    ways of combining the noisy labels while training a model and to test your accuracy on this
    test set. **The test set should only be used for test purposes.**

## 2.1 Details

Try threedifferent methods of combining the annotations froma 1 − 8 and, for each method, eval-
uate its performance on the test set in terms of accuracy. In this case, accuracy is a good metric:
the test set is roughly balanced and we’ll assume that we care equally about detecting positive
and negative reviews.
There are lots of things you could do (more on this below). For this homework, only try three,
and, for each method, answer the following (90%):

1. Provide a thorough description of what you did. (10%)
2. Provide a well-grounded motivation for why you thought it would work. (10%)
3. Provide the accuracy of your method on the test set and discuss how well it worked and
    whyyou think that is. (10%)

Finally, taking into consideration everything you tried and whether or not it worked, provide
a discussion of your overall results. Did you notice any trends? Do you have any ideas for why
these trends occurred? What did you learn? (10%)

## 2.2 Things you might try

- Aggregate the eight redundant labels into one “true” label. There are many ways of doing
    this: majority vote,weightedmajority vote (favoring labels from annotators that you predict
    to be more reliable), etc.
- Instance weighting. Most learning algorithms can accommodate instance weighting in
    some form or another. With Naive Bayes, for example, you can easily weight instances
    differently by simply replicating them in the training set. For example, suppose you want
    to weight instancei 1 twice as much as instancei 2 , for whatever reason. You can do this
    by addingi 1 to the training set twice andi 2 only once. Or, if you want to assign different
    instances a weight of 0.10, 0.50, or 1.0, then you can do so by adding those with a weight of
    0.10 once, those with a weight of 0.50 five times, and those with a weight of 1.0 ten times.
    If you go this route, it’s up to you to decide how to weight instances differently

- Predicting the reliability of an annotator. There are many ways of identifying and down-
    weighting (or completely ignoring) unreliable assessors, for example: (1) you could com-
    pare each assessor with the majority vote, (2) you could test an annotator’s consistency by
    training and testing a model using the assessor’s own annotations (using cross-validation
    on the training set), or (3) you could explore the features that correlate with the assessor’s
    positive/negative predictions and see whether they make sense to you. **Note that you**
    **cannot test the reliability of each individual annotator by training a model on his/her**
    **annotations and then applying it to the test set. The test set should only be used three**
    **times (once for each of your three strategies).**

# 3 Submission

Please submit your report via Sakai (Word and PDF formats only) and be sure to include your
best accuracy.

# References

[1] J. Blitzer, M. Dredze, and F. Pereira. Biographies, bollywood, boom-boxes and blenders: Domain
adaptation for sentiment classification. InProceedings of the 45th Annual Meeting of the Association of
Computational Linguistics, pages 440–447. Association for Computational Linguistics, 2007. 2

[2] V. S. Sheng, F. Provost, and P. G. Ipeirotis. Get another label? improving data quality and data
mining using multiple, noisy labelers. InProceedings of the 14th ACM SIGKDD international conference
on Knowledge discovery and data mining, KDD ’08, pages 614–622, New York, NY, USA, 2008. ACM. 1

# Report

Part 1 Data Preprocessing 

1.1 Method 1: Majority Vote

1.2 Implementation

2.1 Method 2: Cross-validation and Uncertainty Intuition

2.2 Implementation and Interpretation

3.1 Method 3: EM Algorithm and its Variation

3.2 Implementation

3.3 Interpretation

Part 2 Data Test Results

Part 3 References, Reflection and Questions

# Part 1 Data preprocessing

## 1.1 Method 1 : Majority Vote

The final label is the class that have the most votes. The split is equally and randomly for ties.

1.2 Implementation

I did nothing. Because in method 3 mentioned later the tool I used automatically generated
the one “true” label coming out of majority vote:)

## 2.1 Method 2 : Cross-validation and Uncertainty Intuition

For the weight of annotators, I used 2-fold cross validations on the training data to see the
consistency of each of them. _(The reason I didn’t use 3 or more folds or round-robin fashion is
for somehow, I can’t use this function in LightSide. So I have to build excel files separately and
manually, which is tiring. Maybe there is a better way...?)_

For the weight of instances, my intuition is the more uncertain an instance is, the more
information it contains, and the more weight should be put on this instance. The way that I
measure uncertainty is to count the positive/negative votes. There are 8 annotators. If the
outcome of an instance is 4:4, then it belongs to the most uncertain ones. If the outcome of
an instance is 0 :8 or 8:0, then it belongs to the least uncertain ones.

2.2 Implementation and Interpretation
![image](https://github.com/yunwei-cao/practice/assets/107632477/2ca4a732-7e28-4da0-9576-80bc216df075)

From results of cross validation, we can infer that a4 has the poorest performance, which the
accuracy is the lowest and not consistent in n1 and n2. Both a3 and a7 have the best
performance of accuracy and consistency. The second best is a5. The next is a 6. Both a8 and
a2 have relatively high true-negative value and low true-positive value, vice versa for a1.
Based on the analysis, my ranking is:

a3=a7>a5>a6>a1=a2=a8>a

The weights I assign to them are:

a3_25%, a7_25%, a5_20%, a6_12.5%, a1_5%, a2_5%, a8_5%, a4_2.5%

Based on the weight of annotators, I added my weights for instances:
4:4--copy 4 times,

3:5 or 5:3--copy 3 times,

2:6 or 6:2--copy 2 times,

1:7 or 7:1 or 0:8 or 8:0--copy 1 time

The input files are attached with this homework.

Are these too subjective to make sense...? In addition, the inspiration comes from the
Entropy weight method (EWM). The reason I didn’t use it is that I don’t think this method is
well fit for this topic, for the indicator generated from annotators are not proper to measure_

## 3.1 Method 3 : EM algorithm and its variation

(GitHub Page: https://github.com/ipeirotis/Get-Another-Label/wiki)

(Original Workshop Paper: https://dl.acm.org/doi/10.1145/1837885.1837906)


3.2 Implementation
Basically, what I did is following the instructions of this open sources.

Step 1 Download the auto tool Apache Maven to generate the package for this Java project.

Step 2 Input parameters. Here is the description and my input files are attached with the hw.
Categories.txt: just 2 classes, positive and negative. Without priors but rather let them
be estimated from the data (in a maximum likelihood manner).

Labels.txt: the labels assigned by the workers. The required format is 3 columns
workerid<tab>objectid<tab>assigned_label. I used 1-1000 to represent the thousand
instances in case errors occur due to import large amount of original training data.

Cost.txt: I assigned equal costs to the misclassification decisions because we treat both
positive and negative classes equally.

Gold.txt: I manually pre-labeled 10 instances (1-10) in order to help in better estimating
the quality of the workers and, in turn, the correct labels for the objects without my
correct labeling. _(Is it enough...?)_

Evaluation.txt: The content is the same as gold.txt. But unlike the labels in the goldfile,
it does not get passed to the algorithm during execution. Instead, they are used only
for evaluating the quality of the estimations about the worker quality and about the
object labels.

Step 3 Run the program. Here is the command.

Step 4 Output files. Here is the description and the output files are also attached with the hw.

3.3 Interpretation
I want to make a quick summary of the paper’s ideas first, which I assume would be helpful to
explain the analytics results picked from the output files:

(1) EM algorithm is great! We got the estimated class priors, estimated error rates for
each worker, and estimated final one “correct” labels (which I taken to use on test
dataset in part 2 later).

(2) We want to estimate the workers’ quality. But wait! We recognized 2 challenges. One
is some spammers are lazy (marked all as one class) and smart, which leads to some
spam workers pass as legitimate. The other is humans are biased, which leads to
legitimate workers appear to be spammers.

(3) So how can we separate low-quality workers from high-quality, but biased workers?
We can start with transforming “hard” assigned label into the “soft” label. Next, we
estimate the expected cost of a soft label. Certain “posterior” soft labels will tend to
have low estimated cost, while uncertain “posterior” soft labels will tend to have high
associated costs. Then combined with priors (how often the workers assign labels), we
can compute the average misclassification cost of each worker.

(4) Finally, what is the baseline cost for a “random” worker? An easy baseline is to assume
that a worker assigns as label the class with the maximum prior probability.
Alternatively, if we assume a more strategic spammer, we can set as baseline the
expected cost of a worker that always assigns the same label, and this label
assignment results in the minimum expected cost.

I marked positive as 1 and negative as -1 for easy view. From the workers’
statistic above, we could infer that a3 and a7 are relatively reliable workers. Worker a1 is not
good at finding negative labels while a2 is not good at finding positive labels. Workers a5 and
a6 are not that good and a5 could be more biased than a6. Both worker a4 and a8 are not
reliable and a8 tends to mark the most as negative.

# Part 2 Data Test Results

Final Accuracy
naïve Bayes | logistic | regression SVM

method1 majority vote 0.686 0.712 0.69

method2 cross validation 0.688 0.724 0.711

method2 cross validation + instance weighting 0.712 0.725 0.695

method3 EM algorithm 0.714 0.733 0.699

1. The confusion Matrix excel table of each scenario are attached with this homework. Well, my first thought is there is basically no difference and I started to doubt if it is meaningful for me to pursue the data science path and drown in the statistic field ...
2. Vertically speaking, it seems that the latter two is slightly better than the simple majority vote strategy. I think it is because I tried to evaluate the workers’ quality more precisely so the weights assigned to them are fine-tuned, which could change a few of labels to the true right class in training data (majority vote is not always right).
3. Horizontally speaking, for this scenario, logistic regression is slightly better than the naïve bayes. Why and what’s the difference between these two ways? I think the question touches the essence of these two methods. From the theoretical perspective, the answer that I found is “They have different goals for optimization. Logistic regression wants to optimize the posterior likelihood lnp(y|x) while naïve bayes wants to optimize the joint likelihood lnp(y,x).” Still, it is just a fuzzy copy here and I cannot match this explanation to our practical scenario well. There are some interesting discussions on the forum and I put the link in part 3.
4. Well we didn’t cover the topic about SVM in INLS613, I couldn’t help including this method in my final test. It turns out this method is not fit well to more features or EM algorithms. I would pay attention to the principle and special points of it in the future study.

# Part 3 References, Reflection and Questions
https://www.slideshare.net/ipeirotis/crowdsourcing-using-mechanical-turk-quality-management-and-scalability

https://www.youtube.com/watch?v=9UfxFPcgcJ

When I noticed that in the instruction of hw3 the notation said Professor Panos Ipeirotis has a
done a lot of work on this topic, I was wondering could I find something useful? So I googled
his name, entered into his website and that is how I find the git repository “Get-another-label”.
It is surprisingly smooth using this tool, especially to people like me who has no foundation of
computer science. I also find the related slides and video talk listed above. I have to say his
website is really NYU Stern style ha-ha-ha (Yes, I’m a collector of personal websites).
