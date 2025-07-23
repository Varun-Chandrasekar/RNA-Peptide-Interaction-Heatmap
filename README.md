# 🔬 RNA–Peptide Interaction Prediction with Saliency and Heatmaps

This project builds a complete **RNA–peptide interaction analysis pipeline** from raw positive binding pairs, through model training, to interpretable visualizations using **heatmaps** and **saliency maps**.

---

## 📁 Project Workflow Overview

### 1. **Positive Data Loading**
- **Notebook**: `RNA_peptide_positive_load_200chunks.ipynb`
- Loads and merges RNA–peptide **positive interaction pairs** (~2248 total) from chunked `.csv` and `.pkl` files.
- Saves:
  - `rna_protein_pairs_full.csv`
  - `rna_protein_pairs_full.pkl`

### 2. **Generate Balanced Negative Pairs**
- **Notebook**: `Create_negative_RNA_peptide_pairs.ipynb`
- Randomly pairs non-interacting RNA and peptide sequences to generate **2248 negative samples**.
- Combines them with positives into one file:
  - `rna_protein_positive_negative_pairs_full_combined.csv`

### 3. **Train Dual-Input Deep Learning Model**
- **Notebook**: `RNA_peptide_interaction_dual_network_with_layer.ipynb`
- Creates a **dual-input neural network** with:
  - One-hot encoded RNA and peptide inputs
  - Merged dense layer (`interaction_features`)
- Saves trained model to:
  - `rna_peptide_dual_model_with_4486.keras`

### 4. **Heatmap Visualization from Model**
- **Notebook**: `RNA_pepide_heatmap_using_model.ipynb`
- Loads trained model
- Extracts intermediate interaction features
- Visualizes as a **heatmap matrix** using:
  - Standard
  - Clipped (95th percentile)
  - Log scale
  - Interactive (Plotly)

### 5. **Model Prediction on New Sequences**
- **Notebook**: `RNA_pepide_using_model_predictfile.ipynb`
- Accepts a `predict.csv` file with new RNA and peptide pairs
- Visualizes predictions via **heatmap**

### 6. **Saliency Map for Model Explanation**
- **Notebook**: `predict_saliencygrid.ipynb`
- Computes **saliency gradients** from the model using `tf.GradientTape`
- Shows which **bases and residues drive the model decision**
- Outputs:
  - `all_saliency_heatmaps.png`: Combined static plot
  - `interactive_saliency_heatmap.html`: Plotly heatmap with hover interaction

---

## 📦 Files & Folders

| File / Notebook                              | Description |
|---------------------------------------------|-------------|
| `RNA_peptide_positive_load_200chunks.ipynb` | Loads positive examples from 200 chunked files |
| `Create_negative_RNA_peptide_pairs.ipynb`   | Generates random RNA–peptide negative pairs |
| `RNA_peptide_interaction_dual_network_with_layer.ipynb` | Trains the dual-input model |
| `RNA_pepide_heatmap_using_model.ipynb`      | Extracts and visualizes interaction layer features |
| `RNA_pepide_using_model_predictfile.ipynb`  | Predicts and plots heatmap for external inputs |
| `predict_saliencygrid.ipynb`                | Generates saliency maps from model gradients |
| `predict.csv`                               | RNA–peptide pairs for model inference |
| `rna_peptide_dual_model_with_4486.keras`    | Saved trained Keras model |

---

## 📊 Output Visualizations

- **Heatmaps** show dense-layer-based interaction strength
- **Saliency Maps** explain which sequence positions influenced prediction
- Supports **interactive HTML plots**

---

## 🛠️ Setup Instructions

Run in [Google Colab](https://colab.research.google.com/) with:

```python
!pip install -q tensorflow plotly seaborn pandas
```

Mount Google Drive to access models and datasets:

```python
from google.colab import drive
drive.mount('/content/drive')
```

---

## 🧠 Model Summary

- **Inputs**:
  - RNA sequence (100 bases max) → 400-dim one-hot
  - Peptide sequence (100 AAs max) → 2000-dim one-hot
- **Architecture**:
  - 128-dense per branch → merged
  - 64-dim `interaction_features` → sigmoid output
- **Accuracy**: >93% training, ~79% validation

---

## 📌 Future Work

- [ ] Replace dense with attention layers for interpretable alignment
- [ ] Saliency overlay on sequence structure
- [ ] Extend to 3D RNA–peptide binding site prediction

---

## 📬 Citation / Acknowledgments

This tool was developed for explainable prediction of RNA–peptide interactions using interpretable deep learning techniques (saliency and heatmaps) trained on curated sequence data.
