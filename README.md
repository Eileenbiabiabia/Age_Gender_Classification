|-- code
|   |-- Step1_EDA.ipynb                  # Data preprocessing (EDA, label.json generation)
|   |-- Step2_training.ipynb             # Initial training (ResNet50, MobileNetV3)
|   |-- Step3_mobilenetv3_training_only_age_5fold.ipynb  # MobileNetV3 only for age prediction with 5-fold CV
|   |-- Step4_testing.ipynb              # Testing the trained models with external images
|   |-- 附_准确率提取.ipynb                # Accuracy extraction and visualization
|
|-- log
|   |-- fold0_train_log.txt
|   |-- fold1_train_log.txt
|   |-- fold2_train_log.txt
|   |-- fold3_train_log.txt
|   |-- fold4_train_log.txt
|
|-- README.md                            # Project documentation

Dataset
	•	Audience dataset

Environment
	•	Python 3.8
	•	PyTorch

Classification Tasks
	•	Age: 8 classes (0–2, 3–6, 8–13, 15–20, 22–32, 34–43, 45–53, 55–100)
	•	Gender: 3 classes

Steps to Run
	1.	Run Step1_EDA.ipynb
	•	Perform exploratory data analysis and preprocessing.
	•	Classify images by age and gender.
	•	Generate label.json.
	2.	Run Step2_training.ipynb
	•	Initial training without 5-fold cross-validation, using a 10% validation split.
	•	Data augmentation applied via albumentations.
Experiments:
	1.	ResNet50, batch_size=8:
	•	Gender Acc: 92.66%, Age Acc: 57.45%
	•	Training Time: ~60 min
	•	FLOPs: 85,188,107
	•	Params: 24,821,467
	•	Lower accuracy than reported benchmarks. Potential improvements include more epochs or larger batch size. (Original batch size: 256; reduced to 8 due to memory limits.)
	2.	ResNet50 with One-Cycle Training, batch_size=16:
	•	Trained for 9 epochs
	•	Gender Acc: 95.12%, Age Acc: 68.27%
	•	Training Time: ~53 min
	•	Significant accuracy improvement with the same FLOPs and parameters as above.
	3.	MobileNetV3, batch_size=16:
	•	Gender Acc: 96.68%, Age Acc: 82.28%
	•	Training Time: ~48 min
	•	FLOPs: 6,415,043
	•	Params: 3,688,163
	•	Best performance so far. Highest accuracy, fewer parameters, and shorter training time. Future improvements should focus on MobileNetV3.
	3.	Run Step3_mobilenetv3_training_only_age_5fold.ipynb
	•	Remove gender prediction and focus solely on age classification.
	•	Fine-tune MobileNetV3.
	•	Use 5-fold cross-validation for more robust results.
	4.	Run Step4_testing.ipynb
	•	Test the model’s robustness using images from external sources.

Future Work
	•	Focus on MobileNetV3 for further improvements.
	•	Experiment with extended training time, more data augmentation, or refined hyperparameters to enhance accuracy.
	•	Consider larger batch sizes when hardware allows.
