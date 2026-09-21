# GitHub Challenge

<img src="https://octodex.github.com/images/Professortocat_v2.png" align="right" height="200px" />

Hey there!

Your challenge is ready.
Follow the instructions provided for this challenge and complete the required tasks in this repository.

Make sure your work is committed and pushed to your repository before submission.

Good luck!


---

&copy; 2025 GitHub &bull; [Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/code_of_conduct.md) &bull; [MIT License](https://gh.io/mit)

#Tasks

1. AIOPS SCENARIO EXPLANATION
This repository contains a lightweight Python simulation of an AIOps operational workflow for monitoring microservices, specifically payment-service[cite: 3]. The primary goal is to analyze operational metrics and logs, identify abnormal behavior, generate event messages, and publish them through a producer-topic-consumer streaming model to trigger downstream incident handling[cite: 3].


2. DESCRIPTION OF OPERATIONAL DATA
The dataset data/service_data.json contains chronological telemetry logs and metrics recorded sequentially[cite: 3]:
- Metrics: Quantitative fields including response_time_ms, cpu_percent, and memory_percent.
- Logs: Textual records containing log_level (INFO, ERROR) and message.
- Timestamps: Structured ISO 8601 UTC strings (2026-09-20T10:00:00) for event ordering.


3. FINDINGS FROM LOGS AND METRICS
- Normal Operation (Records 1–5 & 8–10): Response times keep under 150ms, CPU usage stays below 50%, memory stays around 50–57%, and logs display INFO status.
- Anomalous Operation (Records 6 & 7):
  - Record 6 (10:05:00): Spike in response time (610ms) and ERROR log level (Payment service timeout).
  - Record 7 (10:06:00): Massive resource spike with 640ms response time, 94% CPU, 91% memory utilization, and ERROR log level (Database connection timeout).


4. ANOMALY DETECTION ENGINE
The detection component (src/anomaly_detector.py) flags records exceeding[cite: 3]:
- response_time_ms > 500
- cpu_percent > 80
- memory_percent > 80
- log_level == ERROR


5. EVENT PROCESSING PIPELINE ARCHITECTURE
Operational Telemetry Data -> Anomaly Detector -> Event Producer -> Event Topic Queue -> Event Consumer -> AIOps Action Output[cite: 3]


6. ISSUES IDENTIFIED AND CORRECTED
1. Topic Mismatch Bug (src/aiops_pipeline.py): 
   - Problem: The producer published to topic service-events while the consumer listened to topic anomaly-events, causing 0 events to be consumed.
   - Fix: Refactored both producer and consumer to share a single topic instance shared_topic.
2. Log Level Detection Bug (src/anomaly_detector.py): 
   - Problem: The detector evaluated log_level == WARNING, missing all actual ERROR log entries in the dataset.
   - Fix: Updated check to evaluate log_level in ["ERROR", "CRITICAL"].
3. Fibonacci Unit Test Failure (tests/calculation_Test.py):
   - Problem: Test expected F10 = 89.
   - Fix: Corrected assertion target to 55.


7. RESULTS OF FINAL EXECUTION
- Records Processed: 10[cite: 3, 4]
- Anomalies Detected: 2[cite: 3, 4]
- Events Consumed: 2[cite: 3, 4]


8. LIMITATIONS AND POTENTIAL IMPROVEMENTS
- Limitation: The current detector uses static metric thresholds that do not adjust for legitimate traffic spikes during peak hours.
- Improvement: Implement adaptive dynamic thresholds using Isolation Forests or Exponential Moving Averages (EMA) to prevent false positives.


9. STEPS TO REPRODUCE
1. Install dependencies[cite: 3]:
   pip install -r requirements.txt
2. Run the AIOps pipeline[cite: 3]:
   python src/aiops_pipeline.py
3. Run validation unit tests[cite: 3]:
   python -m pytest