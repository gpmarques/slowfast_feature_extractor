# SlowFast Feature Extractor

Extract features from videos with a pre-trained SlowFast model using the PySlowFast framework

## Getting Started

Clone the repo and set it up in your local drive.

```
git clone git@github.com:tridivb/slowfast_feature_extractor.git
```

### Prerequisites

Python >= 3.6\
[Pytorch](https://pytorch.org/)  >= 1.3\
[PySlowFast](https://github.com/facebookresearch/SlowFast.git)\
PyAv >= 6.2.0\
Moviepy >= 1.0\
OpenCV >= 3.4
\
The videos can be setup in the following way:

```
|---<path to dataset>
|   |---vid_list.csv
|   |---video_1.mp4
|   |---video_2
|   |   |---video_2.mp4
|   |   |---.
|   |---.

```

or pre-process the videos and extract the frames like below:
```
|---<path to dataset>
|   |---vid_list.csv
|   |---video_1
|   |   |---frame01.jpg
|   |   |---frame02.jpg
|   |   |---.
|   |---video_2
|   |   |---video_2
|   |   |   |---frame01.jpg
|   |   |   |---frame02.jpg
|   |   |   |---.
|   |---.

```

The vid_list.csv should have the paths of all the videos or subdirectories for extracted frames. 
All the videos/image files should have the same type of extension.
Based on the hierarchy above, it should be like:

```
video_1
video2/video_2
...
...
...
```

### Installing
\
Navigate to the slowfast_feature_extractor directory\

```
git clone git@github.com:tridivb/slowfast_feature_extractor.git
cd slowfast_feature_extractor/
```
\
Download the pre-trained [weights](https://github.com/facebookresearch/SlowFast/blob/master/MODEL_ZOO.md) 
from the PySlowFast Model Zoo and copy it to your desired location

### Configure the paramters

Use the existing config file in ./configs or copy over the corresponding one for your desired model from where you cloned the PySlowFast framework.
\
Set the following paths in the ./configs/<config_file>.yaml file:

```
TRAIN:
  # checkpoint file
  CHECKPOINT_FILE_PATH: ""
  # set this to pytorch or caffe2 depending on your checkpoint configuration
  # the default pre-trained weights from PySlowFast Model Zoo are in caffe2 format
  CHECKPOINT_TYPE: caffe2
DATA:
  # Root dir of dataset
  PATH_TO_DATA_DIR: ""
  # Path prefix for each video or subdirectory where extracted frames are kept
  PATH_PREFIX: ""
  # size of sampled window centered on each frame
  NUM_FRAMES: 32
  # original fps of input video
  IN_FPS: 15
  # fps value to sample videos at
  OUT_FPS: 15
  # Flag to turn on/off processing frames from video files. If False, it will try to read extracted image frames.
  READ_VID_FILE: False
  # File extension of video files (case-sensitive). Set this if you want to read the video files.
  VID_FILE_EXT: ".MP4"
  # File extension of image files (case-sensitive). Set this if you want to read the pre-processed frames.
  IMG_FILE_EXT: ".jpg"
  # File naming format of image files (case-sensitive). Set this if you want to read the pre-processed frames.
  IMG_FILE_FORMAT: "frame_{:010d}.jpg"
  # Sampling height and width of each extracted frame. This can be a list or int value
  SAMPLE_SIZE: [256, 256]

TEST:
  # be careful with this, inference will run faster with a higher value but can cause out of memory error
  BATCH_SIZE: 3

# output directory to save features
OUTPUT_DIR: ""
```

### Extracting the features and detections

To extract features, execute the run_net.py as follows:

```
python run_net.py --cfg ./configs/<config_file>.yaml
```

For our case, we used the SlowFast network with a Resnet50 backbone, frame length of 8 and sample rate of 8.\
If you want to use a different model, copy over the corresponding config file and download the weights.

### Results

The detections are saved in the following format for each video:

```
|---<path to output>
|   |---video_1_{NUM_FRAMES}.npy
|   |---video_2
|   |   |---video_2_{NUM_FRAMES}.npy
|   |   |---.
|   |---.
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.\
\
Please note, the original PySlowFast frames is licensed under the Apache 2.0 license. Please respect the original licenses as well.

## Acknowledgments

1. The code was built on top of the [PySlowFast](https://github.com/facebookresearch/SlowFast.git) framework provided by Facebook. Some of the model and dataloader code was modified to fit the needs of feature extraction from videos

2. Readme Template -> https://gist.github.com/PurpleBooth/109311bb0361f32d87a2