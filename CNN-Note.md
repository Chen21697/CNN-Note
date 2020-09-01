## Why CNN for image?
We can always apply regular fully connected DNN on images, however the problem is **it requires way to much parameters**

Hence, what CNN does is **simplifying the structure of DNN**. That is to say, we **filter a lot of unused parameters at the beginning based on our prior knowledge of image processing**.

However, this ensues another problem **why can we discarding parameters??**

## Why can we discard some parameters?
There are three main reasons:

1. ### Some patterns are much smaller than the whole image

   The use of neurons in first hidden layer is to detect whether there's some kind of pattern in the input images. Since patterns are generally smaller than the whole image, for example the beak of the bird, it doesn't have to scan the whole image to find out whether the pattern exists but just looking a small part of the image(red square part).

   <center><img src="https://github.com/Chen21697/CNN-Note/blob/master/Image/CNN1.png" width="60%"></center>

   Therefore, each neuron can actually be connected to a specific region of images not the whole image pixels.

2. ### The same patterns appear in different regions

   Same pattern might be located in different regions for each image, therefore, we can just use a single detector to detect these patterns. For example, these two birds both have beak but in different regions, but we don't have to train two different detector for each image. What we can do is requiring detector that has the similar function to share the same parameters so that the parameters of the model decreases.

   <center><img src="https://github.com/Chen21697/CNN-Note/blob/master/Image/CNN2.png" width="60%"></center>

3. ### Subsampling the pixels will not change the object

   For humanity, the interpretation of images will not be influenced even though we shrink the size of images, for example: remove the pixels of odd columns and even rows.

   <center><img src="https://github.com/Chen21697/CNN-Note/blob/master/Image/CNN3.png" width="60%"></center>

   Therefore, we can use the subsampling concept to shirk the image, reducing the amount of parameters in model.

## The whole CNN structure

The basic structure of CNN is following:
**Image -> Convolution -> Max Pooling -> Convolution -> Max Pooling -> Flatten -> Fully Connected Network**
> You can always decide how many Convolution and Max Pooling you wanna do

<center><img src="https://github.com/Chen21697/CNN-Note/blob/master/Image/CNN4.png" width="60%"></center>


- **The first and second property mentioned above are dealt by Convolution layer!!(important)**
- **The third property mentioned above is dealt by Max Pooling layer!!(important)**

<center><img src="https://github.com/Chen21697/CNN-Note/blob/master/Image/CNN5.png" width="60%"></center>

### Convolution

#### Property 1
Firstly, assume the input is a  6*6 image, therefore we only have to use single value to represent a pixel.

Secondly, there are several filter in convolution layer, notice that **a filter is equal to a neuron in fully connected layer**

<center><img src="https://github.com/Chen21697/CNN-Note/blob/master/Image/CNN6.png" width="60%"></center>

- Each filter is a matrix, and the value inside this matrix is same as the weight and bias of neurons. This means **these value will be leaned during the training phase**
- If it is a 3\*3 filter, it means the filter will only detect the pattern from a 3\*3 pixel in a image instead of detecting the whole image. **This fulfills the property 1**

#### Property 2
The filter start detecting the pattern from the upper left corner of the image, then do a slide window until it reaches the bottom right corner.

<center><img src="https://github.com/Chen21697/CNN-Note/blob/master/Image/CNN7.png" width="60%"></center>

As can be seen in the slide, filter 1 detects whether there's any 1,1,1 pattern(from upper left to bottom right, I don't know how to describe....) in the image.

This patterns appears twice in the whole image(see the slide), and therefore since filter scan the whole image gradually, we don't have to use another filter. **This fulfills the property 2**

#### Feature Map

There are dozen of filter in one convolution layer, **each filter has different parameters inside but the way of doing convolution is the same**.

We will get an new matrix after doing a convolution for each filter, for example: we get the orange matrix for filter 1, blue matrix for filter 2. **Combining all of these output matrix then we get a Feature Map**.

<center><img src="https://github.com/Chen21697/CNN-Note/blob/master/Image/CNN8.png" width="60%"></center>

Example above is black-and-white image. If the image is colorful which is combined by RGB then **the filter will no longer just a matrix but a cubic, for example 3\*3\*3 cubic in the following example**.

<center><img src="https://github.com/Chen21697/CNN-Note/blob/master/Image/CNN9.png" width="60%"></center>

**Each filter considers all three channel when it is doing the convolution(RGB), therefore, the output for each filter will also has three channels**.

#### Max Pooling
What max pooling does is subsampling. If we get a 4\*4 matrix from previous convolution layer, we then group the matrix by 4 parameters and then either select the maximum or calculate the average in each group.

<center><img src="https://github.com/Chen21697/CNN-Note/blob/master/Image/CNN10.png" width="60%"></center>

>Subsampling can make your image even smaller.

## Correlation between Convolution and Fully Connected
- **Filter is just a special neuron**
- **Convolution is just removing some parameters from fully connected layer**.

For example, feature map is same as the output of neurons in the hidden layer (the green squares).

<center><img src="https://github.com/Chen21697/CNN-Note/blob/master/Image/CNN11.png" width="60%"></center>

When we are doing the convolution, we start from putting the filter at the upper left corner of image then do the inner product. Eventually, we will get an output 3. **This process is same as flatten the 6\*6 image and deeming it as the input vector**.
Then assume you have a red neuron that connected to this input vector. So the output of this neuron is 3.

<center><img src="https://github.com/Chen21697/CNN-Note/blob/master/Image/CNN12.png" width="60%"></center>

If you follow the colorful line, we will find out that we only use 9 pixels as our input not 36 pixels(whole image) since we mentioned that we don't have to use the whole image for finding a pattern. Therefor that's why we can use less parameters.

> **Each neuron only detects a small region of the whole image!!(important)**

Then when our stride is 1, we move to the right and the inner product we get will be -1 right now. Again, if you follow the colorful line, you'll find out the input of -1 neuron is different.

In the fully connected layer, these two neuron are basically doing different thing because each has its own weight. **However, when we are doing the convolution, we force these two neurons to share the same weight. Although these two are connected to different input, but there weight is the same.** This is called weight share.

When we are doing this, the parameter is even lesser.

<center><img src="https://github.com/Chen21697/CNN-Note/blob/master/Image/CNN13.png" width="60%"></center>
