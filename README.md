# Team Limbic - BTT Kaggle Competition

## **Project Overview**  
ADHD is one of the most prevalent neuropsychiatric disorders among children and adolescents, with varying manifestations across genders. While boys are typically diagnosed at higher rates, evidence suggests that girls with ADHD often go undiagnosed due to their tendency to exhibit inattentive rather than hyperactive symptoms—making these more challenging to detect. The consequences of undiagnosed ADHD in girls can lead to prolonged struggles with mental health, academic challenges, and difficulties in social functioning. By addressing these inequities, the WiDS Datathon tackles a critical issue in public health: ensuring timely and accurate diagnosis for all individuals, regardless of gender.

The WiDS Datathon 2025 allows participants to use functional brain imaging data, along with socio-demographic and emotional information, to address disparities in ADHD diagnosis between sexes. Participants are tasked with building a multi-outcome model to predict ADHD diagnosis and gender, uncovering critical insights into brain activity patterns associated with these outcomes. Break Through Tech AI supports this effort by fostering inclusion and diversity, ensuring that underrepresented groups in AI and data science are empowered to make impactful contributions to this critical field

The work conducted in this datathon holds significant potential to advance ADHD research and healthcare equity. Improving diagnostic tools, particularly for females who are often underdiagnosed, enables earlier interventions and better mental health outcomes. Insights into sex-specific brain activity patterns can also drive personalized treatments, leading to more effective and targeted therapies. Furthermore, the findings contribute to the scientific understanding of neuropsychiatric disorders while promoting fairness in healthcare access. Ultimately, this project showcases how AI can address societal challenges and improve lives through innovative health research. 


## Objectives and Goals

This project involves building a model following these tasks:
- **Predict ADHD Diagnosis**: Using features derived from functional brain imaging and socio-demographic data, the model determines whether an individual has ADHD (`ADHD_Outcome: 1 for Yes, 0 for No`).
- **Predict Gender**: The model also predicts the participant's sex (`Sex_F: 1 for Female, 0 for Male`), accounting for gender-specific differences in brain activity patterns.  

The model aims to optimize predictions while addressing biases in ADHD diagnosis and improving understanding of how the disorder manifests across the sexes.

The objectives of this challenge include:
1. **Advance ADHD Research**: Shed light on brain activity patterns that differ by gender in individuals with ADHD by understanding brain mechanisms and identifying functional connectivity patterns and socio-demographic factors linked to ADHD across sexes.
2. **Create Multi-Outcome Predictions**: Build a model capable of predicting both ADHD diagnosis and gender in individuals based on their data.
3. **Promote Equity and Drive Impactful Outcomes**: Highlight and address gender disparities in the diagnosis and understanding of ADHD. Contribute to personalized medicine by improving ADHD diagnosis and treatment strategies for both males and females.


## Methodology
1. **Exploratory Data Analysis**:  

We used the primary datasets provided by the WiDS Datathon on Kaggle. Information about them is listed below:

#### **Training Solutions**
- Contains the targets (ADHD diagnosis and sex).
  - **ADHD Diagnosis**: A binary classification (`1= ADHD, 0 = No ADHD`).
  - **Sex**: A binary classification (`1 = Female, 0 = Male`).

#### **fMRI Connectome Matrices**
- Contains approximately 19,900 features, representing the activity measured across about 200 brain regions.

#### **Categorical Metadata**
- Descriptive data, including variables such as:
  - Race
  - Ethnicity
  - Parent's level of education
  - Parent's occupation

#### **Quantitative Metadata**
- Numerical data collected from various questionnaires, including:
  - Age
  - Color vision
  - Parenting measures
  - Emotional well-being measures.


### **Data Exploration**
To begin the analysis, we loaded all the provided datasets and merged them into a single dataset named `train_data`, which consisted of 9 rows and 19,929 columns. This consolidated dataset formed the basis for all subsequent exploratory data analysis (EDA). Key steps in EDA included:

1. **Target Distribution Analysis**:
   - **ADHD Outcome**: 
     - A pie chart was created to examine the distribution of ADHD diagnoses within the dataset.
     - Results indicated that 831 participants were diagnosed with ADHD, while 382 were not.
   - **Sex Distribution**:
     - A similar analysis was performed for the `Sex_F` variable, which revealed that 797 participants were male (`Sex_F: 0`) and 416 participants were female (`Sex_F: 1`).
   - **Combined Analysis**:
     - A bar plot was generated to visualize ADHD outcomes across sexes. The plot revealed that males were more likely to have ADHD in this dataset, with 581 males and 250 females diagnosed.

2. **Feature Correlation**:
   - Correlation matrices were created for quantitative and categorical metadata to identify relationships and dependencies between features and ADHD_Outcome.
   - Feature correlations were created to investigate the relationship between the features of the metadata and the target variable Sex_F
   - This step provided insights into feature importance and the degree of interaction among variables.


