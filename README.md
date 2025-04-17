# 🌿 Plant Disease Classification and Identification using Computer Vision

This project is a **hands-on exploration of computer vision techniques** to automatically identify **plant types** and their **specific diseases** using deep learning.

It is divided into two main stages:

1. **Plant Type Classification**  
2. **Disease Differentiation** using a **Siamese Network with Triplet Loss**

---

## 📁 Dataset

We used a subset of the [Plant Diseases Dataset](https://www.kaggle.com/datasets/vipoooool/new-plant-diseases-dataset) available on Kaggle. The dataset contains leaf images from various plant types, affected by different diseases, along with healthy samples.

- We used **9 plant types**:
  - Apple  
  - Cherry (including sour)  
  - Corn (maize)  
  - Grape  
  - Peach  
  - Pepper (bell)  
  - Potato  
  - Strawberry  
  - Tomato  

- A total of **33 distinct plant-disease combinations** (i.e., folders), including both diseased and healthy leaf samples.

### 📌 Example (Grape):
- `Grape___healthy`  
- `Grape___Black_rot`  
- `Grape___Esca_(Black_Measles)`  
- `Grape___Leaf_blight_(Isariopsis_Leaf_Spot)`

---

## 🚀 Project Pipeline

### 📌 Stage 1: Plant Type Classification

A **multi-class image classification model** is trained to predict the **plant type** from a leaf image.

- **Input:** Leaf image  
- **Output:** One of 9 plant types  

### 📌 Stage 2: Disease Differentiation using Siamese Network

Once the plant type is predicted, a **Siamese Network** with **Triplet Loss** is used to:
- **Learn similarity** between leaf images.
- Compare the test image with reference samples from each disease class of the predicted plant.
- Predict the **specific disease** (or healthy status) based on similarity.

- **Input:** Leaf image + predicted plant type  
- **Output:** Specific disease or healthy label  

---

## 🧪 Experimental Results

### 🧠 Stage 1: Plant Type Classification

#### 📉 On the Small Dataset  
- **Training Samples:** 1320  
- **Validation Samples:** 330  
- **Testing Samples:** 33  

| Model       | Pretrained | Train Acc | Val Acc | Test Acc | Train Time     | Val Time | Test Time |
|-------------|------------|-----------|---------|----------|----------------|----------|-----------|
| Custom CNN  | ❌         | 0.88      | 0.67    | 0.63     | 6s × 5 epochs  | 1s       | 1.48s     |
| VGG16       | ✅         | 0.9152    | 0.9242  | 0.9      | 7s × 20 epochs | 1s       | 1.85s     |
| DenseNet121 | ✅         | 0.9848    | 0.9697  | 0.966    | 7s × 10 epochs | 1s       | 3.5s      |

---

### 📊 On the Large Dataset  
- **Training Samples:** 60,930  
- **Validation Samples:** 15,231  
- **Testing Samples:** 33  

| Model    | Pretrained | Train Acc | Val Acc | Test Acc | Train Time      | Val Time | Test Time |
|----------|------------|-----------|---------|----------|-----------------|----------|-----------|
| Xception | ✅         | 0.9960    | 0.9935  | 1.0      | 375s × 4 epochs | 60s      | 2s        |

---

### 🧠 Stage 2: One/Few-Shot Learning with Siamese Network

We used **Xception** (pre-trained) as the feature extractor for our Siamese Network.

#### ⚙️ Training Details:
- **Classes:** 33 (disease + healthy)
- **Samples/Class:** 40 images from each class to create anchor-positive pairs (totaling 780)
- **Negative Samples:** Chosen from different classes 
- **Triplets Generated:** 25,740 for training, 1,485 for validation

#### 🔍 Testing Strategy:
Each test image was compared against **5 reference images from each class** of the predicted plant type. The **median distance** across those 5 comparisons was used to classify the disease.

#### 📈 Triplet Network Accuracy:
| Dataset      | Triplet Accuracy (Positive/Negative) | Full Classification Accuracy (Plant + Disease) |
|--------------|--------------------------------------|-----------------------------------------------|
| Train        | 0.999                                | 0.929                                         |
| Validation   | 0.93                                 | 0.845                                         |
| Test         | –                                    | 0.757                                         |
