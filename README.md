## Dataset Link: https://www.kaggle.com/datasets/banuprasadb/visdrone-dataset

## Dataset Structure:

The project utilizes the large-scale aerial image dataset called VisDrone2019 Detection Dataset that is captured by using drone-mounted cameras in urban areas. This dataset is divided into several subdirectories: VisDrone2019-DET-train, VisDrone2019-DET-val, VisDrone2019-DET-test-dev and VisDrone2019-DET-test-challenge. There are two separated folders for images and labels in each split (the images are in .jpg format, and the labels are in .txt format). The annotation files are in a "YOLO" format with each line being an object with a class ID and normalized coordinates for the bounding box (center_x, center_y, width, height). Out of the images in the training set, 5926 images and 6471 label files containing multiple object categories (such as pedestrian, people, bicycle, car, van, truck, tricycle, awning-tricycle, bus) were detected during dataset analysis. The main focus in this project was on detecting the pedestrian, people and car classes, as the objective of this project is human and car detection. The number of human related objects, car instances, and the number of dense object labels per image of the dataset was determined using statistical analysis and is approximately 32,945, 21,265, and 51.8 respectively.

## Preprocessing and Augmentation:

The training of the detection model was preceded by several preprocessing and augmentation steps. First, all annotation files were parsed, and the image-label pairs were verified to ensure correct correspondence between images and labels along with valid bounding box information. Since the project focuses only on human and car detection, the original dataset was filtered to retain only the relevant classes. The pedestrian and people categories were merged into a single human class, while the car class was kept separate. Images without any target objects were excluded to reduce unnecessary training noise and improve training efficiency. This filtering process also helped partially reduce the class imbalance problem by removing irrelevant dominant classes from the original dataset. A new YOLO-formatted dataset was then created containing only two classes: human and car, along with a custom 'data.yaml' configuration file for YOLO training. In addition, all images were resized and normalized to maintain consistent input dimensions during training. Several augmentation techniques, including horizontal flipping, random scaling, cropping, brightness adjustment, and contrast enhancement, were applied to improve model generalization and robustness. Sample visualizations with bounding boxes were also generated to manually verify annotation quality and preprocessing correctness.

## Problems identified in the Dataset:

Several problems were noticed while exploring the data sets and pre-processing the dataset. Small object detection was one of the challenges as the images were taken from high altitudes of the drone, making the human and vehicle objects very small in many of the images. There were some pedestrians which were only a few pixels, making it hard to detect accurately. The second challenge was the fact that there were images with overlapping objects, so the localization and classification were more difficult. It was also found that the data was highly imbalanced, with some classes (like bicycle or ignored regions) having majority annotations and others (like target classes) having minority annotations. A big scale difference was noticed, depending on the drone's height in the air and the angle of the camera. Furthermore, the images in the data set were captured from varying lighting conditions, different viewpoints and resolution, adding to the variance of the training process. These difficulties introduced a very realistic and complicated dataset that demanded a well-processed dataset and powerful training techniques to perform accurate detection of human and car.

## Result Analysis:

The trained YOLOv8 model achieved an overall **mAP@0.5 of 0.2071** and **mAP@0.5:0.95 of 0.1178** on the validation dataset. The model obtained a **precision score of 0.6037** and a **recall score of 0.2595**. Although the overall accuracy is moderate, the result is understandable considering the challenging nature of the VisDrone aerial dataset, where objects appear extremely small, densely packed, partially occluded, and captured from varying drone altitudes and viewpoints.

The model achieved higher precision than recall, which indicates that the detector behaves conservatively. In most cases, when the model predicts an object, the prediction is correct. However, the model still misses a significant number of objects, especially small pedestrians. Human detection performance was lower than car detection because pedestrians in aerial imagery occupy very small pixel regions and are often difficult to distinguish from the background. Cars performed comparatively better because vehicles are generally larger and contain more distinguishable visual features.

Training analysis showed that both training and validation losses decreased steadily without large divergence, indicating that the model did not significantly overfit the dataset. The model successfully learned meaningful object features during training. However, performance improvements began to plateau after approximately epoch 80, suggesting that the lightweight YOLOv8 architecture had reached its learning capacity under the current configuration.

Another important observation was the performance drop from 'mAP@0.5' to 'mAP@0.5:0.95', which indicates difficulty in precise localization at higher IoU thresholds. This is expected for VisDrone because tiny aerial objects are difficult to localize accurately. Despite the moderate detection accuracy, the system achieved an average inference speed of approximately **95.85 FPS**, demonstrating that the lightweight YOLOv8 model is highly efficient and suitable for near real-time drone surveillance applications.

---

## Strengths:

- Successfully implemented a complete end-to-end aerial object detection pipeline.
- Performed dataset analysis, preprocessing, training, inference, visualization, and counting within a unified workflow.
- Achieved real-time inference performance of approximately **95.85 FPS**.
- Successfully detected both humans and cars from drone imagery.
- Implemented human counting and bounding box visualization.
- Training remained stable without severe overfitting.
- Efficient preprocessing pipeline filtered unnecessary classes and converted annotations to YOLO format correctly.
- Implemented object tracking as a bonus feature to improve practical usability.

---

## Limitations:

- The primary limitation is the comparatively low recall score, meaning that many small objects remain undetected.
- The lightweight YOLOv8 model prioritizes speed over accuracy, reducing small-object detection performance.
- Human detection performance is lower because pedestrians in aerial images appear extremely small.
- Significant class imbalance exists within the dataset.
- Crowded scenes and heavy occlusion reduce localization accuracy.
- The model struggles with precise localization at higher IoU thresholds.
- Performance plateaued after approximately epoch 80, indicating limited improvement under the current training setup.
- Lighting variation, shadows, and complex urban backgrounds occasionally produce false positives and missed detections.

---

## Challenges Faced:

- Handling extremely small objects in aerial imagery was one of the major challenges.
- Some pedestrians occupied only a few pixels, making accurate detection difficult.
- Crowded scenes created overlapping bounding boxes and heavy occlusion.
- Large scale variation occurred due to different drone altitudes and camera perspectives.
- Dataset conversion and annotation filtering required careful class remapping and label verification.
- Training on high-resolution aerial images required efficient GPU memory management.
- Balancing inference speed and detection accuracy required careful model selection and hyperparameter tuning.

---

## Possible Improvements:

The overall performance of the system could be greatly improved with several steps. However, the use of larger YOLOv8 models like YOLOv8m or YOLOv8l instead of the smaller ones might lead to better performance in feature extraction and small object detection accuracy. If the resolution of the input image is increased to 832×832 or 1024×1024, the model may be able to retain the smallest details of small pedestrians. One possible improvement for the localization of tiny objects is the use of multi-scale training or image tiling methods that would enlarge small objects to be more easily detected. Stronger augmentation like Mosaic augmentation, MixUp and Copy-Paste augmentation could be beneficial for generalization in crowded scenes. In the case of comparatively low recall, lowering the confidence threshold from 0.25 to 0.15 could be useful to identify more objects, but lead to more false positives. Longer training times and better learning-rate schedules and warmup strategies could also help the model. Moreover, the alternative architectures like Faster R-CNN or RT-DETR may have better performance in the small object detection task of aerial image. Lastly, by incorporating more sophisticated tracking algorithms and temporal consistency techniques, the precision and reliability of counting in real-time drone surveillance systems could be further enhanced.
