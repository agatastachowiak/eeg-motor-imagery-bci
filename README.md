# EEG Motor Imagery Classification

Classifying imagined left vs. right fist movement from EEG signals, exploring both
within-subject and cross-subject decoding performance. This project applies signal
processing and decoding techniques from brain-computer interface (BCI) research.

## Motivation
Brain-computer interfaces (BCIs) offer a pathway for individuals with severe motor
impairments (e.g. ALS, locked-in syndrome) to communicate and control devices using
brain activity alone. This project investigates whether imagined hand movements can be
reliably decoded from EEG signals using classical machine learning methods.

## Dataset
[PhysioNet EEG Motor Movement/Imagery Database](https://physionet.org/content/eegmmidb/) —
10 subjects, imagined left/right fist movement runs (runs 4, 8, 12), 64-channel EEG.

## Methods
- **Preprocessing:** band-pass filtering (8-30 Hz, covering mu and beta rhythms) using MNE-Python
- **Epoching:** trials segmented around each movement cue (-0.5s to 2s)
- **Feature extraction:** Common Spatial Patterns (CSP), the standard spatial filtering
  method for motor imagery BCI
- **Classification:** linear SVM, evaluated with 5-fold cross-validation

## Key finding: within-subject vs. pooled decoding
| Approach | Mean Accuracy |
|---|---|
| Pooled across 10 subjects | 53.3% (chance: 50%) |
| Within-subject (avg. across 10 subjects) | 58.9% (std: 12.4%) |
| Best individual subject | 86.7% |

Pooling data across subjects performed close to chance level, while individual subjects
showed a wide range of decodability — two subjects achieved strong, well-above-chance
accuracy (77.8% and 86.7%), while others remained near chance.

This pattern matches a well-documented phenomenon in BCI research known as **"BCI
illiteracy"**: a meaningful subset of individuals do not produce strongly decodable
motor imagery signals, likely due to differences in task engagement, imagery vividness,
and individual neuroanatomy. This finding highlights why subject-specific calibration
is standard practice in real-world BCI systems, rather than one-size-fits-all models.

[Per-subject accuracy] (results/subject_accuracy.png)

## Status
🚧 In progress — classical ML (CSP + SVM) baseline complete. Deep learning (CNN)
comparison next.

## Notebooks
- `01_data_exploration.ipynb` — data loading, filtering, epoching, initial feature extraction
- `02_multi_subject_pipeline.ipynb` — multi-subject CSP + SVM pipeline, within-subject vs. pooled analysis
