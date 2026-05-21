# AI-Assisted-Adaptive-Diffusion-for-Low-Latency-and-Sustainable-Channel-Estimation
AI-Assisted Adaptive Diffusion for Sustainable Channel Estimation (A-CDDCE). A PyTorch &amp; MATLAB framework optimizing 2x2 MIMO-OFDM systems using a dynamic denoising controller, dual-axis attention, and DFT noise truncation to minimize latency and NMSE/BER in high-mobility channels.
# AI-Assisted Adaptive Diffusion for Low-Latency and Sustainable Channel Estimation (A-CDDCE)

An end-to-end simulation and deep learning framework implementing an **Adaptive Conditional Denoising Diffusion Channel Estimator (A-CDDCE)** for high-mobility $2\times2$ MIMO-OFDM wireless communication systems. 

Conventional diffusion-based channel estimators use a rigid, fixed number of denoising steps regardless of signal quality, leading to sub-optimal resource utilization. A-CDDCE introduces a lightweight neural network controller that dynamically adjusts inference steps based on real-time channel quality, striking an optimal balance between estimation accuracy, latency, and computational energy efficiency.

---

## 🛠️ System Architecture & Workflow
The framework follows a modular pipeline implemented in Python using **PyTorch**:
1. **Signal Generation & Channel Modeling:** Generates QPSK-modulated data mapped onto 64 subcarriers across 32 OFDM frames, passing through a realistic 3GPP TDL-C fading channel simulated at high mobility ($120 \text{ km/h}$).
2. **Initial Estimation & Preprocessing:** Obtains a rough channel state using Least Squares (LS) interpolation. A Discrete Fourier Transform (DFT) preprocessing step truncates the Channel Impulse Response (CIR) to its 28 most significant taps to suppress initial noise.
3. **Adaptive Diffusion Denoising:** The coarse estimate is fed into a custom U-Net architecture. An adaptive mechanism maps the Channel Quality Indicator (CQI) to an optimized range of **4 to 12 reverse steps**, bypassing computational waste in high-SNR conditions.
4. **Advanced Learning Modules:** Employs **Dual-Axis Attention** to learn joint time-frequency correlations, integrates sinusoidal **SNR Embeddings** for noise generalization, and activates **Monte Carlo Dropout** ($x8$ draws) during inference to provide robust, uncertainty-aware predictions.

---

## 📊 Technical Parameters & Simulation Setup

| Parameter | Value / Specification | Notes |
| :--- | :--- | :--- |
| **MIMO Configuration** | $2\times2$ | Transmit / Receive Antennas |
| **Subcarriers ($N$)** | 64 | Comb-type pilot spacing |
| **OFDM Frames** | 32 | Symbols per frame |
| **Channel Model** | 3GPP TDL-C | High mobility ($120 \text{ km/h}$ at $3.5 \text{ GHz}$) |
| **CIR Significant Taps** | 28 Taps | Truncation boundary for DFT preprocessing |
| **Modulation Schema** | QPSK | Applied over all transmission bands |
| **SNR Evaluation Range** | $5 \text{ dB}$ to $30 \text{ dB}$ | Evaluated in steps of $5 \text{ dB}$ |
| **Diffusion Inference Depth**| $4 \text{ to } 12 \text{ Steps}$ | Dynamic adaptation based on CQI |
| **Network Training Details**| 350 Epochs | AdamW Optimizer, LR Warm-up + Cosine Decay |

---

## 🚀 Performance & Advantages
* **Superior Accuracy:** Drastically reduces Normalized Mean Square Error (NMSE) compared to classical linear, spline, and low-pass interpolation configurations (e.g., Bagadi & Das baselines).
* **Enhanced Link Reliability:** Provides a clean reduction in Bit Error Rate (BER), directly improving symbol detection accuracy in challenging high-Doppler environments.
* **Low Inference Overhead:** Decreases deployment latency by minimizing reverse diffusion iterations dynamically when channel quality allows, keeping your wireless implementation hardware-sustainable.

## 💻 Environment & Toolboxes Used
* **Python / PyTorch** (Core network training, U-Net optimization, and diffusion modeling)
* **MATLAB / Toolboxes** (Communications and Signal Processing toolboxes utilized during initial architecture exploration and comparative link verification)
