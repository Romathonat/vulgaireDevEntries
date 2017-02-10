
#Introduction
One the projects I am currently working on is about machine learning, more precisely about neural network.
The aim is to train a neural network to detect faces on an image. To do so, we used caffe, a well-known framework for Convolutional Neural Network (CNN).
In short, a CNN is a neural network processing on images. The main problem with caffe is its documentation, so I'd like to explain how to use it, as simply as I can.

#Tutorial
##Install caffe
The installation of caffe can be quite difficult. Some people use a Virtual Machine, I prefer using docker: it is lighter, and I had some problems with VM and caffe.
Of course, you need docker installed [https://docs.docker.com/engine/installation/](https://docs.docker.com/engine/installation/)

First, you need to download caffe.

```bash
git clone https://github.com/BVLC/caffe.git
```

Then, we build the docker image (it will take some time):

```bash
cd caffe/docker
docker build -t caffe:cpu standalone/cpu
```
This step installed all needed dependencies to make caffe work.

Now, we will launch the docker container, with a shell access. Moreover, we mount a volume (/datas into the docker container) of datas from our computer to be able to access it from docker.

```bash
docker run -ti -v /path/to/folder/conf_and_images_for_caffe/:/datas caffe:cpu bash
```
**Important**: When you are in the docker container, caffe is installed in /opt/caffe/


##Convert your training images to the right format
To create a CNN, you need to give it datas so that it can learn. You need to have many images, divided in two categories here: faces and non-faces. Caffe needs a specific format to be trained : LMDB (default) or LevelDB.
To convert you images to LMDB, you can use a tool included in caffe :

```bash
cd /datas
/opt/caffe/build/tools/convert_imageset --shuffle --gray train_images/ posneg.txt train_lmdb
```
*train_images* is the folder when you have your training images. posneg.txt is a file with a line for each image, containing the path to the image relative to *train_images*, and the corresponding class (face or non-face), it looks like this :

```
Image000605.pgm 1
Image000607.pgm 1
Image000608.pgm 0
```

Finally, train_lmdb is the output, used in the next steps to train the CNN.

##Train the CNN
To train the CNN, you will need to specify two files : 

1. a config file : number of iterations, the learning rate ...
2. a file describing the architecture of the CNN (number of layers etc).

Take a look [here](https://github.com/BVLC/caffe/tree/master/models/bvlc_googlenet). The solver.prototxt is the config file, and the *train_val.prototxt* is the description of the architecture of the CNN. As you can see, the solver.prototxt contains the *train_val.prototxt*.

Last thing before training your CNN, you need to specify the path to your *train_lmdb* (that you generated with *convert_imageset*) in the *train_val.protoxt*. Open it, find the TRAIN layer, and replace the source field, in *data_param*, by the path to your *train_lmdb*.

To train the CNN, you will do :

```bash
caffe train -solver solver.prototxt
```

This will generate two files. A .caffemodel, ready for deployment, and a .solverstate, used to keep doing modifications to the CNN later. 
To deploy, I recommand making a deploy.protoxt file, like [here](https://github.com/BVLC/caffe/blob/master/models/bvlc_googlenet/deploy.prototxt). It is the same as *train_val.prototxt*, without a data layer instead of TRAIN and TEST layers, and an ending layer specifying that we want a classification as the output:

```
layer {
  name: "prob"
  type: "Softmax"
  bottom: "fc8"
  top: "prob"
}
```
The bottom field is the name of the last layer of your CNN.


##Deploy to production with python

Now that we have a working CNN, we want to use it with pycaffe, the python interface for caffe. It is not very well documented, but it works.

The basic usage you have of a CNN is to use it with an image to determine the class of this image, that's what we are going to do.
Create a cnn.py file into the volume you share with docker, and add a code like this :

```python

import caffe
import numpy as np
from PIL import Image

#we choose the mode (cpu or gpu)
caffe.set_mode_cpu()

#we use our CNN. the first argument is the architecture of our CNN, the second one are the weights
net = caffe.Net('/datas/deploy.prototxt', '/datas/facenet_iter_200000.caffemodel', caffe.TEST)

#we create a transformer that resize the shape of our datas to give it to the CNN
#here my image is read as width*height*colorchannel (1 here because I work in black and white)
transformer = caffe.io.Transformer({'data': net.blobs['data'].data.shape})
transformer.set_transpose('data', (2,0,1))  # (36,36,1) -> (1,36,36), seen as (1,1,36,36) to the network
 
#we load our image, and we preprocess it                          
im = caffe.io.load_image('/datas/train_images/1/Image000605.pgm', color=False)
image_transformed = transformer.preprocess('data', im)

#we give it as the data to our CNN
net.blobs['data'].data[...] = image_transformed

#we make the CNN work
out = net.forward()

#we get the class
print out['prob']

# result [[ 0.01239852  0.98760146]] -> the more likely class is the second one (face), so this image is a face.

#we have to change the train_val.prototxt to deploy.prototxt. Then, we remove the train and test label, and put this one 
#layer {
#  name: "data"
#  type: "Input"
 # top: "data"
 # top: "label"
 # input_param { shape: {dim: 1 dim: 1 dim: 36 dim: 36 } }
#} 
# and at the end we specify that we want a classification as the output
# layer {
#  name: "prob"
#  type: "Softmax"
#  bottom: "fc8"
#  top: "prob"
#}
```

I think it is commented enough to understand.

I hope this tutorial helped!




