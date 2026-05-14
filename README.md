# Plant Disease Detection Using Deep CNNs with Transfer Learning

Student: Jerusha Johanna Janetrin Julies David Dass  
Student Number: 25867550  
Module: 6G7V0024 Deep Learning  
University: Manchester Metropolitan University

---

## About

For this project I trained and compared two deep learning models to detect
plant diseases from leaf photos. The idea came from thinking about how
difficult it is for farmers in remote areas to get help diagnosing crop
diseases quickly. I wanted to see if a smartphone photo of a leaf could
be enough to identify the disease automatically.

I used the PlantVillage dataset which has 54,306 leaf images across 38
disease types. My main model is a ResNet18 fine-tuned with transfer learning
which reached 95.95% accuracy. I also built a Gradio web app so anyone
can upload a leaf photo and get a diagnosis without any technical knowledge.

---

## Results

| Model | Accuracy | Precision | Recall | F1 |
|---|---|---|---|---|
| CNN (scratch) | 88.76% | 86.21% | 84.21% | 83.92% |
| ResNet18 (my model) | 95.95% | 95.00% | 93.66% | 93.98% |

The ResNet18 performed much better than the CNN I built from scratch.
The biggest reason for this is ImageNet pre-training — the model already
knows how to detect edges, textures and shapes before it even sees a
single leaf image.

---

## How to Run

### Easiest Way — GitHub
All code and outputs are available here:
https://github.com/jerushajuliesdaviddass23-blip/plant-disease-detection-25867550

### Run It Yourself

Step 1 — Install the required libraries:
Step 2 — Put your kaggle.json file in the same folder

Step 3 — Open and run 25867550_DEEPLEARNING.ipynb from top to bottom

---

## Files in This Repository

| File | What it is |
|---|---|
| 25867550_DEEPLEARNING.ipynb | My main notebook with all code and outputs |
| 25867550_Deep_Learning.pdf | My written report in IEEE format |
| README.md | This file |

---

## Models I Built

**CNN from scratch**  
I built this as a baseline to show what happens when you train without
any pre-trained weights. It has 4 convolutional blocks with filters
going from 32 to 64 to 128 to 256, with batch normalisation, ReLU
activation and max pooling in each block. Two dropout layers stop it
from overfitting. It got 88.76% accuracy but the recall was 84.21%
meaning it still missed some of the rarer disease classes.

**ResNet18 with Transfer Learning**  
This is my main model. I took a ResNet18 pre-trained on ImageNet and
replaced the final layer with a new 38-class layer for PlantVillage. I
used a lower learning rate (2e-4 instead of 1e-3) to avoid catastrophic
forgetting — basically to stop the training from overwriting the useful
features the model already learned from ImageNet. This worked really
well and got 95.95% accuracy.

---

## Hyperparameters

| Setting | CNN | ResNet18 |
|---|---|---|
| Input size | 128x128 | 128x128 |
| Epochs | 10 | 10 |
| Batch size | 64 | 64 |
| Optimizer | Adam | Adam |
| Learning rate | 0.001 | 0.0002 |
| Starting weights | Random | ImageNet |
| Dropout | 2 layers | None |
| Loss function | Cross-entropy | Cross-entropy |
| GPU | NVIDIA T4 | NVIDIA T4 |

---

## Ablation Study

I ran experiments to figure out which parts of the setup actually made
a difference to the final accuracy:

| What I tested | Starting weights | Augmentation | Accuracy |
|---|---|---|---|
| CNN from scratch | Random | Yes | 88.76% |
| ResNet18, no augmentation, lr=0.001 | ImageNet | No | 93.52% |
| ResNet18, with augmentation, lr=0.001 | ImageNet | Yes | 94.76% |
| ResNet18, with augmentation, lr=0.0002 | ImageNet | Yes | 97.40% |

The biggest difference by far was using ImageNet pre-training. Without
it the model struggled a lot. Adding augmentation helped a bit, and
dropping the learning rate gave the biggest single improvement by
protecting the pre-trained features during fine-tuning.

---

## Datasets Used

**PlantVillage**  
54,306 colour leaf photos across 38 disease classes and 14 crops.  
All images show a single leaf against a plain background.  
Link: https://www.kaggle.com/datasets/abdallahalidev/plantvillage-dataset

**PlantDoc**  
2,569 real outdoor field images across 27 classes.  
I used this to test how well my model handles real-world photos
compared to the controlled PlantVillage images. The accuracy dropped
significantly which shows the domain shift problem is real.  
Link: https://github.com/pratikkayal/PlantDoc-Dataset

---

## Web Application

I built a simple web app using Gradio so that farmers or anyone else
can upload a leaf photo and get a disease prediction without needing
to know anything about machine learning. You just upload an image and
it shows the top predicted disease with a confidence score and two
alternative options.

The app code is in the notebook — run the last cell to launch it.

---

## References

Key papers I used in this project:

- He et al. (2016) — Deep Residual Learning for Image Recognition (ResNet)
- Hughes and Salathe (2015) — PlantVillage Dataset
- Mohanty et al. (2016) — Deep Learning for Plant Disease Detection
- Singh et al. (2020) — PlantDoc Dataset
- Kingma and Ba (2015) — Adam Optimiser
- Kirkpatrick et al. (2017) — Catastrophic Forgetting
