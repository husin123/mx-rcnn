# Faster R-CNN in MXNet

![example detections](https://cloud.githubusercontent.com/assets/13162287/22101032/92085dc0-de6c-11e6-9228-67e72606ddbc.png)

### Set up environment
* Require latest MXNet. Set environment variable by `export MXNET_CUDNN_AUTOTUNE_DEFAULT=0`.
* Install Python package `mxnet` or `mxnet-cu90`, `cython` then `opencv-python matplotlib pycocotools tqdm`.

### Out-of-box inference models
Download any of the following models and run `python3 sym_demo.py  --image $YOUR_IMAGE$ --params $MODEL_FILE$ --network $Network$ --dataset $Dataset$` to get single image inference.
For example `python3 sym_demo.py --image myimage.jpg --params vgg16_voc0712.params --network vgg16 --dataset voc`, add `--gpu 0` to use GPU optionally.
Different network has different configuration. Different dataset has different object class names. You must pass them explicitly as command line arguments.

| Network | Dataset | Imageset | Reference | Result | Link  |
| :------ | :------------ | :----------- | :-------: | :----: | :---: |
| vgg16 | voc | 07/07 | 69.9 | 70.23 | [Dropbox](https://www.dropbox.com/s/gfxnf1qzzc0lzw2/vgg_voc07-0010.params?dl=0) |
| vgg16 | voc | 07++12/07 | 73.2 | 75.97 | [Dropbox](https://www.dropbox.com/s/rvktx65s48cuyb9/vgg_voc0712-0010.params?dl=0) |
| resnet101 | voc | 07++12/07 | 76.4 | 79.35 | [Dropbox](https://www.dropbox.com/s/ge2wl0tn47xezdf/resnet_voc0712-0010.params?dl=0) |
| vgg16 | coco | train2017/val2017 | 21.2 | 22.8 | [Dropbox](https://www.dropbox.com/s/e0ivvrc4pku3vj7/vgg_coco-0010.params?dl=0) |
| resnet101 | coco | train2017/val2017 | 27.2 | 26.1 | [Dropbox](https://www.dropbox.com/s/bfuy2uo1q1nwqjr/resnet_coco-0010.params?dl=0) |

### Download data and label
Follow `py-faster-rcnn` for data preparation instructions.
* [Pascal VOC](http://host.robots.ox.ac.uk/pascal/VOC/) should be in `data/VOCdevkit` containing `VOC2007`, `VOC2012` and `annotations`.
* [MSCOCO](http://mscoco.org/dataset/) should be in `data/coco` containing `train2017`, `val2017` and `annotations/instances_train2017.json`, `annotations/instances_val2017.json`.

### Download pretrained ImageNet models
* [VGG16](http://www.robots.ox.ac.uk/~vgg/research/very_deep/) should be at `model/vgg16-0000.params` from [MXNet model zoo](http://data.dmlc.ml/models/imagenet/vgg/).
* [ResNet](https://github.com/tornadomeet/ResNet) should be at `model/resnet-101-0000.params` from [MXNet model zoo](http://data.dmlc.ml/models/imagenet/resnet/).

### Training and evaluation
Use `python3 sym_train.py --pretrained $IMAGENET_MODEL_FILE$ --network $Network$ --dataset $Dataset$ --gpus $GPUS$` to train,
for example, `python3 sym_train.py --pretrained model/vgg16-0000.params --network vgg16 --dataset voc --gpus 0,1$`.
Use `python3 sym_test.py --params $MODEL_FILE$ --network $Network --dataset $Dataset$ --gpu $GPU$` to evaluate,
for example, `python3 sym_test.py --params model/vgg16-0010.params --network vgg16 --dataset voc --gpu 0$`.

### History
* May 25, 2016: We released Fast R-CNN implementation.
* July 6, 2016: We released Faster R-CNN implementation.
* July 23, 2016: We updated to MXNet module solver.
* Oct 10, 2016: tornadomeet released approximate end-to-end training.
* Oct 30, 2016: We updated to MXNet module inference.
* Jan 19, 2017: We accelerated our pipeline and supported ResNet training.
* Jun 22, 2018: We simplified code. 

### Disclaimer
This repository used code from [MXNet](https://github.com/dmlc/mxnet),
[Fast R-CNN](https://github.com/rbgirshick/fast-rcnn),
[Faster R-CNN](https://github.com/rbgirshick/py-faster-rcnn),
[caffe](https://github.com/BVLC/caffe),
[tornadomeet/mx-rcnn](https://github.com/tornadomeet/mx-rcnn),
[MS COCO API](https://github.com/pdollar/coco).  
Thanks to tornadomeet for end-to-end experiments and MXNet contributers for helpful discussions.

### References
1. Tianqi Chen, Mu Li, Yutian Li, Min Lin, Naiyan Wang, Minjie Wang, Tianjun Xiao, Bing Xu, Chiyuan Zhang, and Zheng Zhang. MXNet: A Flexible and Efficient Machine Learning Library for Heterogeneous Distributed Systems. In Neural Information Processing Systems, Workshop on Machine Learning Systems, 2015
2. Ross Girshick. "Fast R-CNN." In Proceedings of the IEEE International Conference on Computer Vision, 2015.
3. Shaoqing Ren, Kaiming He, Ross Girshick, and Jian Sun. "Faster R-CNN: Towards real-time object detection with region proposal networks." In IEEE Transactions on Pattern Analysis and Machine Intelligence, 2016.
4. Yangqing Jia, Evan Shelhamer, Jeff Donahue, Sergey Karayev, Jonathan Long, Ross Girshick, Sergio Guadarrama, and Trevor Darrell. "Caffe: Convolutional architecture for fast feature embedding." In Proceedings of the ACM International Conference on Multimedia, 2014.
5. Mark Everingham, Luc Van Gool, Christopher KI Williams, John Winn, and Andrew Zisserman. "The pascal visual object classes (voc) challenge." International journal of computer vision 88, no. 2 (2010): 303-338.
6. Jia Deng, Wei Dong, Richard Socher, Li-Jia Li, Kai Li, and Li Fei-Fei. "ImageNet: A large-scale hierarchical image database." In Computer Vision and Pattern Recognition, IEEE Conference on, 2009.
7. Karen Simonyan, and Andrew Zisserman. "Very deep convolutional networks for large-scale image recognition." arXiv preprint arXiv:1409.1556 (2014).
8. Kaiming He, Xiangyu Zhang, Shaoqing Ren, Jian Sun. "Deep Residual Learning for Image Recognition". In Computer Vision and Pattern Recognition, IEEE Conference on, 2016.
9. Tsung-Yi Lin, Michael Maire, Serge Belongie, James Hays, Pietro Perona, Deva Ramanan, Piotr Dollár, and C. Lawrence Zitnick. "Microsoft COCO: Common Objects in Context" In European Conference on Computer Vision, pp. 740-755. Springer International Publishing, 2014.
