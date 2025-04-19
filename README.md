# Counterfeit Gold Coin Detection

This repository documents the implementation of the paper **"Statistical edge-based feature selection for counterfeit coin detection"** by Hussein Sheikh Wassouf. The project focuses on detecting counterfeit coins through advanced image processing and statistical edge feature analysis.

## 📌 Project Overview
Counterfeit coins are a growing concern, with a reported 12.03% increase in detected counterfeit coins between 2017-2018. Traditional detection methods (weight, sound, chemical tests) are no longer reliable due to sophisticated forgery techniques. This method leverages **edge-based features** (width, thickness, orientation) to distinguish genuine coins from counterfeit ones. The original paper achieved **99.4% accuracy** on the Danish Coin Dataset, while this prototype uses a custom-collected dataset.

## 🎯 Problem Statement
- Counterfeit coins exhibit subtle edge defects (wider, taller, detached, or missing strokes) invisible to traditional methods.
- Manual inspection is time-consuming and requires expertise.
- **Goal**: Automate counterfeit detection using statistical edge analysis and machine learning.

## 🛠 Methodology
The pipeline involves 8 key steps:
1. **Gold Segmentation/Cropping**  
   - Use **HSV thresholding** to isolate the coin from the background.
   - Output: Cropped circular coin image (e.g., 905x905 pixels).

2. **Choosing Reference Coins**  
   - Select multiple reference coins (genuine and worn) to account for natural variations in wear and contamination.

3. **Rotation Alignment**  
   - Rotate test coins to match reference coin orientation using **Euclidean distance minimization** across 360 degrees.

4. **Defect Map Extraction**  
   - Apply **Sobel edge detection** on reference and rotated test images.
   - Generate a defect map via **pixel-wise difference** to highlight discrepancies.

5. **ROI Extraction**  
   - Crop regions of interest (ROI) from defect maps to reduce computational load.

6. **Feature Extraction**  
   - Extract edge metrics:  
     - Horizontal/vertical edge counts (`Nεh`, `Nεv`).  
     - Edge lengths, positions, directions (horizontal, ±45° diagonals).  
     - Similarity metrics (SSIM, MSE, SNR).  
   - Two feature dataframes:  
     - **Dataframe A**: 52,584 rows × 14 columns.  
     - **Dataframe B**: Reduced to 26,292 rows × 15 columns.  

7. **Dimensionality Reduction**  
   - Apply **PCA** with Min-Max scaling, retaining 97% variance with 6 components.

8. **Classification**  
   - Train/test classifiers on the reduced feature set (specific classifiers not listed in the presentation).

## 📂 Dataset
- **Original Paper Dataset**: Danish Coin Dataset (4 subsets, 2,264 images each).  
  - High-resolution grayscale images (3500x3500 pixels) scanned via 2D scanner.  
- **Prototype Dataset**:  
  - **Custom-collected dataset** (due to unavailability of the Danish Dataset).  
  - Captured using a digital microscope.  
  - Split into:  
    - **Reference** (1 image),  
    - **Real** (2 images),  
    - **Fake** (4 images).  

## 📊 Results
- The original paper achieved **99.4% accuracy** on the Danish Coin Dataset.  
- Prototype testing on the custom dataset showed clear distinction between real and fake defect maps:  
  - **Real coins**: Organized, consistent edges.  
  - **Counterfeit coins**: Chaotic, irregular edges.  

## 🏁 Conclusion
This method effectively identifies counterfeit coins by analyzing edge defects, outperforming traditional techniques. While the original paper validated results on the Danish Dataset, this prototype demonstrates feasibility using a smaller custom dataset. Future work includes expanding the custom dataset and testing additional classifiers for robustness.

## 🔗 References
- Paper: *"Statistical edge-based feature selection for counterfeit coin detection"* by Hussein Sheikh Wassouf.  

---

*Note: Code implementation is not yet available. This repository serves as a documentation hub for the methodology. Contributions or inquiries are welcome!*
