
#Introduction
One the projects I am currently working on is about machine learning, more precisely about neural network.
The aim is to train a neural network to detect faces on an image. To do so, we used caffe, a well-known framework for Convolutional Neural Network (CNN).
In short, a CNN is a neural network processing on images. The main problem with caffe is its documentation, so I'd like to explain how
to use it, as simply as I can.

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
cd caffe
docker build -t caffe:cpu standalone/cpu
```
This step installed all needed dependencies to make caffe work.

Now, we will launch the docker container, with a shell access. Moreover, we mount a volume (/datas into the docker container) of datas from our computer to be able to access it from docker.

```bash
docker run -ti -v /path/to/folder/caffe_datas/:/datas caffe:cpu bash
```
Important Note: When you are in the docker container, caffe is installed in /opt/caffe/


##Convert your training images to the right format
To create a CNN, you need to give it datas so that it can learn. You need to have many images, divided in two categories here: faces and non-faces. Caffe needs a specific format to be trained : LMDB (default) or LevelDB.
To convert you images to LMDB, you can use a tool included in caffe :

```bash
/opt/caffe/build/tools/convert_imageset --shuffle --gray train_images/ posneg.txt train_lmdb
```
train_images is the folder when you have you training images. posneg.txt is a file with a line for each image, containing the path to the image relative to train_images, and the corresponding class (face or non-face), it looks like this :

```
Image000605.pgm 1
Image000607.pgm 1
Image000608.pgm 1
```

Finally, train_lmdb is the output, used in the next steps to train the CNN.

##Train the CNN
To train the CNN, you will need to specify two files : 
- a config file : number of iterations, the learning rate ...
- a file describing the architecture of the CNN (number of layers etc).

Take a look [here](https://github.com/BVLC/caffe/tree/master/models/bvlc_googlenet). The solver.prototxt is the config file, and the train_val.prototxt is the description of the architecture of the CNN. As you can see, the solver.prototxt contains the train_val.prototxt.

To train the CNN, you will make :

```bash
caffe train -solver solver.prototxt
```

This will generate two files. A .caffemodel, ready for deployment, and a .solverstate, used to keep doing modifications to the CNN later.






