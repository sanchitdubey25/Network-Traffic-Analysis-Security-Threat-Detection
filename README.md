<!-- # Network Traffic Analysis & Security Threat Detection

A comprehensive Python-based analysis of 2+ million real-world network flows from a firewall/IDS dataset. The project identifies security threats, detects anomalies, and produces a risk-ranked report using statistical and sublinear space techniques.

---

## Project Highlights

- Analysed **2,071,657 network flows** across 6 dataset files
- Implemented **HyperLogLog** for memory-efficient distinct IP estimation (<10% error)
- Detected **DDoS, port scans, brute-force attempts, and C2 beaconing** using statistical thresholding (Mean + 2×SD)
- Produced a structured **risk-ranked security report** classifying threats as High / Medium / Low
- Visualised traffic patterns, protocol distributions, and threat breakdowns

---

## Structure

```
network-traffic-analysis/
├── data/                  ← place dataset CSV files here (see Dataset section)
├── network_traffic_analysis_final.ipynb   ← main analysis notebook
├── .gitignore
└── README.md
```

---

## Analysis Sections

| Section | Description |
|--------|-------------|
| 1. Basic Traffic Statistics | Exploratory analysis — flows, protocols, IPs, packet sizes |
| 2. Sublinear Space Estimation | HyperLogLog, Count-Min Sketch, Bloom Filter |
| 3. Anomaly Detection | Statistical, behavioural, and pattern-based anomalies |
| 4. Deep Security Threat Analysis | Port scans, DDoS, C2 beaconing, risk classification |

---

## Dataset

The dataset consists of 6 CSV files containing real firewall/IDS network flow records (~2M rows total).

📁 **Download the dataset here:** [Google Drive Link] ← *(add your Drive link here)*

Once downloaded, place all 6 CSV files inside the `data/` folder:

```
data/
├── network_analysis_data1.csv
├── network_analysis_data2.csv
├── network_analysis_data3.csv
├── network_analysis_data4.csv
├── network_analysis_data5.csv
└── network_analysis_data6.csv
```

---

## Requirements

- Python 3.10+
- Jupyter Notebook / JupyterLab

### Libraries used

| Library | Purpose |
|--------|---------|
| pandas | Data loading and manipulation |
| numpy | Numerical computations |
| matplotlib | Visualisations |
| seaborn | Statistical plots |
| mmh3 | MurmurHash for HyperLogLog |
| bitarray | Bit array for Bloom Filter |

All libraries are available via Anaconda or pip:

```bash
pip install pandas numpy matplotlib seaborn mmh3 bitarray
```

---

