# airbus-kaggle

## Objective

To build a semantic segmentation model for https://www.kaggle.com/competitions/airbus-ship-detection/overview using U-net and dice score as a target metric.

## Relevant kaggle dataset to upload my results

- https://www.kaggle.com/datasets/yehorpanchenko/airbus-models - models 
- https://www.kaggle.com/datasets/yehorpanchenko/six-paths-93504 - list of (image_id, square_i, square_j) of with-ships image squares (after spliting it into 6x6)
- https://www.kaggle.com/datasets/yehorpanchenko/airbus-loads - contains train and val split
- https://www.kaggle.com/code/yehorpanchenko/notebookb24de9085f - whole pipeline notebook

## Approach

### EDA

- To check for corrupted images
- To confirm same image resolution 768x768x3
- To investigate images with/without ships distribution
- To explore pixels imbalance
- To patch image into 6x6 squares, size of each is 128x128 and remove squares without ships pixels

### Data preparation for model training

- To split images from train folder into train / val (95%/5%) by image ID
- From train data remove images with small portion of ship-pixels
- To exploit custom dataset and dataloaders, add augmentation for training set - Flip, Rotate, Noise, Blur, Contrast - without Scaling / Shifting / Crops. 

### Model training

- To exploit U-net with pretrained weights from [segmentation_models](https://github.com/qubvel/segmentation_models) library. To use
  1. Adam/Adagrad as optimizers;
  2. DiceLoss / BinaryFocalLoss / Their combination / Boundary loss as losses;
  3. IoU as a main target metric for validation
  4. ImageNet freezed weights for encoder
- To experiment with training procedures

### Evaluation

- To evaluate models on validation set using IoU metric

### Results

