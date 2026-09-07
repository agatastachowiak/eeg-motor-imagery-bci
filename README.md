# EEG Motor Imagery Classification

Classifying imagined left vs. right fist movement from EEG signals, comparing
classical machine learning and deep learning approaches, and investigating how
both individual variability and dataset size affect decoding performance. This
project applies signal processing and decoding techniques from brain-computer
interface (BCI) research.

## Motivation
Brain-computer interfaces (BCIs) offer a pathway for individuals with severe motor
impairments (e.g. ALS, locked-in syndrome) to communicate and control devices using
brain activity alone. This project investigates whether imagined hand movements can be reliably decoded from EEG signals, comparing a classical machine learning approach (CSP + SVM) against a deep learning approach (CNN) to see which performs better — and how that comparison changes depending on the amount of available training data.

## Dataset
[PhysioNet EEG Motor Movement/Imagery Database](https://physionet.org/content/eegmmidb/) —
up to 50 subjects, imagined left/right fist movement runs (runs 4, 8, 12), 64-channel EEG.

## Methods
- **Preprocessing:** band-pass filtering (8-30 Hz, covering mu and beta rhythms) using MNE-Python
- **Epoching:** trials segmented around each movement cue (-0.5s to 2s)
- **Classical approach:** Common Spatial Patterns (CSP) feature extraction + linear SVM, the standard method in motor imagery BCI research
- **Deep learning approach:** compact EEGNet-style CNN, learning spatial and temporal patterns directly from raw epoch data
- All results evaluated with 5-fold cross-validation (CSP+SVM) or held-out validation
  split (CNN)

## Key finding

### 1. Individual variability strongly affects pooled decoding
| Approach (10 subjects)| Accuracy |
|---|---|
| Pooled across 10 subjects | 53.3% (chance: 50%) |
| Within-subject (avg. across 10 subjects) | 58.9% (std: 12.4%) |
| Best individual subject | 86.7% |

Pooling data across subjects performed close to chance level, while individual subjects showed a wide range of decodability. Two subjects achieved strong accuracy (77.8%, 86.7%) while others remained near chance — a pattern consistent with **"BCI
illiteracy,"** a documented phenomenon in BCI research where a meaningful subset of
individuals do not produce strongly decodable motor imagery signals, likely due to differences in task engagement, imagery vividness, and individual neuroanatomy. 

![Per-subject accuracy](Result1_subject_accuracy.png)

## Status
🚧 In progress — classical ML (CSP + SVM) baseline complete. Deep learning (CNN)
comparison next.

## Notebooks
- `01_data_exploration.ipynb` — data loading, filtering, epoching, initial feature extraction
- `02_multi_subject_pipeline.ipynb` — multi-subject CSP + SVM pipeline, within-subject vs. pooled analysis
