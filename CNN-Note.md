### Why CNN for image?
==**We can always apply regular fully connected DNN on images, however the problem is it requires way to much parameters**==

Hence, what CNN does is **simplifying the structure of DNN**. That is to say, we **filter a lot of unused parameters at the beginning based on our prior knowledge of image processing**.

However, this ensues another problem **why can we discarding parameters??**

### Why can we discard some parameters?
There are three main reasons:

**1. Some patterns are much smaller than the whole image**<br>
The use of neurons in first hidden layer is to detect whether there's some kind of pattens in the input images. Since some patterns are smaller than the whole image, for example the beak of the bird, it doesn't have to scan the whole image to find out whether the pattern exists but just looking a small part of the image(red square part).

<center><img src="https://github.com/Chen21697/CNN-Note/blob/master/Image/CNN1.png" width="60%"></center>

Therefore, each neuron can actually be connected to a specific region of images not the whole image pixels.

**2. The same patterns appear in different regions**<br>
Same pattern might be located in different regions for each image, therefore, we can just use a single detector to detect these pattens. For example, these two birds both have beak but in different regions, but we don't have to train two different detector for each image. What we can do is requiring detector that has the similar function to share the same parameters so that the parameters of the model decreases.

<center><img src="https://github.com/Chen21697/CNN-Note/blob/master/Image/CNN2.png" width="60%"></center>

**3. Subsampling the pixels will not change the object**<br>

For humanity, the interpretation of images will not be influenced even though we shrink the size of images, for example: remove the pixels of odd columns and even rows.

<center><img src="https://github.com/Chen21697/CNN-Note/blob/master/Image/CNN3.png" width="60%"></center>

Therefore, we can use the subsampling concept to shirk the image, reducing the amount of parameters in model.
