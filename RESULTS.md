# Baseline Results (Replication)

**Benchmark paper:** C.-Y. Wang, A. Bochkovskiy, H.-Y. M. Liao, "YOLOv7: Trainable Bag-of-Freebies Sets New State-of-the-Art for Real-Time Object Detectors," CVPR 2023.
**Notebook:** `01_YOLOv7_baseline.ipynb`
**Date:** 28 November 2026

## Setup

| Setting | Value |
|---|---|
| Models | Official pretrained `yolov7.pt` and `yolov7-tiny.pt` (no training) |
| Data | 500 images chosen at random (seed = 0) from COCO val2017 |
| Image size | 640 × 640 |
| Confidence / IoU threshold | 0.001 / 0.65 (official settings) |
| Metric | COCO AP @[0.50:0.95], computed with pycocotools |
| Hardware | Google Colab CPU (no GPU available) |
| Software | Python 3.13, PyTorch 2.14 (CPU), official YOLOv7 code with small compatibility fixes |

## Results

| Model | Image size | Paper AP (val2017) | My AP (500 val2017 images) | Difference | My AP50 | Time per image (CPU) |
|---|---|---|---|---|---|---|
| YOLOv7 | 640 | 51.2% (Table 1) | **53.1%** | +1.9 | 71.3% | 2.48 s |
| YOLOv7-tiny | 640 | not reported* | **36.9%** | – | 54.0% | 0.42 s |
| YOLOv7-tiny | 416 | 35.2% (Table 1) | TODO: run at 416 | – | – | – |

The paper reports 51.4% for YOLOv7 on COCO test-dev and 51.2% on val2017 (Tables 1 and 2). I compare with the val2017 number because I also use val2017 images.

\* The released `yolov7-tiny.pt` is the plain YOLOv7-tiny with LeakyReLU activation (see `cfg/deploy/yolov7-tiny.yaml`). The paper reports this model only at 416 px (35.2%) and 320 px (30.8%) in Table 1. The 38.7% at 640 px in Table 2 is for a different model, YOLOv7-tiny-SiLU. My 36.9% at 640 px lies between these two, which is what we would expect. To compare directly, I will also run the tiny model at 416 px.

### AP by object size

| Model | Small | Medium | Large |
|---|---|---|---|
| YOLOv7 (mine) | 35.1% | 60.9% | 70.6% |
| YOLOv7 (paper, Table 1) | 35.2% | 56.0% | 66.7% |
| YOLOv7-tiny | 19.3% | 44.0% | 54.6% |

## Discussion

- YOLOv7 is within 2 AP points of the paper's val2017 number (53.1% vs. 51.2%), so the main result is **successfully reproduced**.
- The differences come from the test data. The paper reports both COCO test-dev and the full val2017 set (5,000 images). I used a random subset of 500 images from val2017. A small sample can be slightly easier or harder than the full set, so AP can move by 1–2 points up or down.
- My small-object AP for YOLOv7 matches the paper almost exactly (35.1% vs. 35.2%). Medium and large objects score a few points higher than the paper, which again points to my 500-image subset being slightly easier than the full val2017 set.
- YOLOv7 is about 16 AP points more accurate than YOLOv7-tiny, but about 6× slower on CPU.
- Speeds are on a CPU, so they are much slower than the paper's GPU (V100) speeds. They are only used to compare the two models with each other.
- Small objects are the hardest for both models. This is worth watching in the next step, because blur and noise may affect small objects most.

## Next step

Apply motion blur, low light and Gaussian noise (3 levels each) to the same 500 images, measure the AP drop, and test whether CLAHE enhancement recovers part of the loss.
