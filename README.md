# Speed-Estimation-Network
The goal of this project was to develop a model to predict the speed of a vehicle given at every frame of a video from within the vehicle.

## Results
---

For preprocessing I use undersampling and oversampling to equalize the training set classes, dense optical flow to calculate movements between frames, coarse pixel dropout to augment the training data, and normalizaion to make training easier. After testing the Nvidia architecture and the Comma ai architecture, I found that a combination of the two worked well. For the combination model I used the conv layers from Comma ai, and the fully connected layers from Nvidia. That said throughout training, the combination model and the Nvidia models worked almost identically, except that the combination model trained faster. In the end I used the Nvidia model as the difference between the validaion set and the training set was smaller, implying that the model had generalized more. 

The model had a validation MSE loss of 2.0843 after being trained for 8 epochs. Given the limited data and that frames of a video right next to each other are similiar, I created the validation set using the last 2 seconds of every 10 seconds. The goal was to get a good sample of the classes (speeds) of the video while trying not to let the model overfit on back to back images. This validation set was chosen based on the restricted size of the data. After watching the trian video it felt like every part of the video was a unique situation and therefore important to training. I was able to get a lower validaion loss over 20 epochs but since the validaion set was prone to overfitting I didn't want to use a model trained past 10 epochs.


Moving forward, I see two improvements that can be made. First, it feels like when the car is stopped the speed is at a speical case, and if a model can classify that special case then the model can give an exact output of 0. Secondly, the results in test.txt are very jittery frame by frame. It's clear that the network would gain from the insights of the previous layers. Because of this I think wrapping the model within an RNN would improve the accuracy. Although relying on previous results makes the speed prediction from the last frame even more important, so having a car stopped classifier would also be more important.


| Layer         		|     Description	        												| 
|:---------------------:|:-------------------------------------------------------------------------:| 
| Input         		| 2 RGB images 																|
| Pre-processing 		| Dense Optical Flow -> Normalized -> Flip Image -> Coarse Pixel Dropout	|
| Convolution 5x5     	| 1x1 stride, valid padding, SELU, 2x2 Max Pool , 24 output layers			|
| Convolution 5x5     	| 1x1 stride, valid padding, SELU, 2x2 Max Pool , 36 output layers			|
| Convolution 5x5     	| 1x1 stride, valid padding, SELU, 2x2 Max Pool , 48 output layers			|
| Convolution 3x3     	| 1x1 stride, valid padding, SELU, 64 output layers 						|
| Convolution 3x3     	| 1x1 stride, valid padding, SELU, 64 output layers  						|
| Flatten				| 																			|
| Fully Connected 		| outputs 100 -> SELU														|
| Fully Connected 		| outputs 50 -> SELU														|
| Fully Connected 		| outputs 10 -> SELU														|
| Fully Connected 		| outputs 1																	|






