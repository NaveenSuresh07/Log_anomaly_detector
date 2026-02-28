
🔍 AIOps Log Anomaly Detector
Automated detection of system failures in distributed log data using Unsupervised Machine Learning

📌 Overview
This project implements an end-to-end AIOps anomaly detection pipeline designed for massive, unstructured system log datasets. It transforms raw text logs into actionable reliability signals—automatically surfacing session-level failures without the need for labeled training data or manual rule-setting.

Core Objective: Detect anomalous system behaviors with high precision using a fully unsupervised pipeline adaptable to various enterprise observability frameworks.

🏗️ Architecture
Raw System Logs (.log)
        │
        ▼
[Phase 1] Regex Parsing ──────────► Structured DataFrame
        │                            (Timestamp, Level, Component, Content)
        ▼
[Phase 2] Sessionization ─────────► Transactional Grouping
        │                            (Correlating related events into sessions)
        ▼
[Phase 3] NLP Preprocessing ──────► Noise Filtering
        │                            (Removing IPs, hex codes, and variable IDs)
        ▼
[Phase 4] Vectorization ──────────► Feature Matrix Generation
        │                            (TF-IDF with sublinear scaling)
        ▼
[Phase 5] Isolation Forest ───────► Unsupervised Scoring
        │                            (Isolation-based outlier detection)
        ▼
[Phase 6] Analytics & PCA ────────► 2D Visualization & Reporting



⚙️ Key Technical Decisions
Why Isolation Forest?
Unlike clustering algorithms that define "normal" regions, Isolation Forest explicitly targets anomalies. It works by randomly partitioning features; because anomalies are rare and different, they are isolated in significantly fewer steps than normal points. This results in faster, more accurate detection in high-dimensional log data.

Why TF-IDF Vectorization?
Standard word counts often over-emphasize common operational messages (e.g., "Received", "Starting"). TF-IDF (Term Frequency-Inverse Document Frequency) statistically devalues these common terms while amplifying rare, high-signal tokens like Timeout, Exception, Failed, or Refused that indicate genuine system stress.

Why Sessionization?
System logs are rarely informative in isolation. An anomaly usually manifests as an unusual sequence of events over time. By grouping logs by a common transaction ID (Sessionization), the model analyzes the entire lifecycle of an operation rather than disconnected fragments.

📁 Project Structure
log-anomaly-detector/
├── Anomaly_Detector_Pipeline.ipynb   # Complete 6-phase implementation
├── requirements.txt                  # Environment dependencies
├── README.md                         # Documentation
└── outputs/                          # Generated artifacts
    ├── anomaly_report.csv            # Final prioritized anomaly list
    ├── score_distribution.png        # Statistical scoring histogram
    └── pca_visualization.png         # 2D anomaly cluster visualization

    📊 **Results & Visualization**
The pipeline effectively separates standard operational patterns from system outliers. Using Principal Component Analysis (PCA), the high-dimensional log features are projected into a 2D space, providing clear visual evidence of the model's ability to isolate anomalous sessions for root-cause analysis.
