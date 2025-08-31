# Voice Cloning: Fireship (CSM-1B Model)

A project focused on cloning the voice of the YouTuber Fireship using a Coqui Studio Model (CSM-1B). This repository contains the necessary code, a log of the development process, and a report on the outcomes.

---
Checkout the colab using this google Driver link https://colab.research.google.com/drive/1he2pXht611BbaVs0PsTt1XSV7sVc_uBI?usp=sharing

## 🚀 Setup & Usage

To run this project yourself, follow these steps:

1.  **Clone the Repository:**
    ```bash
    git clone (https://github.com/MetalCloth/Fireship-Voice-Clone)
    ```
2.  **Set up the Environment:**
    ```bash
    pip install -r requirements.txt
    ```
3.  **Run the Notebook/Script:**
    Open and run the `Untitled1.ipynb` notebook in an environment with the required GPU resources. Follow the steps inside the notebook to process your own audio samples.

---

## 🛠️ Technical Details

| Component | Specification |
| :--- | :--- |
| **Model Used** | Coqui Studio Model (CSM-1B) |
| **GPU Required** | NVIDIA Tesla T4 / P100 (or better) |
| **VRAM Required** | Approx. 8-12 GB |
| **Key Libraries** | `TTS`, `torch`, `librosa` |

---

##  journey & Challenges (Error Log)

This section documents the development journey, including errors, confusing steps, and their resolutions.

| Phase / Task | Challenge & Error Encountered | Confusing Aspect | Solution & Learning |
| :--- | :--- | :--- | :--- |
| **Environment Setup** | `CUDA version mismatch` error when installing PyTorch. | The error message was unclear about which NVIDIA driver version was needed for the specific CUDA toolkit. | Had to downgrade the CUDA toolkit version to 11.8 to match the installed driver. **Learning:** Always check driver compatibility *before* installing ML libraries. |
| **Data Preprocessing** | Audio files were not being normalized correctly, leading to distorted input. | The documentation for the preprocessing function wasn't clear about the expected audio format (e.g., 16-bit PCM, 22050 Hz). | Manually resampled and reformatted all input audio clips using FFmpeg before passing them to the script. **Learning:** Never trust input data; always enforce a strict format. |
| **Model Training** | `OutOfMemoryError` even on a capable GPU. | It was confusing why the memory was spiking, as the batch size seemed reasonable. | The issue was the audio clip length. Implemented a chunking strategy to keep all audio segments under 15 seconds. **Learning:** VRAM usage is tied to both batch size *and* sequence length. |
| **Inference / Cloning** | The final cloned voice had a robotic, monotone sound. | The parameters for the vocoder were poorly documented, and it wasn't clear how they affected the output's naturalness. | After trial and error, I found that adjusting the `speaker_wav` parameter to use multiple reference clips improved the prosody significantly. **Learning:** Inference quality is highly dependent on tweaking hidden parameters. |

---

## 📋 Project Report

### What Worked Well

* **CSM-1B Model:** The base model was powerful and capable of capturing Fireship's core vocal timbre with relatively small amounts of training data.
* **Data Cleaning:** The time spent meticulously cleaning and preparing the input audio clips (removing background noise, normalizing volume) had the biggest positive impact on the final output quality.
* **Colab Environment:** Using Google Colab's free T4 GPU was sufficient for running the inference process without major investment.

### What Failed or Was Difficult

* **Capturing Intonation:** The model struggled to replicate Fireship's signature fast-paced, sarcastic, and dynamic intonation. The cloned voice often sounded flatter and more neutral than the original.
* **Initial Setup:** The initial environment setup was the most time-consuming and frustrating part due to conflicting library dependencies and CUDA issues.
* **Real-Time Generation:** The process is far from real-time. Generating a 30-second clip took several minutes, making rapid testing difficult.

### Suggestions for Improvement

* **More Varied Training Data:** To better capture the speaker's prosody, the training dataset should include a wider range of emotional tones (e.g., excitement, sarcasm, neutral narration).
* **Fine-Tuning:** With more resources, fine-tuning the base CSM-1B model on a larger, high-quality dataset of the target voice could yield significantly better results.
* **Better Documentation:** The underlying library could benefit from more examples and clearer documentation, especially regarding inference parameters.

---

## ⏱️ Time Log

| Section | Time Taken |
| :--- | :--- |
| **Initial Research & Setup** | ~4 hours |
| **Data Collection & Cleaning** | ~3 hours |
| **Model Training / Fine-Tuning** | ~[1] hours |
| **Inference & Testing** | ~2 hours |
| **Documentation & Reporting**| ~1 hour |
| **Total** | **~[10] hours** |
