# Lab 5 - Edge Detection and Segmentation

This script loads a color image, converts it to grayscale and double precision, then applies several classic edge detectors and a simple segmentation workflow. The figures show the key outputs for each step. 

## 0) Setup

Reads `peppers.png`, converts to grayscale with `rgb2gray`, and to double precision with `im2double`. This gives a single-channel image `I` ready for filtering. 

<img width="1045" height="420" alt="Figure_1" src="https://github.com/user-attachments/assets/1d31fe7b-c95c-4692-bff8-edd63b2bfc64" />


## 1) Basic derivative filters: Sobel and Prewitt

Computes binary edge maps using `edge(I,'Sobel')` and `edge(I,'Prewitt')`. Both are gradient-based masks that emphasize horizontal and vertical intensity changes, then threshold to produce edges. A montage displays them side by side for quick comparison. 

<img width="883" height="687" alt="Figure_2" src="https://github.com/user-attachments/assets/450b5f2a-0b15-4687-9801-615bce5efe9c" />


## 2) Canny detector

Runs `edge(I,'Canny',[0.05 0.2])`. Canny is a multi-stage method: smoothing, gradient computation, non-maximum suppression to thin edges, then hysteresis with low and high thresholds to link true edges and reject noise. The figure shows the resulting thin, connected edges. 

<img width="883" height="687" alt="Figure_3" src="https://github.com/user-attachments/assets/3c5de21e-0946-412e-bb29-3524a2b86556" />

## 3) Laplacian of Gaussian (LoG)

Applies `edge(I,'log')`. LoG smooths the image with a Gaussian, then uses the Laplacian to find zero-crossings that indicate edges. This can capture fine detail and closed contours, often with more texture sensitivity than Canny. 

<img width="1045" height="420" alt="Figure_4" src="https://github.com/user-attachments/assets/d3acb174-f507-49a3-99a9-2213ff74023d" />

## 4) Segmentation with Otsu threshold

Computes a global threshold with `graythresh(I)` and binarizes using `imbinarize`. Otsu’s method analyzes the grayscale histogram and chooses the threshold that best separates foreground and background, producing a binary mask `BW`. A montage shows the original image and the mask. 

<img width="880" height="687" alt="Figure_5" src="https://github.com/user-attachments/assets/011a5fe5-b64d-4a07-bce5-7c3f0b7bad44" />

## 5) Region labeling and visualization

Labels connected components in `BW` with `bwlabel`, then converts labels to a color image using `label2rgb`. The title includes the count of detected regions to summarize the segmentation outcome. 

---

## Reflections

* Thinnest, cleanest edges
  Canny, because non-maximum suppression thins edges and hysteresis removes weak noise responses.

* Why Canny often outperforms simple gradient filters
  It combines smoothing, accurate gradient localization, edge thinning, and dual-threshold linking, which yields cleaner and more continuous contours.

* How Otsu relates to histogram-based thresholding
  It picks the threshold that maximizes between-class variance in the grayscale histogram, which is a principled way to split foreground and background without manual tuning.
