#Convolutional Neural Network

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

To detect faces in a picture, we used a slippery window of 36\*36 that we applied on each part of the image (with a step of 5px),
and extracted this 36\*36 image to check if is a face or not, thanks to our CNN. Moreover, each time the slippery window
end the image, we resize it with a factor 0.9, in order to detect bigger faces than 36*36.
This process worked quite good actually :


<img src="https://raw.githubusercontent.com/Romathonat/vulgaireDevEntries/master/CNNarticle/iteration1.jpg" width="600">

As you can see, most of faces are detected, but some non-faces are also detected. In addition, we had many detections, and our goal was to detect faces with just a single square or a single circle per face !

##Second iteration
To solve the many squares problem, we used a clustering algorithm : DBSCAN. You can read more about it [here](https://en.wikipedia.org/wiki/DBSCAN). In short, it detects highest densities. Here we detected highest densities of 
square, and placed a circle on it.

![](https://raw.githubusercontent.com/Romathonat/vulgaireDevEntries/master/CNNarticle/cluster.jpg)

With correct parameters, it was pretty good !

##Third iteration
We wanted to increase our detection precision. First we tried to change our architecture, but it was not very concluding. 

Then, we tried to add a bootstraping step in the creation of the CNN. The principle is as follow:

1. We create a big set of images of non faces : 36\*36 extraction of textures images
2. We train our CNN like before.
3. We make it work on textures we extracted on the step 1, and select images that are detected as faces with a threshold of 0.9. In fact, we choose images where our CNN makes big mistakes.
4. We train our CNN again, but now we add the non-faces from the previous step to the training set. We take caution to have as many faces as non-faces in the training set.
5. We lower the threshold.
6. We go back to step 3, until the threshold value is 0

The advantage of bootstraping is that we train the CNN to better separate faces from non-faces. The fact that we first take into account big mistakes makes the CNN more performant, just like when you teach to a children, you first make him correct his big mistakes, not subtles errors.

With this approach, we manages to reach a precision of 93.3% !

Here are some results :

![](https://raw.githubusercontent.com/Romathonat/vulgaireDevEntries/master/CNNarticle/iteration3.jpg)

It does not seems so much better on this image than in the one from iteration 2, but trust me the detection is way better on other images.

![](https://raw.githubusercontent.com/Romathonat/vulgaireDevEntries/master/CNNarticle/twd.jpg_with_clusters_on_source.jpg)
 	
Here we can se that Daryl (on the right) is not detected because his face is a little turn away from the camera, and our training only included faces "in front" of the camera. In the same way, I think black faces may be more difficult to detect for our CNN because there was less black faces in our training set.

![](https://raw.githubusercontent.com/Romathonat/vulgaireDevEntries/master/CNNarticle/irlande.jpg_with_clusters_on_source.jpg)

##Improvement axes
We could keep only one circle when there are multiple on a the same location. A more important and reprensentative set of faces could also be a good improvement. Finally, we did not pay attention to performances, we could optimize the process because actually, it takes something like 30 seconds to detect faces in an image, depending on it size.

Authors: 
- BASEILHAC Theo
- CACHARD Côme
- MATHONAT Romain
- NATIVEL Nicolas
- NOUVELLET Victor
