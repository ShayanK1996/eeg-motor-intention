# EEG Motor Intention Classification with FBCSP

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![MNE](https://img.shields.io/badge/MNE-1.0+-green.svg)](https://mne.tools/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Filter Bank Common Spatial Patterns (FBCSP) pipeline for classifying motor intention from EEG signals. Discriminates between grasp types (bottle vs. empty hand) using spatial filtering across multiple frequency bands.

## Results

| Subject | Accuracy | F1 Score | ROC-AUC |
|---------|----------|----------|---------|
| S3  | 99.3% ± 1.3% | 0.993 | 1.000 |
| S4  | 92.7% ± 1.3% | 0.925 | 0.962 |
| S5  | 65.3% ± 7.5% | 0.634 | 0.714 |
| S14 | 81.3% ± 3.4% | 0.811 | 0.872 |
| **Mean** | **84.7%** | **0.841** | **0.887** |

FBCSP improves accuracy by up to **24%** over standard CSP for challenging subjects.

## Method

**Filter Bank:** 9 overlapping bands from 4-40 Hz (4-8, 8-12, 12-16, ..., 36-40 Hz)

**Pipeline:**
1. Bandpass filter EEG into frequency sub-bands
2. Apply CSP to each band → spatial filters that maximize class separability
3. Extract log-variance features from CSP-filtered signals
4. Select best features using mutual information (SelectKBest)
5. Classify with Linear Discriminant Analysis (LDA)

**Evaluation:** 5-fold stratified cross-validation

## Quick Start

```bash
git clone https://github.com/Shayank1996/eeg-motor-intention.git
cd eeg-motor-intention
pip install -r requirements.txt
```

Open `fbcsp_pipeline.ipynb` in Jupyter and update `DATA_DIR` to your data path.

## Data Format

Expected structure:
```
Data/Subjects/
├── subject_3/
│   ├── trial_001.csv
│   ├── trial_002.csv
│   └── ...
├── subject_4/
└── ...
```

Each CSV contains 16-channel EEG (256 Hz) with columns for each electrode.

## Configuration

Key parameters in the notebook:

```python
EEG_CHANNELS = ['FP1', 'FP2', 'F3', 'FZ', 'F4', 'T7', 'C3', 'CZ',
                'C4', 'T8', 'P3', 'PZ', 'P4', 'PO7', 'PO8', 'OZ']
ROI_CHANNELS = ['F3', 'FZ', 'F4', 'C3', 'CZ', 'C4', 'P3', 'PZ', 'P4']

FS = 256  # Sampling rate
PLANNING_SLICE = slice(128, 768)   # 0.5-3s (motor planning)
MOVEMENT_SLICE = slice(768, 1536)  # 3-6s (movement execution)
```

## Dependencies

- numpy, scipy, pandas
- scikit-learn
- mne (for CSP implementation)
- matplotlib

## Citation

If you use this code, please cite:

```bibtex
@article{ang2008filter,
  title={Filter bank common spatial pattern algorithm on BCI competition IV datasets 2a and 2b},
  author={Ang, Kai Keng and Chin, Zheng Yang and Zhang, Haihong and Guan, Cuntai},
  journal={Frontiers in neuroscience},
  year={2012}
}
```

## License

MIT
