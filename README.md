# Brain Tumor Detection from MRI Images 🧠🔬

An implementation of a convolutional neural network to detect presence of brain tumours from MRI scans. This repository contains training and inference code, two pretrained Keras model files (.h5), a dataset layout expectation, a small conversion utility for .mha medical images, and the project report.

This README replaces the original content and documents how to reproduce the results locally, what each file does, and practical notes about missing pieces you may need to add before running the web demo.

---

## Quick summary
- Problem: Binary classification of MRI slices into "No Brain Tumor" and "Yes Brain Tumor" using a small CNN trained with Keras/TensorFlow.
- Intended audience: students or researchers who want a minimal end-to-end example (preprocessing → training → web demo) for brain tumor classification on 2D MRI slices.

## Stack
- Language(s): Python 3.x
- Framework / runtime: TensorFlow / Keras (TF 2.x recommended), Flask for the demo
- Notable libraries: tensorflow (keras API), opencv-python, Pillow, scikit-learn, SimpleITK (for .mha), Flask

## Top-level layout
```
BrainTumor10Epochs.h5                # pretrained model (saved by author)
BrainTumor10Epochscategorical.h5     # pretrained model (categorical variant)
Major_project_report_p1.pdf          # project report
README.md                            # (this file) replaced/updated
app.py                               # Flask demo app (serves index.html, expects uploads/)
mainTrain.py                         # training script (expects datasets/no and datasets/yes)
mainTest.py                          # small test/inference script (uses a hardcoded path)
mritopng.py                          # .mha -> PNG utility (SimpleITK)
```

Notes: There is no templates/index.html or uploads/ directory included in the repo; create them before running the web app. mainTest.py contains a hard-coded Windows path you will likely want to change.

## How it works (high-level)
- Data: mainTrain.py expects a `datasets/` folder containing two subfolders: `datasets/no/` and `datasets/yes/` holding JPG images of MRI slices.
- Training: mainTrain.py loads all JPG images from those folders, resizes to 64×64, normalizes, one-hot encodes labels and trains a small Sequential CNN for 10 epochs, then saves the model to `BrainTumor10Epochscategorical.h5`.
- Inference: app.py loads `BrainTumor10Epochs.h5` at startup, provides an upload endpoint (`/predict`) that saves the uploaded file to `uploads/`, resizes the image to 64×64 and calls model.predict. The response is a plain text label: "No Brain Tumor" or "Yes Brain Tumor".

## Reproduce: fresh clone → demo run
Follow these steps on a machine with Python 3.8+ (adjust versions to your environment):

1) Clone the repo
```bash
git clone https://github.com/Shashi028/Bachelors-Final-Year-Project.git
cd Bachelors-Final-Year-Project
```

2) Create and activate a virtual environment (recommended)
```bash
python -m venv .venv
# Linux / macOS
source .venv/bin/activate
# Windows (PowerShell)
.\.venv\Scripts\Activate.ps1
```

3) Install dependencies
```bash
pip install --upgrade pip
pip install tensorflow pillow opencv-python scikit-learn flask simpleitk numpy
```
If you want to keep a reproducible set of versions, create a requirements.txt and pin versions (example below).

4) Prepare folders used by the code
```bash
mkdir -p uploads
mkdir -p datasets/no datasets/yes
```
- Put training images (JPG) into the appropriate `datasets/no` and `datasets/yes` directories.
- The app expects a `templates/index.html` file. Create a `templates/` directory and add a minimal `index.html` (example provided below).

5) (Optional) Convert medical .mha / .nii files to PNG using mritopng.py
Edit mritopng.py and set `file_path` to your .mha file. Then run:
```bash
python mritopng.py
```
This will write `output.png` (you may want to extend the script to export multiple slices).

6) (Optional) Train the model from scratch
```bash
python mainTrain.py
```
- The script trains for 10 epochs with a batch size of 16 and saves `BrainTumor10Epochscategorical.h5`.
- If your dataset is small, reduce batch size or use data augmentation.

7) Test inference locally using the included model file (or the model you trained)
- Edit `mainTest.py` and replace the hardcoded path with a path to a local image, or run the Flask app and use the web interface.

8) Run the Flask demo app
```bash
python app.py
```
- The app loads `BrainTumor10Epochs.h5` on startup. If you want to use the categorical model produced by training, modify app.py to load `BrainTumor10Epochscategorical.h5` instead.
- Open your browser at http://127.0.0.1:5000/ (app prints the URL on load). The `/predict` route expects a POST multipart form with a `file` field.

## Minimal templates/index.html (example)
Create `templates/index.html` with this content to get a simple upload UI:

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Brain Tumor Detector</title>
  </head>
  <body>
    <h1>Upload MRI image</h1>
    <form action="/predict" method="post" enctype="multipart/form-data">
      <input type="file" name="file" accept="image/*" required />
      <button type="submit">Predict</button>
    </form>
    <p>After submitting you will receive a plain-text label.</p>
  </body>
</html>
```

## Notes, gotchas and suggested improvements
- Missing files: `templates/index.html` and an `uploads/` directory are not present in the repository and are required by app.py. Create them before running the app.
- Model mismatch: The repo contains two .h5 files. app.py currently loads `BrainTumor10Epochs.h5`; mainTrain saves `BrainTumor10Epochscategorical.h5`. Ensure you load the intended file.
- mainTest.py uses a hard-coded Windows path — update it to a local image path or parameterize it.
- The current training pipeline does no data augmentation, no class-balance handling, and resizes images forcefully to 64×64. These choices limit accuracy; consider transfer learning (MobileNet/ResNet) and augmentation for better results.
- Security: Uploaded files are saved without validation. For production, add file-type checks and more robust handling.

## Example requirements (create `requirements.txt`)
```
tensorflow>=2.6
numpy
opencv-python
Pillow
scikit-learn
flask
simpleitk
```

## Try asking
- "Where should I put new MRI JPG files so mainTrain.py will see them?" (Answer: datasets/no and datasets/yes)
- "How can I switch app.py to use the model saved by mainTrain.py?" (Answer: change the load_model filename in app.py)
- "Can I run the app without GPU?" (Answer: yes; TensorFlow will run on CPU but may be slower; pin a CPU-only TF build if necessary)

## License & Author
Author: Shashi028
This repository is provided as-is for educational purposes. Please attribute the author and do not use the pretrained model for clinical decision-making.
