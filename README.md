# keras-frcnn
Keras implementation of [Faster R-CNN with region proposal networks](https://github.com/yhenon/keras-frcnn/)

Tools and default dataset used:
- Theano
- TensorFlow
- Cuda
- OpenCV
- Keras

Data sets
- Pascal VOC dataset (images and annotations for bounding boxes around the classified objects) [can be obtained here](http://host.robots.ox.ac.uk/pascal/VOC/voc2012/VOCtrainval_11-May-2012.tar).
- Cervical images. This data set still needs to be annotated to accomodate this coding framework. As it will be described below, this annotation can be xml or plain text. 

Running codes:
- the file `train_frcnn.py` can be used to train a model. 
- `python train_frcnn.py -p /path/to/pascalvoc/` can be used to train a model on the Pascal VOC dataset. 
- the simple_parser.py provides an alternative way to input data, using a text file. 
    - to use a file parser, format an input config file (e.g. text file) in the following:

    `filepath,x1,y1,x2,y2,class_name`

    - for example:

    /data/imgs/img_001.jpg,563,54,944,554, advanced
    /data/imgs/img_002.jpg,443,203,590,600, normal
    /data/imgs/img_003.jpg,3,20,340,410, normal

    - to use the simple parser, add the following tag to the command line: `-o simple`. 
    - for example if training_data.txt is the input config file, then run `python train_frcnn.py -o simple -p training_data.txt`.

- running `train_frcnn.py` will write weights to disk to an hdf5 file, as well as all the setting of the training run to a `pickle` file. 
- these settings can then be loaded by `test_frcnn.py` for any testing.

- test_frcnn.py can be used to perform inference, given pretrained weights and a config file. Specify a path to the folder containing
images:
    `python test_frcnn.py -p /path/to/test_data/`
- Data augmentation can be applied by specifying `--hf` for horizontal flips, `--vf` for vertical flips and `--rot` for 90 degree rotations



NOTES:
- config.py contains all settings for the train or test run. The default settings match those in the original Faster-RCNN
paper. The anchor box sizes are [128, 256, 512] and the ratios are [1:1, 1:2, 2:1].
- The theano backend by default uses a 7x7 pooling region, instead of 14x14 as in the frcnn paper. This cuts down compiling time slightly.
- The tensorflow backend performs a resize on the pooling region, instead of max pooling. This is much more efficient and has little impact on results.


Example output:

![ex1](http://i.imgur.com/7Lmb2RC.png)
![ex2](http://i.imgur.com/h58kCIV.png)
![ex3](http://i.imgur.com/EbvGBaG.png)
![ex4](http://i.imgur.com/i5UAgLb.png)

ISSUES:

- If you get this error:
`ValueError: There is a negative shape in the graph!`    
    than update keras to the newest version

- This repo was developed using `python2`. `python3` should work thanks to the contribution of a number of users.

- If you run out of memory, try reducing the number of ROIs that are processed simultaneously. Try passing a lower `-n` to `train_frcnn.py`. Alternatively, try reducing the image size from the default value of 600 (this setting is found in `config.py`.
