Periodontal Ligament Segmentation using U-Net

Overview

This project focuses on segmenting the periodontal ligament (PDL) using U-Net.

Dataset
	•	The dataset consists of 100 high-resolution PDF images (~7000×4000), where dental specialists manually marked the periodontal ligament regions using a red pen on a tablet.
	•	Challenges:
	•	The large image size makes processing difficult.
	•	The manual annotations may be imprecise, affecting segmentation accuracy.

Preprocessing (preprocess.ipynb)

This script extracts the input images and segmentation masks from PDF files.

Steps:
	1.	Convert to Grayscale & Remove Borders
	•	Convert RGB to Grayscale:

gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)


	•	Detect and remove black borders using contours.

	2.	Extract Red Annotations (PDL Regions) Using HSV Color Space
	•	Convert RGB to HSV:

hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)


	•	Define HSV ranges to detect red markings:

lower_red1 = np.array([0, 120, 70])  # Low-saturation red  
upper_red1 = np.array([10, 255, 255])  
lower_red2 = np.array([170, 120, 70])  # High-saturation red  
upper_red2 = np.array([180, 255, 255])  


	3.	Fill the Segmentation Mask
	•	The provided annotations only outline the PDL region, so the interior must be filled.
	•	Contour Filling:

contours, _ = cv2.findContours(filled_mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_NONE)
cv2.drawContours(mask, contours, -1, (255), thickness=cv2.FILLED)


	•	Handling Disconnected Annotations:
	•	Fill small gaps (≤ 5×5 pixels).
	•	Downscale the image, detect contours, and upscale again to minimize information loss.
	•	Iteratively increase the filling size from 5×5 to 100×100 for optimal results.

Model & Training (unet.ipynb)

Preprocessing:
	•	Convert to grayscale and apply standard scaling:

normalized_image = (image - mean) / std



Challenges in Applying U-Net to Large Images
	1.	U-Net requires square images with power-of-2 dimensions (e.g., 256×256, 512×512).
	2.	Directly loading the full images in a DataLoader causes memory overflow.

Solutions:

✅ Crop and Pad: Center-crop the image and pad it into a square shape.
✅ Resize: Reduce image size while preserving key features.
✅ Sliding Window Approach (Chosen Method):
	•	Divide the image into 1024×1024 patches, ensuring that each patch contains meaningful segmentation regions.

Training:
	•	Batch Size: 1
	•	Architecture: Standard U-Net (no modifications to depth or layers).
	•	Initial Result:
	•	The model predicts only black images (no segmentation).
	•	Likely due to a severe class imbalance → Most patches contain only background (0s), with few containing the periodontal ligament.

Addressing Class Imbalance

Strategies to Improve Segmentation Performance:
	1.	Hard Negative Mining:
	•	Increase sampling frequency of patches containing periodontal ligament (e.g., train on PDL patches 3× more often than background patches).
	2.	Focal Loss:
	•	Use Focal Loss instead of Cross Entropy to focus training on difficult cases.
	3.	ROI-Based Segmentation (Region Proposal + Segmentation):
	•	Step 1: Use Faster R-CNN or YOLO to detect the periodontal ligament region.
	•	Step 2: Apply U-Net only on detected regions to improve efficiency.
	4.	Semi-Supervised Learning:
	•	Leverage unlabeled data using self-training or consistency regularization techniques.

Conclusion & Next Steps

This project tackles large-scale medical image segmentation by:
	•	Developing a robust preprocessing pipeline for extracting high-quality segmentation masks.
	•	Addressing image size limitations using patch-based training.
	•	Proposing class imbalance handling techniques to improve U-Net’s segmentation performance.

🔹 Next Steps: Experiment with Hard Negative Mining, Focal Loss, and ROI-based segmentation to enhance results. 🚀
