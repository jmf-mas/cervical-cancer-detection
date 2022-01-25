# early stage of keras-frcnn for cervical cancer screening

- orginal code source can be found [here](https://github.com/kbardool/keras-frcnn)
- Keras implementation of [Faster R-CNN with region proposal networks](https://github.com/yhenon/keras-frcnn/)
- multiple changes were made to accomodate with versions of tools used here.

Tools and default dataset used:
- Python 3
- Theano
- TensorFlow
- Cuda
- OpenCV
- Keras

Data sets
- Pascal VOC dataset (images and annotations for bounding boxes around the classified objects) [can be obtained here](http://host.robots.ox.ac.uk/pascal/VOC/voc2012/VOCtrainval_11-May-2012.tar).
- Cervical images. This data set still needs to be annotated to accomodate this coding framework. As it will be described below, this annotation can be xml or plain text. 
- VoTT (Visual Object Tagging Tool) can be used for image annotations.

Running codes:
- the file `train_frcnn.py` can be used to train a model. 
- `python train_frcnn.py -p /path/to/pascalvoc/` can be used to train a model on the Pascal VOC dataset. 
- `simple_parser.py` provides an alternative way to input data, using a text file. 
    - to use a file parser, format an input config file (e.g. text file) in the following: `filepath,x1,y1,x2,y2,class_name`

    - for example:

        - `/data/imgs/img_001.jpg,563,54,944,554, advanced`
        - `/data/imgs/img_002.jpg,443,203,590,600, normal`
        - `/data/imgs/img_003.jpg,3,20,340,410, normal`

    - to use the simple parser, add the following tag to the command line: `-o simple`. 
    - for example if `training_data.txt` is the input config file, then run `python train_frcnn.py -o simple -p training_data.txt`.

- running `train_frcnn.py` will write weights to disk to an hdf5 file, as well as all the setting of the training run to a `pickle` file. 
- these settings can then be loaded by `test_frcnn.py` for any testing.

- `test_frcnn.py` can be used to perform inference, given pretrained weights and a config file. Specify a path to the folder containing
images:
    `python test_frcnn.py -p /path/to/test_data/`
- data augmentation can be applied by specifying `--hf` for horizontal flips, `--vf` for vertical flips and `--rot` for 90 degree rotations


Settings:
- `config.py` contains settings for the training and testing. 
- the anchor box sizes are [128, 256, 512] and the ratios are [1:1, 1:2, 2:1].
- the theano backend by default uses a 7x7 pooling region.

Next steps:
- Cervical cancer images annotations
- Replacement of default data set (Pascal VOC) by the cervical images.
- Predictions on new images. This will require classification task as in [the benchmark paper](https://academic.oup.com/jnci/article/111/9/923/5272614).
