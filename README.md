#Vision System — Environmental Degradation Test

## What this project does

This project tests how well an AI model can identify underwater objects
when the camera conditions are bad — like murky water, deep water color
changes, and low-light noise. It simulates what a real underwater robot
camera would see and checks how much the AI struggles.

---

## What to install

```bash
pip install torch torchvision pillow opencv-python numpy
```

---

## What i need to put in the folder

Before running, i made sure these images are in the same folder as the code:

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


## What the code does

It takes each image and creates 3 damaged versions of it:

- **Turbidity** — makes the image blurry and hazy like murky water using Gaussian Blur (21x21) blended with a pale haze layer
- **Color Shift** — removes 85% of the red channel to simulate deep underwater color loss
- **Sensor Noise** — adds Gaussian noise (mean=0, std=35) to simulate a low-light camera sensor

Then it runs the AI on all 4 versions (original + 3 damaged) and prints
what it thinks the image is, how confident it is, and how long it took.

---

## Actual Results

High confidence (close to 1.0) means the model is sure.
Low confidence (close to 0.0) means the model is confused.

### aquarium.jpg
| Condition | Prediction | Confidence | Latency |
|-----------|-----------|------------|---------|
| Baseline (Clean) | eel | 0.4804 | 62.38 ms |
| Turbidity | jacamar | 0.0355 | 20.65 ms |
| Color Shift | eel | 0.5497 | 23.79 ms |
| Sensor Noise | coral reef | 0.0731 | 23.41 ms |

### jellyfish.jpg
| Condition | Prediction | Confidence | Latency |
|-----------|-----------|------------|---------|
| Baseline (Clean) | jellyfish | 0.9949 | 21.90 ms |
| Turbidity | rapeseed | 0.0616 | 20.41 ms |
| Color Shift | jellyfish | 0.9939 | 20.45 ms |
| Sensor Noise | wool | 0.2716 | 19.88 ms |

### set_f20_SESR.png (jellyfish from below)
| Condition | Prediction | Confidence | Latency |
|-----------|-----------|------------|---------|
| Baseline (Clean) | hen-of-the-woods | 0.2156 | 22.17 ms |
| Turbidity | rapeseed | 0.1483 | 21.65 ms |
| Color Shift | coral reef | 0.3079 | 20.64 ms |
| Sensor Noise | coral reef | 0.1963 | 21.72 ms |

### set_f46_SESR.png (fish near coral)
| Condition | Prediction | Confidence | Latency |
|-----------|-----------|------------|---------|
| Baseline (Clean) | king crab | 0.3312 | 20.05 ms |
| Turbidity | tarantula | 0.1387 | 20.76 ms |
| Color Shift | king crab | 0.6030 | 28.51 ms |
| Sensor Noise | tarantula | 0.3688 | 27.89 ms |

### set_o20_SESR.png (sea turtle)
| Condition | Prediction | Confidence | Latency |
|-----------|-----------|------------|---------|
| Baseline (Clean) | loggerhead | 0.7938 | 35.55 ms |
| Turbidity | loggerhead | 0.6403 | 23.74 ms |
| Color Shift | loggerhead | 0.6165 | 21.07 ms |
| Sensor Noise | loggerhead | 0.5735 | 23.30 ms |

### set_u106_SESR.png (spider crabs)
| Condition | Prediction | Confidence | Latency |
|-----------|-----------|------------|---------|
| Baseline (Clean) | rock beauty | 0.8001 | 21.92 ms |
| Turbidity | dugong | 0.3380 | 22.40 ms |
| Color Shift | rock beauty | 0.8828 | 22.69 ms |
| Sensor Noise | rock beauty | 0.7632 | 25.96 ms |

### set_u113_SESR.png (yellow tang fish)
| Condition | Prediction | Confidence | Latency |
|-----------|-----------|------------|---------|
| Baseline (Clean) | king crab | 0.2095 | 23.49 ms |
| Turbidity | kite | 0.2279 | 20.93 ms |
| Color Shift | leatherback turtle | 0.2165 | 22.44 ms |
| Sensor Noise | tarantula | 0.3626 | 19.29 ms |

---

## Key Observations from Results

- **Turbidity** was the most damaging — confidence dropped below 0.07 on most images and predictions changed to completely wrong classes
- **Color Shift** was surprisingly less damaging — jellyfish maintained 0.9939 confidence, some images even gained confidence
- **Sensor Noise** had variable impact — sea turtle survived well (0.7938 → 0.5735) while jellyfish failed completely (0.9949 → 0.2716)
- **Sea turtle** was the most robust subject — correctly predicted in all 4 conditions
- **Model becomes unreliable** when confidence drops below 0.15

---

## Latency Summary

| Model | Avg Latency | Max Latency | 30 FPS Budget |
|-------|------------|-------------|---------------|
| ResNet18 | 22.84 ms | 35.55 ms | 33.33 ms |
| MobileNetV2 | 16.04 ms | 31.28 ms | 33.33 ms |

ResNet18 occasionally exceeds the 30 FPS budget (max 35.55 ms). MobileNetV2 is faster and safer for real-time use.

---

## Models used

- **ResNet18** — main model (11.69M parameters)
- **MobileNetV2** — bonus comparison model (3.50M parameters, faster)

---


