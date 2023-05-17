# VisText: A Benchmark for Semantically Rich Chart Captioning
![Saliency card teaser.](teaser.png)

VisText is a benchmark dataset of over 12,000 charts and semantically rich captions! In the VisText dataset, each chart is represented as its rasterized image, scene graph specification, and underlying datatable. Each chart is paired with a synthetic L1 caption that describes the chart's elemental and ecoded properties and a human-generated L2/L3 caption that describes trends and statistics about the chart.

In the VisText paper, we train text-based models (i.e, models that use the scene graph and data table chart representations) as well as image-guided models that include the chart image. We also include semantic prefix-tuning, allowing our models to customize the level of semantic content in the chat. Our models output verbose chart captions that contain varying levels of semantic content.

This repository contains code for training and evaluating the VisText models. For more info, see: [VisText: A Benchmark for Semantically Rich Chart Captioning (ACL 2023)](http://vis.csail.mit.edu/pubs/vistext)

## Repository Structure
```
run.sh # Main script used to train and evaluate models

./code # Training and evaluation code
    image_guided/ # image-guided models use chart images
    text_only/ # text_only models use chart scene graphs and data tables
    ...
    
./data # Stores the VisText dataset
    feature_extraction/ # Code to extract chart visual features
    ...

./data_generation # Converts the raw data to VisText format
    dataset_generation.ipynb # Creates data files in ./data
    ...
    
./models # Stores model outputs and pretrained model checkpoints
```

## Set Up
### Clone the VisText repo

### Download the raw data
Download the raw data from the [dataset site](http://vis.csail.mit.edu/) and unzip to `data/`.
Ensure that you have three folders, `data/images`, `data/scenegraphs`, and `data/features`.

### Generate the VisText dataset from raw data
Run the `dataset_generation.ipynb` notebook from start to finish, which will generate the three split dataset files `data/data_train.json`, `data/data_test.json`, and `data/data_validation.json`.

### Download pretained model checkpoints [image-guided only]
For image-guided models, we finetune the pretrained checkpoints from [VLT5](https://arxiv.org/abs/2102.02779). Download the `pretrain` folder from the [VLT5 Google Drive](https://drive.google.com/drive/folders/1wLdUVd0zYFsrF0LQvAUCy5TnTGDW48Fo?usp=share_link) and add it to the `models` folder.

## Usage
Call `run.sh` with your desired parameters. Options are:
```
-c model_class    # Class of models to use ('text_only' or 'image_guided').
-b batch_size     # Batch size for training, validation, and testing.
-e num_epochs     # Number of epochs to train for.
-g num_gpus       # Number of gpus to parallelize across.
-i input_type     # Chart text representation ('scenegraph', 'datatable', or 'imageonly').
-m model_backbone # Model architecture to finetune ('byt5', 't5', or 'bart').
-s seed           # Seed number.
--prefix_tuning   # Flag to use sematic prefix tuning.
```
For example, to train and evaluate the `scene-graph` model with prefix tuning, run:
```
bash run.sh -c text_only -b 4 -e 50 -g 4 -i scenegraph -m byt5 -s 42 --prefix_tuning
```

## Citation
For more information about VisText, check out [VisText: A Benchmark for Semantically Rich Chart Captioning](http://vis.csail.mit.edu/pubs/vistext/)
```
@inproceedings{saliencycards,
  title={{VisText: A Benchmark for Semantically Rich Chart Captioning}},
  author={Tang, Benny J. and Boggust, Angie and Satyanarayan, Arvind},
  booktitle = {Annual Meeting of the Association for Computational Linguistics ({ACL})},
  year={2023}
}
```
