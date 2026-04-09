# Brain Tumor Detection from MRI Images 🧠🔬

[![Python](https://img.shields.io/badge/Python-100%25-blue.svg)](https://www.python.org/)
[![Deep Learning](https://img.shields.io/badge/Deep_Learning-Keras%20%7C%20TensorFlow-orange.svg)]()

This repository contains the source code and pre-trained models for my Bachelor's Final Year Project. The project leverages Deep Learning (Convolutional Neural Networks) to detect and classify brain tumors from MRI scans. It includes scripts for data preprocessing, model training, evaluation, and a web application interface for easy inference.

## 📁 Repository Structure

*   **`app.py`**: The main web application script (likely using Flask or similar) to provide a user interface for uploading an MRI image and receiving a tumor prediction.
*   **`mainTrain.py`**: The training script used to build, train, and compile the CNN model on the MRI dataset.
*   **`mainTest.py`**: The testing/inference script used to evaluate the model's accuracy and performance on unseen data.
*   **`mritopng.py`**: A preprocessing utility script to convert raw MRI files into PNG image formats suitable for model input.
*   **`BrainTumor10Epochs.h5` & `BrainTumor10Epochscategorical.h5`**: Pre-trained Keras model weights trained for 10 epochs. You can use these to run predictions without having to retrain the model from scratch.
*   **`Major_project_report_p1.pdf`**: Detailed documentation and the first part of the formal major project report.

## 🚀 Getting Started

### Prerequisites

Ensure you have Python 3.x installed. You will also need to install the required libraries. While a `requirements.txt` is not provided, you will typically need:

```bash
pip install tensorflow keras numpy opencv-python flask pillow
```
*(Adjust the installations depending on the exact frameworks used in `app.py` and `mainTrain.py`)*

### Running the Application

To start the web application and test the model with your own MRI images:

1. Clone the repository:
   ```bash
   git clone https://github.com/Shashi028/Bachelors-Final-Year-Project.git
   cd Bachelors-Final-Year-Project
   ```
2. Run the application:
   ```bash
   python app.py
   ```
3. Open your web browser and navigate to the local host address provided in your terminal (usually `http://127.0.0.1:5000/`) to interact with the web interface.

### Training the Model (Optional)

If you want to train the model from scratch or tune the hyperparameters:
1. Ensure your dataset is properly configured in the directories expected by the script.
2. Run the training script:
   ```bash
   python mainTrain.py
   ```
   This will generate a new `.h5` model file upon completion.

### Data Preprocessing

If you have raw MRI files that need to be converted to PNG format before testing or training, you can utilize the provided conversion script:
```bash
python mritopng.py
```

## 📄 Documentation

For an in-depth explanation of the methodology, architecture, and results, please refer to the `Major_project_report_p1.pdf` included in this repository.

## 👨‍💻 Author

**Shashi028**
- [GitHub Profile](https://github.com/Shashi028)
