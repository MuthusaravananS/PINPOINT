# **PINPOINT** – <u>P</u>rotease <u>I</u><u>N</u>hibitor <u>P</u>redicti<u>O</u><u>N</u> at <u>P</u>lant–pathogen <u>I</u>N<u>T</u>erface

# Open pipeline proposed for discovering novel protease inhibitor at plant-pathogen interface

[![Project Status: Active – The project has reached a stable, usable state and is being actively developed.](https://www.repostatus.org/badges/latest/active.svg)](https://www.repostatus.org/#active)

This repository contains the code and notebooks for **SSP-protease-inhibitor-prediction**, a deep learning pipeline for identifying small secretory proteins (SSPs) with no known Pfam domain as protease inhibitors based on mature protein sequence and structure. All modules run directly in Google Colab—no local installation required. The free tier GPU can handle up to ~4000 SSPs efficiently; for larger datasets, we recommend **Colab Pro** (for extended runtime/GPU) or local installation with GPU acceleration.

## Fine-Tuned Models (Openly Available on Hugging Face)

The core prediction models from this study were publicly hosted and free to use/download:

All models focus on small proteins (<250 AA) and are trained on curated MEROPS/UniProt/AlphaFold data.

## Available Models on Hugging Face

| Model | Type | Base | Key Features | Link |
|-------|------|------|--------------|------|
| **PIPES-M** | Binary sequence classifier | ESM-2 (150M) | Large-scale sequence-based screening | [![Open PIPES-M](https://img.shields.io/badge/%F0%9F%A4%97%20PIPES-M-blue?style=flat&logo=huggingface&logoColor=white)](https://huggingface.co/MuthuS97/PIPES-M) |
| **PIP-BERT** | Binary sequence classifier | ProtBERT | Large-scale sequence-based screening| [![Open PIP-BERT](https://img.shields.io/badge/%F0%9F%A4%97%20PIP-BERT-blue?style=flat&logo=huggingface&logoColor=white)](https://huggingface.co/MuthuS97/PIP-BERT) |
| **structuralmodule-protease_inhibitors** | Unsupervised one-class autoencoder (PyOD) | RCSB embeddings | Filters non-inhibtior protein structures by reconstruction error, trained on ~18k PI structures | [![Open Structural Module](https://img.shields.io/badge/%F0%9F%A4%97%20Structural%20Module-blue?style=flat&logo=huggingface&logoColor=white)](https://huggingface.co/MuthuS97/structuralmodule-protease_inhibitors) |

### Quick Overview

- **[PIPES-M](https://huggingface.co/MuthuS97/PIPES-M)**  
  Fine-tuned **ESM-2** (150M params) binary classifier. Predicts if a protein sequence is a potential protease inhibitor using only the primary sequence. Ideal for fast, structure-free screening of small proteins (<250 AA).

- **[PIP-BERT](https://huggingface.co/MuthuS97/PIP-BERT)**  
  Fine-tuned **ProtBERT** binary classifier. Predicts if a protein sequence is a potential protease inhibitor using only the primary sequence. Ideal for fast, structure-free screening of small proteins (<250 AA).

- **[structuralmodule-protease_inhibitors](https://huggingface.co/MuthuS97/structuralmodule-protease_inhibitors)**  
  Unsupervised one-class autoencoder (PyOD/PyTorch) for structural filtering. Detects non-PI-like structures via high reconstruction error. Trained on ~18k curated protease inhibitor structures from MEROPS + AlphaFold.  
  → Input: standardized RCSB embeddings (not raw PDB/CIF). Use the provided scaler.

If you use these in your work, please cite the repo / Hugging Face pages.

Star ⭐ the repo if you find it useful! 

## Authors
* __Muthusaravanan S__ <a itemprop="sameAs" content="https://orcid.org/0000-0002-0600-4534" href="https://orcid.org/0000-0002-0600-4534" target="orcid.widget" rel="me noopener noreferrer" style="vertical-align:top;"><img src="https://orcid.org/sites/default/files/images/orcid_16x16.png" style="width:1em;margin-right:.5em;" alt="ORCID iD icon"></a>   </br>
_Department of Biological Sciences, Birla Institute of Technology and Science, Pilani, India_

* __Balakumaran Chandrasekar__ <a itemprop="sameAs" content="https://orcid.org/0000-0001-5696-6777" href="https://orcid.org/0000-0001-5696-6777" target="orcid.widget" rel="me noopener noreferrer" style="vertical-align:top;"><img src="https://orcid.org/sites/default/files/images/orcid_16x16.png" style="width:1em;margin-right:.5em;" alt="ORCID iD icon"></a>   </br>
_Indian Institute of Technology, Jodhpur, India_

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

## Key tools and resources used in this pipeline:

- **ESM-2 & ProtBERT base models** — Lin, Z., Akin, H., Rao, R., Hie, B., Zhu, Z., Lu, W., Smetanin, N., Verkuil, R., Kabeli, O., Shmueli, Y., et al. (2023). Evolutionary-scale prediction of atomic-level protein structure with a language model. Science, 379(6638); Elnaggar A, Heinzinger M, Dallago C, Rehawi G, Wang Y, Jones L, Gibbs T, Feher T, Angerer C, Steinegger M, Bhowmik D, Rost B. ProtTrans: Toward Understanding the Language of Life Through Self-Supervised Learning. IEEE Trans Pattern Anal Mach Intell. 2022 Oct;44(10):7112-7127. 
- **ESMFold / ESMAtlas API** — Lin et al. (2023). Evolutionary-scale prediction of atomic-level protein structure with a language model. Science. DOI: 10.1126/science.ade2574
- **AlphaFold (precomputed models & Multimer)** — Jumper et al. (2021). Highly accurate protein structure prediction with AlphaFold. Nature. DOI: 10.1038/s41586-021-03819-2; Evans et al. (2022). Protein complex prediction with AlphaFold-Multimer. bioRxiv. DOI: 10.1101/2021.10.04.463034; Fleming J. et al. AlphaFold Protein Structure Database and 3D-Beacons: New Data and Capabilities. Journal of Molecular Biology, (2025). 
- **ColabFold (GPU-accelerated implementation)** — Mirdita et al. (2022). ColabFold: making protein folding accessible to all. Nature Methods. DOI: 10.1038/s41592-022-01488-1. 
- **PyOD (for one-class autoencoder in structural filtering)** — Zhao et al. (2019). PyOD: A Python Toolbox for Scalable Outlier Detection. Journal of Machine Learning Research.
- **SignalP (external signal peptide prediction, recommended)** — Teufel et al. (2022). SignalP 6.0 predicts all five types of signal peptides using protein language models. Nature Biotechnology. DOI: 10.1038/s41587-021-01156-3. 
- **MEROPS database** — Rawlings et al. (2018). The MEROPS database of proteolytic enzymes, their substrates and inhibitors in 2017 and a comparison with peptidases in the PANTHER database. Nucleic Acids Research. DOI: 10.1093/nar/gkx1134.
- **UNIPROT database** — UniProt: the Universal Protein Knowledgebase in 2025.  Nucleic Acids Research, Volume 53, Issue D1, 6 January 2025, Pages D609–D617, https://doi.org/10.1093/nar/gkae1010. 
 
## Have data to contribute? 
----
We welcome datasets of known/putative protease inhibitors or SSPs to improve accuracy. Contact us!

> Open models and  access to data democratize discovery. We encourage sharing sequences/structures in public repositories for open use.

## For inquiries, contact us.__

# Contact 
----
Muthusaravanan S - [@Muthu_Sivaram](https://x.com/Muthu_Sivaram) - muthusaravanan.ind@gmail.com
