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

Therefore, **each filter considers all three channel once(RGB)**
