# **Project: Build a Traffic Sign Recognition Program** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<p align="center"><img src="examples/title.png" width="480" alt="Combined Image"></p>

Overview
---
In this project we are going to use convolutional neural networks(CNN) to classify [German Traffic Sign Dataset](http://benchmark.ini.rub.de/?section=gtsrb&subsection=dataset). There are 43 different traffic signs in the dataset. CNN model is trained with 30 total number of epochs on a training set of 34,799 sample images, model is evaluated at the end of each epoch on 4410 validation images. In order to see how does the model performs, it will be tested on 12,630 new images that model have not seen before. In the following sections we are going to describe the pipeline in more details.


Pipeline
---
*The pipeline along with the corresponding python function or helper function is described. For more details please refer to the code Traffic_Sign_Classifier.ipynb*


<br>

I. Reading input images. Training, validation, and test sets are already given as .p files. Python `pickle` package is used in order to load them. The data summarization stats shows that data has the following stats. Also each images has the size of (32, 32, 3), which means 32 pixels width, 32 pixels height, and 3 RGB channels.
</br>
<br>
Number of training images = 34799
</br>
Number of validation images = 4410
<br>
Number of testing images = 12630
</br>
<br></br>
II. Statistics of traffic signs and number of times that each traffic sign occurs. In the following plot we showed these stats for three cases of training, validation, and test sets.
<br></br>
<p align="center"><img src="examples/visualize_data.png" alt="Combined Image"></p>
<br></br>
III. Plotting one image from each category with the corresponding label at the top of it.
<br></br>
<p align="center"><img src="examples/title.png" alt="Combined Image"></p>
<br></br>
IV. Image Augmetation: In order to increase the model accuracy in diffent noisy situations we used data aumentatoin techniques. By getting help from <a href="https://github.com/vxy10/ImageAugmentation">this</a>, we increased the size of training data by adding aumented data to the training set. The augmentation techniques that were use include rotation, translation, shear, and brightness. Without data aumentation the model accuracy on the test set is so bad (bellow 90) that we avoid reporting them in our final results. Data augmentation is just applied to the training set and it doubled the size of training set by adding mentioned noises to each training image and stack it to the training set. ```augment_brightness_camera_images``` and ```transform_image```.
<br></br>
V. Image normalization: Each RGB image is changed into grayscale image and the value of each pixel is normalized by (pixel_value - 128)/ 128. ```normalize```
<br></br>
VI. Model architecture: The model architecture is based on <a href="http://yann.lecun.com/exdb/publis/pdf/sermanet-ijcnn-11.pdf">this</a> publication by Pierre Sermanet and Yann Lecun. The following image which is from the paper shows the over block diagram of the CNN used in this project. The network consists of the following layers: ```conv2d```, ```maxpool2d```, ```conv_net```, ```LeNet```.
<br>
</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. 2D convlution layer with 12 windows of size 5x5 and strides of 1. 
<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. Relu.
</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3. Maxpooling with 2X2 window and stride of 2.
<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4. 2D convlution layer with 32 windows of size 5x5 and strides of 1. 
</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5. Maxpooling with 2X2 window and stride of 2.
<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6. Relu.
</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7. 2D convlution layer with 50 windows of size 3x3 and strides of 1. 
<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8. Relu.
</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;9. 2D convlution layer with 400 windows of size 1x1 and strides of 1.
<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;10. Relu.
</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;11. Flattening steps 9 and 11 and concatenating them together.
<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;12. Fully connected layer with 400 hidden units.
</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;13. Relu.
<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;14. Fully connected layer with 100 hidden units.
</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;15. Relu.
<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;15. Output layer with 43 units.
</br>

<br></br>
<p align="center"><img src="examples/CNN.png" alt="Combined Image" ></p>
<br></br>
VII. Tensorflow was used to implement the aforementioned CNN network. The hyper parameters that were chosen are:
<br>
Learning Rate = 0.001
</br>
EPOCHS = 30
<br>
Batch Size = 128
</br>
<br></br>
VIII. After running the training data through the network for 30 epoch, the following validation accuracy achieved.
<br></br>
<br>
<p align="center"><img src="examples/Valid_accuracy.png" width = "250" height="600" align="middle"></p>
</br>
IX. Testing the saved model on the data lead to accuracy of *94.3* percent
<br></br>
X. The first model that I chose was similar to LeNet5, a neural network architecture with two convolutional layer and three fully connected layer. The model accuracy went as high as around 90 percent for the test data. The next model that I tried was based on <a href="http://yann.lecun.com/exdb/publis/pdf/sermanet-ijcnn-11.pdf">this</a> paper, the model accuracy was again around 90 percent for the test data. However I added one more convlolutional layer with window size of 1 and 1 more fully connected layer which increased the model accuracy on the test data up to 94.3 percent.
<br></br>
XI. Next, the model is tested on some images that are downloaded from internet as shown in the following image. The images were chosen from Google search. They of different sizes but their size has been reduced in order to fit the network requirement of 32x32. The images were bright enough at least for human eyes but they have something going on in the back ground that makes it hard for the model to classify them. Given the kind of the data that we had from training set it is not so much difficult for the model to classify them. It can be also observed from the softmax probabilities that is shown in one of the following figures. The reason for this is, images are pretty clear and boundaries are not that much hard for the model to capture. One of the sign that was hard for the model to classify was *ahead only* because on some of the runs the model will predict it as turning right which has similar sign.
<br>
<p align="center"><img src="examples/othersigns.png" alt="Combined Image" >	</p>
</br>
<br></br>
XII. The model prediction and actual labels are shown in the following figure. Also a comparison between model 
accuracy on the 6 new images and test data have been provided here.
<br>
<p align="center"><img src="examples/Label_pred_act.png" width = "350" alt="Combined Image" >	</p>
<p align="center"><img src="examples/accu_test_new.png" width = "350" alt="Combined Image" >	</p>
</br>
<br></br>
XIII. Top 5 softmax probabilities and the next most probable labels that the model predicted are as follows.
<br>
<p align="center"><img src="examples/Labels.png" alt="Combined Image" >	</p>
</br>
<br></br>
XIX. Visualize the Neural Network's State with Test Images: The first conv layer of the stop sign is shown in the following figure. As it can be seen, it focuses mostly on edges and lower level fitures such as alphabets and shapes of them.
<br>
<p align="center"><img src="examples/ConvLayer_StopSign.png" alt="Combined Image" >	</p>
</br>
<br></br>
