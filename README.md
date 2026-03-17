# License-Plate-Corner-Keypoint-Detection
1. Introduction
Accurate localization of license plates is a fundamental step in automated traffic monitoring, vehicle identification, and intelligent transportation systems. Beyond coarse object detection, many applications require precise geometric localization of license plates to enable normalization, rectification, and character analysis. This project focuses on detecting the four corner keypoints of a vehicle license plate using deep learning–based keypoint regression.
The provided dataset consists of vehicle images annotated with the coordinates of the plate’s four vertices. The dataset is pre-split into train and val partitions. The training subset is used for both model optimization and internal validation, while the validation subset is used exclusively for final performance evaluation.
<img width="593" height="395" alt="image" src="https://github.com/user-attachments/assets/f584aa7f-a06e-4fc5-be15-3f9231ed8764" />
Figure 1: Example image from the dataset showing annotated license plate corner keypoints.
2. Task Description
The objective of the task is to design and train a deep learning system that directly regresses the coordinates of the four plate corners. Each target consists of 8 normalized values (x_1,y_1,…,x_4,y_4)representing the keypoints’ positions relative to the image size.
Challenges of this task include:
	significant perspective deformation depending on the viewing angle,
	varying illumination conditions,
	partially occluded or blurred license plates,
	differences in plate size across images.
3. Methods
3.1 Data Processing
Images were resized to a fixed resolution of 256×256, and keypoint coordinates were normalized accordingly. Batches of images and keypoints were loaded using a PyTorch DataLoader.
3.2 Model Architecture
A convolutional regression model based on MobileNetV3 was employed due to its efficiency and strong feature extraction capabilities. The network was modified by replacing the classification head with a fully connected regression layer predicting eight continuous values corresponding to the four keypoints.
The model was trained using:
	Loss function: Smooth L1 loss (Huber)
	Optimizer: Adam
	Learning rate: chosen empirically
	Training device: T4 (Google Colab)
After training, the best checkpoint (epoch 25) was selected and used for evaluation.
4. Evaluation Metric
Performance was assessed using Object Keypoint Similarity (OKS), a standard metric for evaluating keypoint localization. OKS measures the similarity between predicted and ground-truth keypoints while accounting for:
	scale of the object,
	per-keypoint variability,
	keypoint visibility.
An OKS score of 1.0 indicates perfect alignment, while 0 indicates complete mismatch. The average OKS over the validation set is used as the main quantitative indicator.
5. Results
5.1 Quantitative Performance
Using the final model checkpoint, OKS was computed across all validation samples.
	Mean OKS: 0.8266
	Number of evaluation samples (keypoint sets): 1140
This score indicates that the predicted keypoints are consistently close to ground-truth locations, with errors typically small relative to the object scale.
<img width="407" height="408" alt="image" src="https://github.com/user-attachments/assets/452f0589-ac80-485b-aaff-deb76a67fb0b" />
<img width="408" height="408" alt="image" src="https://github.com/user-attachments/assets/d87795c2-9e00-4862-b17d-0a69f50e8f4f" />
Figure 2: Preticted and true annotated license plate corner keypoints.
5.2 Qualitative Analysis
Visual inspection of predictions confirms that the model reliably localizes all four corners even under:
•	strong perspective distortions,
•	shadowing,
•	partial blurring of the plate region.
Overlay plots show that red (predicted) points generally align closely with green (ground-truth) points. In most cases, corner ordering is preserved, and prediction noise is minimal.
6. Discussion
The model demonstrates strong performance on the provided dataset. Several factors contribute to successful detection:
•	MobileNetV3’s lightweight architecture is expressive enough for precise keypoint regression.
•	Consistent preprocessing and normalization simplify the geometric relationships the model must learn.
•	OKS scoring confirms that predictions remain accurate across diverse vehicle viewpoints.
Potential improvements include:
•	using heatmap-based keypoint detectors instead of direct regression for better spatial precision,
•	adding perspective-aware augmentation,
•	incorporating transformer-based backbones to capture more global context.
7. Conclusion
This project successfully developed a deep learning model capable of detecting the four corner keypoints of license plates in vehicle images. The trained MobileNetV3 regression network achieved a mean OKS of 0.8266 on the validation set, demonstrating reliable performance across varied imaging conditions. The resulting model provides accurate geometric localization suitable for downstream tasks such as plate rectification, recognition, or tracking within intelligent transportation systems.

