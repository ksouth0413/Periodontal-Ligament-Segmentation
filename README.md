다음은 README.md 파일에 바로 붙여넣을 수 있도록 Markdown 형식으로 정리한 내용입니다.

# 🦷 Periodontal Ligament Segmentation using U-Net

## 📌 Overview  
This project focuses on **segmenting the periodontal ligament (PDL)** using **U-Net**.

### 📝 Dataset  
- **100 high-resolution PDF images (~7000×4000)**
- Annotated by **dental specialists** using a red pen on a tablet.
- **Challenges:**
  - Large image size makes processing difficult.
  - Manual annotations may be imprecise, affecting segmentation accuracy.

---

## 🔄 Preprocessing (`preprocess.ipynb`)  
This script extracts **input images and segmentation masks** from PDF files.

### **1️⃣ Convert to Grayscale & Remove Borders**
```python
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

	•	Detect and remove black borders using contours.

2️⃣ Extract Red Annotations (PDL Regions) Using HSV Color Space

hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)

lower_red1 = np.array([0, 120, 70])  # Low-saturation red  
upper_red1 = np.array([10, 255, 255])
lower_red2 = np.array([170, 120, 70])  # High-saturation red  
upper_red2 = np.array([180, 255, 255])

3️⃣ Fill the Segmentation Mask
	•	The original annotations only outline the PDL region.
	•	Fill the interior to create complete segmentation masks.

contours, _ = cv2.findContours(filled_mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_NONE)
cv2.drawContours(mask, contours, -1, (255), thickness=cv2.FILLED)

4️⃣ Handling Disconnected Annotations
	•	Fill small gaps (≤ 5×5 pixels).
	•	Downscale the image, detect contours, then upscale again to reduce information loss.
	•	Iteratively increase filling size from 5×5 to 100×100.

🎯 Model & Training (unet.ipynb)

Preprocessing
	•	Convert to grayscale and apply standard scaling:

normalized_image = (image - mean) / std

Challenges in Applying U-Net to Large Images

❌ U-Net requires square images with power-of-2 dimensions (e.g., 256×256, 512×512).
❌ Directly loading full images in a DataLoader causes memory overflow.

Solutions

✅ Crop & Pad: Center-crop the image and pad it into a square shape.
✅ Resize: Reduce image size while preserving key features.
✅ Sliding Window (Chosen Method):
	•	Divide images into 1024×1024 patches, ensuring meaningful segmentation per patch.

Training
	•	Batch Size: 1
	•	Model: Standard U-Net (no modifications to depth or layers).
	•	Initial Result:
	•	Model predicts only black images → likely due to Class Imbalance (most patches contain only background).

⚖️ Addressing Class Imbalance

Strategies to Improve Segmentation Performance

1️⃣ Hard Negative Mining
	•	Increase training frequency for PDL-containing patches (e.g., sample PDL patches 3× more often).

2️⃣ Focal Loss
	•	Replace Cross Entropy with Focal Loss to focus on difficult cases.

3️⃣ ROI-Based Segmentation (Region Proposal + Segmentation)
	•	Step 1: Use Faster R-CNN or YOLO to detect the periodontal ligament region.
	•	Step 2: Apply U-Net only on detected regions to improve efficiency.

4️⃣ Semi-Supervised Learning
	•	Use unlabeled data with self-training or consistency regularization.

🚀 Conclusion & Next Steps

This project addresses large-scale medical image segmentation by:
✅ Developing a robust preprocessing pipeline for high-quality masks.
✅ Using patch-based training to overcome memory issues.
✅ Proposing Class Imbalance solutions for better U-Net performance.

🔹 Next Steps: Experimenting with Hard Negative Mining, Focal Loss, and ROI-based segmentation to further enhance results. 🚀

### **📌 최종 정리**  
✔ **GitHub README.md** 형식에 맞게 **Markdown 문법 적용**  
✔ **코드 블록(```python```) 활용** → 가독성 향상  
✔ **이모지 & 굵기 강조** → 깔끔한 시각적 구성  

바로 GitHub에 붙여넣으면 완벽한 `README.md`가 됩니다! 🚀
