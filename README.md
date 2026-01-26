# SSP-protease-inhibitor-prediction

# Pipeline proposed for discovering novel protease inhibitor at plant-pathogen interface

[![Project Status: Active – The project has reached a stable, usable state and is being actively developed.](https://www.repostatus.org/badges/latest/active.svg)](https://www.repostatus.org/#active)

This repository contains the code and notebooks for **SSP-protease-inhibitor-prediction**, a deep learning pipeline for identifying small secretory proteins (SSPs) with no known Pfam domain as protease inhibitors based on mature protein sequence and structure. All modules run directly in Google Colab—no local installation required. The free tier GPU can handle up to ~4000 SSPs efficiently; for larger datasets, we recommend **Colab Pro** (for extended runtime/GPU) or local installation with GPU acceleration.

## Fine-Tuned Models (Openly Available on Hugging Face)

The core prediction models are publicly hosted and free to use/download:

- **[PIPES-M](https://huggingface.co/MuthuS97/PIPES-M)**: Fine-tuned ESM-2 (150M parameters) binary classifier for potential protease inhibitor from primary sequences only (no structure needed). Trained on MEROPS/UniProt data for small proteins (<250 AA). Use for fast sequence-based screening.
- **[PIP-BERT](https://huggingface.co/MuthuS97/PIP-BERT)**: Fine-tuned ProtBERT classifier for rapid screening of potential protease inhibitors. Outputs class probabilities and confidence scores. Ideal for large-scale sequence datasets.
- **[structuralmodule-protease_inhibitors](https://huggingface.co/MuthuS97/structuralmodule-protease_inhibitors)**: Unsupervised one-class autoencoder (via PyOD) that filters protein structures based on reconstruction error. Trained on ~18k curated protease inhibitor structures from MEROPS/AlphaFold. 

These models power the pipeline and can also be used independently (see example notebooks linked on each HF page).

## Authors
* __Muthusaravanan S__ <a itemprop="sameAs" content="https://orcid.org/0000-0002-0600-4534" href="https://orcid.org/0000-0002-0600-4534" target="orcid.widget" rel="me noopener noreferrer" style="vertical-align:top;"><img src="https://orcid.org/sites/default/files/images/orcid_16x16.png" style="width:1em;margin-right:.5em;" alt="ORCID iD icon"></a>   </br>
_Department of Biological Sciences, Birla Institute of Technology and Science, Pilani, India_

* __Balakumaran Chandrasekar__ <a itemprop="sameAs" content="https://orcid.org/0000-0001-5696-6777" href="https://orcid.org/0000-0001-5696-6777" target="orcid.widget" rel="me noopener noreferrer" style="vertical-align:top;"><img src="https://orcid.org/sites/default/files/images/orcid_16x16.png" style="width:1em;margin-right:.5em;" alt="ORCID iD icon"></a>   </br>
_Indian Institute of Technology Jodhpur_

## Abstract

> To be included...

## Usage Instructions

To use protease-inhibitor-prediction, first prepare mature protein sequences by removing signal peptides using an external tool such as SignalP (highly recommended and required for accurate sequence-based predictions). Then run the **Sequence Module** Colab notebook to screen candidates with the fine-tuned PIPES-M and PIP-BERT models (handles up to ~4000 sequences on free Colab GPU). For promising hits, obtain 3D structures either via the **ESMFold API** notebook (de novo prediction) or the **AlphaFold Fetch** notebook (download precomputed models by uploading UniProt IDs in a .txt file). Next, apply the **Structural Module** notebook to filter candidates using the dedicated autoencoder model and retain only those with protease inhibitor-like structural features. Finally, for the retained candidates, model heterodimer interactions with target immune proteases using your preferred protein multimeric structure modeling platform. (GPU-accelerated Local ColabFold is strongly recommended for efficient screening). All steps except mutimeric complex modeling can be run directly in Google Colab with no installation required; simply open the notebooks and enable GPU runtime for optimal performance. Just follow the usage instrcution in each Notebook. 

## Colab Notebooks (Pipeline Modules)

1. **Sequence Module** — Initial screening with fine-tuned sequence based models  
   [Open in Colab](https://colab.research.google.com/drive/1v6fegGSLdlyv4GWx8C7sL22wZBunVtEr?usp=sharing)

2. **ESMFold API** — De novo structure prediction for screened candidates  (need positive hits mature sequences as .fasta)  
   [Open in Colab](https://colab.research.google.com/drive/1CML84K8KxbpijUnjN15uEoBxXgMQKeYL?usp=sharing)

3. **AlphaFold Fetch** — Download precomputed AlphaFold structures (need positive hits UniProt IDs as .txt)  
   [Open in Colab](https://colab.research.google.com/drive/1J9AxU9C3dt2s4VVAJ-RUskXHlIOwtyHH?usp=sharing)

4. **Structural Module** — Filter structures lacking protease inhibitor-like features  
   [Open in Colab](https://colab.research.google.com/drive/1JLhLpvXG4plzPtIliG_CJnYui6P8Pu1J?usp=sharing)

5. **Heterodimer Modeling** — Recommended tool for final interaction prediction  
   [GPU-accelerated ColabFold](https://github.com/sokrypton/ColabFold?tab=readme-ov-file#gpu-accelerated-search-with-colabfold_search)

**Note:** Free Colab has session limits (~12h runtime, occasional disconnects); Colab Pro removes most restrictions for heavy use.

## Computational Requirements

- **Google Colab** (free tier sufficient for <4000 sequences; Pro recommended for larger batches or longer runs).  
- GPU acceleration enabled (Runtime → Change runtime type → GPU).  
- No local install needed, but for very large-scale screening: local GPU setup is recommended. 

__If you use this tool or models, please cite:__ </br>
Muthusaravanan S et al. (in preparation).

Have data to contribute? 
----
We welcome datasets of known/putative protease inhibitors or SSPs to improve accuracy. Contact us!

> Open models and Colab access democratize protein discovery. We encourage sharing sequences/structures in public repositories (UniProt, PDB, Zenodo) to accelerate inhibitor engineering and biological insights.

__For inquiries, contact us.__

Contact 
----
Muthusaravanan S - [@Muthu_Sivaram](https://x.com/Muthu_Sivaram) - muthusaravanan.ind@gmail.com
