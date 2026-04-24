# Real-Time ASL Recognition System

A real-time American Sign Language (ASL) recognition system for **36 classes**, covering **letters A–Z** and **digits 0–9**. This project combines **transfer learning**, **custom webcam fine-tuning**, **MediaPipe hand detection**, and **LLM-based post-processing** to move from offline image classification to a more practical live webcam recognition pipeline. 

## Project Overview

The goal of this project was to build an ASL recognition system that could accurately classify static hand signs and then extend that system into a real-time webcam demo. The project began with image classification using two public Kaggle datasets for ASL alphabet signs and ASL digits. After evaluating multiple pretrained CNN models, the best-performing model was selected and further fine-tuned on a custom-collected webcam dataset to improve live performance. :contentReference[oaicite:2]{index=2}

A major challenge in this project was the **domain gap** between benchmark datasets and real webcam input. The public datasets were collected in controlled settings, while live webcam inference introduced variation in lighting, framing, background, handedness, and motion. To address this, I collected a custom webcam dataset, fine-tuned the selected model, and improved the live inference pipeline using **MediaPipe-based hand localization** and **LLM-based post-processing**. 

## Features

- 36-class ASL recognition for **A–Z** and **0–9**
- Comparison of multiple pretrained CNN models:
  - **ResNet18**
  - **MobileNetV2**
  - **EfficientNet-B0**
- Robustness testing under:
  - Blur
  - Brightness changes
  - Contrast changes
  - Noise perturbations
- Background segmentation experiment to reduce dataset mismatch
- Custom webcam dataset for domain adaptation
- Fine-tuning of **ResNet18** on webcam images
- Real-time webcam demo using **MediaPipe** hand detection
- LLM-based post-processing for more meaningful word and sentence formation :contentReference[oaicite:4]{index=4}

## Datasets

This project used three data sources:

### 1. ASL Alphabet Dataset
- Source: [Kaggle ASL Alphabet Dataset](https://www.kaggle.com/datasets/grassknoted/asl-alphabet)
- Approximately 78,000 RGB images
- 26 letter classes: A–Z :contentReference[oaicite:5]{index=5}

### 2. ASL Digits Dataset
- Source: [Kaggle ASL Digits Dataset](https://www.kaggle.com/datasets/rayeed045/american-sign-language-digit-dataset)
- Approximately 5,000 RGB images
- 10 digit classes: 0–9 :contentReference[oaicite:6]{index=6}

### 3. Custom Webcam Dataset
- Source: [Custom ASL Webcam Dataset](https://huggingface.co/datasets/sidb27/Custom_ASL_Webcam_Dataset/tree/main)
- Custom-collected dataset created for this project
- 7,200 images total
- 200 images per class across all 36 classes :contentReference[oaicite:7]{index=7}

## Running the Final Webcam Demo

Run only:

`3) ASL_Webcam_Demo.ipynb`

The demo uses the included checkpoint:

`runs_asl36_final/best_resnet18_finetuned.pt` :contentReference[oaicite:8]{index=8}

### Notes
- The training and data collection notebooks are included for implementation reference and do not need to be run.
- The original Kaggle datasets and the custom webcam dataset are not included in the zip/repository due to size constraints.

## How the Webcam Demo Works

- The notebook loads the included fine-tuned checkpoint and opens the webcam.
- The demo supports 3 modes:
  - `L` = Letter Mode
  - `D` = Digit Mode
  - `B` = Both Mode
- It is recommended to use **Letter Mode** when signing letters and **Digit Mode** when signing digits, rather than **Both Mode**, because some letters and digits are visually similar or identical and may cause confusion during prediction.
- Characters are predicted in real time from the detected hand sign.
- `SPACE` commits the current word and moves to the next word to be signed.
- `ENTER` performs LLM-based word/sentence refinement and acts as a sentence-completion command.
- `C` clears the entire output.
- `Q` exits the demo.
- The demo also shows the top predictions and a debug crop window, which helps provide an additional visual aid in confirming that the displayed hand sign is being interpreted correctly. :contentReference[oaicite:10]{index=10}

## Groq API Key Setup

The LLM-based post-processing part of the webcam demo requires a **Groq API key**. For security reasons, the actual API key is **not included** in this project. :contentReference[oaicite:11]{index=11}

### Steps
1. Go to [Groq Console](https://console.groq.com/home) and create an account.
2. Open the **API Keys** tab.
3. Click **Create API Key**.
4. Create a file named `.env` in the project folder.
5. Add the following line:

```env
GROQ_API_KEY=your_api_key_here
