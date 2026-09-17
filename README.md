# Adaptive Satellite Image Analysis under Temporal and Spatio-Temporal Domain Shifts

Code for the paper Adaptive Satellite Image Analysis under Temporal and Spatio-Temporal Domain Shifts (Nguyen Ba, Iovine, Ziffer, Della Valle — Motus ML & Politecnico di Milano).

##What this is

Offline land-cover models degrade as soon as they leave their training distribution, and in Earth observation they always do: satellites keep imaging the same regions while the surface beneath them changes. This repository implements a streaming alternative, where representation learning and adaptation are decoupled and the model never stops updating.

A frozen DINOv3 backbone (ViT-L/16, pre-trained on satellite imagery) turns each image into patch embeddings and is never fine-tuned. The embeddings are projected and normalised, then fed to an incremental classifier that runs under a prequential protocol: each monthly batch is predicted on before the model is updated with it, so performance is always measured on data the model has not yet learned from. There is no retraining from scratch and no stored dataset.

##What it evaluates

Experiments run on DynamicEarthNet (75 areas of interest worldwide, monthly semantic maps across 2018–2019, six strongly imbalanced land-cover classes) under two distribution shifts:

Temporal — same regions, future observations. Train on 2018, test on 2019.
Spatio-temporal — future observations over geographically unseen regions. Train on 55 areas, test on 20 areas never seen during training.

Each shift is evaluated under two protocols: a static one, where the model is frozen after initial training, and a prequential one with monthly online updates. Metrics cover land-cover classification (per-class IoU, mIoU) and change detection (BC, SC, SCS).
