# airbus-kaggle

## Objective

To build a semantic segmentation model for https://www.kaggle.com/competitions/airbus-ship-detection/overview using U-net and dice score as a target metric.

## Relevant kaggle dataset to upload my results

- https://www.kaggle.com/datasets/yehorpanchenko/airbus-models - models 
- https://www.kaggle.com/datasets/yehorpanchenko/six-paths-93504 - list of (image_id, square_i, square_j) of with-ships image squares (after spliting it into 6x6)
- https://www.kaggle.com/datasets/yehorpanchenko/airbus-loads - contains train and val split
- https://www.kaggle.com/code/yehorpanchenko/pipeline-airbus - whole pipeline notebook
- https://www.kaggle.com/code/yehorpanchenko/model-training-airbus - training model code
- https://www.kaggle.com/code/yehorpanchenko/inference-airbus - inference code

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
The best validation metric is **iou_score: 0.6957** despite model didn't converge and may be trained more
```
best_model.h5
Evaluating best_model.h5...
35/35 [==============================] - 236s 7s/step - loss: 0.2666 - iou_score: 0.6029 - dice_loss: 0.2666 - binary_focal_loss: 0.0201

best_model (2)_maybe_best.h5
Evaluating best_model (2)_maybe_best.h5...
35/35 [==============================] - 211s 6s/step - loss: 0.1817 - iou_score: 0.6957 - dice_loss: 0.1817 - binary_focal_loss: 0.0452

best_model (3).h5
Evaluating best_model (3).h5...
35/35 [==============================] - 234s 7s/step - loss: 0.5053 - iou_score: 0.3808 - dice_loss: 0.5053 - binary_focal_loss: 0.1010

best_model_24_05876_val.h5
Evaluating best_model_24_05876_val.h5...
35/35 [==============================] - 217s 6s/step - loss: 0.2655 - iou_score: 0.5876 - dice_loss: 0.2655 - binary_focal_loss: 0.0463

25-dice-small_train2-usual_3e-3.h5
Evaluating 25-dice-small_train2-usual_3e-3.h5...
35/35 [==============================] - 211s 6s/step - loss: 0.2186 - iou_score: 0.6506 - dice_loss: 0.2186 - binary_focal_loss: 0.0592

25-dice-small_train.h5
Evaluating 25-dice-small_train.h5...
35/35 [==============================] - 209s 6s/step - loss: 0.3245 - iou_score: 0.5166 - dice_loss: 0.3245 - binary_focal_loss: 0.0299

2-boundary_2-dice.h5
Evaluating 2-boundary_2-dice.h5...
35/35 [==============================] - 209s 6s/step - loss: 0.3556 - iou_score: 0.5263 - dice_loss: 0.3556 - binary_focal_loss: 0.0404

best_model (1).h5
Evaluating best_model (1).h5..
35/35 [==============================] - 210s 6s/step - loss: 0.1992 - iou_score: 0.6775 - dice_loss: 0.1992 - binary_focal_loss: 0.0457
```
