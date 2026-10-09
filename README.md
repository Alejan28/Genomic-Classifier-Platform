# DNA Sequence Analysis via Multi-Task Deep Learning (MultiTaskCNNDev.ipyb)

This repository/notebook implements and evaluates various Deep Learning and Hybrid Neural Network architectures to solve a biological multi-task classification problem: predicting both the **Species** and the **Domain Group** (Bacteria, Metazoa, Viridiplantae) directly from genomic DNA sequences.

---

## 🧬  Objectives

Traditional DNA sequence classification typically relies on statistical k-mer frequencies or computationally expensive alignment tools. This project explores modern Deep Learning methodologies, showing how one-hot encoded sequence vectors can be fused with tabular statistical representations (k-mers) under a **Multi-Task Learning (MTL)** paradigm to predict taxonomic hierarchies simultaneously.

### Multi-Task Targets:
1. **Domain Group Classification (3 classes):** Bacteria, Metazoa, Viridiplantae.
2. **Species Classification (12 classes):** Specific genomic organisms (e.g., *Arabidopsis thaliana*, *Escherichia coli*, *Homo sapiens*, *Zea mays*).

---

## 🏗️ Tested Model Architectures 

To identify the optimal methodology, six diverse neural network architectures were built and thoroughly validated:

1. **Basic CNN Model (`build_cnn`)**
   - Dual-branch Conv1D layers (kernel sizes of 9 and 21) capturing multi-scale sequence motifs.
   - Global Average Pooling with Batch Normalization.
   - Independent classifier heads with weighted multi-task losses.

2. **K-mer Mimic CNN (`build_kmer_mimic_cnn`)**
   - Replicates a bag-of-words/k-mer representation inside a network.
   - Concatenates outputs from kernels of sizes 3, 7, and 15, then discards spatial alignment details using global pooling.

3. **Hybrid Multitask Model (`build_hybrid_multitask`)**
   - A multi-modal pipeline merging raw genomic sequence patterns (via a dilated Conv1D CNN branch) and statistical k-mer frequencies (via a traditional Dense tabular branch).

4. **Optimized Small-Data Model (`build_small_data_model`)**
   - Specifically customized to prevent overfitting on low-sample subsets.
   - Integrates Gaussian noise layers, spatial dropouts (0.2), concatenated Average/Max global poolings, and aggressive dense dropouts (0.6).

5. **Gated Hybrid Multitask Model (`build_gated_hybrid_multitask`)**
   - Implements a feature-selection **gating mechanism** (Sigmoid activation) that dynamically decides whether to weight structural sequence patterns (CNN branch) or token frequencies (K-mer branch) higher.

6. **Multi-Scale Attention Model (`build_multiscale_attention_model`)**
   - State-of-the-art implementation featuring three parallel multi-scale Conv1D lanes (kernels: 7, 15, 31 with dilation).
   - Integrates a **Squeeze-and-Excitation (SE)** attention block to emphasize highly distinctive, species-specific motifs.
   - Optimized via **AdamW** with weight decay and gradient norm clipping.

---

## 📊 Key Highlights & Preprocessing

- **Strand-Invariant Data Augmentation:** DNA is double-stranded. We double the training capacity by dynamically generating the one-hot encoded **reverse complement** of sequences, forcing the classifiers to be strand-invariant.
- **K-mer Tokenization:** Implements a custom sliding-window string k-mer tokenizer mapped through `CountVectorizer` to generate vocabulary vectors.
- **Weighted Cross-Entropy Loss:** Since taxonomic classification has different granularities, we balance optimization with a custom loss ratio (e.g., `0.1 * domain_loss + 1.0 * species_loss`).



# 🧬 Genomic Classifier Platform (SoftwareApp.ipyb)

*   List item
*   List item



This repository contains a Streamlit web application powered by a hybrid Deep Learning model designed to perform taxonomic classification on genomic sequences (DNA) in real-time.

## 🚀 Project Overview

The Genomic Classifier uses a hybrid neural network model trained to classify DNA sequences into taxonomic domains and species. It utilizes two representations of DNA sequence inputs:
1. **One-Hot Encoded Sequences**: Processed directly via deep learning networks (e.g., CNNs/LSTMs).
2. **K-mer Frequency Vectors**: Processed through a Count Vectorizer to extract global sequence composition patterns.

### Taxonomy Classifications Supported
* **Domains**: Bacteria, Metazoa, Viridiplantae
* **Species**: *Arabidopsis thaliana*, *Bacillus subtilis*, *Bos taurus*, *Escherichia coli*, *Glycine max*, *Homo sapiens*, *Mus musculus*, *Oryza sativa*, *Pseudomonas aeruginosa*, *Rattus norvegicus*, *Staphylococcus aureus*, *Zea mays*

---

## 📁 Project Structure

* `app.py`: The main Streamlit web application script.
* `genomic_hybrid_83pct.keras`: The trained Keras deep learning model file (expected in root).
* `k5_vectorizer.joblib`: The Joblib-serialized 5-mer vectorizer (expected in root).

---

## 🛠️ Tech Stack & Dependencies

The app relies on the following major libraries:
* **Streamlit**: Web application framework.
* **TensorFlow**: Backend deep learning framework.
* **Joblib**: Loading the scikit-learn CountVectorizer.
* **Biopython**: Fast and robust parsing of FASTA/TXT genomic file formats.
* **Pandas & NumPy**: Data processing and numeric matrix representations.
* **pyngrok**: To expose the local Streamlit port to a secure, shareable public URL.

---

## ⚙️ Setup and Installation

### 1. Install Dependencies
```bash
pip install streamlit biopython pandas tensorflow numpy joblib pyngrok
```

### 2. Prepare Model Assets
Ensure you have placed the model and vectorizer binaries in your working directory:
* `genomic_hybrid_83pct.keras`
* `k5_vectorizer.joblib`

### 3. Run Locally
```bash
streamlit run app.py
```

### 4. Deploy via Ngrok (Google Colab Environment)
If you are running the project inside Google Colab, you can expose the local server using ngrok:
```python
from pyngrok import ngrok

# Set your authtoken
!ngrok config add-authtoken <YOUR_NGROK_TOKEN>

# Connect a tunnel to port 8501
public_url = ngrok.connect(8501, proto="http")
print(f"Stable App Link: {public_url.public_url}")
```

---

## 💡 Features
* **Multi-File Uploader**: Upload multiple FASTA/TXT files containing DNA sequence records simultaneously.
* **Sequence Validation**: Automatically ignores/warns the user about sequences shorter than the required 500 bp.
* **Interactive Table**: Displays final classification domains and species with associated model confidence scores.
* **Downloadable Report**: Exports analysis results directly into a structured CSV file.
