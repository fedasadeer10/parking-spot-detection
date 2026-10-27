# Parking Spot Detection (Faster R-CNN)

Detects parking spots and classifies each one as **empty** or **occupied**,
using Faster R-CNN (ResNet50-FPN) with transfer learning from COCO.

## Results (PKLot test set)

| Metric | Value |
|---|---|
| mAP@50 | 0.955 |
| mAP@[.5:.95] | 0.727 |
| AP empty | 0.735 |
| AP occupied | 0.719 |

![PKLot result](pklot_result.png)

## Training data

- 500 training and 100 validation images from [PKLot](LINK)
  (fixed, angled cameras over 3 parking lots).
- 6 epochs, SGD (lr 0.005), batch size 4, backbone layers 1-2 frozen,
  light augmentation (color jitter, blur, small shift/scale/rotate).

Results hold for lots similar to PKLot. A different site (camera angle,
spot size, density) needs data that matches its conditions.

## Limitation: drone view

Tested on a top-down drone video ([Parking Lot Detection Counter](LINK), Apache 2.0).
On full frames the model produced only 1-3 boxes among hundreds of spots,
even at a confidence threshold of 0.2.

![Drone failure](drone_fail.png)

Zooming into a crop made the model output dozens of boxes (67 at threshold 0.5),
so spot size is a factor. But most predictions were wrong: occupied spots
labeled empty, and several boxes on one spot. So viewing angle and appearance
matter as well, not size alone.

![Drone zoom](drone_zoom.png)

## Lessons

- A high mAP does not mean the model generalizes to any lot.
- Before adapting a model to a new site, check camera angle, spot size,
  density, and fixed vs moving camera, then test on sample images
  before any training.
- Fixed-camera setups may suit a different approach: define spot locations once,
  then classify each spot.

## Files

- `notebook7567021e1c (1).ipynb`: training, evaluation (mAP), and external tests.
- Model weights (165 MB): [LINK to Kaggle/Hugging Face]

## Credits

PKLot dataset (cite the original authors), Parking Lot Detection Counter
by iasadpanwhar (Apache 2.0), torchvision Faster R-CNN.
