##Introduction
The aim of our project was to create a convolutional neural network (CNN) to determine if a small image of 36*36 is
a face or not, with caffe (machine learning framework).
In a second time, and once the CNN was trained, we had to create a tool using it to detect faces in any image.

##Architecture
We used a simple one, with convolution, pooling, and only two neurons layers (so it is not really "deep" learning).
Here is a schema of the architecture :
![](https://raw.githubusercontent.com/Romathonat/vulgaireDevEntries/master/CNNarticle/architecture.png)

##First iteration
On our first iteration, testing our CNN on test images (~7600 images) gave us a precision of ~84%, depending on our luck
because the training of the CNN is not deterministic. It was decent. 

To detect faces in a picture, we used a slippery window of 36*36 that we applied on each part of the image (with a step of 5px),
and extracted this 36*36 image to check if is a face or not, thanks to our CNN. Moreover, each time the slippery window
end the image, we resize it with a factor 0.9, in order to detect bigger faces than 36*36.
This process worked quite good actually :
![](https://raw.githubusercontent.com/Romathonat/vulgaireDevEntries/master/CNNarticle/iteration1.jpg)
