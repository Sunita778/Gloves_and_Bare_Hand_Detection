## README: Gloved vs Ungloved Hand Detection with YOLOv8
This project addresses a real-world computer vision problem: building a safety compliance system to detect whether factory workers are wearing gloves. It provides a complete object detection pipeline, from model fine-tuning to generating structured detection logs.

***
## 🚀 Project Overview

The objective is to accurately classify hands in images as either `gloved_hand` or `bare_hand`. The solution is deployed as a Python script that processes a batch of images and outputs annotated visuals and structured JSON data.

| Component | Description |
| :--- | :--- |
| **Model** | A fine-tuned YOLOv8 model for specialized hand detection. |
| **Pipeline** | A Python script that automates detection, annotation, and logging. |
| **Output** | Annotated images and a JSON file per image with detection details. |

***

<p align="center">
  <img src="https://github.com/Sunita778/Gloves_and_Bare_Hand_Detection/blob/main/output_annotated/runs/1596612561735_jpg.rf.5bbba331bbbf08b7bf69eab7729db958.jpg" width="200"/>
  <img src="https://github.com/Sunita778/Gloves_and_Bare_Hand_Detection/blob/main/output_annotated/runs/IMG20220224162651_jpg.rf.047066a84a7c1a7390953cbc3ac24ce0.jpg" width="200"/>
  <img src="https://github.com/Sunita778/Gloves_and_Bare_Hand_Detection/blob/main/output_annotated/runs/1597378231696_jpg.rf.22442a29caf1a827dd387f8f31acbb72.jpg" width="200"/>
  <img src="https://github.com/Sunita778/Gloves_and_Bare_Hand_Detection/blob/main/output_annotated/runs/IMG20220224173903_jpg.rf.a86d604e03b8a96a9ef8580d6f4a69fa.jpg" width="200"/>
</p>


## ⚙️ Implementation

### 1. Model Selection and Fine-Tuning

* **Initial Problem:** A standard object detection model (like YOLOv8 pre-trained on the COCO dataset) is not suitable for this task because its class list does not include `gloved_hand` or `bare_hand`. Using such a model would result in irrelevant detections, such as "person" or "handbag." 
* **Solution:** To solve this, a **custom dataset** was created and annotated with the required class labels. The `yolov8n.pt` model was then fine-tuned on this dataset. This approach leverages the powerful feature extraction capabilities of a pre-trained model while specializing it for a unique domain.

### 2. Dataset and Preprocessing

* **Data Organization:** The dataset was organized into the following structure to be compatible with YOLOv8 training:
    ```
    dataset/
    ├── train/
    │   ├── Images/
    │   └── labels/
    └── Valid/
        ├── Images/
        └── labels/
    ```

* **Dataset:** A custom dataset was manually created, consisting of images with both `gloved_hand` and `bare_hand`.
* **Format:** The dataset was organized into the standard YOLO format to facilitate training. This included separate directories for training and validation images and their corresponding labels.
* **YAML Configuration:** A `data.yaml` file was used to map dataset paths and define the two custom classes, ensuring the training process correctly identified the data.

### 3. Pipeline Development

* **Core Logic:** The pipeline is a Python script built using the `ultralytics` library. It automates the entire process, making it simple to use.
* **Dynamic Paths:** To prevent `ERROR` during execution, the script was designed to use a single configuration point for input/output folders and to dynamically retrieve the path to the trained model after the training phase. This makes the pipeline robust and easy to deploy.
* **Structured Output:** The script not only saves annotated images but also generates a JSON file for each image. This provides a machine-readable record of all detections, including `label`, `confidence`, and `bbox` coordinates, which is crucial for integration into a larger safety compliance system.

***

## ▶️ How to Run the Script

### Prerequisites

#### 📦 Installation

Clone this repo and install dependencies:
```
git clone https://github.com/ultralytics/ultralytics.git
cd ultralytics
pip install -r requirements.txt
```
Or install YOLOv8 directly via PyPI:
```
pip install ultralytics
```

### Instructions

*  **Set up your folder structure:** Organize your project with the following directories in your Google Drive or local machine:
    * `dataset/` (contains your `train` and `validation` data)
    * `input_images/` (for new images you want to test)
    * `output_annotated/`
    * `output_logs/`


*  **Execute the Script:** Run the Python script. It will perform two main tasks sequentially:
    * **Training:** It will train a new model and save it to `training_runs/`.
    * **Inference:** It will then load the newly trained model and run detection on the images in the `input_images` folder.

The results, including annotated images and JSON logs, will be saved to the `output_annotated/` and `output_logs/` folders, respectively.

-----


## 🙌 How to Use

- Fork this repository

- Clone it locally
bash
```
git clone https://github.com/Sunita778/Gloves_and_Bare_Hand_Detection.git
```

- Install dependencies (see Installation
)

- Place your images in images/ and your trained model weights (best.pt) in the root folder

- Run detection with detect.py

- Check the output/ folder for results


## 🤝 Contributing

I welcome contributions! Here’s how you can help:

- Fork the repository

- Create a new branch
bash
```
git checkout -b feature-new-functionality
```

- Make your changes (bug fix, new feature, documentation improvement, etc.)

- Commit and push your changes
```
git commit -m "Add new feature"
git push origin feature-new-functionality
```

- Open a Pull Request describing your changes