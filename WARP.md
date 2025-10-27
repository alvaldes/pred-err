# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

This repository contains coursework for "Predicción de errores en Ingeniería de Software" (Error Prediction in Software Engineering), a graduate-level course focused on data preprocessing, traditional and modern error prediction methods in software engineering, and evaluation techniques.

## Common Development Commands

### Jupyter Notebooks
```bash
# Start Jupyter Lab/Notebook server
jupyter lab

# Convert notebook to Python script
jupyter nbconvert --to python <notebook_name>.ipynb

# Run notebook programmatically
jupyter nbconvert --execute --inplace <notebook_name>.ipynb
```

### LaTeX Documents
```bash
# Compile LaTeX documents
pdflatex "Ciclo de vida del software.tex"
pdflatex "Curse of Dimesionality.tex"
pdflatex "Historia de la Ingeniería de Software.tex"

# Clean auxiliary files
latexmk -C
```

### Python Environment
```bash
# Install required packages for data analysis
pip install pandas numpy scikit-learn matplotlib seaborn ucimlrepo

# Run Python scripts with datasets
python -c "import pandas as pd; df = pd.read_csv('datasets/ObesityDataSet_raw_and_data_sinthetic.csv'); print(df.info())"
```

## Repository Structure and Architecture

### Core Learning Modules
- **intro.md**: Course introduction with program outline and tooling comparison
- **clase01.md - clase05.md**: Theoretical foundations covering bug types, decision theory, dimensionality, data formats, and data quality
- **clase06.ipynb**: Advanced outlier detection using MAD (Median Absolute Deviation) technique
- **clase06_practice.ipynb**: Practical implementation with obesity dataset from UCI ML Repository

### Data Processing Pipeline
The course follows a structured approach to error prediction:

1. **Data Preprocessing** (`clase04.md`, `clase05.md`): 
   - Data quality assessment
   - Missing data handling
   - Normalization and discretization
   - Feature engineering

2. **Outlier Detection** (`clase06.ipynb`):
   - Statistical methods (Z-scores, MAD)
   - Robust outlier detection algorithms
   - Practical implementation with real datasets

3. **Data Integration** (`clase03.md`):
   - Multiple format handling (CSV, JSON, XML, SQL)
   - Dataset downloading and preprocessing workflows

### Academic Components
- **Historia de la ISW/**: Software Engineering history research with bibliography
- **Ciclo de vida del software/**: Software lifecycle documentation in LaTeX
- **Curse of Dimensionality/**: Mathematical foundations research

### Datasets
- **datasets/ObesityDataSet_raw_and_data_sinthetic.csv**: Primary dataset for obesity level prediction
- Integration with UCI ML Repository via `ucimlrepo` package
- Support for mushroom classification dataset (ID: 73) and obesity dataset (ID: 544)

## Key Technical Concepts

### Data Preprocessing Techniques
- **Missing Data Handling**: Imputation strategies, global constants, manual filling
- **Outlier Detection**: MAD-based Z-score modification, statistical thresholds
- **Dimensionality Reduction**: Feature extraction, selection, and generation
- **Data Integration**: Multi-source data handling and format conversion

### Error Classification Framework
- **Error Types**: Functional, logical, integration, security, boundary errors
- **Severity Levels**: Critical/catastrophic, serious, minor, trivial
- **Priority Classification**: Low, medium, high, urgent

### Machine Learning Pipeline
- **Feature Engineering**: ROI extraction, characteristic generation
- **Model Evaluation**: Generalization assessment, overfitting prevention
- **Decision Theory**: Cost minimization, decision boundaries

## Development Workflow

1. **Exploratory Analysis**: Use Jupyter notebooks for data exploration and visualization
2. **Data Preprocessing**: Apply techniques from class notes to clean and prepare datasets
3. **Documentation**: Maintain LaTeX documents for formal academic reports
4. **Version Control**: Repository tracks progression through course materials

## Tools and Libraries
- **Primary**: pandas, numpy, scikit-learn for data processing
- **Visualization**: matplotlib, seaborn for data exploration
- **Data Sources**: ucimlrepo for UCI dataset integration
- **Academic Writing**: LaTeX for formal documentation
- **Environment**: Jupyter ecosystem for interactive development

## Course Context
The repository supports learning objectives around traditional ML methods (Weka, R) and modern approaches (Scikit-learn, Pandas, TensorFlow, PyTorch) for software engineering error prediction, with emphasis on preprocessing as a critical foundation for effective prediction models.
