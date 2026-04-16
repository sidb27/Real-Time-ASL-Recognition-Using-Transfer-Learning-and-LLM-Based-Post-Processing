# Real-Time ASL Recognition System

A real-time American Sign Language (ASL) recognition system for **36 classes**, covering **letters A–Z** and **digits 0–9**. This project combines **transfer learning**, **custom webcam fine-tuning**, **MediaPipe hand detection**, and **Llama-based post-processing** to move from offline image classification to a more practical live webcam recognition pipeline. :contentReference[oaicite:0]{index=0}

---

## Datasets

This project used three data sources:
### 1. ASL Alphabet Dataset
- Source: [Kaggle ASL Alphabet Dataset](https://www.kaggle.com/datasets/grassknoted/asl-alphabet)
- Approximately 78,000 RGB images
- 26 letter classes: A–Z

### 2. ASL Digits Dataset
- Source: [Kaggle ASL Digits Dataset](https://www.kaggle.com/datasets/grassknoted/asl-alphabet)
- Approximately 5,000 RGB images
- 10 digit classes: 0–9

### 3. Custom Webcam Dataset
- Custom-collected dataset created for this project
- 7,200 images total
- 200 images per class across all 36 classes

---

## Project Overview

The goal of this project was to build an ASL recognition system that could accurately classify static hand signs and then extend that system into a real-time webcam demo. The project began with image classification using two public Kaggle datasets for ASL alphabet signs and ASL digits. After evaluating multiple pretrained CNN models, the best-performing model was selected and further fine-tuned on a custom-collected webcam dataset to improve live performance. :contentReference[oaicite:1]{index=1}

A major challenge in this project was the **domain gap** between benchmark datasets and real webcam input. The public datasets were collected in controlled settings, while live webcam inference introduced variation in lighting, framing, background, handedness, and motion. To address this, I collected a custom webcam dataset, fine-tuned the selected model, and improved the live inference pipeline using **MediaPipe-based hand localization** and **Llama-based post-processing**. :contentReference[oaicite:2]{index=2}

---

## Features

- 36-class ASL recognition for **A–Z** and **0–9**
- Comparison of multiple pretrained CNN models:
  - **ResNet18**
  - **MobileNetV2**
  - **EfficientNet-B0**
- Robustness testing under:
  - blur
  - brightness changes
  - contrast changes
  - noise perturbations
- Background segmentation experiment to reduce dataset mismatch
- Custom webcam dataset for domain adaptation
- Fine-tuning of **ResNet18** on webcam images
- Real-time webcam demo using **MediaPipe** hand detection
- Post-processing with **Llama-3.1-8B-Instant API** for more meaningful word and sentence formation :contentReference[oaicite:3]{index=3}

---

## Motivation

ASL recognition is an important application of deep learning in **assistive technology**, **computer vision**, and **human-computer interaction**. A reliable ASL recognition system can help improve accessibility by allowing sign language users to interact more naturally with digital systems. :contentReference[oaicite:4]{index=4}

While many image classification models perform well on benchmark datasets, real-time deployment is significantly harder. This project explores not only classification performance, but also the practical issues involved in taking a model from offline evaluation to a live webcam setting. :contentReference[oaicite:5]{index=5}

---

## Datasets

This project used three data sources: :contentReference[oaicite:6]{index=6}

### 1. ASL Alphabet Dataset
- Source: **Kaggle**
- Approximately **78,000 RGB images**
- **26 letter classes:** A–Z
- Original dataset also included extra classes such as **space**, **delete**, and **nothing**, which were removed for this project
- Images were captured in a relatively controlled environment

### 2. ASL Digits Dataset
- Source: **Kaggle**
- Approximately **5,000 RGB images**
- **10 digit classes:** 0–9
- Smaller than the alphabet dataset, which introduced imbalance when combined
- Used a pure black background, unlike the alphabet dataset
- Created by different people, introducing additional variation in appearance and pose

### 3. Custom Webcam Dataset
- **7,200 images total**
- **200 images per class** across all 36 classes
- For each class:
  - **100 images captured using the left hand**
  - **100 images captured using the right hand**
- Collected to reduce the mismatch between benchmark datasets and real webcam input

---

## Task Definition

The main task is **36-class static hand-sign classification**, where the model predicts the correct label from an input image. The label space includes: :contentReference[oaicite:7]{index=7}

- **26 letters:** A–Z
- **10 digits:** 0–9

The project was later extended into a **real-time webcam recognition system**, where the pipeline:

1. detects the hand region from webcam frames,
2. classifies the detected sign,
3. accumulates character predictions,
4. refines the output into more meaningful text using an LLM. :contentReference[oaicite:8]{index=8}

---

## Methodology

### 1. Baseline Model Comparison
The first stage of the project involved comparing multiple pretrained CNN models on the combined ASL alphabet and digits dataset. The following models were evaluated:

- **ResNet18**
- **MobileNetV2**
- **EfficientNet-B0**

The purpose of this comparison was to identify the most effective architecture for static ASL sign classification. :contentReference[oaicite:9]{index=9}

### 2. Preprocessing and Training
Because the public datasets had different original resolutions, all images were resized to a common size before training. Standard normalization was applied using ImageNet statistics, and augmentation techniques were used to improve generalization. Typical preprocessing included:

- image resizing
- normalization
- rotations
- color jitter
- other augmentation steps depending on the experiment :contentReference[oaicite:10]{index=10}

### 3. Robustness Testing
To better understand model behavior beyond clean benchmark images, robustness experiments were performed under:

- blur
- brightness changes
- contrast changes
- additive noise :contentReference[oaicite:11]{index=11}

### 4. Segmentation Experiment
A background segmentation experiment was performed to reduce the visual mismatch between the alphabet dataset and the digits dataset, especially because the digits dataset had a pure black background while the alphabet dataset had more variation. :contentReference[oaicite:12]{index=12}

### 5. Webcam Fine-Tuning
After model comparison, **ResNet18** was selected as the final model and fine-tuned on the custom webcam dataset. This step was critical for reducing the domain gap between public datasets and real-time webcam input. :contentReference[oaicite:13]{index=13}

### 6. Real-Time Inference Pipeline
The live demo used **MediaPipe** to detect and localize the hand region from webcam frames before classification. This ensured that the classifier operated on a cropped hand image rather than the full webcam frame. :contentReference[oaicite:14]{index=14}

### 7. LLM-Based Post-Processing
After frame-level predictions were accumulated into character sequences, the output was passed through the **Llama-3.1-8B-Instant API** to improve word and sentence formation. This was especially useful for converting noisy character streams into more meaningful text while preserving important cases such as digits and acronym-like outputs. :contentReference[oaicite:15]{index=15}

---

## Model Fine-Tuning Details

The final webcam-adapted model was based on **ResNet18**. During fine-tuning:

- pretrained weights from the earlier classification stage were loaded
- the final classifier head was replaced
- dropout was added before the final linear layer
- the model was fine-tuned on the custom webcam dataset
- early stopping was used to avoid unnecessary training :contentReference[oaicite:16]{index=16}

This fine-tuning step was one of the most important improvements for real-time performance. :contentReference[oaicite:17]{index=17}

---

## Real-Time Demo Pipeline

The real-time demo pipeline works as follows:

1. Capture a frame from the webcam
2. Use MediaPipe to detect the hand
3. Crop/localize the hand region
4. Preprocess the crop for the classifier
5. Run sign classification using the fine-tuned ResNet18 model
6. Accumulate predictions over time
7. Use Llama-based post-processing to convert the sequence into more meaningful output :contentReference[oaicite:18]{index=18}

This allowed the project to go beyond simple image classification and move toward a more interactive ASL recognition system. :contentReference[oaicite:19]{index=19}

---

## Key Challenges

One of the biggest findings of this project was that high offline accuracy did not automatically translate to strong webcam performance. The major challenges included:

- domain gap between benchmark datasets and live webcam input
- differences in background and image style between the alphabet and digit datasets
- handedness mismatch
- crop and preprocessing inconsistencies during live inference
- difficulties with visually similar signs
- challenges in handling dynamic signs or motion-heavy cases in a frame-based pipeline :contentReference[oaicite:20]{index=20}

---

## Improvements Made

This project did not stop at training a single model. Several strategies were explored to improve performance:

- model comparison across multiple pretrained CNNs
- robustness testing under perturbations
- segmentation to reduce background mismatch
- custom webcam data collection
- webcam fine-tuning
- MediaPipe hand localization
- debugging deployment-time preprocessing and crop issues
- LLM prompt design and constraints for more reliable text output :contentReference[oaicite:21]{index=21}

---

## Results Summary

The project achieved very strong offline classification performance on benchmark data, especially with **ResNet18**. However, live webcam performance initially lagged behind, which highlighted the importance of domain adaptation and pipeline debugging. :contentReference[oaicite:22]{index=22}

After webcam fine-tuning and improvements to the real-time inference pipeline, live performance improved substantially. The final system demonstrated that deployment quality depends not only on benchmark accuracy, but also on preprocessing consistency, data distribution, and real-world adaptation. :contentReference[oaicite:23]{index=23}
