# ROV Vision System — Environmental Degradation Test

## What this project does

This project tests how well an AI model can identify underwater objects
when the camera conditions are bad — like murky water, deep water color
changes, and low-light noise. It simulates what a real underwater robot
camera would see and checks how much the AI struggles.

---

## What you need to install

```bash
pip install torch torchvision pillow opencv-python numpy
```

---

## What to put in the folder

Before running, make sure these images are in the same folder as the code:

```
aquarium.jpg
jellyfish.jpg
set_f20_SESR.png
set_f46_SESR.png
set_o20_SESR.png
set_u106_SESR.png
set_u113_SESR.png
```

---

## How to run

```bash
python vision_test.py
```

The first time you run it, it will automatically download the AI model
and a list of class names from the internet. This only happens once.

---

## What the code does

It takes each image and creates 3 damaged versions of it:

- **Turbidity** — makes the image blurry and hazy like murky water
- **Color Shift** — removes the red color like deep underwater looks
- **Sensor Noise** — adds grain like a low-light camera

Then it runs the AI on all 4 versions (original + 3 damaged) and prints
what it thinks the image is, how confident it is, and how long it took.

---

## What gets saved

For each image, 3 new files are saved in the same folder:

```
imagename_turbid.jpg
imagename_colorshift.jpg
imagename_noise.jpg
```

---

## What the output looks like

```
IMAGE: jellyfish.jpg
Condition : Baseline (Clean)   Prediction: jellyfish   Confidence: 0.9949
Condition : Turbidity          Prediction: rapeseed    Confidence: 0.0616
Condition : Color Shift        Prediction: jellyfish   Confidence: 0.9939
Condition : Sensor Noise       Prediction: wool        Confidence: 0.2716
```

High confidence (close to 1.0) means the model is sure.
Low confidence (close to 0.0) means the model is confused.

---

## Models used

- **ResNet18** — main model (11.69M parameters)
- **MobileNetV2** — bonus comparison model (3.50M parameters, faster)

---

## Project Structure

```
rov-vision-task/
├── vision_test.ipynb        # main notebook
├── README.md                # this file
├── ROV_Vision_Report.pdf    # analysis report
├── aquarium.jpg             # input images
├── jellyfish.jpg
├── set_f20_SESR.png
├── set_f46_SESR.png
├── set_o20_SESR.png
├── set_u106_SESR.png
├── set_u113_SESR.png
└── outputs/                 # generated degraded images
    ├── aquarium_turbid.jpg
    ├── aquarium_colorshift.jpg
    ├── aquarium_noise.jpg
    └── ...
```
