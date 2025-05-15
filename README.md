# P2M-signal-compression
# Wavelet Compression of Energy Consumption Signals

This project explores the application of wavelet transforms combined with estimated Huffman coding for compressing energy consumption signals from a dataset. The goal is to reduce the size of the data while maintaining the fidelity of the signal, assessed using the Root Mean Squared Error (RMSE).

## Table of Contents

- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Analysis of Results](#analysis-of-results)
- [How to Run the Code](#how-to-run-the-code)
- [Dependencies](#dependencies)
- [Acknowledgements](#acknowledgements)

## Project Overview

The project focuses on compressing time-series energy consumption data for various household appliances using different wavelet families. The process involves:

1.  Loading and cleaning the energy consumption dataset.
2.  Isolating the energy consumption signal for specific appliances.
3.  Applying a Discrete Wavelet Transform (DWT).
4.  Thresholding wavelet coefficients based on a `keep_ratio` to achieve compression.
5.  Reconstructing the signal from the compressed coefficients.
6.  Estimating the compressed size using the entropy of the non-zero coefficients (approximating Huffman coding).
7.  Evaluating the reconstruction quality using RMSE.
8.  Visualizing the original and reconstructed signals using Plotly.

The project also includes an initial exploration of unique equipment names present in the dataset.

## Dataset

The project uses a CSV file named `sfmconnect.disaggregationP2M.csv`. This dataset contains energy consumption data for various appliances over time. The relevant columns used are:

-   `date`: Timestamp of the reading (in milliseconds).
-   `equipments[0].equipment.name`: Name of the primary appliance.
-   `equipments[0].consumption.Active_energy`: Active energy consumption for the primary appliance.
-   `equipments[1].equipment.name`: Name of a secondary appliance (used for finding unique names).
-   `equipments[2].equipment.name`: Name of a tertiary appliance (used for finding unique names).

The dataset is expected to be located at `/content/sample_data/sfmconnect.disaggregationP2M.csv` in the Google Colab environment.

## Methodology

The core compression method implemented in the `compress_with_stats` and `compress_wavelet_huffman` functions follows these steps:

1.  **Wavelet Decomposition:** The `pywt.wavedec` function is used to perform a multi-level wavelet decomposition of the signal, yielding a set of wavelet coefficients.
2.  **Thresholding:** A threshold is calculated based on the `keep_ratio` (proportion of coefficients to keep). Coefficients with absolute values below this threshold are set to zero.
3.  **Reconstruction:** The signal is reconstructed from the thresholded coefficients using `pywt.waverec`.
4.  **RMSE Calculation:** The Root Mean Squared Error is calculated between the original and reconstructed signals to quantify the reconstruction error.
5.  **Estimated Huffman Size:** For the `compress_wavelet_huffman` function, the entropy of the non-zero coefficients is calculated. This entropy, multiplied by the number of non-zero coefficients, provides an estimate of the size of the compressed data using Huffman coding.
6.  **Compression Ratio:** The ratio of the estimated compressed size to the original signal size (in bytes) is calculated.

The project tests several common wavelet families: 'db8', 'sym8', 'coif5', 'bior3.5', and 'haar'.

## Analysis of Results

The project provides output for the compression results on several appliance signals:

-   micro-onde
-   refrigerator
-   clim-salle
-   clim-salle-re
-   eclairage-salle
-   clim\_port
-   retro-projecteur

For each appliance and wavelet, the RMSE, original size, transmitted size (number of non-zero coefficients), and compression ratio are reported. The interactive plots visualize the original signal and the reconstructed signals for comparison.

A summary table and analysis are provided for the 'retro-projecteur' signal using estimated Huffman coding, highlighting the effectiveness of different wavelets in achieving high compression ratios with minimal reconstruction error.

Key observations from the analysis:

-   Wavelets like 'db8', 'sym8', and 'coif5' generally show good compression with low RMSE.
-   'bior3.5' and 'haar' may not always result in significant compression depending on the signal characteristics.
-   Some signals ('clim-salle-re') may be less compressible by these methods due to their specific properties (e.g., limited variation, short length).
-   The wavelet + Huffman estimation shows promising compression ratios, especially with 'haar' for the 'retro-projecteur' signal.

## How to Run the Code

The code is designed to be run in a Google Colab environment.

1.  Upload the `sfmconnect.disaggregationP2M.csv` file to the `/content/sample_data/` directory in your Colab session or link your Google Drive and update the `file_path` accordingly.
2.  Run the notebook cells sequentially.

The notebook performs the following actions:

-   Mounts Google Drive (if enabled).
-   Installs necessary libraries (`PyWavelets`, `pandas`, `numpy`, `scikit-learn`, `plotly`).
-   Identifies unique equipment names in the dataset.
-   Applies wavelet compression to the signals of several specific appliances ('micro-onde', 'refrigerator', 'clim-salle', 'clim-salle-re', 'eclairage-salle', 'clim\_port', 'retro-projecteur').
-   Generates interactive plots comparing the original and reconstructed signals.
-   Provides a detailed analysis for the 'retro-projecteur' signal with estimated Huffman coding.
-   (Optional) Generates a visual representation of the compression pipeline using Graphviz.

## Dependencies

The project requires the following Python libraries:

-   `pandas`
-   `numpy`
-   `pywavelets` (`PyWavelets`)
-   `scikit-learn`
-   `plotly`
-   `csv` (standard library)
-   `scipy` (for `entropy`)
-   `graphviz` (for pipeline visualization - optional)

These dependencies are installed at the beginning of the notebook.

## Acknowledgements

This project utilizes the `PyWavelets`, `pandas`, `numpy`, `scikit-learn`, and `plotly` libraries for data processing, wavelet analysis, evaluation, and visualization.
