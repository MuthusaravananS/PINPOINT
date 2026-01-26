---
language:
- en
library_name: pyod
tags:
- protein-structure
- anomaly-detection
- one-class
- autoencoder
- protease-inhibitor
- structural-filtering
- biology
- bioinformatics
- unsupervised-learning
datasets:
- MEROPS
- UniProt
- AlphaFold
model_name: Structural_module-protease_inhibitor
metrics:
- reconstruction_error
---

# Structural_module-protease_inhibitor

This model is an unsupervised, one-class deep learning autoencoder designed to filter protein structures. It identifies candidates that are structurally inconsistent with curated protease inhibitor (PI) features learned from known PI databases.

Use colab userinterface for making prediction from protein structure files in .cif format. 
[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1JLhLpvXG4plzPtIliG_CJnYui6P8Pu1J?usp=sharing)

## Model Description
An autoencoder model trained on the structural embedding  of known protease inhibitors and assigns a **reconstruction error** to inputs. High reconstruction errors indicate potential non-PIs based on structural features learned from the training distribution, allowing for the filtering of non-PI-like structures.


* **Model type:** Fully connected Autoencoder (via PyOD)
* **embeddings calculated using:** [RCSB embedding model](https://github.com/rcsb/rcsb-embedding-model)

## Intended Uses & Limitations

### Intended Use
* Structural filtering and pre-selection of protease inhibitor–like protein structures in large-scale datasets.
* Quality control for generated or predicted protein structures in the context of PIs.

### Limitations
* **Novel Folds:** Novel PI folds absent from the MEROPS training data may be incorrectly rejected (false positives for anomalies).
* **Not for Clinical Use:** This model is not intended for functional annotation or clinical decision-making.

## Training Data
The model was trained on **17,889 curated protease inhibitor structures**:
1.  **Source:** MEROPS database.
2.  **Mapping:** Sequences were mapped via similarity search against taxonomy-restricted UniProt datasets (fungi, plants, bacteria).
3.  **Structures:** Corresponding 3D structures were obtained from the **AlphaFold Protein Structure Database using uniprot Ids**.

## Technical Specifications

### Input Format
The model accepts **fixed-length continuous protein structure embeddings** derived from the RCSB embedding model. 
> **Important:** Embeddings must be standardized using the provided `scaler.pkl` before inference.

### Architecture
Implemented in PyTorch via the PyOD library:
* **Encoder:** Geometrically decreasing layers.
* **Bottleneck:** Latent representation layer.
* **Decoder:** Symmetric reconstruction layers.
* **Regularization:** Batch normalization and dropout.

### Training Procedure
* **Optimizer:** Adam with weight decay.
* **Hyperparameter Tuning:** Optimized via Bayesian optimization (Optuna, TPE sampler) on a 10% validation split (~1,789 structures).
* **Objective:** Minimize Mean Squared Error (MSE) reconstruction loss on validation set.

