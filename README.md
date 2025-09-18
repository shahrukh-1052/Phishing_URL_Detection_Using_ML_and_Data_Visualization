# Phishing URLS Detection Using Machine Learning Techniques and Data Visualisation

**Repository:** https://github.com/shahrukh-1052/Phishing_URL_Detection_Using_ML_and_Data_Visualization

## Objective
A phishing website is a social-engineering attack that mimics legitimate URLs and webpages to steal user credentials or data.  
The objective of this project is to build and evaluate machine learning models (and a simple neural approach) that predict whether a URL is phishing or legitimate. The pipeline includes data collection, URL & domain feature extraction (lexical + lightweight domain metadata), model training/evaluation, visualization of insights, and saving the best model for inference.

## Data Collection
Two public sources are used:

- **PhishTank** — open dataset of reported phishing URLs (updated frequently). Phishing URLs used in this project are labeled **1**.  
  Documentation / developer info: https://www.phishtank.com/developer_info.php

- **Majestic Million** — list of top legitimate domains (used as benign examples) labeled **0**.  
  Download: https://majestic.com/reports/majestic-million

From these sources we compiled a balanced dataset; the processed files used in the project are stored in the repository under `All_Data_files/`:
- `final_features.csv` — feature-engineered dataset (URLs + extracted features).  
- `majestic_million.csv` — raw Majestic Million dataset.  
- `best_model.pkl` — saved best model (initial).  
- `best_model_retrained.pkl` — retrained & final model object (contains model + `feature_cols`).  
- `Realistic Phishing-style Domains.txt` — sample phishing-style domains list.

## Feature Extraction
Feature extraction focuses on **lexical** URL features and lightweight domain information. Categories include:

**Address bar / lexical features**
- URL length, hostname length, path length, URL depth (path segments)
- Digit count, count of special characters (`@`, `-`, `?`, `=`, `/`, `.`), number of dots
- Presence of `@`, IP address in domain, double-redirect `//`, URL shortener check, suspicious keywords (e.g., `login`, `secure`, `update`)
- Presence/abuse of `http/https` in domain or path

**Domain-related features (lightweight)**
- Number of subdomains, SLD/TLD lengths, `contains_ip` flag

**(Optional / not in base pipeline)**
- WHOIS / DNS domain age, Alexa/Majestic rank, and HTML/JS features (iframe, right-click disable) — these require network/WHOIS lookups and are left for future extension.

All extracted features are saved into `All_Data_files/final_features.csv`. The exact extraction logic and helper functions are available in `URL_Feature_Extraction.ipynb`.

## Models & Training
Data is split using an **80/20 train-test split** (stratified). Supervised classification models trained and evaluated in the project include:

- Decision Tree
- Random Forest
- Multilayer Perceptron (MLP)
- XGBoost
- Support Vector Machine (SVM)
- Autoencoder-based anomaly detection (unsupervised component, trained on legit samples)

**Evaluation metrics**
- Accuracy, Precision, Recall, F1-score
- ROC-AUC
- Confusion matrix
Model training, hyperparameters, and evaluation code are in `Phising_Website_Detection.ipynb`. Results are aggregated into a comparison table and plotted for quick inspection.

## Notebooks (key files)
- `URL_Feature_Extraction.ipynb` — ingest raw URL datasets, extract lexical & domain features, generate exploratory visualizations, and save `final_features.csv`.  
- `Phising_Website_Detection.ipynb` — load features, perform preprocessing/scaling, train models (Decision Tree, RF, MLP, XGBoost, SVM, Autoencoder), evaluate models, display comparison charts, and save best model(s).

Visualizations

Notebooks produce multiple visualizations for EDA and model understanding:

Class distribution (bar/pie)

Histogram (URL length)

Box/Violin plots (URL length, digit counts)

Scatter / Bubble charts (length vs digits vs dots)

Heatmap (feature correlations)

Radar chart (average feature profile)

Sankey/Gantt (project/data flow visualizations)

Results / Key Findings

Lexical features (URL length, digit count, special characters, number of subdomains) strongly correlate with phishing labels.

Classical tree ensembles (Random Forest / XGBoost) performed robustly as baseline classifiers.

The repository includes the final retrained model (best_model_retrained.pkl) and earlier best_model.pkl. See Phising_Website_Detection.ipynb for detailed metrics and per-model reports.
Future Work

Add WHOIS / DNS features (domain age, registrar, expiry) and Majestic/Alexa ranks (if accessible).

Extend HTML/JS based feature extraction (iframe checks, script analysis) by crawling pages (careful with safety and rate limits).

Convert the model to a small web API or browser extension that can do real-time URL checks.

Build a lightweight GUI or CLI tool for demoing predictions.

## Conclusion
In this project, we successfully demonstrated how **lexical features of URLs** can be used to detect phishing websites with high accuracy using Machine Learning techniques.  

Key takeaways:
- Extracted **length-based, character-based, and domain-related features** from phishing and legitimate URLs.  
- Performed **data preprocessing, visualization, and correlation analysis** to understand feature importance.  
- Trained multiple supervised ML models (Decision Tree, Random Forest, XGBoost, SVM, MLP) and an Autoencoder, then compared their performance.  
- Identified **tree-based ensemble models (Random Forest, XGBoost)** as top performers for phishing detection.  
- Saved the **best retrained model (`best_model_retrained.pkl`)** for future inference and deployment.  

This work lays a strong foundation for building **browser extensions, web applications, or APIs** that can perform **real-time phishing detection**. Future improvements can include WHOIS/domain age features, page content-based features, and deployment in security toolchains.  


