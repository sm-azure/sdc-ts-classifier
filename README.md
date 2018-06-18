# Udacity Traffic Sign Classifier
This is the report for the traffic sign classification problem.

## Submission Files
* Ipthon file - [Traffic_Sign_Classifier.ipynb](Traffic_Sign_Classifier.ipynb)
* HTML output of Code - [Traffic_Sign_Classifier.html](Traffic_Sign_Classifier.html)
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
  <b>Training Set Distribution</b>
</p>

<p align="center">
  <img src="/results/distrib_2.png">
  <br>
  <b>Validation Set Distribution</b>
</p>

## Design and Test a Model Architecture

### Preprocessing

#### Brightness


A casual observation showed that most of the images had low levels of brightness. A very simple algorithm was used to increase the brighness and these images were added back to the training set. This did not have a large impact on the improving the accuracy of the prediction during validation runs.  

#### Random Oversampling


A google search provided some explanation on how to handle the training set data imbalance on [Quora](https://www.quora.com/How-can-you-train-convolutional-neural-networks-on-highly-unbalanced-datasets) pointed to a recent [paper](https://arxiv.org/pdf/1710.05381.pdf). A simple random oversampling was used to remove this imbalance. To counter the negative side effect of overfitting, a dropout was added to the neural network. 

The resultant distribution was as shown below. **This step single handedly improved the validation result to ~95%.** However, the negative impact was the large size of the dataset, which was now - 172860. 

<p align="center">
  <img src="/results/distrib_3.png">
  <br>
  <b>Training Set Distribution Post Random Oversampling</b>
</p>

#### RGB to Grayscale


The images were then converted to grayscale. 

#### Normalization


Each image was then normalized to center around 0 mean and sd. 

### Model Architecture


The architecture used was a modified Lenet. The two primary modifications were, 
* Increase the number of output classes to 43
* Create a dropout to compensate for the oversampling

The final picture looks like the one below. All activations used were RELU.
<p align="center">
  <img src="/results/nn.png">
  <br>
  <b>Modified Lenet</b>
</p>


### Model Training


For training the model, the AdamOptimizer was used. The following hyperparameters were used,
* EPOCHS = 10
* BATCH_SIZE = 128
* learning_rate for the optimizer = 0.001 
* Dropout probability in training = 0.5

### Solution Approach

To understand the hyperparameter tuning better, only one change was performed at a time. Higher values of learning rate were resulting in wide oscillations in accuracy and were not convering - hence the value of 0.001 seemed most apporpriate (ignore the results showing 0.002 - just incorrect printing). Larger epochs were not increasing the validation accuracy. Batch size was increased, but this was resulting in lower validation accuracy. The dropout probability was not changed. 

Due to shortage of time additional transformations to the training data including rotations, blurring, shifts were not applied. However, it is understood as to why these transformations would enhance the accuracy and are planned for implementation in future work. 

Final accuracy achieved on the validation set was ~95%. 

## Test a Model on New Images

### Acquiring New Images


The 5 new images were downloaded from the Internet and are shown below,
<p align="left">
  <img src="/results/t1.png">
  <img src="/results/t2.png">
  <img src="/results/t3.png">
  <img src="/results/t4.png">
  <img src="/results/t5.png">
  <br>
  <b>New Test Images</b>
</p>

Comparision with some random images from the training set for the above categories.
<p align="left">
  <img src="/results/t11.png">
  <img src="/results/t12.png">
  <img src="/results/t13.png">
  <img src="/results/t14.png">
  <img src="/results/t15.png">
  <br>
  <b>Training Data Sets</b>
</p>

The images from the web should not be too difficult to classify. However, the rectangular shape below the "Children crossing" could make it difficult to identify.  

### Performance on New Images


The accuracy of the new images is 100%. The accuracy is better than the test accuracy of 94%. 

### Model Certainty - Softmax Probabilities

```
[[9.9999392e-01, 6.0321086e-06, 2.8503568e-09, 7.5968095e-12, 1.7686936e-12],
[1.0000000e+00, 3.9893177e-17, 1.2053429e-22, 1.3232147e-29, 2.7770240e-33],
[9.9999905e-01, 1.0071300e-06, 2.1129049e-10, 5.5836256e-11, 1.6759392e-11],
[1.0000000e+00, 8.9110941e-09, 5.3192712e-09, 4.0176173e-09, 3.2131988e-11],
[9.9579382e-01, 2.1311750e-03, 1.6901150e-03, 3.5209738e-04, 1.4136428e-05]]
```

The softmax probabilites indicate that the model is really certain about the classes it is predicting (prob ~1) while all other classes is with prob ~ 0. 

