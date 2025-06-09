# 🧠 ImageNet-Subset: Semi-Supervised Classification + Segmentation Challenge

Welcome to the **ImageNet-Subset Semi-Supervised Learning Challenge**! In this project, your goal is to build models that can solve **two tasks** using a combination of labeled and unlabeled data:

- **Image classification**: Predict the object category in an image
- **Semantic segmentation**: Predict a pixel-level mask indicating the object region

You may approach the two tasks separately or jointly — for example:
- Train a classification model and a segmentation model independently
- Reuse the encoder from a classification model to initialize a segmentation model
- Or implement a unified multi-task model


---

## 📁 Dataset Structure
```text
ImageNet-Subset/
├── train-unlabeled/ # 5000 unlabeled training images (randomized, no class info)
├── train-semi/ # 500 labeled images (50 classes × 10 images), with class labels
├── train-semi-segmentation/ # Corresponding pixel-level segmentation masks for train-semi images
├── train_semi_annotations_with_seg_ids.json # Classification + segmentation metadata
├── test/ # 752 test images (randomized, class-agnostic)
└── (you will submit results for test/)
```

---

## 📝 Task Description

You are given:
- A small **labeled set** (`train-semi`) with both image-level class labels and pixel-level segmentation masks.
- A large **unlabeled set** (`train-unlabeled`) without any label.
- A **test set** (`test/`) with 752 randomized images. The labels and masks are hidden from you.

Your goals are:

1. **Train a model** using the labeled and unlabeled data.
2. **Predict for each test image**:
   - The **class label** (index from `0` to `49`)
   - The **segmentation mask** (PNG format, with class ID encoded as `R + G × 256`)

---

## 📤 Submission Format

Submit a zipped folder with the following contents:
```text
submission/
├── classification.json # classification predictions
├── segmentation/ # predicted segmentation masks (.png)
```

### 1. `classification.json`

A list of classification predictions for test images:

```json
[
  {"image": "00000.jpg", "class_id": 17},
  {"image": "00001.jpg", "class_id": 3},
  ...
]
```

### 2. `segmentation/`
A folder containing segmentation masks named exactly like the test images but with .png extension:
```text
segmentation/
├── 00000.png
├── 00001.png
├── ...
```
Each PNG should:

Be the same resolution as the original image

Use class IDs encoded as R + G * 256

Background should be 0, ignored region (if any) should be 1000

✅ Evaluation
You will be evaluated on:

Image classification accuracy

Semantic segmentation mIoU (mean Intersection over Union)

Both metrics will be computed using hidden ground-truth from the instructors.