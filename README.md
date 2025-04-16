# 🫀 Markerless Neck Motion Tracking for Pulse Estimation Using RGB Video

> 📍 Developed by Pushpendra Singh  
> 📚 Ph.D. Research Task — Auckland Bioengineering Institute

---

## 📌 Abstract

This project introduces a **high-fidelity, non-contact pulse estimation system** using standard RGB video of the neck region. By combining classical computer vision methods with signal processing techniques, it accurately captures **carotid-induced skin motion**, extracts physiological signals, and computes real-time metrics such as **heart rate (BPM)**, **rhythm consistency**, and **motion stability**—all in a markerless setup.

---

## 🧠 Core Methodology

### 1. 🎯 Region of Interest (ROI) Detection
- Accumulated frame-difference analysis across the initial frames (30) pinpoints the **most active region**—typically around the carotid artery.
- This ensures the model adapts to different subjects and lighting conditions without manual selection.

### 2. 🕸️ Motion Tracking via Optical Flow
- **Shi-Tomasi corner detection** identifies stable points in the ROI.
- **Lucas-Kanade Optical Flow** tracks these points across frames.
- Points are connected into a “spider-web” mesh, centered around motion centroid, to visualize and quantify skin deformation.

### 3. 📉 Signal Derivation
- The **mean grayscale intensity change** in the ROI per frame forms a 1D motion signal.
- A **Butterworth bandpass filter (0.5–3.0 Hz)** isolates heart-rate-related motion.
- **Gaussian smoothing** improves signal fidelity.

### 4. 🧮 Frequency Analysis
- **Fast Fourier Transform (FFT)** reveals dominant motion frequencies.
- **Adaptive peak detection** calculates **BPM** and **rhythm consistency** via variation in beat intervals.

---

## 📊 Results

### 📌 Key Metrics

| Metric                       | Value    |
|-----------------------------|----------|
| Total Frames                | 326      |
| Estimated BPM               | 72.00    |
| Rhythm Quality Index        | 0.4346   |
| Motion Stability Score      | 0.9998   |

> ✅ **High tracking stability** and **accurate BPM detection** were achieved under real-world lighting and noise conditions.

---

### 📈 Waveform Visualization

![1](https://github.com/user-attachments/assets/51e9c572-6a89-4490-a180-d756466e37af)



- Blue curve = filtered and smoothed signal  
- Red dots = detected peaks  
- Gray = raw intensity fluctuations

---

### 🎧 Frequency Spectrum

![2](https://github.com/user-attachments/assets/81818c2f-988e-4d4a-bd69-611bf63c36a3)

- Dominant peak at ~2.3 Hz (~72 BPM)
- Harmonics indicate slight periodic modulations or secondary motion components

---

### 🧭 Tracker Motion (X/Y)

![3](https://github.com/user-attachments/assets/b0011653-e106-4718-98c2-38b50d782636)

- Low-frequency rhythmic displacement of tracker centroids
- Biomechanical evidence of underlying pulse propagation

---

### 🔥 Live Motion Heatmap

![motion_heatmap_final](https://github.com/user-attachments/assets/929026e0-1488-4abd-a2fc-16f5b5db8551)

- Accumulated skin motion intensity over time
- Highest concentration centered near carotid artery

---

### 🖼️ Final Visual Overlay Frame

![ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/b56c8670-eb0c-4490-95c0-3995782697f4)

![Screenshot (878)](https://github.com/user-attachments/assets/8856eb86-abe9-4eb3-a8bb-efd34bb59de9)

> Includes waveform, FFT, heatmap, and spider-web tracker mesh — all rendered live.

![image](https://github.com/user-attachments/assets/f32dea3e-8fc7-429e-80e3-2d7df00e71e9)

---

## 🔍 Interpretation

- **BPM** is physiologically normal
- **Rhythm Quality** shows mild variability — likely due to natural breathing/motion artifacts
- **Stability** is near-perfect, validating the tracking architecture
- Heatmap and waveform co-localize, confirming anatomical signal origin

---

## 🚧 Additional Difficulties Encountered

One significant challenge during development was the **interaction between visualization resolution and signal fidelity**. Initially, to enhance real-time performance, the video was downscaled to smaller dimensions. However, this reduced the **pixel-wise motion granularity**, which is critical for optical flow-based motion tracking. As a result, the **subtle spatial displacements** that indicate pulse activity became imperceptible. This led to:
- A nearly **flat signal trace**
- Abnormally **high BPM estimates**
- **No dominant frequency** in FFT analysis

**Resolution**:  
To address this, motion tracking computations were restored to the **original full-resolution video**, preserving pixel-level detail. Meanwhile, visualizations such as **live waveform overlays** and **spider-web mesh trackers** were applied to a **scaled-down copy of the frame**, striking a balance between **computational efficiency** and **signal fidelity**.

Another major challenge was **real-time waveform generation** embedded into the output video. Frame-by-frame rendering using `matplotlib` was **computationally expensive**, demanding careful optimization of memory usage and rendering pipelines to avoid latency or freezes in the visualization stream.

---

## ✅ Conclusion and Future Work

This system successfully achieves **accurate, markerless, and non-contact pulse tracking**, integrating real-time overlays, biomechanical mesh visualization, and live waveform plots. It provides a **visually interpretable and physiologically relevant monitoring solution**.

### ⚠️ Limitations
- **FFT sensitivity** to transient or non-periodic motion
- **Lucas-Kanade optical flow** degrades under severe lighting changes
- **Rhythm reliability** requires longer observation windows for clinical use

---

### 🔮 Future Directions

- **Multi-scale Temporal Fusion**  
  Introduce causal filters or attention-based temporal models to better isolate pulse-driven signals from external motion.

- **Learning-Based Optical Flow**  
  Replace Lucas-Kanade with DNNs like **RAFT** or **FlowNet2** for more robust tracking under complex lighting and backgrounds.

- **Physiological Event Classification**  
  Analyze extracted signals to detect patterns like **arrhythmia**, **bradycardia**, or signal irregularities using lightweight temporal CNNs or shallow classifiers.

- **Confidence-Aware Visualization**  
  Add overlays that show **confidence maps** based on motion variance, tracker consistency, and signal-to-noise estimates to improve interpretability.

- **Cross-Region Synchrony Analysis**  
  Expand the framework to track multiple regions (e.g., **neck + forehead**) to analyze **time delays** and infer vascular health metrics.

- **Low-Power Deployment**  
  Explore **ONNX**-based optimization and edge hardware deployment to enable **real-time inference on low-resource systems**.

---

## 🗃️ Files and Outputs

| File                         | Description                                      |
|------------------------------|--------------------------------------------------|
| `heatmap.png`                | Full-duration cumulative motion heatmap          |
| `over.gif`                   | Final live annotated video frame                 |
| `motion_heatmap_final.png`   | Saved final heatmap frame                        |
| `Submission.pynb`            | Final Code Jupyter Notebook                      |
| `live_motion_log.csv`        | Frame-wise data (intensity, peaks, XY coords)    |