## How to Run

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/network-traffic-analysis.git
   cd network-traffic-analysis
   ```

2. Download the dataset and place CSVs in the `data/` folder (see above)

3. Open the notebook:
   ```bash
   jupyter notebook network_traffic_analysis_final.ipynb
   ```

4. Run all cells from top to bottom (`Kernel → Restart & Run All`)

---

## Key Findings

- Identified top protocols by flow count and bandwidth usage
- Flagged high-risk source IPs exceeding Mean + 2×SD bandwidth threshold
- Detected port scan behaviour, DDoS patterns, and C2 beaconing candidates
- Generated a final risk report with actionable security recommendations

---

## Tech Stack

`Python` `Pandas` `NumPy` `Matplotlib` `HyperLogLog` `Bloom Filter` `Count-Min Sketch` -->


# Network Traffic Analysis & Security Threat Detection

A comprehensive Python-based security analysis of **2,071,657 real-world network flows** from a firewall/IDS dataset. The project performs exploratory traffic analysis, implements sublinear space data structures from scratch, detects anomalies using statistical methods, and produces a risk-ranked security threat report.

---

## Table of Contents
- [Project Highlights](#project-highlights)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [Methodology](#methodology)
- [Key Findings](#key-findings)
- [Requirements & Setup](#requirements--setup)
- [How to Run](#how-to-run)
- [Visualisations](#visualisations)
- [Security Recommendations](#security-recommendations)
- [Tech Stack](#tech-stack)

---

## Project Highlights

- Analysed **2,071,657 network flows** across 6 real firewall/IDS CSV files
- Built **HyperLogLog, Count-Min Sketch, and Bloom Filter from scratch** in Python — no ML libraries
- Detected **DDoS candidates, port scanners, C2 beaconing, and IP hopping** using statistical thresholding
- Bloom Filter achieved **0% false negatives and 0% false positives** on test set with only 117 KB memory
- Count-Min Sketch estimated top-10 destination IP frequencies with **<10% error** using sublinear space
- Produced a structured **risk-ranked security report** classifying all threats as High / Medium / Low

---

## Dataset

The dataset contains real firewall/IDS network flow records (~2M rows, 25 columns) including source/destination IPs, ports, protocols, byte/packet counts, TCP flags, payload data, and attack labels.

📁 **Download dataset here:** `[Add your Google Drive link here]`

Place all 6 files inside the `data/` folder before running:

```
data/
├── network_analysis_data1.csv
├── network_analysis_data2.csv
├── network_analysis_data3.csv
├── network_analysis_data4.csv
├── network_analysis_data5.csv
└── network_analysis_data6.csv
```

Key columns used:

| Column | Description |
|--------|-------------|
| `source` / `destination` | Source and destination IP addresses |
| `protocolName` | Network protocol (tcp_ip, udp_ip, icmp, etc.) |
| `totalSourceBytes` / `totalDestinationBytes` | Bandwidth usage per flow |
| `totalSourcePackets` / `totalDestinationPackets` | Packet counts |
| `startDateTime` / `stopDateTime` | Flow timestamps |
| `duration` | Connection duration in seconds |
| `Label` | Ground truth — `Normal` or `Attack` |

---

## Project Structure

```
network-traffic-analysis/
├── data/                                      ← place CSV files here
├── network_traffic_analysis_final.ipynb       ← main analysis notebook
├── .gitignore
└── README.md
```

---

## Methodology

### Section 1 — Basic Network Traffic Statistics
Exploratory analysis to understand the baseline traffic landscape:
- Total flow count, unique IPs, protocol distribution
- Top-K protocols by flow count vs. bandwidth — dual metric comparison
- Top-10 most active source and destination IPs by total bytes
- Packet size distribution and flow duration statistics

### Section 2 — Sublinear Space Estimation
Three probabilistic data structures implemented from scratch:

#### 2a — HyperLogLog (Distinct IP Estimation)
- Estimates unique IP count without storing every IP in memory
- Uses bit-level tricks on hash values to approximate cardinality
- Achieved **<10% estimation error** across the full 2M+ flow dataset

#### 2b — Count-Min Sketch (Heavy Hitter Detection)
- A `width × depth` hash table that approximates item frequency from a stream
- Built with `width=2000, depth=7` — O(n) time, O(width×depth) space
- Validated against exact pandas value counts on top-10 destination IPs
- **Max error <10%, Mean error <10%** across all tested IPs

#### 2c — Bloom Filter (IP Blocklist Membership Testing)
- Probabilistic set-membership structure: zero false negatives guaranteed
- Double hashing (MD5 + SHA1) with optimal bit array size formula
- Parameters: `capacity=100,000`, `error_rate=1%`
- Result: **958,505-bit array (117 KB)** vs 150.1 KB for an exact Python set
- Test: **0 false negatives, 0 false positives** on 100 sampled IPs

### Section 3 — Advanced Anomaly Detection

#### 3a — Statistical Anomalies
- High-byte and high-packet flows using z-score thresholding (z > 2σ)
- Rare protocol flows — protocols with <1% share of total traffic
- High-volume source IPs exceeding the Mean + 2σ bandwidth threshold

#### 3b — Behavioural Anomalies
- **Burst detection:** IPs generating traffic spikes beyond rolling average + 2σ within hourly windows
- **DDoS candidate detection:** destinations receiving abnormally high inbound flow counts

#### 3c — Suspicious Communication Patterns
- **Long-duration connections:** flows exceeding Mean + 3σ (~1,908 seconds) — indicative of persistent backdoors or data exfiltration tunnels
- **Multi-protocol IPs:** hosts using 3+ distinct protocols within a single hour — indicative of scanning tools or protocol tunnelling

### Section 4 — Deep Security Threat Analysis

#### Port Scan Detection
- Flags IPs probing an abnormally high number of distinct ports and targets per day
- Dual-criterion threshold (ports AND targets) reduces false positives

#### Slow DDoS Detection
- Rolling 3-window average of inbound flow count per destination IP
- Flags sustained, escalating inbound traffic patterns

#### IP Hopping Detection
- Identifies source IPs rapidly cycling through many destination IPs on a single port
- Characteristic of lateral movement and network reconnaissance

#### C2 Beaconing Detection
- Hunts for flows with unusually regular inter-arrival timing — a hallmark of Command & Control malware

#### Risk Classification
All detected threats are scored and ranked:
- 🔴 **High** — Immediate action required (block/isolate)
- 🟡 **Medium** — Investigate and monitor
- 🟢 **Low** — Log and review

---

## Key Findings

### Traffic Overview
| Metric | Value |
|--------|-------|
| Total flows analysed | 2,071,657 |
| Columns | 25 |
| Top source IP by bytes | 192.168.2.109 — 7.37 GB |
| Top destination IP by bytes | 192.168.5.122 — 11.94 GB |

### Anomaly Detection Summary
| Category | Type | Count |
|----------|------|-------|
| Statistical | High-byte flows (z > 2σ) | 2,333 |
| Statistical | High-packet flows (z > 2σ) | 2,208 |
| Statistical | Rare protocol flows (<1%) | 8,355 |
| Statistical | High-volume source IPs | 21 |
| Behavioural | IPs with burst activity | 26 |
| Behavioural | DDoS candidate targets | 511 |
| Suspicious | Long-duration flows (>1,908s) | 7,628 |
| Suspicious | Multi-protocol IPs (3+ protocols/hr) | 160 |

### Threat Detection Highlights
| Threat | Finding |
|--------|---------|
| Port Scan | `192.168.2.107` probed **14,509 distinct ports** and **19,291 targets** in a single day |
| Port Scan | 3 confirmed scanner IPs detected (threshold: 588 ports/day) |
| Multi-protocol | `192.168.2.107` used TCP, UDP, IGMP, and ICMP in a single hour window |
| Long Duration | 7,628 flows exceeded 1,908s — potential exfiltration or persistent backdoor tunnels |
| DDoS | 511 destination IPs flagged for sustained high-inbound traffic patterns |

### Sublinear Space Results
| Structure | Memory | Accuracy |
|-----------|--------|----------|
| Bloom Filter | 117 KB | 0% FP, 0% FN |
| Count-Min Sketch | O(width × depth) | <10% mean error |
| HyperLogLog | Sublinear | <10% cardinality error |

---

## Requirements & Setup

- Python 3.10+
- Anaconda (recommended) or pip

### Libraries

| Library | Purpose |
|---------|---------|
| `pandas` | Data loading, cleaning, aggregation |
| `numpy` | Numerical operations, z-score thresholding |
| `matplotlib` | Bar charts, line charts, histograms |
| `seaborn` | Statistical plots, heatmaps, boxplots |
| `mmh3` | MurmurHash3 for HyperLogLog |
| `bitarray` | C-backed bit array for Bloom Filter |
| `scipy` | Additional statistical utilities |

```bash
pip install pandas numpy matplotlib seaborn mmh3 bitarray scipy
```

---

## How to Run

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/network-traffic-analysis.git
   cd network-traffic-analysis
   ```

