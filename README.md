# AI-Powered Drug Solubility Prediction

Copyright (c) 2026 Shrikara Kaudambady. All rights reserved.

## 1. Introduction

In early-stage drug discovery, scientists must evaluate millions of potential compounds. A key property that determines if a compound can be a viable drug is its **aqueous solubility**—its ability to dissolve in water. A drug that is not soluble cannot be effectively absorbed by the body. Physical lab experiments to measure solubility are slow and expensive.

This project provides a Jupyter Notebook that implements a machine learning model to **predict the solubility of a molecule** directly from its chemical structure. This allows for rapid, large-scale virtual screening, helping chemists prioritize which compounds are most promising for synthesis and further testing.

## 2. The Solution Explained: Molecular Fingerprinting & Regression

The core of this solution is to teach a machine learning model to recognize the relationship between a molecule's structure and its measured solubility. This is achieved through a cheminformatics workflow.

### 2.1 Representing Molecules for AI

1.  **SMILES Notation:** We start with the standard text-based representation of a molecule, the SMILES string (Simplified Molecular-Input Line-Entry System). For example, `CCO` represents ethanol.

2.  **Molecular Fingerprints:** This is the key step for making molecules understandable to an ML model. A molecular fingerprint is a long sequence of 0s and 1s (a bit vector) that acts as a digital signature for the molecule. Each position in the vector corresponds to the presence (1) or absence (0) of a specific chemical substructure or feature. In this notebook, we use **Morgan Fingerprints**, a widely used and effective type of fingerprint.

### 2.2 The Machine Learning Workflow

1.  **Dataset:** The notebook uses a well-known public dataset (the Delaney dataset) containing hundreds of molecules, their SMILES strings, and their experimentally measured `logS` values (the logarithm of solubility).

2.  **Featurization:** Using the powerful `RDKit` library, we convert each molecule's SMILES string into its Morgan Fingerprint. This vector of 0s and 1s becomes the feature set (`X`) for our model.

3.  **Model Training:** A `RandomForestRegressor` from `scikit-learn` is trained on the data. The model learns to associate the patterns in the input fingerprints with the output `logS` solubility value. The Random Forest is an excellent choice for this task as it handles high-dimensional, sparse data (like fingerprints) very well.

4.  **Evaluation:** The model's performance is evaluated on a held-out test set. We use two metrics:
    *   **R-squared (R²):** A statistical measure of how well the model's predictions approximate the real data points. A value of 1 indicates a perfect fit.
    *   **Scatter Plot:** A visual plot of `Predicted logS` vs. `Actual logS`. A good model will show points clustered tightly around a 45-degree line.

## 3. How to Use the Notebook

### 3.1. Prerequisites

This project requires specialized cheminformatics libraries. The recommended way to install `rdkit` is via Conda.

**1. Install Conda (if you don't have it):**
Follow the instructions at [conda.io](https://conda.io/projects/conda/en/latest/user-guide/install/index.html).

**2. Create a new Conda environment and install libraries:**
```bash
conda create -c conda-forge -n drug-discovery rdkit scikit-learn pandas numpy matplotlib
conda activate drug-discovery
```

### 3.2. Running the Notebook

1.  Clone this repository:
    ```bash
    git clone https://github.com/shrikarak/drug-discovery-solubility-prediction.git
    cd drug-discovery-solubility-prediction
    ```
2.  Start the Jupyter server (ensure your conda environment is active):
    ```bash
    jupyter notebook
    ```
3.  Open `solubility_predictor.ipynb` and run the cells sequentially.

## 4. Deployment and Customization

This notebook provides a complete and powerful template for predicting molecular properties.

1.  **Predicting Other Properties:** The same workflow can be used to predict almost any other molecular property (e.g., toxicity, boiling point, "drug-likeness"). You would simply need to find a dataset that contains SMILES strings and the corresponding target values for that property.

2.  **Using Different Fingerprints:** RDKit supports many types of fingerprints (e.g., MACCS keys, Torsion fingerprints). You can experiment with different featurization methods to see if they improve model performance for your specific task.

3.  **Trying Different Models:** While Random Forest is a strong baseline, you could swap it out for other regression models like Gradient Boosting (`XGBoost`, `LightGBM`) or a simple deep neural network (`TensorFlow`/`PyTorch`) to compare results.
