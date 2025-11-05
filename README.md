# BirdCLEF 2024 Audio Classification - Milestone 1

## Team Information

**Team Name:** [Your Team Name]

**Team Members:**
- Pribék Barnabás - ETWSGU
- Csernák Gergő - TVEXOV
- Molnár Bence - JINVFB

## Project Description

This project uses the BirdCLEF 2024 competition dataset for bird species identification from audio recordings. The solution is built using Keras and TensorFlow, implementing the following main steps:

1. **Data Source**: BirdCLEF 2024 competition dataset (182 bird species, ~24,459 audio recordings)
2. **Data Processing**: Converting OGG format audio files to mel-spectrograms
3. **Model**: EfficientNetV2-B2 based image classifier (pretrained on ImageNet)
4. **Augmentation**: MixUp, time-masking, and frequency-masking techniques

### Dataset Download

The dataset can be downloaded from the [BirdCLEF 2024 Kaggle competition](https://www.kaggle.com/competitions/birdclef-2024). This project works with data stored in Google Drive for easier access.

## Repository Files and Their Functions

### `BirdCLEF24_NHF.ipynb`
The primary notebook containing the complete data pipeline:

**1. Data Source and Download:**
- Google Drive integration for accessing the BirdCLEF 2024 dataset
- Dataset verification and validation
- Metadata loading (`train_metadata.csv`)

**2. Data Exploration and Visualization:**
- Audio file loading and playback
- Waveform visualization
- Mel-spectrogram visualization
- Species distribution statistics

**3. Data Preparation:**
- **Train/Validation/Test Split**: 80/20 ratio split
- **Audio Preprocessing**:
  - Cropping/padding to uniform 15-second length
  - 32,000 Hz sampling rate
  - Mel-spectrogram generation (128x384 pixels)
- **Augmentation** (training data only):
  - MixUp (α=0.4)
  - Time-masking (6-12%)
  - Frequency-masking (6-10%)
- **TensorFlow Dataset Pipeline**: Efficient, batch-based data loading

**Final Output:**
- `train_ds`: Training dataset (19,567 samples, augmented)
- `valid_ds`: Validation dataset (4,892 samples, not augmented)
- Format: (128, 384, 3) spectrograms + one-hot encoded labels (182 classes)

## Execution Guide

### Prerequisites

1. **Google Colab account**
2. **Google Drive**: Dataset uploaded at the following path:
   ```
   My Drive/Deep_Learning_NHF/birdclef-2024/
   ├── train_audio/
   │   ├── asbfly/
   │   ├── ashdro1/
   │   └── ...
   └── train_metadata.csv
   ```

### Running the Notebook

1. **Open the notebook in Google Colab:**
   
   [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/BarnaP02/Deep-Learning-NHF/blob/4266bea0f3fd50093292e726b95d2c151ad7ac28/BirdCLEF24_NHZ.ipynb)

2. **Execute cells in order:**
   - **Import Libraries**: Install dependencies
   - **Kaggle Setup and Data Download**: Mount Google Drive
   - **Configuration**: Set hyperparameters
   - **Load Class Names**: Initialize classes
   - **Meta Data**: Load and filter data
   - **Data Split**: Create train/validation split
   - **Data Loaders**: Build TensorFlow Dataset pipeline
   - **Visualization**: Display and verify batches

3. **Drive Authorization**: On first run, authorize Google Drive access

4. **Verification**: Sample images and spectrograms will be displayed at the end of the notebook

## Technical Details

### Configuration (CFG class)
```python
img_size = [128, 384]      # Spectrogram dimensions
batch_size = 64            # Batch size
duration = 15              # Audio length (seconds)
sample_rate = 32000        # Sampling rate
epochs = 10                # Training epochs
preset = 'efficientnetv2_b2_imagenet'  # Model
```

### Libraries Used
- TensorFlow 2.19.0
- Keras 3.10.0
- KerasCV 0.9.0
- Librosa (audio processing)
- Pandas, NumPy (data handling)
- Matplotlib (visualization)

## Results

- **Total samples loaded**: 24,459
- **Number of bird species**: 182
- **Training samples**: 19,567
- **Validation samples**: 4,892
- **Spectrogram dimensions**: 128 (frequency) × 384 (time) × 3 (channels)

## Notes

- The original Kaggle notebook has been modified with Google Drive support
- Compatibility issues fixed for the latest library versions
- Reproducibility: `seed = 42`

## License

This project follows the BirdCLEF 2024 competition rules and dataset license.
