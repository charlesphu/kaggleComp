# 🧠 ImageNet-Subset: Semi-Supervised Classification + Segmentation Challenge

## Instructions

### 1. Environment Setup

- Install requirements (PyTorch, torchvision, numpy, pandas, PIL, tqdm, etc.).
- Recommended: Python 3.8+ and CUDA-enabled GPU.

### 2. Data Structure

Ensure the following folders and files exist in your workspace:

- `train-semi/`: Labeled training images.
- `train-semi-segmentation/`: Segmentation masks for labeled images.
- `train-unlabeled/`: Unlabeled images for semi-supervised learning.
- `test/`: Test images for submission.
- `train_semi_annotations_with_seg_ids.json`: JSON mapping images to segmentation masks (and class IDs if needed).

### 3. Training

#### Classification

- Run `classificationv1.ipynb` or `classificationv2.ipynb` for supervised and semi-supervised classification.
- `classification1` was my first try at classification, had no data augmentation, and only trained on purely labeled images
- `classification2` was my second attempt at classification, used the same model, but also added data augmentation such as horizontal flips, random rotation, collor jitter. I also had a training loop of semi supervised learning, with a variable psuedo confident threshold that would increase per epoch, but would decrease if not enough labeled images were not being added. I played around with the number of training rounds, each with its own amount of supervised epochs and semi supervised epochs.
- Models are saved in `saved_models/` + name

#### Segmentation

- Run `segmentation.ipnyb` to train segmentation model.
- Used a custom Unet structure with convolutional blocks consisting of convolutional layers and attention layers.

#### Data Augmentation

- Use `dataAugmenter.ipynb` to augment your training data and generate new annotations. (was later scrapped after I realized that you can randomize data augmentation in the get_item function)

### 4. Submission

- Use `submission.ipynb` to generate the final CSV for submission.
- The script will output `submission.csv` with predicted class and segmentation mask RLE for each test image.
- Set paths to both segmentation and classification models (needs to be full model not just dict)
- Set dummy-RLE to false if you want to have dummy masks of all 0's

## Experimental Setup

- **Classification Model:** Custom ResNet-18 (see `classificationv1.ipynb` and `classificationv2.ipynb`)
- **Segmentation Model:** Custom UNet or ConvNeXt-based model (see `submission.ipynb`)
- **Loss Functions:** CrossEntropyLoss for classification; CrossEntropy + Dice Loss for segmentation.
- **Augmentation:** Random rotations, flips, color jitter, resizing.
- **Semi-Supervised Learning:** Pseudo-labeling and self-training rounds using high-confidence predictions on unlabeled data.
- **Validation:** Training set split or cross-validation.

## Results Summary

- **Classification:**
  - Supervised accuracy (ResNet-18): ~55-60% on labeled validation set after 7 epochs.
  - Semi-supervised self-training improved accuracy up to ~90% on training data when doing 10 epochs
- **Segmentation:**
  - UNet/ConvNeXt models trained with CrossEntropy + Dice Loss.
  - Achieved mean Dice score and IoU improvements with data augmentation and semi-supervised learning.
  - Decent fitting on images, but not very good, has a much broader scope
  - [Validation] Loss: 5.8962 | Dice: 0.6731 | IoU: 0.0002

---
