# Quantum Machine Learning Qiskit
Original repository by Matthew Maccelari
## Overview  
In this repository, quantum-enhanced support vector machines (QSVMs) are explored by embedding classical data into a high-dimensional Hilbert space via a parameterised quantum feature map and comparing performance against a classical RBF-kernel SVM. The programs:  
1. Clean and preprocess a binary classification dataset.  
2. Reduce dimensionality with PCA.  
3. Train and evaluate a QSVM on both a statevector simulator and real IBM quantum hardware.  
4. Benchmark accuracy and runtime against a classical SVM baseline.  
5. Visualise quantum kernel matrices as heatmaps.  

---

## Repository Structure  
```

ELEN4022_LAB3_2025_MATTHEW_MACCELARI/
├── README.md
├── .env                      ← pinned package versions
├── src/
│   ├── simulatorQML.ipynb    ← QSVM on Qiskit statevector simulator
│   └── hardwareQML.ipynb     ← QSVM on IBM quantum hardware
├── Heatmaps/
│   └── kernel_*.png          ← Saved quantum-kernel heatmaps
└── Dataset/
    └── bots_vs_users.csv     ← Raw binary-classification dataset

````

---

## Requirements  
- Python 3.8+  
- `pandas`  
- `numpy`  
- `scikit-learn`  
- `matplotlib`  
- **Qiskit stack (exact versions)**  
  ```bash
  pip install \
    qiskit==1.4.3 \
    qiskit-aer==0.11.0 \
    qiskit-ibm-runtime==0.13.1 \
    qiskit-machine-learning==0.8.2
  ```

---

## Installation & Setup

1. **Clone the repo**

   ```bash
   git clone https://github.com/your-repo/ELEN4022_LAB3_2025_MATTHEW_MACCELARI.git
   cd ELEN4022_LAB3_2025_MATTHEW_MACCELARI
   ```
2. **Install dependencies**
Create and activate the Conda environment from `environment.yml`:

```bash
conda env create -f environment.yml
conda activate elen4022_lab3

3. **Configure IBM Quantum credentials**

   ```bash
   export IBMQ_TOKEN="YOUR_IBM_QUANTUM_API_TOKEN"
   ```

   or run `QiskitRuntimeService.save_account(…)` in a Python REPL.

---

## Usage

### 1. Simulator Notebook

Open and run:

```bash
jupyter notebook src/simulatorQML.ipynb
```

This notebook:

* Loads & cleans `Dataset/bots_vs_users.csv`
* Applies variance filtering and PCA
* Trains QSVM with `ZZFeatureMap + FidelityStatevectorKernel` on a statevector simulator
* Trains a classical RBF-kernel SVM baseline
* Records accuracies and saves kernel-matrix heatmaps to `Heatmaps/`

### 2. Hardware Notebook

Open and run:

```bash
jupyter notebook src/hardwareQML.ipynb
```

This notebook follows the same preprocessing pipeline, then:

1. Builds a 1-repetition `ZZFeatureMap` circuit on $n$ qubits.
2. Uses `QiskitRuntimeService` & `SamplerV2` to dispatch fidelity-circuit jobs to the least-busy IBM quantum backend.
3. Computes Gram matrices from measurement probabilities of $\lvert0\cdots0\rangle$.
4. Trains a classical SVM with `kernel='precomputed'` on the real-device kernel.
5. Prints test accuracy and optionally visualises the train-kernel heatmap.

> **Note:** Real-device runs can take several minutes and incur queue times.

---

## Results

* **Heatmaps:** `Heatmaps/kernel_n{n}_size{m}.png` showing quantum kernel matrices for each PCA dimension $n$ and sample size $m$.
* **Accuracy tables:** Displayed in both notebooks, comparing QSVM vs classical SVM across experimental conditions.

