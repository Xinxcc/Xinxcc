# Projects

One-page write-ups. Full source in private repositories — read access on request.

---

## 1. AIModule-Next — ML training-platform architecture
**Problem.** A legacy desktop app for the AI model-training workflow (classification / object detection / anomaly detection) had grown into a four-language call chain (UI → native → shell → Python) with a monolithic UI, magic numbers, hard-coded paths and no real progress protocol.

**Approach.** Re-architected it into a clean two-process design with an explicit API contract:
- **Tauri (Rust) shell** — native window/menus/dialogs; owns the backend lifecycle.
- **React + TypeScript frontend** — design-system components, a 7-step wizard state machine, live charts driven by WebSocket.
- **FastAPI backend** — explicit workflow state machine, a **cancellable async job queue**, **WebSocket** push of training metrics (epoch/loss/acc/progress/log), Pydantic config.
- **Offline packaging** — PyInstaller-frozen backend shipped as a Tauri sidecar; single installer.

**Shows.** Software architecture, full-stack engineering, REST + WebSocket API design, async systems, refactoring judgment, offline desktop delivery.
**Status.** Working architecture prototype; the compute layer is stubbed behind a simulated training loop so the full UI/contract runs end-to-end.
**Stack.** Rust (Tauri), TypeScript/React, Tailwind, FastAPI, Pydantic, WebSocket, PyInstaller.

---

## 2. Local RAG Service — on-prem retrieval-augmented Q&A
**Problem.** Answer questions over a private document collection without sending anything to the cloud.

**Approach.** A containerized microservice:
- **FastAPI** API (`/ingest`, `/query`, `/health`) with API-key auth.
- **Ingestion** of PDF / DOCX / Markdown / text, with text cleaning and overlapping chunking (deterministic chunk IDs).
- **Embeddings & generation via Ollama** (local models), **vector search via Qdrant** (cosine), with **access-group filtering** so answers respect document visibility.
- **Cited answers** — responses reference the retrieved chunks.
- **docker-compose** orchestrating the API, Ollama and Qdrant with persistent volumes.

**Shows.** LLM application engineering, RAG design, vector databases, container orchestration, document processing, retrieval + citation.
**Status.** Working prototype. Planned hardening: batch embeddings (currently per-item), evaluation harness (RAGAS-style), streaming responses, larger models.
**Stack.** FastAPI, Ollama, Qdrant, Docker Compose, pypdf, python-docx.

---

## 3. Neuroevolution + AutoML + Active Learning — property prediction
**Problem.** Predict material properties (regression) from limited experimental data, and decide which experiments to run next.

**Approach.**
- **AutoKeras** for neural architecture search of an initial model.
- A **genetic algorithm that evolves the network weights directly** — custom crossover, mutation and selection over flattened weight vectors, with a relative-error fitness; plus a population-based iterative-improvement loop.
- An **active-learning loop**: estimate prediction uncertainty across a model committee (coefficient of variation) to rank the most informative next samples.

**Shows.** Evolutionary computation / neuroevolution, AutoML, uncertainty quantification, custom Keras metrics, experiment automation.
**Stack.** Python, TensorFlow/Keras, AutoKeras.
*Independent research project.*

---

## 4. 3D Motion-Quality Analysis (M.Sc. thesis)
**Problem.** Quantify the quality of human 3D motion from cameras and compare it against a reference.

**Approach.** End-to-end pipeline in Python/OpenCV:
- **MediaPipe** pose estimation → **multi-view 3D reconstruction** (camera calibration, epipolar geometry, triangulation, `solvePnP`);
- **Fusion with a wearable motion sensor** via cross-correlation time-synchronization;
- **Multi-dimensional Dynamic Time Warping** features → **Random Forest** classification of motion quality (with grid-search cross-validation and confusion-matrix evaluation).

**Shows.** Computer vision, multi-view geometry, sensor fusion, signal synchronization, time-series + ML.
**Stack.** Python, OpenCV, MediaPipe, NumPy/SciPy, scikit-learn. Grade: good.

---

## 5. ECG Atrial-Fibrillation Detection (team)
**Problem.** Classify single-lead ECG into 4 classes (PhysioNet/CinC 2017).

**Approach.** A **Squeeze-and-Excitation ResNet** built in TensorFlow/Keras on ECG **mel-spectrograms**, iterated from a 2D-CNN baseline; full preprocessing pipeline (denoising, segmentation, normalization, spectrogram) with a custom data generator and augmentation.

**Shows.** Deep-learning implementation (not just library calls), attention mechanisms, biosignal processing.
**Stack.** TensorFlow/Keras, NumPy/SciPy. Team project — credits to all members; my contribution noted in the repo.
