# Network Intrusion Detection using NSL-KDD Dataset

## Project Overview

This project implements a comprehensive machine learning pipeline for network intrusion detection using the NSL-KDD dataset. The goal is to build and evaluate multiple classification models to distinguish between normal network traffic and malicious attacks.

**Project Name**: Detekcija napada na mrežu (Network Attack Detection)  
**Language**: Python 3  
**Framework**: Jupyter Notebook with scikit-learn, pandas, numpy

## Dataset

### NSL-KDD Dataset Details
- **Source**: Network Security Laboratory & KDD
- **Training Data**: KDDTrain+_20Percent.txt (25,193 samples)
- **Test Data**: KDDTest+.txt (22,543 samples)
- **Features**: 41 network connection attributes
- **Target Variable**: Binary classification (Normal vs Attack)

### Features (41 total)
1. duration - Length of connection
2. protocol_type - Protocol (tcp, udp, icmp)
3. service - Service/port type
4. flag - Connection status flag
5. src_bytes - Bytes sent from source
6. dst_bytes - Bytes sent to destination
7-20. Various connection and traffic characteristics
21-40. Statistical features (rates, counts, host history)

### Class Distribution
- **Training Set**: Normal (~67%), Attack (~33%)
- **Test Set**: Normal (~58%), Attack (~42%)

## Notebook Structure

### 1. Import Required Libraries
- Data manipulation: pandas, numpy
- Machine learning: scikit-learn
- Visualization: matplotlib, seaborn
- Model serialization: joblib

### 2. Load and Explore Dataset
- Load training and test data from CSV files
- Display dataset shape, columns, and data types
- Analyze attack type distribution
- Create binary classification labels

### 3. Data Preprocessing and Cleaning
- Check for missing values and duplicates
- Remove duplicate records if necessary
- Drop unnecessary columns
- Prepare data for feature engineering

### 4. Feature Engineering and Encoding
- Identify categorical vs numerical features
- Apply LabelEncoder to categorical variables:
  - protocol_type: tcp, udp, icmp
  - service: 70+ different services
  - flag: 11 different connection flags
- All 41 features properly encoded

### 5. Train-Test Split and Feature-Target Separation
- Separate X (features) and y (target)
- Use provided NSL-KDD test set for evaluation
- Maintain class distribution
- Document feature and sample counts

### 6. Normalize and Scale Features
- Apply StandardScaler for normalization
- Zero mean and unit variance
- Fit scaler on training data
- Transform both training and test sets
- Save scaler for deployment

### 7. Build Machine Learning Models
Initialize 5 different algorithms:

1. **Logistic Regression**
   - Linear model for binary classification
   - Solver: lbfgs
   - Max iterations: 1000

2. **Random Forest**
   - Ensemble of decision trees
   - 100 estimators
   - Max depth: 20

3. **Support Vector Machine (SVM)**
   - Kernel: RBF (Radial Basis Function)
   - C parameter: 1.0
   - Probability estimates enabled

4. **Gradient Boosting**
   - Sequential ensemble learning
   - 100 estimators
   - Learning rate: 0.1
   - Max depth: 5

5. **Neural Network (MLP)**
   - Hidden layers: 100, 50 neurons
   - Activation: ReLU
   - Optimizer: Adam
   - Max iterations: 200

### 8. Model Training
- Train all 5 models
- Record training time for each
- Models trained on 25,193 samples
- Store trained models in dictionary

### 9. Model Evaluation and Metrics
Evaluation metrics calculated:
- **Accuracy**: Overall correctness
- **Precision**: False positive control
- **Recall**: Detection rate (important for attacks)
- **F1-Score**: Harmonic mean (balanced metric)
- **ROC-AUC**: Area under ROC curve
- **Confusion Matrix**: TP, TN, FP, FN breakdown

### 10. Results Comparison and Analysis
- Create comparison dataframe of all models
- Rank models by each metric
- Generate visualizations:
  - Confusion matrices (2x3 subplot grid)
  - ROC curves (all models overlaid)
  - Performance metrics bar charts
  - Precision vs Recall scatter plot

### 11. Save Models and Artifacts
Saved artifacts in `models/` directory:
- `best_model.joblib` - Best performing model
- `all_trained_models.joblib` - All 5 models
- `scaler.joblib` - StandardScaler instance
- `label_encoders.joblib` - Categorical encoders
- `feature_names.joblib` - Feature list
- `model_comparison_results.csv` - Performance table
- `summary_report.txt` - Project summary

### 12. Project Summary and Next Steps
- Complete overview of work done
- Recommendations for optimization
- Guide for hyperparameter tuning

## File Structure

```
src/
├── Network_Intrusion_Detection.ipynb    # Main Jupyter notebook
├── models/                               # Saved models directory
│   ├── best_model.joblib
│   ├── all_trained_models.joblib
│   ├── scaler.joblib
│   ├── label_encoders.joblib
│   ├── feature_names.joblib
│   ├── model_comparison_results.csv
│   └── summary_report.txt
└── README.md                             # This file

data/
├── KDDTrain+_20Percent.txt            # Training data (20% subset)
├── KDDTrain+.txt                      # Full training data
├── KDDTest+.txt                       # Test data
├── KDDTest-21.txt                     # Test data (difficulty < 21)
├── class_distribution.png             # Class distribution chart
├── confusion_matrices.png             # Confusion matrices grid
├── roc_curves.png                     # ROC curves comparison
└── model_comparison.png               # Comprehensive comparison chart
```

