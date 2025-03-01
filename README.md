# 🦷 Periodontal Ligament Segmentation using U-Net  

## 1. Project Overview  
> [!NOTE]  
> This project focuses on **segmenting the periodontal ligament (PDL)** using **U-Net**.  
> The dataset consists of **100 high-resolution (7000×4000) PDF images**, annotated by **dental specialists** with a red pen.  
> Challenges include **large image size** and **imprecise manual annotations**, which affect segmentation accuracy.  

This project covers **data preprocessing, U-Net model implementation, and class imbalance handling** for medical image segmentation.  
&#9654; [see details](https://github.com/your-repo-link)  

---

## 2. Preprocessing (`preprocess.ipynb`)  
> [!NOTE]  
> This script extracts **input images and segmentation masks** from the PDF dataset.

### 🔹 Key Steps  
✅ Convert RGB to **Grayscale** & remove black borders.  
✅ Extract **red annotations** (PDL regions) using **HSV color space**.  
✅ **Fill segmentation masks** to create fully labeled regions.  
✅ Handle **disconnected annotations** using contour filling & iterative patch expansion.  

&#9654; [see details](https://github.com/your-repo-link/preprocess)  

---

## 3. Model & Training (`unet.ipynb`)  
> [!NOTE]  
> Implementing U-Net for **large-scale medical image segmentation** and addressing **class imbalance issues**.

### 🔹 Challenges & Solutions  
❌ **U-Net requires square images (power-of-2 sizes).**  
✔ **Sliding Window Approach:** Patch-based training with **1024×1024 patches**.  
❌ **Loading full images causes memory overflow.**  
✔ **Patch extraction & efficient batch processing.**  

&#9654; [see details](https://github.com/your-repo-link/unet)  

---

## 4. Addressing Class Imbalance  
> [!TIP]  
> Most patches contain **only background pixels**, leading to severe class imbalance.

### 🔹 Solutions  
🔹 **Hard Negative Mining:** Increase sampling frequency for **PDL patches**.  
🔹 **Focal Loss:** Reduce the impact of easily classified background regions.  
🔹 **ROI-Based Segmentation:** First detect PDL regions using **YOLO/Faster R-CNN**, then apply segmentation.  
🔹 **Semi-Supervised Learning:** Use unlabeled data with self-training or consistency regularization.  

&#9654; [see details](https://github.com/your-repo-link/class-imbalance)  

---

## 5. Next Steps  
✅ Further experiments with **Hard Negative Mining, Focal Loss, and Region Proposal techniques**.  
✅ Optimization for **better segmentation accuracy in large-scale medical images**.  
✅ Applying **semi-supervised learning** to leverage unlabeled data.  

&#9654; [see details](https://github.com/your-repo-link)  
