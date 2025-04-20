# Counterfeit Turkish Gold Coin Detection (Altın) 

This repository documents the implementation of the paper "Statistical edge-based feature selection for counterfeit coin detection", adapted specifically for authenticating Turkish gold coins (locally called "altın"). The project focuses on detecting counterfeit çeyrek altın (¼-size coins) through advanced image processing and statistical edge feature analysis.

![Alt text](Images/1.jpg)

Turkish "çeyrek altın" coins - identical design across sizes but varying diameters (14mm-22mm). This project focuses on the 18mm çeyrek altın.

## 📌 Project Overview
Counterfeit coins are a growing concern, with a reported 12.03% increase in detected counterfeit coins between 2017-2018. Traditional detection methods (weight, sound, chemical tests) are no longer reliable due to sophisticated forgery techniques. This method leverages **edge-based features** (width, thickness, orientation) to distinguish genuine coins from counterfeit ones. The original paper achieved **99.4% accuracy** on the Danish Coin Dataset, while this prototype uses a custom-collected dataset.

**Key Insight**:  
*"It has been experimentally established by other related works [9, 16, 21] that the edges of counterfeit coin stamp are the prime indicator to distinguish between genuine and counterfeit coins, since even the high-quality forged coins have wider, taller, detached or missing strokes."*  
*(This line is directly quoted from the paper [1].)*

## 🎯 Problem Statement
- Counterfeit coins exhibit subtle edge defects (wider, taller, detached, or missing strokes) invisible to traditional methods.
- Manual inspection is time-consuming and requires expertise.
- **Goal**: Automate counterfeit detection using statistical edge analysis and machine learning.

## 🛠 Methodology
The pipeline involves 8 key steps:
1. **Gold Segmentation/Cropping**  
   - Use **HSV thresholding** along side other opencv methods to isolate the coin from the background.
   - Output: Cropped circular coin image (e.g., 905x905 pixels).
   
   ![Alt text](Images/Step1.png)

2. **Choosing Reference Coins**  
   - Select multiple reference coins (genuine and worn) to account for natural variations in wear and contamination, in our custom database one coin was set as the referance

3. **Rotation Alignment**  
   - Adapted the paper’s method: Rotated the test coin incrementally (1° steps across 360°) and calculated Euclidean distance between the reference and test coin’s edge maps at each angle.
   - The rotation angle with the minimum Euclidean distance (identified from the 360-value vector) was applied to align the test coin with the reference.
   - Result: Perfect edge overlap between reference and aligned test coin (critical for accurate defect detection).
   - 
   ![Alt text](Images/Step2.png)
     
4. **Defect Map Extraction**  
   - Apply **Sobel edge detection** on reference and rotated test images.
   - Generate a defect map via **pixel-wise difference** to highlight discrepancies.
   
   ![Alt text](Images/Step4.png)

5. **ROI Extraction**  
   - Method: Split the defect map into concentric donut-shaped regions (2D torus) and a central circle.
   - Parameters: Radius r = 50 for a 500x500 image → (500/2)/50 = 5 regions (4 torus + 1 center circle).
   - Final ROIs: Vertically split each region into halves → 10 total ROIs.
   - Impact: Skipping ROI extraction dropped accuracy from 99.7% to ~70% in tests on the custom çeyrek altın dataset.

   ![Alt text](Images/Step5.png)
   
6. Feature Extraction (Core of the Project)
   - **Edge Detection**:  
      - Scanned ROI pixel values row-wise to identify edges (transition from 0 → non-zero → 0).  
      - Calculated edge count (`Nεh`/`Nεv`), lengths, start/end positions in 3 directions:  
         1. Horizontal  
         2. +45° diagonal  
         3. -45° diagonal  
      - **Similarity Metrics**: Computed SNR, MSE, SSIM between test/reference ROIs.  
   **Data Pipeline**:  
      - **Dataframe A**: 52,584 rows × 14 columns.  
      - **Dataframe B**: 26,292 rows × 15 columns (Same data as A, optimized order to reduce computational load).
   **Example Workflow in case 6 reference coin are being used**:  
      1. Generate 6 defect maps (1 per reference coin).  
      2. Extract features → 6 dataframes → average into 1.  
      3. Final features retain only persistent edge patterns (suppress transient defects).

   ![Alt text](Images/Step6A.png)    ![Alt text](Images/Step6B.png)

8. **Dimensionality Reduction**  
   - Applied **Min-Max scaling** to normalize features.  
   - Reduced 15 features → **6 PCA components**, preserving 97% variance.  
   - Resulting shape: **26,292 × 6** (from 26,292 × 15), optimizing computational efficiency.  

9. **Classification**  
   Classifiers trained on the reduced feature set are presented in the results section.

## 📂 Dataset
- **Original Paper Dataset**: Danish Coin Dataset (4 subsets, 2,264 images each).  
  - High-resolution grayscale images (3500x3500 pixels) scanned via 2D scanner.  
- **Prototype Dataset**:  
  - **Custom-collected dataset**
  - Captured using a digital microscope croped images are 910x910 pixels
  - Split into:  
    - **Reference** (1 image),  
    - **Real** (2 images),  
    - **Fake** (4 images).  

   ![Alt text](Images/Data.png) ![Alt text](Images/Data2.png)
  
## 📊 Results
- The original paper achieved **99.4% accuracy** on the Danish Coin Dataset.  
- Prototype testing on the custom dataset showed clear distinction between real and fake defect maps:  
  - **Real coins**: Organized, consistent edges.  
  - **Counterfeit coins**: Chaotic, irregular edges.  

## 🏁 Conclusion
This method effectively identifies counterfeit coins by analyzing edge defects, outperforming traditional techniques. While the original paper validated results on the Danish Dataset, this prototype demonstrates feasibility using a smaller custom dataset. Future work includes expanding the custom dataset and testing additional classifiers for robustness.

## 🔗 References
- Paper: *"Statistical edge-based feature selection for counterfeit coin detection"*.  

---

*Note: Code implementation is not yet available. This repository serves as a documentation hub for the methodology. Contributions or inquiries are welcome!*
