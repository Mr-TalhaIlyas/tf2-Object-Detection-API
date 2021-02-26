# Setup TF2 Object Detection API (Linux)

In this file I assume that you have installed the Nvidia and CuDNN dependencies for you Linux 16.04 86x_64, 
also that you already have some working tf_gpu enviorment.

In this file we will only see how to install TF2 objedct detection API on Linux 16.04 (86x_64)

## Sysetem Specs:

	  Operating System: Ubuntu 16.04.6 LTS
		    Kernel: Linux 4.15.0-115-generic
	      Architecture: x86-64

	  CUDA compilation tool: 10.1
	  GPU Driver version   : 440.64.00


### NOTE: 
Follow the same order of steps


mostly i'll be following the steps in 
[Tutorial](https://tensorflow-object-detection-api-tutorial.readthedocs.io/en/latest/install.html)
but following in steps of above i had some probelms with installin protobuf library so i just simplify the 
installing process of protobuf library in this file.


1. if you have already tried other instllation methods than you probably have an uncompatable protobuf version in your base env.
   1st uninstall it by typing
	
	sudo apt-get remove --auto-remove protobuf-compiler

2. create a new conda env.
```
# creat a new env 
conda create -n tfod python=3.7.6
# or clone form and existing one and upgrade
conda create --name yourname_env --clone base_tf_20 
```
3. Install tf-gpu by typing (assuming you already have working nvidia and cudnn libraries installed)
```python
# install tensorflow, be mindful of the version to use
pip install tensorflow-gpu==2.3.0	
```
4. Test tf installation via 
```python	
python -c "import tensorflow as tf;print(tf.reduce_sum(tf.random.normal([1000, 1000])))"
# or try this one
python -c "import tensorflow as tf;tf.config.list_physical_devices('GPU')"
```
5. In command line (cl) cd to your desired directory where you wanna install library
```
 cd /data_ssd/tfod/
```
5. Form there in cl type 
```python
git clone https://github.com/tensorflow/models.git
```
it'll download the modesl (~600mb)

6. now important part before doing anything if you have protobuf/protoc --version <=2 installed (check via protoc --version), uninstall it via step 1.
   if you have grater version > 3.4 installed then skip the following step.

7. type in the same cd direcotry (/data_ssd/tfod/)
```python
!wget https://github.com/google/protobuf/releases/download/v3.6.1/protoc-3.6.1-linux-x86_64.zip
```
it'll download a bunch of files, then 

```	
unzip protoc-3.6.1-linux-x86_64.zip -d protoc3
```
now copy following cmd to;
move protoc to `/usr/local/bin/`
```
sudo cp -r protoc3/bin/* /usr/local/bin/
```
move protoc3/include to `/usr/local/include/`
```
# copy files
sudo cp -r protoc3/include/* /usr/local/include/
```
Finally type
```
sudo ldconfig
```
Check the protoc version:
```
# check version
protoc --version
```
if it shows **'libprotoc 3.6.1'**

then this step is succesful and move next, else there is problem

now form cmd open at directory (data_ssd/tfod/models/research/) type
```
# install all protocols
protoc object_detection/protos/*.proto --python_out=.
```
8. Install coco API for evaluation its better to install now rather than saving the trouble for later;
type in same cd (/data_ssd/tfod/)
```
git clone https://github.com/cocodataset/cocoapi.git
```
form cmd open at smae dir type
```
cd cocoapi/PythonAPI 
```
type 
```
# copy files
cp -r pycocotools  data_ssd/tfod/models/research/
```
9. Almost done, final step install tf2 object detction api

type 
```
cd /data_ssd/tfod/models/research/
```
with cmd open at dir (/data_ssd/tfod/models/research/) type
```
cp object_detection/packages/tf2/setup.py .
```
finelly type,
```
# install all the requirements 
python -m pip install .
```
### Note :
If you have CUDA version 10.1 or less you might need to specify the `tf-models-official` veresion to be installed 
TO do that open the `setup.py` file and specify the version like `tf-models-official==2.3.0`

Now you have successfully installed objectdetction api on linux 16.04 x86_64, you can test installation by typing,
```
# check the installation
python object_detection/builders/model_builder_tf2_test.py
```
# Possible Errors
You might face following errors

## Error 1
```python
ERROR: tensorflow-gpu 2.3.1 has requirement tensorflow-estimator<2.4.0,>=2.3.0, but you'll have tensorflow-estimator 2.4.0 which is incompatible.
ERROR: tensorflow 2.4.1 has requirement absl-py~=0.10, but you'll have absl-py 0.9.0 which is incompatible.
ERROR: tensorflow 2.4.1 has requirement numpy~=1.19.2, but you'll have numpy 1.18.0 which is incompatible.
ERROR: tensorflow 2.4.1 has requirement six~=1.15.0, but you'll have six 1.13.0 which is incompatible.
```
## Solution 1

This might be because of the cudnn or CUDA version mismatch so match versions for me it was
| library        | Version     |
| ------------- |:-------------:|
|tf-models-official| 2.3.0 |
|tensorflow-gpu    | 2.3.0 |
| CUDA | 10.1      |
|cuDNN |7.6|
|protoc | 3.61 |


