# DPH
Caffe implementation for ["Deep Priority Hashing"]().

## Prerequisites
Linux or OSX

NVIDIA GPU + CUDA-7.5 or CUDA-8.0 and corresponding CuDNN

Caffe

Python 2.7

## Modification on Caffe
- Add multi label layer which enable "ImageDataLayer" to process multi-label dataset.
- Add "PairwiseLoss" layer implementing the weighted pairwise loss described in our paper. The 'sigmoid_param_' in the code is the `` in the adaptive sigmoid function.

## Datasets
We use ImageNet, NUS-WIDE and COCO dataset in our experiments. You can download the ImageNet dataset and NUS-WIDE dataset [here](https://drive.google.com/drive/folders/0B7IzDz-4yH_HOXdoaDU4dk40RFE?usp=sharing).
As for COCO dataset, we use COCO 2014, which can be downloaded [here](http://mscoco.org/dataset/#download). And in case of COCO changes in the future, we also provide a download link [here](https://drive.google.com/drive/folders/0B7IzDz-4yH_HOXdoaDU4dk40RFE?usp=sharing) on google drive.
After downloading, you need to move the imagenet.tar.gz to [./data/imagenet](./data/imagenet) and extract the file there.
```
mv imagenet.tar.gz ./data/imagenet
cd ./data/imagenet
tar -zxvf imagenet.tar.gz
```
Also, for NUS-WIDE, you need to move the nus_wide.tar.gz to [./data/nuswide_81](./data/nuswide_81) and extract the file there. 
```
mv nus_wide.tar.gz ./data/nuswide_81
cd ./data/nuswide_81
tar -zxvf nus_wide.tar.gz
```
For COCO dataset, you need to extract both train and val archive for COCO in [./data/coco](./data/coco).
If you download from [COCO download page](http://mscoco.org/dataset/#download),
```
mv train2014.zip ./data/coco
mv val2014.zip ./data/coco
cd ./data/coco
unzip train2014.zip
unzip val2014.zip
```
If you use our shared [link](https://drive.google.com/drive/folders/0B7IzDz-4yH_HOXdoaDU4dk40RFE?usp=sharing)
```
mv coco.tar.gz ./data/coco
cd ./data/coco
tar -zxvf coco.tar.gz
unzip train2014.zip
unzip val2014.zip
```

You can also modify the list file(txt format) in ./data as you like. Each line in the list file follows the following format:
```
<image path><space><one hot label representation>
```
## Compiling
The compiling process is the same as caffe. You can refer to Caffe installation instructions [here](http://caffe.berkeleyvision.org/installation.html).

## Training
First, you need to download the AlexNet pre-trained model on ImageNet from [here](http://dl.caffe.berkeleyvision.org/bvlc_reference_caffenet.caffemodel) and move it to [./models/bvlc_reference_caffenet](./models/bvlc_reference_caffenet).

VGG pre-trained model on ImageNet can be downloaded [here](http://www.robots.ox.ac.uk/~vgg/software/very_deep/caffe/VGG_ILSVRC_16_layers.caffemodel).

Then, you can train the model for each dataset using the followling command.
```
AlexNet
dataset_name = imagenet, nuswide_81 or coco
./build/tools/caffe train -solver models/train/dataset_name/solver.prototxt -weights ./models/bvlc_reference_caffenet/bvlc_reference_caffenet.caffemodel -gpu gpu_id
```
or
```
VGG
dataset_name = imagenet, nuswide_81 or coco
./build/tools/caffe train -solver models/train/dataset_name/solver_vgg.prototxt -weights ./models/bvlc_reference_caffenet/bvlc_reference_caffenet.caffemodel -gpu gpu_id
```
For more instructions about training and parameter setting, see the instructions in the [training directory](./models/train).

## Evaluation
You can evaluate the Mean Average Precision(MAP) result on each dataset using the followling command.
```
dataset_name = imagenet, nuswide_81 or coco
python models/predict/dataset_name/predict_parallel.py --gpu gpu_id --model_path your_caffemodel_path --save_path the_path_to_save_your_hash_code
```
We provide some trained models for each dataset for each code length in our experiment for evaluation. You can download them [here](https://drive.google.com/drive/folders/1RxH7Dj08FEPiOuD9GsAAO9lpqzqQQgWm?usp=sharing) if you want to use them.

If you have generated the hash code by the previous step or by other method and want to test the MAP of the hash code. You can specify the code_path parameter.
```
dataset_name = imagenet, nuswide_81 or coco
python models/predict/dataset_name/predict_parallel.py --code_path the_path_of_your_hash_code
```

For more instructions about training and parameter setting, see the instructions in the [predicting directory](./models/predict).

## Citation
If you use this code for your research, please consider citing:
```
bibtex under developing
```
## Contact
If you have any problem about our code, feel free to contact caozhangjie14@gmail.com or describe your problem in Issues.
