# *Faster* R-CNN: Towards Real-Time Object Detection with Region Proposal Networks

By Shaoqing Ren, Kaiming He, Ross Girshick, Jian Sun at Microsoft Research

### Introduction

Fast**er** R-CNN is an object detection framework based on deep convolutional networks, which includes a Region Proposal Network (RPN) and an Object Detection Network. Both networks are trained for sharing convolutional layers for fast testing. 

Faster R-CNN was initially described in an [arXiv tech report](http://arxiv.org/abs/1506.01497).

This repo contains a MATLAB re-implementation of Fast R-CNN. Details about Fast R-CNN are in: [rbgirshick/fast-rcnn](https://github.com/rbgirshick/fast-rcnn).

### License

Faster R-CNN is released under the MIT License (refer to the LICENSE file for details).

### Citing Faster R-CNN

If you find Faster R-CNN useful in your research, please consider citing:

    @article{ren15fasterrcnn,
        Author = {Shaoqing Ren, Kaiming He, Ross Girshick, Jian Sun},
        Title = {{Faster R-CNN}: Towards Real-Time Object Detection with Region Proposal Networks},
        Journal = {arXiv preprint arXiv:1506.01497},
        Year = {2015}
    }

### Main resutls
                          | training data                          | test data            | mAP   | time/img
------------------------- |:--------------------------------------:|:--------------------:|:-----:|:-----:
Faster RCNN, VGG-16       | VOC 2007 trainval                      | VOC 2007 test        | 69.9% | 196ms
Faster RCNN, VGG-16       | VOC 2007 trainval + 2012 trainval      | VOC 2007 test        | 73.2% | 196ms
Faster RCNN, VGG-16       | VOC 2012 trainval                      | VOC 2012 test        | 67.0% | 196ms
Faster RCNN, VGG-16       | VOC 2007 trainval&test + 2012 trainval | VOC 2012 test        | 70.4% | 196ms

The mAP results are subject to random variations, which we estimate are ~0.5% mAP.


### Contents
0. [Requirements: software](#requirements-software)
0. [Requirements: hardware](#requirements-hardware)
0. [Preparation for Testing](#preparation-for-testing)
0. [Testing Demo](#testing-demo)
0. [Preparation for Training](#preparation-for-training)
0. [Training](#training)
0. [Resources](#resources)
 
### Requirements: software

0. `Caffe` build for Faster R-CNN (included in this repository, see `external/caffe`)
    - If you are using Windows, you may download a compiled mex file by running `fetch_data/fetch_caffe_mex_windows_vs2013_cuda65.m`
    - If you are using Linux or you want to compile for Windows, please follow the [instructions](https://github.com/ShaoqingRen/caffe/tree/faster-R-CNN) on our Caffe branch.
0.	MATLAB
 
    
### Requirements: hardware

GPU: Titan, Titan Black, Titan X, K20, K40, K80.

0. Region Proposal Network (RPN)
    - 2GB GPU memory for ZF net
    - 5GB GPU memory for VGG-16 net
0. Ojbect Detection Network (Fast R-CNN)
    - 3GB GPU memory for ZF net
    - 8GB GPU memory for VGG-16 net


### Preparation for Testing:
0.	Run `fetch_data/fetch_caffe_mex_windows_vs2013_cuda65.m` to download a compiled Caffe mex (for Windows only).
0.	Run `faster_rcnn_build.m`
0.	Run `startup.m`


### Testing Demo:
0.	Run `fetch_data/fetch_faster_rcnn_final_model.m` to download our trained models.
0.	Run `experiments/script_faster_rcnn_demo.m` to test a single demo image.
    - The first run might be slower due to memory load.
    - The running time on K40 of this code is about 220ms/image, 10% more than we reported in the paper. This is because of unknown issues when we switch from our older version of Caffe to the newer one.
    - The speed on Titan X is about 2x of on K40.


### Preparation for Training:
0.	Run `fetch_data/fetch_model_ZF.m` to download an ImageNet-pre-trained ZF net.
0.	Run `fetch_data/fetch_model_VGG16.m` to download an ImageNet-pre-trained VGG-16 net.
0.	Download VOC 2007 and 2012 data to ./datasets


### Training:
0. Run `experiments/script_faster_rcnn_VOC2007_ZF.m` to train a model with ZF net. It runs four steps as follows:
    - Train RPN with conv layers tuned; compute RPN results on the train/test sets.
    - Train Fast R-CNN with conv layers tuned using step-1 RPN proposals; evaluate detection mAP.
    - Train RPN with conv layers fixed; compute RPN results on the train/test sets. 
    - Train Fast R-CNN with conv layers fixed using step-3 RPN proposals; evaluate detection mAP.
    - **Note**: the entire training time is ~12 hours on K40.
0. Run `experiments/script_faster_rcnn_VOC2007_VGG16.m` to train a model with VGG net.
    - **Note**: the entire training time is ~2 days on K40.
0. Check other scripts in `./experiments` for more settings.

### Resources

0. Experiment logs: [OneDrive](https://onedrive.live.com/download?resid=4006CBB8476FF777!17290&authkey=!AGhH4z667tHYYEw&ithint=file%2czip), [DropBox](https://www.dropbox.com/s/wu841r7zmebjp6r/faster_rcnn_logs.zip?dl=0), [BaiduYun](http://pan.baidu.com/s/1nt48EJB)
0. Regions proposals of our trained RPN:
    - ZF net trained on VOC 07 trainval [To Add]
    - ZF net trained on VOC 07/12 trainval [To Add]
    - VGG net trained on VOC 07 trainval [To Add]
    - VGG net trained on VOC 07/12 trainval [To Add]

If the automatic "fetch_data" fails, you may manually download resouces from:

0. Pre-complied caffe mex:
    - Windows-based mex complied with VS2013 and Cuda6.5: [OneDrive](https://onedrive.live.com/download?resid=4006CBB8476FF777!17255&authkey=!AHOIeRzQKCYXD3U&ithint=file%2czip), [DropBox](https://www.dropbox.com/s/m6sg347tiaqpcwy/caffe_mex.zip?dl=0), [BaiduYun](http://pan.baidu.com/s/1i3m0i0H)
0. ImageNet-pretrained networks:
    - Zeiler & Fergus (ZF) net [OneDrive](https://onedrive.live.com/download?resid=4006CBB8476FF777!17255&authkey=!AHOIeRzQKCYXD3U&ithint=file%2czip), [DropBox](https://www.dropbox.com/s/sw58b2froihzwyf/model_ZF.zip?dl=0), [BaiduYun](http://pan.baidu.com/s/1sj3K21B)
    - VGG-16 net [OneDrive](https://onedrive.live.com/download?resid=4006CBB8476FF777!17319&authkey=!APSc546R6TbCCl4&ithint=file%2czip), [DropBox](https://www.dropbox.com/s/z5rrji25uskha73/model_VGG16.zip?dl=0), [BaiduYun](http://pan.baidu.com/s/1dDoAna5)
0. Final RPN+FastRCNN models: [OneDrive](https://onedrive.live.com/download?resid=4006CBB8476FF777!17292&authkey=!AFCIads9CKr5-4s&ithint=file%2czip), [DropBox](https://www.dropbox.com/s/jswrnkaln47clg2/faster_rcnn_final_model.zip?dl=0), [BaiduYun](http://pan.baidu.com/s/1hq2brlq)


