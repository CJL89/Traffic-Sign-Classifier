#**Traffic Sign Recognition** 

##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./examples/visualization.jpg "Visualization"
[image2]: ./examples/grayscale.jpg "Grayscaling"
[image3]: ./examples/random_noise.jpg "Random Noise"
[image4]: ./examples/placeholder.png "Traffic Sign 1"
[image5]: ./examples/placeholder.png "Traffic Sign 2"
[image6]: ./examples/placeholder.png "Traffic Sign 3"
[image7]: ./examples/placeholder.png "Traffic Sign 4"
[image8]: ./examples/placeholder.png "Traffic Sign 5"

## Rubric Points
###Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/CJL89/Traffic-Sign-Classifier/blob/master/Traffic_Sign_Classifier.ipynb)

###Data Set Summary & Exploration

####1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the numpy library to calculate summary statistics of the traffic signs data set:

* The size of training set is 34,799
* The size of the validation set is 4,410
* The size of test set is 12,630
* The shape of a traffic sign image is 32, 32, 3
* The number of unique classes/labels in the data set is 43

####2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. It is a bar chart showing how the different signs are represented in the data set.

(https://github.com/CJL89/Traffic-Sign-Classifier/blob/master/traffic_sing_histogram.png)

###Design and Test a Model Architecture

####1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

As a first step, I decided to grayscale the images since it has been discovered that color does not actually help with the overall prediction accuracy.

Here is an example of a traffic sign image before and after grayscaling.

https://github.com/CJL89/Traffic-Sign-Classifier/blob/master/traffic_sign_after_grayscaling.png

As a second step, I normalize each individual picture using the formula: image 128.0 / 128.0 which brings the value of the image between -0.5 to 0.5 which helps bringing the data to a mean zero and equal variance. After the normalization formula I did print the means just to verified the correct results were obtained.

Here is an example of a traffic sign image before and after grayscaling.

https://github.com/CJL89/Traffic-Sign-Classifier/blob/master/traffic_sign_after_grayscaling.png

As a third step, I created some extra data for those signs that are misrepresented since some of them have more samples than others. This would help with the overall trainning and prevent any biases. The function is based on Jeremy Shannon: https://github.com/jeremy-shannon/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb.

####2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x1 gray image   							| 
| Convolution 3x3     	| 1x1 stride, same padding, outputs 14x14x6 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 14x14x6                  |
| Drop out              | 0.5                                           |
| Input         		| 14x14x6 gray image   							| 
| Convolution 3x3     	| 1x1 stride, same padding, outputs 5x5x16      |
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 1x1x16                   |
| Drop out              | 0.5                                           |
| Input         		| 14x14x1 gray image   							| 
| Convolution 3x3     	| 1x1 stride, same padding, outputs 1x1x400     |
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 1x1x400                  |
| Drop out              | 0.5                                           |
| Flatten               | 400                                           |
| Flatten               | 400                                           |
| Concatonation of Flat | 800                                           |
| Fully connected		| shape (800, 400)                              |
| RELU                  |                                               |
| Dropout               | 0.5                                           |
| Fully connected		| shape (400, 200)                              |
| RELU                  |                                               |
| Dropout               | 0.5                                           |
| Fully connected		| shape (200, 43)                               |
| Matmul                |                                               |
|                       |												|


####3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

My parameters are:
rate = 0.001
epochs = 30
batch_size = 128
mean = 0
stddev = 0.1
keep_prob = 0.5

####4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of .954
* validation set accuracy of .948
* test set accuracy of ?

If an iterative approach was chosen:
* What was the first architecture that was tried and why was it chosen?
I followed LeNet as my primary architecture.

* What were some problems with the initial architecture?
Low accuracy, valid was giving me NaN given a typo though.

* How was the architecture adjusted and why was it adjusted? Typical adjustments could include choosing a different model architecture, adding or taking away layers (pooling, dropout, convolution, etc), using an activation function or changing the activation function. One common justification for adjusting an architecture would be due to overfitting or underfitting. A high accuracy on the training set but low accuracy on the validation set indicates over fitting; a low accuracy on both sets indicates under fitting.
I added a 3rd convolutional layer, flatten the last 2 convolutions and concatonated them, and made 3 dully connected layers. In each layer I also added drop out so I could prevent overfitting.

* Which parameters were tuned? How were they adjusted and why?
I added drop out and changed the epochs because I could see that my model could train more after 10 layers.

* What are some of the important design choices and why were they chosen? For example, why might a convolution layer work well with this problem? How might a dropout layer help with creating a successful model?
Making the model longer and making the data pass through more layers, permitted it to have a better accuracy. However, this slowed the training time and I had some overfitting which I fixed with drop out.

If a well known architecture was chosen:
* What architecture was chosen?
* Why did you believe it would be relevant to the traffic sign application?
* How does the final model's accuracy on the training, validation and test set provide evidence that the model is working well?
 

###Test a Model on New Images

####1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

The following folder has all the pictures: https://github.com/CJL89/Traffic-Sign-Classifier/tree/master/personal-traffic-signs-data

I think none of the pictures were difficult for the model to learn so I was expecting a high accuracy to begin with.

####2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| Bycicles      		| Stop sign   									| 
| Bumpy Road  			| U-turn 										|
| Double Curve			| Yield											|
| Right Lane Ends  		| Bumpy Road					 				|
| Stop                  | Slippery Road      							|
| Straight Ahea         | Slippery Road      							|


The model was able to correctly guess 4 of the 5 traffic signs, which gives an accuracy of 80%. This compares favorably to the accuracy on the test set of ...

####3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the 11th cell of the Ipython notebook.

For the first image, the model is relatively sure that this is a stop sign (probability of 0.6), and the image does contain a stop sign. The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| .60         			| Stop sign   									| 
| .20     				| U-turn 										|
| .05					| Yield											|
| .04	      			| Bumpy Road					 				|
| .01				    | Slippery Road      							|


For the second image ... 

### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
####1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?


