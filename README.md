# Lab 4: Noise and Filtering

## Learning outcomes
* Model common noise types (Gaussian, salt-and-pepper).
* Measure mean-square error (MSE) and signal-to-noise ratio (SNR).
* Compare mean, median, and Gaussian filters for noise reduction.

---

## 1) Add different noise types

**Code snippet:**
I_gauss = imnoise(I,'gaussian',0,0.01);
I_sp = imnoise(I,'salt & pepper',0.05);
figure; montage({I, I_gauss, I_sp},'Size',[1 3]);
title('Original | Gaussian noise | Salt & pepper noise');

yaml
Copiar código

**Explanation:**
In this section, Gaussian and salt & pepper noise are added to the original grayscale image.  
* Gaussian noise * introduces small random variations across all pixels.  
* Salt and pepper noise * introduces random black (0) and white (1) pixels.

The corresponding image results are stored in the *screenshots/* folder.

---

## 2) Compute simple quality metrics

**Code snippet:**
MSE_gauss = immse(I_gauss, I);
MSE_sp = immse(I_sp, I);
fprintf('MSE Gaussian: %.4f | MSE S&P: %.4f\n', MSE_gauss, MSE_sp);

SNR_gauss = snr(I, I_gauss - I);
SNR_sp = snr(I, I_sp - I);
fprintf('SNR Gaussian: %.2f dB | SNR S&P: %.2f dB\n', SNR_gauss, SNR_sp);

yaml
Copiar código

**Explanation:**
* MSE (Mean Square Error) * measures the average squared difference between the noisy and original image.  
* SNR (Signal-to-Noise Ratio) * quantifies how much desired signal exists relative to the background noise.

* MSE is higher for salt & pepper noise, indicating stronger distortion.*  
* SNR is lower for salt & pepper noise.*

Results are shown in the console and saved visually in the *screenshots/* folder.

---

## 3) Linear filtering (mean, Gaussian)

**Code snippet:**
h_avg = fspecial('average',3);
I_avg_gauss = imfilter(I_gauss,h_avg,'replicate');
I_avg_sp = imfilter(I_sp,h_avg,'replicate');

h_gauss = fspecial('gaussian',[3 3],0.7);
I_gauss_gauss = imfilter(I_gauss,h_gauss,'replicate');

yaml
Copiar código

**Explanation:**
* The average filter * reduces noise by averaging neighboring pixels, but it tends to blur edges.  
* The Gaussian filter * applies weighted smoothing that better preserves image structure while reducing Gaussian noise.

The filtered images are saved separately and shown in *screenshots/*.

---

## 4) Non-linear filtering (median)

**Code snippet:**
I_med_gauss = medfilt2(I_gauss,[3 3]);
I_med_sp = medfilt2(I_sp,[3 3]);
figure; montage({I_avg_sp, I_med_sp, I_avg_gauss, I_med_gauss},'Size',[2 2]);
title('Top: Avg vs Median (S&P) | Bottom: Avg vs Median (Gaussian)');

yaml
Copiar código

**Explanation:**
* The median filter * replaces each pixel with the median of its neighborhood, making it very effective against salt & pepper noise.

* Median filter removes most of the salt & pepper noise while preserving edges.*  
* For Gaussian noise, median filtering is less effective than linear methods.*

The visual comparison results are stored in *screenshots/*.

---

## 5) Compare metrics after filtering

**Code snippet:**
fprintf('After filtering, MSE S&P avg=%.4f, med=%.4f\n',...
immse(I_avg_sp,I), immse(I_med_sp,I));

yaml
Copiar código

**Explanation:**
MSE is recalculated after filtering to measure improvement.

* The median filter achieves a much lower MSE for salt & pepper noise.*  
* The Gaussian filter performs better for Gaussian noise.*

Metric results are visible in the MATLAB console. Filtered image outputs are saved in the *screenshots/* folder.

---

## 6) Reflections

**Which noise is best removed by median filter? Why?**  
The median filter is most effective for salt & pepper noise because it removes extreme pixel values without affecting edges significantly.

**Why does linear filtering blur edges more?**  
Linear filters average all neighboring values, including those across edges, which leads to loss of sharpness and detail.

**How could we design adaptive filters to preserve detail?**  
Adaptive filters can analyze local variance or edge presence and adjust the degree of smoothing accordingly, reducing noise in uniform areas while maintaining detail in textured or edge regions.
