# Diabetes Prediction

## Overview

This project implements a machine learning model to predict the likelihood of diabetes in individuals using the PIMA Indian Diabetes Database. The model employs Support Vector Machine (SVM) classification to categorize patients as diabetic or non-diabetic based on various health metrics.

## Table of Contents

- [Dependencies](#dependencies)
- [Data Collection and Analysis](#data-collection-and-analysis)
- [Data Preprocessing](#data-preprocessing)
- [Model Training](#model-training)
- [Model Evaluation](#model-evaluation)
- [Making Predictions](#making-predictions)
- [Usage](#usage)

## Dependencies

To run this code, ensure you have the following Python libraries installed:

- `numpy`: For numerical operations.
- `pandas`: For data manipulation.
- `scikit-learn`: For machine learning algorithms and preprocessing.

Install these libraries using pip:

```bash
pip install numpy pandas scikit-learn
```

## Data Collection and Analysis

This project uses the PIMA Indian Diabetes Dataset, which is loaded from a CSV file named `diabetes.csv`. The dataset contains various health measurements along with an outcome label indicating diabetes status.

### Key Steps:

1. **Load the Dataset:**
   ```python
   diabetes_dataset = pd.read_csv('/content/diabetes.csv')
   ```

2. **Explore the Data:**
   The first few rows and basic statistics of the dataset are displayed to understand its structure.
   ```python
   diabetes_dataset.head()
   diabetes_dataset.describe()
   ```

3. **Label Distribution:**
   The target variable, `Outcome`, has two values:
   - `0` for Non-Diabetic
   - `1` for Diabetic

   The distribution of these labels is printed for analysis:
   ```python
   diabetes_dataset['Outcome'].value_counts()
   ```

4. **Group Statistics:**
   The average values of features are computed based on the outcome to identify trends.
   ```python
   diabetes_dataset.groupby('Outcome').mean()
   ```

5. **Separating Features and Labels:**
   The feature set (X) is separated from the labels (Y).
   ```python
   X = diabetes_dataset.drop(columns='Outcome', axis=1)
   Y = diabetes_dataset['Outcome']
   ```

## Data Preprocessing

### Standardization

Data standardization is performed to ensure that each feature contributes equally to the distance calculations in the SVM algorithm.

```python
scaler = StandardScaler()
scaler.fit(X)
standardized_data = scaler.transform(X)
X = standardized_data
```

## Model Training

The dataset is split into training and test sets using an 80-20 ratio.

```python
X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.2, stratify=Y, random_state=2)
```

A Support Vector Machine classifier with a linear kernel is used for training:

```python
classifier = svm.SVC(kernel='linear')
classifier.fit(X_train, Y_train)
```

## Model Evaluation

The accuracy of the model is evaluated using both the training and test datasets.

- **Training Accuracy:**
  ```python
  X_train_prediction = classifier.predict(X_train)
  training_data_accuracy = accuracy_score(X_train_prediction, Y_train)
  ```

- **Test Accuracy:**
  ```python
  X_test_prediction = classifier.predict(X_test)
  test_data_accuracy = accuracy_score(X_test_prediction, Y_test)
  ```

Both accuracy scores are printed for evaluation.

## Making Predictions

The model can make predictions based on new input data. An example input is provided as a tuple of health metrics:

```python
input_data = (5, 166, 72, 19, 175, 25.8, 0.587, 51)
```

This input is converted to a NumPy array and reshaped for prediction:

```python
input_data_as_numpy_array = np.asarray(input_data)
input_data_reshaped = input_data_as_numpy_array.reshape(1, -1)
std_data = scaler.transform(input_data_reshaped)
```

The model predicts the diabetes status, and the result is displayed:

```python
prediction = classifier.predict(std_data)
```

If the predicted label is `0`, the person is classified as non-diabetic; otherwise, they are classified as diabetic.

## Usage

1. Ensure the dataset `diabetes.csv` is in the specified path.
2. Run the script to train the model and evaluate its performance.
3. Modify the `input_data` tuple with new health metrics to classify other individuals.

This project serves as an introductory example of applying machine learning to medical data and can be further expanded with additional features or models for improved accuracy.