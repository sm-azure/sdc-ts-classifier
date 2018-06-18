# Udacity Traffic Sign Classifier
This is the report for the traffic sign classification problem.

## Submission Files
* Ipthon file - Traffic_Sign_Classifier.ipynb
* HTML output of Code - Traffic_Sign_Classifier.html
* Writeup - this Readme

## Dataset Exploration
### Dataset Summary
There are 34799 training examples, 4410 validation samples and 12630 test samples. Each image is of size 32x32 and has 3 color channels. The total number of image classes in the training set is 43. 

### Exploratory Visualization
A first look at the typical images in the dataset (taking any random 2 for the report).  
![Label 1](/results/random_training.png)
![Label 1](/results/random_training_2.png)

The distribution of the training set and validation set. 
<p align="center">
  <img src="/results/distrib_1.png">
  <br>
  <b>Training Set Distribution<b>
</p>

<p align="center">
  <img src="/results/distrib_2.png">
  <br>
  <b>Validation Set Distribution<b>
</p>

## Design and Test a Model Architecture
### Brightness
A casual observation showed that most of the images had low levels of brightness. A very simple algorithm was used to increase the brighness and these images were added back to the training set. This did not have a large impact on the improving the accuracy of the prediction during validation runs.  

### Random Oversampling
A google search provided some explanation on how to handle the training set data imbalance on [Quora](https://www.quora.com/How-can-you-train-convolutional-neural-networks-on-highly-unbalanced-datasets) pointed to a recent [paper](https://arxiv.org/pdf/1710.05381.pdf). A simple random oversampling was used to remove this imbalance. To counter the negative side effect of overfitting, a dropout was added to the neural network. 

The resultant distribution was as shown below. **This step single handedly improved the validation result to ~95%.** However, the negative impact was the large size of the dataset, which was now - 172860. 

<p align="center">
  <img src="/results/distrib_3.png">
  <br>
  <b>Training Set Distribution Post Random Oversampling<b>
</p>