3. **Dimensionality Reduction**:
   - Principal Component Analysis (PCA) was applied to the fMRI data, which contains approximately 19,900 features representing activity across 200 brain regions.
   - This allowed for visual exploration and reduction of high-dimensional data, ensuring better understanding and improved model input processing.

   Graphs included in the [Visualizations](https://github.com/AnushaAbdulla/LimbicTeam?tab=readme-ov-file#visualizations) section below

### **Challenges**

1. **Missing Data**:
   - In the training dataset, we observed:
     - `MRI_Track_Age_at_Scan` had 360 missing values.
     - `PreInt_Demos_Fam_Child_Ethnicity` had 11 missing values.
   - Missing values were addressed using median imputation, which replaces missing values with the median of the respective columns. This approach is effective for numerical data as it is robust to outliers.
   The testing dataset exhibited more missing columns than the training dataset, which indicates some mismatch in the distribution properties between the training and testing datasets.

2. **Imbalanced Data**:
   - The dataset showed imbalanced distributions for both target variables in both the training and testing dataset:
     - Males were more prone to ADHD compared to females, as reflected in the dataset.
   - This imbalance introduces potential bias into the model and requires careful handling during model training and evaluation.

These combined exploration and preprocessing steps ensured the dataset was clean, consistent, and ready for subsequent model development and evaluation.


2. **Data Preparation**:  
   - Performed data cleaning e.g. filled null values in PreInt_Demos_Fam_Child_Ethnicity and MRI_Track_Age_at_Scan
   - Adjused for skewness of MRI_Track_Age_at_Scan
   - Used z-scores to find and remove outliers from data
   - Filtered out rows outide of 1.5 IQR
   - Checked for imbalance between ADHD diagnosis and gender identification, then adjusted the scale and weights

3. **Model Development**:  
   - Multi-output models are designed to handle prediction tasks where there are multiple target variables simultaneously. MultiOutputClassifier: used for multi-target classification, trains one classifier per target.
   - Logistic regression is primarily used for classification problems. It estimates the probability that an instance belongs to a particular class. The core of this algorithm is the sigmoid function (logistic function) which transforms a linear combination of input features into a probability value (0-1). The model's coefficients are typically esitmated using maximum likelihood estimation to maximize the likelihood of observing the given data.

4. **Evaluation**:  
   | Baseline Model | ADHD Diagnosis Accuracy (%) | Gender Prediction Accuracy (%) | Total Accuracy (%)
   | --- | --- | --- | --- |
   | Multi-output Logistic Regression | 73 | 76 | 56 |
   
   | Multi-output Logistic Regression Hyperparameters (solver, penalty) | ADHD Diagnosis Accuracy (%) | Gender Prediction Accuracy (%) | Total Accuracy (%) |
   | --- | --- | --- | --- |
   | newton-cg & l2 | 80 | 77 | 61 |
   | newton-cg & None | 79 | 79 | 61 |
   | lbfgs & l2 | 79 | 77 | 60 |
   | lbfgs & None | 78 | 74 | 58 |
   | sag & l2 | 80 | 70 | 57 |
   | sag & None | 80 | 70 | 57 |
   | newton-cholesky & l2 | 79 | 77 | 61 |
   | newton-cholesky & None | 78 | 74 | 58 |
   | saga & l1 | 80 | 70 | 58 |
   | saga & l2 | 80 | 72 | 59 |
   | saga & elasticnet (l1_ratio = 0.5) | 80 | 70 | 58 |
   | saga & None | 80 | 71 | 58 |
   | liblinear & l1 | 81 | 70 | 58 |
   | liblinear & l2 | 79 | 77 | 60 |
    

## Results and Key Findings
- We found that using a Multi-Output model while intializing each classifier as a different specialized model worked the best.

## Visualizations
### Dataset Distribution    
![DistOfADHDOutcome](https://i.imgur.com/zRNiqkq.png)  
![DistGenderOutcome](https://i.imgur.com/eZhALQp.png)  
![ADHDOutcomeBySex](https://i.imgur.com/v4kJNaG.png)  

### Correlation graphs  
Quantitative metadata graphs
![Quant Correlation Matrix Heatmap for ADHD Outcome](https://i.imgur.com/9me6LaM.png)  
![Quant Feature SexF](https://i.imgur.com/6ArzssT.png)  


Categorical metadata graphs
![corrmatrix](https://i.imgur.com/812xbjM.png)
![corrfeaturegraph](https://i.imgur.com/o8bkwWH.png)

### PCA Graph
![pcagraph](https://i.imgur.com/U6zIH1W.png)

## Potential Next Steps
1.   Implement neural network architecture. This architecture has a better chance for accuracy growth given time. Current total accuracy using a simple neural network model is 51%.

## Individual Contributions
- **Anusha Shakir Abdulla**: 
- **Bhanavi Senthil**:
- **Tina Tran**: Worked on Multi-Output model and Logistic Regression research and implementation.
- **Sruti Karthikeyan**:

## Contact
For questions or collaboration, reach out to the team:  

Anusha:
 - [**GitHub**](https://github.com/AnushaAbdulla)
 - [**LinkedIn**](https://www.linkedin.com/in/AnushaAbdulla)

Bhanavi:
 - [**GitHub**](https://github.com/navi1121)
 - [**LinkedIn**](https://www.linkedin.com/in/bhanavisenthil/)

Tina:
 - [**GitHub**](https://github.com/TTrumpet)
 - [**LinkedIn**](https://www.linkedin.com/in/tinaungtran/)

Sruti Karthikeyan:
 - [**GitHub**](https://github.com/srutiswathi)
 - [**LinkedIn**](https://www.linkedin.com/in/sruti-karthikeyan)
