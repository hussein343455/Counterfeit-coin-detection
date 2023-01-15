# Counterfeit-coin-detection
-implementing a statistical edge-based feature selection method based on a research paper **("Statistical edge-based feature selection for counterfeit coin detection")** with the use of a microscope to collected data/images of real and fake coins.


-This paper presents a solution for detecting counterfeit coins, by providing a method based on edge differences. The method compares the edge width, edge thickness, number of horizontal and vertical edges, the total number of edges, and number of pixels. This approach has been experimentally established by other related works [9, 16, 21] that the edges of counterfeit coin stamp are the prime indicator to distinguish between genuine and counterfeit coins, since even the high-quality forged coins have wider, taller, detached or missing strokes. The references provided in the paper supports this method as an effective way to detect counterfeit coins.

## Steps to apply this implementation:

1-Coin Segmentation / Cropping: Isolate the coin within the images by removing any surrounding background, so that only the coin is visible.

2-Choosing Reference Coins: Both counterfeit and genuine coins can appear in variant qualities, from mint state to highly degraded coins, These variations may arise from coin wear or contamination caused by daily use, The variation appears clearly on the edge strokes, which protrude from the background of the coin. Therefore, considering multiple reference coins having different wear levels to compare to every test coin can highly render those variations in between interclass coins again this set of reference coins should represent all real coins. All the reference coins should be in the exact same position/rotation

3-Rotating the input coin to match the exact rotation of the reference coins

4-feature extraction a lot of details about this in the paper.

5-PCA to reduce the extracted features dimensions.

6-Trained a classfayer on the extracted data from the two classes (real , fake).


This is the microscope i used:


![Capture](https://user-images.githubusercontent.com/57813196/212535949-37dacb9e-ed4b-42ba-acbd-72fe664fa641.PNG)

