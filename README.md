# Audience Dataset Classification Project

This project aims to classify face images from the **Audience** dataset into both age and gender categories. The environment is based on PyTorch (Python 3.8), and the architectures tested include **ResNet50** and **MobileNetV3** (from [pytorch-image-models](https://github.com/rwightman/pytorch-image-models)).

## Project Structure

|-- code  
|   |-- Step1_EDA.ipynb                  # Data preprocessing (EDA, label.json generation)  
|   |-- Step2_training.ipynb             # Initial training (ResNet50, MobileNetV3)  
|   |-- Step3_mobilenetv3_training_only_age_5fold.ipynb  # MobileNetV3 only for age prediction with 5-fold CV  
|   |-- Step4_testing.ipynb              # Testing the trained models with external images  
|   |-- additional_accuracy.ipynb                # Accuracy extraction and visualization  
|
|-- log  
|   |-- fold0_train_log.txt  
|   |-- fold1_train_log.txt  
|   |-- fold2_train_log.txt  
|   |-- fold3_train_log.txt  
|   |-- fold4_train_log.txt  
|  
|-- README.md                            # Project documentation  

## Dataset

- **Audience** dataset

## Environment

- Python 3.8
- PyTorch

## Classification Tasks

- **Age:** 8 classes (0–2, 3–6, 8–13, 15–20, 22–32, 34–43, 45–53, 55–100)
- **Gender:** 3 classes

## Instructions

1. **Data Preprocessing**  
   Run `code/Step1_EDA.ipynb` to:
   - Perform exploratory data analysis (EDA).
   - Preprocess and categorize images.
   - Generate `label.json`.

2. **Initial Training**  
   Run `code/Step2_training.ipynb`:
   - Uses a 10% validation split (no 5-fold CV yet).
   - Applies data augmentation with `albumentations`.

   **Experiments:**
   
   - **ResNet50 (batch_size=8)**  
     - **Gender Acc:** 92.66%  
     - **Age Acc:** 57.45%  
     - **Time:** ~60 min  
     - **FLOPs:** 85,188,107  
     - **Params:** 24,821,467  
     - *Note:* Lower accuracy than public benchmarks. Consider more epochs or larger batch sizes. (Reduced batch size from 256 to 8 due to memory limits.)
   
   - **ResNet50 (One-Cycle, batch_size=16)**  
     - **Epochs:** 9  
     - **Gender Acc:** 95.12%  
     - **Age Acc:** 68.27%  
     - **Time:** ~53 min  
     - Significant improvement over previous results, same FLOPs and parameter count.

   - **MobileNetV3 (batch_size=16)**  
     - **Gender Acc:** 96.68%  
     - **Age Acc:** 82.28%  
     - **Time:** ~48 min  
     - **FLOPs:** 6,415,043  
     - **Params:** 3,688,163  
     - Best performance so far—higher accuracy, fewer parameters, shorter training time.
     - Future improvements should focus on MobileNetV3.

3. **Fine-Tuning and 5-Fold CV**  
   Run `code/Step3_mobilenetv3_training_only_age_5fold.ipynb`:
   - Removes gender prediction, focusing solely on age.
   - Fine-tunes MobileNetV3.
   - Implements 5-fold cross-validation for robustness.

4. **Testing**  
   Run `code/Step4_testing.ipynb` to:
   - Test the model’s robustness using external images.

## Future Work

- Focus on MobileNetV3 for further improvements.
- Experiment with extended training epochs, additional data augmentation, or refined hyperparameters.
- Consider larger batch sizes if memory permits.

---

**Note:** This README provides an overview of the project structure, instructions for running the code, and details on the experiments and results.
