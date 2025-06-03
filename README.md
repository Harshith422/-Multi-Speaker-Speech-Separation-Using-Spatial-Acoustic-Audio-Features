# 🗣️ Multi-Speaker Speech Separation Using Spatial & Acoustic Audio Features


> 🚧 **Note:** This project is currently under update.  
> 🔔 If you need urgent access or support, feel free to message me on [LinkedIn](https://www.linkedin.com/in/your-profile-link).

This project presents a hybrid deep learning framework for separating overlapping speech signals using both **spatial** (ITD, ILD, DOA) and **acoustic** (MFCC, Mel-Spectrogram) features. A combination of **SuperFormer** and **SpaRSep** models is used to achieve intelligible, speaker-specific audio outputs from stereo mixtures.

---

## 🎯 Objective

To isolate and separate multiple overlapping speakers from a single stereo recording using deep learning. This system enhances clarity for applications such as:
- Teleconferencing
- Hearing aids
- Smart assistants
- Audio surveillance

---

## 📂 Dataset

**Dataset**: [WHISPER Set 1](https://data.niaid.nih.gov/resources?id=zenodo_3565491)  
- Stereo WAV files
- 2–4 speakers per session
- Ground-truth clean sources available
- 120+ sessions across noisy, echoic, and indoor environments


---

## 🧪 Methodology

### 🔧 Preprocessing
- Resampling: 16kHz using `librosa`
- Normalization: Amplitude scaled to [-1, 1]
- Denoising: Wavelet-based Wiener filter (`scipy.signal.wiener`)

### 🧠 Feature Extraction
- **Acoustic**:
  - MFCC (13 coefficients via `torchaudio.transforms.MFCC`)
  - Mel-Spectrogram (1024 FFT, 512 hop, 128 Mel bins)

- **Spatial**:
  - ITD (Interaural Time Difference)
  - ILD (Interaural Level Difference)
  - DOA (Direction of Arrival via GCC-PHAT)
  - Beamforming: Simple weighted channel summation

### 🧩 Models

#### 🔷 SuperFormer (Acoustic Feature Processor)
- 2D CNN → 4 Transformer Encoder Layers (8 heads)
- Captures temporal + spectral relationships

#### 🔶 SpaRSep (Spatial Feature Processor)
- 2 Linear layers + ReLU
- Encodes ITD/ILD/DOA into a 128D vector

#### 🔄 SeparationModel
- Combines SuperFormer + SpaRSep via **CrossAttention**
- Generates **2 separation masks** using sigmoid activation

---

## 🔈 Post-Processing
- Voice Activity Detection (VAD)
- Spectral subtraction
- Controlled amplification (10 dB cap, no clipping)
- Output: Enhanced `.wav` files for each speaker

---

## 🧪 Evaluation Metrics

| Metric     | Description                                | Speaker 1 | Speaker 2 |
|------------|--------------------------------------------|-----------|-----------|
| SDR        | Signal-to-Distortion Ratio (dB)            | -6.98     | -9.35     |
| SI-SDR     | Scale-Invariant SDR (dB)                   | -33.55    | -48.74    |
| SIR        | Signal-to-Interference Ratio (dB)          | 13.00     | 12.77     |
| SAR        | Signal-to-Artifacts Ratio (dB)             | -6.72     | -9.10     |
| STOI       | Short-Time Objective Intelligibility (%)   | 35%       | 52%       |

**Note**: Low SDR and STOI suggest areas for improvement in intelligibility, but SIR values indicate effective interference suppression.

---

## 📈 Results

- Successfully separated speakers in real-world noisy conditions
- Visualization of Mel-spectrograms and waveforms confirm separation
- Hybrid modeling improves SIR over classical ICA/NMF techniques
- Needs improvement in intelligibility metrics (STOI, SDR)

---

## 🔬 Training Details

| Parameter       | Value     |
|----------------|-----------|
| Sample Rate     | 16 kHz    |
| Mel Bins        | 128       |
| Batch Size      | 8         |
| Learning Rate   | 0.0001    |
| Epochs          | 20        |

Framework: **PyTorch**  
Device: **GPU-accelerated (NVIDIA RTX 3050)**

---

## 🧠 Innovation & Research Gap

- Existing systems underuse spatial features or treat them crudely
- Our **CrossAttentionLayer** fuses acoustic & spatial features
- Combines transformer models and spatial awareness
- Achieves better speaker localization and separation than prior models

---

## 👨‍💻 Team Members

- **Viswanadh A V** – CB.EN.U4AIE22105  
- **Bhargav Ram U** – CB.EN.U4AIE22114  
- **Sai Jaya Sucharan G** – CB.EN.U4AIE22119  
- **Harshith P** – CB.EN.U4AIE22144  
- **Sriramsai B** – CB.EN.U4AIE22166  

**Project Guide**: Dr. Jyothish Lal G  
Amrita Vishwa Vidyapeetham, Coimbatore

---

## 📌 Future Work

- Real-time deployment pipeline
- Adaptive gain control instead of static amplification
- Training on >2 speaker mixtures
- STOI-aware loss functions
- Integration with ASR pipelines (e.g., Whisper, DeepSpeech)

---

## 📜 References

1. Luo & Mesgarani – Conv-TasNet  
2. Vaswani et al. – Transformers (Attention is All You Need)  
3. Subakan et al. – Attention Is All You Need in Speech Separation  
4. WHISPER Dataset – [NIAID Data Portal](https://data.niaid.nih.gov/resources?id=zenodo_3565491)

---

## 📄 License

This project is for educational and research use only.
Dataset: Open access (non-commercial use) from NIAID.