## How to Run the Notebook

### Prerequisites
```bash
pip install pandas numpy scikit-learn matplotlib seaborn joblib
```

### Steps
1. Open Jupyter Notebook:
   ```bash
   jupyter notebook src/Network_Intrusion_Detection.ipynb
   ```

2. Execute cells in order (Cell → Run All)

3. Monitor console output for progress

4. Review generated visualizations

5. Check `src/models/` for saved artifacts

## Expected Results (Base Version)

The notebook should produce results similar to:

| Model | Accuracy | Precision | Recall | F1-Score |
|-------|----------|-----------|--------|----------|
| Logistic Regression | ~0.810 | ~0.72 | ~0.65 | ~0.68 |
| Random Forest | ~0.890 | ~0.85 | ~0.80 | ~0.82 |
| SVM (RBF) | ~0.820 | ~0.75 | ~0.70 | ~0.72 |
| Gradient Boosting | ~0.920 | ~0.90 | ~0.88 | ~0.89 |
| Neural Network | ~0.870 | ~0.82 | ~0.78 | ~0.80 |

*Note: Actual results may vary slightly due to randomness*

## Hyperparameter Optimization (Next Phase)

For improved performance, consider:

### Random Forest
```python
GridSearchCV(RandomForestClassifier(), {
    'n_estimators': [50, 100, 200],
    'max_depth': [10, 15, 20, 30],
    'min_samples_split': [2, 5, 10]
})
```

### SVM
```python
GridSearchCV(SVC(), {
    'C': [0.1, 1, 10, 100],
    'gamma': ['scale', 'auto', 0.001, 0.01],
    'kernel': ['rbf', 'poly']
})
```

### Gradient Boosting
```python
GridSearchCV(GradientBoostingClassifier(), {
    'learning_rate': [0.01, 0.05, 0.1],
    'n_estimators': [100, 200, 300],
    'max_depth': [3, 5, 7],
    'subsample': [0.8, 0.9, 1.0]
})
```

### Neural Network
```python
GridSearchCV(MLPClassifier(), {
    'hidden_layer_sizes': [(50,), (100,50), (150,100,50)],
    'learning_rate_init': [0.001, 0.01, 0.1],
    'alpha': [0.0001, 0.001, 0.01]
})
```

## Model Selection Criteria

Choose the best model based on your requirements:

- **Highest Accuracy**: Best overall performance
- **Highest Recall**: Detect all attacks (minimize false negatives)
- **Highest Precision**: Reduce false alarms (minimize false positives)
- **Fastest Training**: For real-time systems
- **Best F1-Score**: Balance precision and recall

## Performance Optimization Tips

1. **Handle Class Imbalance**
   - Use `class_weight='balanced'` in models
   - Apply SMOTE for synthetic oversampling
   - Adjust decision threshold

2. **Feature Selection**
   - Remove low-importance features
   - Apply PCA for dimensionality reduction
   - Reduce training time and complexity

3. **Cross-Validation**
   - K-fold cross-validation (k=5)
   - Stratified k-fold for imbalanced data
   - More robust performance estimates

4. **Ensemble Methods**
   - Voting Classifier (combine predictions)
   - Stacking (meta-learner)
   - Boosting (sequential ensemble)

## Model Deployment

To use trained models for predictions:

```python
import joblib
import pandas as pd

# Load artifacts
model = joblib.load('src/models/best_model.joblib')
scaler = joblib.load('src/models/scaler.joblib')
encoders = joblib.load('src/models/label_encoders.joblib')
feature_names = joblib.load('src/models/feature_names.joblib')

# Preprocess new data
X_new = pd.DataFrame(raw_data, columns=feature_names)
X_new_encoded = X_new.copy()
for col, encoder in encoders.items():
    if col in X_new_encoded.columns:
        X_new_encoded[col] = encoder.transform(X_new_encoded[col])

X_new_scaled = scaler.transform(X_new_encoded)

# Make predictions
predictions = model.predict(X_new_scaled)
probabilities = model.predict_proba(X_new_scaled)
```

## References

- NSL-KDD Dataset: https://www.unb.ca/cic/datasets/nsl-kdd.html
- Scikit-learn Documentation: https://scikit-learn.org/
- Pandas Documentation: https://pandas.pydata.org/
- Jupyter Notebook: https://jupyter.org/

## Notes

- **Base Version**: This notebook provides the foundation with all major algorithms
- **No Hyperparameter Tuning**: Using default parameters as specified
- **Training Time**: ~2-5 minutes total depending on hardware
- **Memory Usage**: ~500MB should be sufficient
- **Python Version**: 3.7+

## Author Notes

This is a comprehensive first version designed to:
1. Establish a solid baseline for model performance
2. Provide a framework for systematic evaluation
3. Create a foundation for optimization
4. Document the complete pipeline

The notebook is well-structured for easy modification and extension. Each section is clearly marked and can be enhanced independently.

---

**Status**: Base version complete ✓  
**Next Phase**: Hyperparameter optimization and advanced techniques