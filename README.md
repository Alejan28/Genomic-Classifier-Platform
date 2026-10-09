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
