# IA-Embarque

This is an artificial intelligence project focused on the prediction of failures in industrial machines. The model is trained with a dataset containing 10,000 samples, where data preprocessing and balancing techniques are applied, since the number of machines with failures is significantly lower than those operating normally.

Initially, the dataset presents five types of failures: TWF, HDF, PWF, OSF, and RNF. However, failures of the RNF type are removed, as they are random failures, which makes it unfeasible to create a reliable prediction pattern.

One of the main challenges of the project is the low number of samples for the TWF failure type, even after removing the random failures. To handle this imbalance, techniques such as undersampling and class weighting are applied during training.

After finalizing and validating the model, the proposal is to deploy it on an STM32 board, completing the main goal of the project, from model development to embedded hardware implementation.

## Data Preprocessing and Feature Engineering

During the data preparation phase, the following steps were taken:

- **Creation of Dummy Variables**:  
  The categorical column `Type`, which represents the type of machine (e.g., L, M, H), was converted into numerical format using one-hot encoding (`pd.get_dummies`). This allows the model to interpret each type as an independent input variable without assuming any ordinal relationship between them.

- **Creation of the `No failure` Column**:  
  Since the dataset contains separate binary columns indicating specific failure types (`TWF`, `HDF`, `PWF`, `OSF`), a new column called `No failure` was created to explicitly represent the absence of any failure. This helps transform the problem into a mutually exclusive classification (multi-class), where only one output class is active at a time.

  ```python
  df['No failure'] = (df['Machine failure'] == 0).astype(int)
  ```

- **Removal of Redundant Columns**:  
  The column `Machine failure` was removed, as it became redundant after introducing `No failure` and keeping the four specific failure types.  
  The column `RNF` was also removed from training since it represents random failures with no predictable pattern and few samples.

These transformations help the model focus on learning from meaningful patterns and reduce redundancy in the output labels.

## Exporting the Model and Test Data

At the end of the training process, the following files are saved:

```python
#  Save test data and model
np.save('X_test_labels.npy', X_test_under)
Y_test_under = Y_test_under.astype(np.float32)
np.save('Y_test_labels.npy', Y_test_under)
model.save('model_test.h5')
```

### Purpose of Saved Files:

- **`model_test.h5`**:  
  This file contains the trained neural network model in HDF5 format. It includes the model architecture, weights, and optimizer configuration. This model is intended to be **converted and deployed on an STM32 embedded board**, where the trained AI will be used in real-time to classify machine failures.

- **`X_test_labels.npy` and `Y_test_labels.npy`**:  
  These `.npy` files store the test input features and corresponding labels.  
  They are useful for:
  - Validating model predictions during integration with embedded systems
  - Debugging or running inference on a fixed input set inside STM32
  - Comparing real-time STM32 results with original Python outputs

These saved files are essential for the final stage of the project: embedding the trained AI model into a hardware


## How to Install and Run the Project

### Installation

1. **Clone this repository**:

```bash
git clone https://github.com/LuizSoares001/IA-Embarque.git
cd IA-Embarque
```

2. **Run with Google Colab (Recommended)**  
Open the `.ipynb` notebook directly in Google Colab:

```bash
Notebook Colab "TP Colab Python/TP_IA_EMBARQUEE.ipynb"
```
---

3. **Run Locally via Python script**  
If you prefer, you can also run the script locally:

```bash
python "TP Colab Python/tp_ia_embarquee.py"
```

## How to Use the Project

After installing the requirements and running the notebook or script, the model will:

- Preprocess and normalize the dataset
- Remove random or redundant failure types
- Handle data imbalance using undersampling and class weights
- Train a neural network to classify the failure type
- Display evaluation metrics such as accuracy, recall, precision, F1-score, and confusion matrix

The output includes:
- Classification report for each class (No Failure, TWF, HDF, PWF, OSF)
- Normalized confusion matrix
- Training and validation plots (loss and accuracy curves)

You can modify parameters like the learning rate, batch size, or class weights in the training cell for experimentation.

## Deployment on STM32

After training and validating the model in Python, it was successfully embedded into a microcontroller for real-time inference.

The deployment was done on an **STM32L4R9** development board using **STM32CubeIDE** and **STM32CubeMx**. The trained neural network (`model_test.h5`) was converted and integrated into a C project to classify machine failures directly on the embedded hardware.

### Results and Embedded Code

The final embedded implementation, including the inference logic and model integration, can be found in the following folder of this repository:

```bash
CubeIDE/FinalProject_Soares
```

### Communication Test with STM32

A communication test was conducted to validate the model's predictions running on the embedded STM32 board. The test involved sending the first 100 samples from the test dataset and collecting the predictions returned by the board.

The results of this test are saved in the following file:

```bash
CubeIDE/test_communication.txt
```

This validated the successful integration of the AI model with the STM32 system and confirmed that the predictions are consistent with those produced during model evaluation in Python.
