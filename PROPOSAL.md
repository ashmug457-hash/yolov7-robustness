# Project Proposal

**Title:** How Well Does YOLOv7 Handle Bad Camera Images? A Robustness Study for Robot Vision

**Name:** [Your Name]
**Track:** A (Research)

## 1. Benchmark paper

C.-Y. Wang, A. Bochkovskiy, and H.-Y. M. Liao, "YOLOv7: Trainable Bag-of-Freebies Sets New State-of-the-Art for Real-Time Object Detectors," in *Proc. IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)*, 2023, pp. 7464–7475. doi: 10.1109/CVPR52729.2023.00721

- IEEE Xplore: https://doi.org/10.1109/CVPR52729.2023.00721
- Free PDF: https://openaccess.thecvf.com/content/CVPR2023/html/Wang_YOLOv7_Trainable_Bag-of-Freebies_Sets_New_State-of-the-Art_for_Real-Time_Object_Detectors_CVPR_2023_paper.html
- Venue: CVPR, ranked A* in the CORE ranking. The paper has more than 7,000 citations.
- Code: https://github.com/WongKinYiu/yolov7 (official, Python/PyTorch, trained weights included)
- Dataset: MS COCO 2017 (public)

## 2. Result to reproduce

I will reproduce the main COCO result for the base YOLOv7 model at 640×640 input size. The paper reports **51.4% AP** for this model. I will also run YOLOv7-tiny as a smaller model to compare against.

The paper's number is on COCO test-dev, which has no public labels. So I will test on 500 images chosen at random (with a fixed seed, so the same images are used every time) from COCO val2017, using the official evaluation script. Because I use a smaller and different image set, I expect a small difference from the paper's number. I will report it and explain it.

## 3. Research question

How much does YOLOv7's AP drop when the input images have motion blur, low light, or sensor noise (each at three levels: mild, medium, strong)? Can a simple enhancement step (CLAHE contrast enhancement) recover some of the lost accuracy?

Why this matters: a camera on a moving robot rarely gives clean images. The robot shakes, rooms can be dark, and cheap cameras add noise. YOLOv7 is tested on clean photos, so I want to measure how well it holds up in these conditions.

## 4. Data and compute

- **Data:** 500 randomly chosen images (fixed seed) from MS COCO val2017, with labels. I will make the blurred, dark and noisy versions myself with OpenCV and NumPy.
- **Model:** the authors' trained weights (yolov7.pt and yolov7-tiny.pt). I do not need to train anything.
- **Compute:** Google Colab with the free T4 GPU. I do not have my own GPU, so all work runs in Colab and results are saved to Google Drive.
- **Tools:** Python, PyTorch, OpenCV, NumPy, pycocotools, Matplotlib, GitHub.

## 5. Plan

| By | What will be done |
|---|---|
| 19 Nov | Proposal and GitHub repository submitted |
| 4 Dec | Baseline done: AP of YOLOv7 and YOLOv7-tiny on the 500 clean images, compared with the paper. Code to create the three image degradations ready |
| 11 Dec | All tests done: AP for every degradation and level, with and without CLAHE. Tables and plots made |
| 18 Dec | Final IEEE paper (4–6 pages) and code submitted |
| 21–24 Dec | Presentation with a live demo |

**Fallback:** if a full run is too slow or Colab limits my GPU time, I will use only YOLOv7-tiny and test only blur and low light. If my baseline does not match the paper, I will explain why (test-dev vs. val2017, software versions, settings) and continue with my own baseline numbers.

---
*LLM use: Claude helped me choose the paper and plan this proposal. The experiments, results and analysis will be my own.*