2. Download the dataset and place all 6 CSVs in the `data/` folder

3. Launch the notebook:
   ```bash
   jupyter notebook network_traffic_analysis_final.ipynb
   ```

4. Run all cells: `Kernel → Restart & Run All`

> **Note:** Loading 2M+ rows takes ~30–60 seconds on first run. Subsequent cells are faster once the dataframe is in memory.

---

## Visualisations

Charts produced by this notebook:

- Protocol distribution — flow count vs. bandwidth (side-by-side bar)
- Top-10 source and destination IPs by bytes (horizontal bar)
- Packet size distribution by protocol (boxplot)
- Count-Min Sketch vs. exact counts for top-10 IPs (grouped bar)
- Burst activity timeline per flagged IP (line chart)
- Port scanner activity — distinct ports probed per day (bar)
- DDoS rolling average detection (line chart)
- Risk level breakdown — High / Medium / Low (bar)
- Threat type distribution (horizontal bar)

---

## Security Recommendations

1. **Block/isolate all High-risk source IPs immediately** — especially `192.168.2.107` (14,509 ports probed in one day)
2. **Enable rate limiting** on ports flagged by the IP hopping analysis
3. **Deploy deep packet inspection** on flows identified as C2 beaconing candidates
4. **Set automated alerts** for rolling traffic increases sustained over 3+ time windows
5. **Review all long-duration connections** (>1,908s) for evidence of data exfiltration

---

## Tech Stack

`Python 3.10` · `Pandas` · `NumPy` · `Matplotlib` · `Seaborn` · `HyperLogLog` · `Count-Min Sketch` · `Bloom Filter` · `Jupyter Notebook`