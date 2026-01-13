# DNNLS Final Assessment

**Author:** Jahangeer Khan  
**Student ID:** 35033473  

---

## Introduction and Problem Statement

This repository contains the final assessment for the **Deep Neural Networks and Learning Systems (DNNLS)** course.  
The objective of this project is to extend a provided baseline multimodal storytelling model by incorporating semantic tags (objects, actions, and locations) and to evaluate whether this additional information improves learning behavior and prediction quality.

The task focuses on video/image-driven storytelling, where a model must understand a sequence of visual frames and textual descriptions in order to predict the next frame and generate a coherent continuation of the story. This requires the model to capture temporal consistency, scene structure, and semantic relationships between events.

We use the StoryReasoning dataset for training and evaluation:

- Dataset: https://huggingface.co/datasets/daniel3303/StoryReasoning  

While the baseline model relies only on images and textual descriptions, the dataset also provides explicit semantic tags describing who is present, what actions are occurring, and where events take place. Ignoring this structured information can limit the model’s ability to reason accurately about the scene.

Therefore, this project investigates whether adding tag-based supervision as an auxiliary task can help the model learn richer representations and improve its overall storytelling performance.

---

## Problem Definition

Given a baseline sequence-to-sequence image- and text-based storytelling model, the goal of this project is to investigate whether incorporating semantic tags can improve the model’s ability to understand and predict story progressions.

We compare two models:

- **Baseline Model:**  
  Uses only image frames and textual descriptions to predict the next frame and generate the corresponding story text.

- **Tag-Augmented Model (+Tags):**  
  Extends the baseline by incorporating semantic tags (objects, actions, and locations) as additional inputs and as an auxiliary prediction task.

The tag-augmented model introduces tag information as an additional learning signal, allowing the model to reason more explicitly about:
- who is present,
- what actions are occurring,
- where events take place.

The model is also trained to predict the tags of the next frame, encouraging better semantic understanding through multi-task learning.

Performance is evaluated using:
- training and validation losses,
- tag prediction accuracy,
- qualitative visual comparisons.

---

## Methods

### Model Architecture Overview

The baseline model consists of:
- an image encoder to extract visual features,
- a text encoder to process textual descriptions,
- a GRU-based recurrent decoder to generate the next-frame representation and story text.

### Tag-Augmented Model (+Tags)

Semantic tags are incorporated in two ways:

1. **Tag Input Conditioning**
   - Tags are encoded into vector representations.
   - They are concatenated with image and text features before decoding.

2. **Auxiliary Tag Prediction Task**
   - The model predicts tags of the next frame.
   - This introduces an auxiliary loss and forms a multi-task learning setup.

Total loss includes:
- image reconstruction loss,
- text prediction loss,
- context consistency loss,
- tag prediction loss.

---

## Implementation Details

Key modifications:
- Encoding semantic tags into vectors.
- Concatenating tag embeddings with image and text features.
- Adding a tag prediction head trained using binary cross-entropy loss.

Implementation is provided in:
- `JK_Final_Assessment.ipynb`

Screenshots and outputs are included in:
- `results/`

---

## Results

### Quantitative Evaluation

Plots included:

**Baseline model**
- `results/baseline_model/5_epochs/baseline_train_vs_val_loss.png`
- `results/baseline_model/10_epochs/Baseline_Training_&_Validation_Loss.png`

**Tag-augmented model**
- `results/tag_model/5_epochs/Tag_Model_Training_Loss.png`
- `results/tag_model/5_epochs/Tag_Loss.png`
- `results/tag_model/5_epochs/tag_accuracy.png`
- `results/tag_model/10_epochs/Tag_Model_Training_Loss.png`
- `results/tag_model/10_epochs/Tag_Loss.png`
- `results/tag_model/10_epochs/Tag_Accuracy.png`

**Comparison**
- `results/comparison/5_epoch/baseline_vs_tags_total_loss.png`
- `results/comparison/5_epoch/Text_Prediction_Loss.png`
- `results/comparison/10_epoch/baseline_vs_tags_total_loss.png`
- `results/comparison/10_epoch/Text_Prediction_Loss.png`

These show stable convergence and successful learning of the auxiliary tag task.

---

## Qualitative Analysis

Example outputs are located in:
- `results/samples/5_epoch/`
- `results/samples/10_epoch/`

These show:
- input frames,
- target images and text,
- predicted images and generated text,
for both baseline and tag-augmented models.

---

## Conclusions

This project demonstrates that:
- semantic tags can be learned with high accuracy,
- training remains stable with auxiliary supervision,
- qualitative results remain coherent.

While overall loss improvements are modest, tag supervision improves semantic grounding.

---

## Future Work

Possible improvements:
- train for more epochs,
- tune tag loss weights,
- filter noisy tags,
- evaluate objects/actions/locations separately,
- perform human evaluation of story quality.
