# 🎯 Phishing URLS Detection Using Machine Learning Techniques and Data Visualisation

**Repository:** [Phishing_URL_Detection_Using_ML_and_Data_Visualization](https://github.com/shahrukh-1052/Phishing_URL_Detection_Using_ML_and_Data_Visualization)

---

## 🔹 Objective
A phishing website is a social-engineering attack that mimics legitimate URLs and webpages to steal user credentials or data.  
The objective of this project is to build and evaluate **machine learning models** (and a simple neural approach) that predict whether a URL is phishing or legitimate.  

The pipeline includes:  
✔️ Data collection  
✔️ URL & domain feature extraction (lexical + lightweight domain metadata)  
✔️ Model training & evaluation  
✔️ Visualization of insights  
✔️ Saving the best model for inference  

---

## 🔹 Data Collection
Two public sources are used:

- **PhishTank** → open dataset of reported phishing URLs (updated frequently). Phishing URLs used in this project are labeled **1**.  
  📖 Docs: https://www.phishtank.com/developer_info.php  

- **Majestic Million** → list of top legitimate domains (used as benign examples) labeled **0**.  
  🌐 Download: https://majestic.com/reports/majestic-million  

Processed files are stored in the repo under `All_Data_files/`:  
- `final_features.csv` → feature-engineered dataset (URLs + extracted features)  
- `majestic_million.csv` → raw Majestic Million dataset  
- `best_model.pkl` → saved best model (initial)  
- `best_model_retrained.pkl` → retrained & final model object (with `feature_cols`)  
- `Realistic Phishing-style Domains.txt` → sample phishing-style domains list  

---

## 🔹 Feature Extraction
Feature extraction focuses on **lexical URL features** and lightweight domain information.  

**Address bar / lexical features:**  
- URL length, hostname length, path length, URL depth (path segments)  
- Digit count, count of special characters (`@`, `-`, `?`, `=`, `/`, `.`), number of dots  
- Presence of `@`, IP address in domain, double-redirect `//`, URL shortener check, suspicious keywords (e.g., `login`, `secure`, `update`)  
- Presence/abuse of `http/https` in domain or path  

**Domain-related features (lightweight):**  
- Number of subdomains, SLD/TLD lengths, `contains_ip` flag  

**Optional (future extension):**  
- WHOIS / DNS domain age, Alexa/Majestic rank, HTML/JS features (iframe, right-click disable).  

All extracted features are saved into `All_Data_files/final_features.csv`. Detailed logic is in `URL_Feature_Extraction.ipynb`.  

---

## 🔹 Models & Training
Dataset split: **80/20 (stratified)**.  

Models trained:  
- Decision Tree  
- Random Forest  
- Multilayer Perceptron (MLP)  
- XGBoost  
- Support Vector Machine (SVM)  
- Autoencoder (unsupervised, trained on legit samples)  

**Evaluation metrics:**  
- Accuracy, Precision, Recall, F1-score  
- ROC-AUC  
- Confusion Matrix  

📓 Training & evaluation code → `Phising_Website_Detection.ipynb`.  
Results are aggregated into comparison tables + plots.  

---

## 🔹 Notebooks
- `URL_Feature_Extraction.ipynb` → raw dataset ingestion, feature extraction, exploratory visualizations, and saving `final_features.csv`.  
- `Phising_Website_Detection.ipynb` → preprocessing, training ML models, evaluation, comparison, visualization, and saving best model(s).  

---

## 🔹 Visualizations
The notebooks generate rich visualizations for **EDA** and **model understanding**:  
📊 Class distribution (bar/pie)  
📊 Histogram (URL length)  
📊 Box/Violin plots (URL length, digit counts)  
📊 Scatter / Bubble charts (length vs digits vs dots)  
📊 Heatmap (feature correlations)  
📊 Radar chart (average feature profile)  
📊 Sankey / Gantt (project & data flow)  

---

## 🔹 Results / Key Findings
✔️ Lexical features (URL length, digit count, special chars, subdomains) strongly correlate with phishing labels.  
✔️ **Tree ensembles (Random Forest, XGBoost)** delivered the most robust results.  
✔️ Final retrained model: `best_model_retrained.pkl`.  
✔️ Detailed metrics & reports are in `Phising_Website_Detection.ipynb`.  

---

## 🔹 Future Work
🚀 Add WHOIS / DNS features (domain age, registrar, expiry) and Majestic/Alexa ranks.  
🚀 Extend HTML/JS feature extraction (iframe, script analysis).  
🚀 Convert model to API / browser extension for **real-time detection**.  
🚀 Build a lightweight **GUI or CLI tool** for predictions.  

---

## 🔹 Conclusion
In this project, we successfully demonstrated how **lexical features of URLs** can be used to detect phishing websites with high accuracy using Machine Learning techniques.  

Key takeaways:  
- Extracted **length-based, character-based, and domain-related features** from phishing and legitimate URLs.  
- Performed **data preprocessing, visualization, and correlation analysis** to understand feature importance.  
- Trained multiple supervised ML models (Decision Tree, Random Forest, XGBoost, SVM, MLP) and an Autoencoder, then compared their performance.  
- Identified **tree-based ensemble models (Random Forest, XGBoost)** as top performers for phishing detection.  
- Saved the **best retrained model (`best_model_retrained.pkl`)** for future inference and deployment.  

This work lays a strong foundation for building **browser extensions, web applications, or APIs** that can perform **real-time phishing detection**. Future improvements can include WHOIS/domain age features, page content-based features, and deployment in security toolchains.  
