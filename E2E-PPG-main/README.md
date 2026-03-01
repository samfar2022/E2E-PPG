[![PyPI](https://img.shields.io/pypi/v/neurokit2.svg?logo=pypi&logoColor=FFE873)](https://pypi.org/project/e2epyppg/)
[![PyPI](https://img.shields.io/pypi/pyversions/neurokit2.svg?logo=python&logoColor=FFE873)](https://pypi.org/project/e2epyppg/)

# An End-to-End PPG Processing Pipeline for Wearables: From Quality Assessment and Motion Artifacts Removal to HR/HRV Feature Extraction (E2E-PPG)

Welcome to the PPG Signal Processing Pipeline, a comprehensive package designed for extracting accurate Heart Rate (HR) and Heart Rate Variability (HRV) data from Photoplethysmogram (PPG) signals.

## Introduction

This project provides a robust pipeline for processing PPG signals, extracting reliable HR and HRV parameters. The pipeline encompasses various stages, including filtering, signal quality assessment (SQA), signal reconstruction, peak detection and interbeat interval (IBI) extraction, and HR and HRV computation.

<img src="https://github.com/HealthSciTech/E2E-PPG/assets/67778755/896be83f-4709-4444-bac9-2fef0449f739" alt="overview" width="800" height="200">

## Preprocessing
The input raw PPG signal undergoes filtering to remove undesired frequencies. A second-order Butterworth bandpass filter is employed, allowing frequencies within a specific range to pass while rejecting frequencies outside.


## Signal Quality Assessment (SQA)
SQA involves identifying clean and noisy parts within PPG signals. Our SQA approach requires PPG signals in a fixed length, which necessitates segmenting the input signals. To this end, we apply a moving window segmentation technique, where the PPG signals are divided into overlapping segments, each spanning 30 seconds, by sliding a window over the signal. The SQA process includes PPG feature extraction and classification, employing a one-class support vector machine model to distinguish between "Reliable" and "Unreliable" segments.


<img src="https://github.com/HealthSciTech/E2E-PPG/assets/67778755/c0ffee6c-f7b5-4d27-9f34-34cb86a698b5" alt="seg" width="300" height="300">

<img src="https://github.com/HealthSciTech/E2E-PPG/assets/67778755/f63e40d3-74b3-497b-ac91-dc940e669f03" alt="sample" width="400" height="300">



## Noise Reconstruction

Noisy parts within PPG signals, shorter than a specified threshold, are reconstructed using a deep convolutional generative adversarial network (GAN). The GAN model includes a generator trained to produce synthetic clean PPG segments and a discriminator to differentiate real from synthetic signals. The reconstruction process is applied iteratively to ensure denoising.

<img src="https://github.com/HealthSciTech/E2E-PPG/assets/67778755/bb00f079-7341-4ac9-84e2-553eb6a62672" alt="rec-arch" width="550" height="300">
<br />
<br />
<br />
<img src="https://github.com/HealthSciTech/E2E-PPG/assets/67778755/8cf57fa6-94fc-4906-b4c3-8416fffced4e" alt="rec-iter" width="600" height="200">
<br />
<br />
<br />
<img src="https://github.com/HealthSciTech/E2E-PPG/assets/67778755/ef0ce7aa-ab34-4176-a0a3-9192c7bd94de" alt="rec-iter" width="480" height="300">




## Peak Detection and IBI Extraction
Systolic peaks in PPG signals are identified using a deep-learning-based method with dilated Convolutional Neural Networks (CNN) architecture. PPG peak detection enables the extraction of IBI values that serve as the basis for obtaining HR and HRV. IBI represents the time duration between two consecutive heartbeats and is computed by measuring the time interval between systolic peaks within the PPG signals. 


<img src="https://github.com/HealthSciTech/E2E-PPG/assets/67778755/5f1dce78-b1a5-4155-9400-744c71049648" alt="rec-arch" width="550" height="300">
<br />
<br />


![200779269-c0cfc80a-cb53-4dc7-91e3-7b7590977e7f](https://github.com/HealthSciTech/E2E-PPG/assets/67778755/82ba92d8-b012-4202-8e17-127b0a5df4e5)




## HR and HRV Extraction
HR and HRV parameters are computed from the extracted IBIs. A variety of metrics are calculated, including:

- HR: Heart rate
- MeanNN: The mean of the RR intervals
- SDNN: The standard deviation of the RR intervals
- SDANN: The standard deviation of average RR intervals
- SDNNI: The mean of the standard deviations of RR intervals
- RMSSD: The square root of the mean of the squared successive differences between adjacent RR intervals
- SDSD: The standard deviation of the successive differences between RR intervals
- CVNN: The standard deviation of the RR intervals (SDNN) divided by the mean of the RR intervals (MeanNN)
- CVSD: The root mean square of successive differences (RMSSD) divided by the mean of the RR intervals (MeanNN)
- MedianNN: The median of the RR intervals
- MadNN: The median absolute deviation of the RR interval
- MCVNN: The median absolute deviation of the RR intervals (MadNN) divided by the median of the RR intervals (MedianNN)
- IQRNN: The interquartile range (IQR) of the RR intervals
- Prc20NN: The 20th percentile of the RR intervals
- Prc80NN: The 80th percentile of the RR intervals
- pNN50: The proportion of RR intervals greater than 50ms, out of the total number of RR intervals
- pNN20: The proportion of RR intervals greater than 20ms, out of the total number of RR intervals
- MinNN: The minimum of the RR intervals
- MaxNN: The maximum of the RR intervals
- TINN: A geometrical parameter of the HRV, or more specifically, the baseline width of the RR intervals distribution obtained by triangular interpolation, where the error of least squares determines the triangle. It is an approximation of the RR interval distribution
- HTI: The HRV triangular index, measuring the total number of RR intervals divided by the height of the RR intervals histogram
- ULF: The spectral power of ultra low frequencies (by default, .0 to .0033 Hz)
- VLF: The spectral power of very low frequencies (by default, .0033 to .04 Hz)
- LF: The spectral power of low frequencies (by default, .04 to .15 Hz)
- HF: The spectral power of high frequencies (by default, .15 to .4 Hz)
- VHF: The spectral power of very high frequencies (by default, .4 to .5 Hz)
- LFHF: The ratio obtained by dividing the low frequency power by the high frequency power
- LFn: The normalized low frequency, obtained by dividing the low frequency power by the total power
- HFn: The normalized high frequency, obtained by dividing the low frequency power by the total power
- LnHF: The log transformed HF
- SD1: Standard deviation perpendicular to the line of identity
- SD2: Standard deviation along the identity line
- SD1/SD2: ratio of SD1 to SD2. Describes the ratio of short term to long term variations in HRV


## Usage

Install the required packages available in `requirements.txt`. 

See the `example.py` file for usage details. Load your PPG signal and call the `e2e_hrv_extraction()` function in `e2e_ppg_pipeline.py` to extract HR and HRV parameters. Define the sample rate and window length for HR and HRV extraction.



