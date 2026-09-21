# GitHub Challenge

<img src="https://octodex.github.com/images/Professortocat_v2.png" align="right" height="200px" />

Hey there!

Your challenge is ready.
Follow the instructions provided for this challenge and complete the required tasks in this repository.

Make sure your work is committed and pushed to your repository before submission.

Good luck!


## AIOps Assessment Scenario

This assessment monitors a `payment-service` using operational telemetry and log
records stored in `data/service_data.json`. The data includes response time, CPU
utilization, memory utilization, log level, and service messages.

The operational problem is identifying service degradation, such as slow payment
requests, high resource utilization, and timeout or error conditions, before those
conditions affect users.

AIOps brings these signals together to detect anomalies automatically and move
the resulting events through the pipeline. In this exercise, the detector
examines each record, the producer publishes detected anomalies to an in-memory
topic, the consumer retrieves them, and `aiops_pipeline.py` reports the final
results.

## Operational Data Analysis

The operational data is stored in `data/service_data.json` and contains 10
records for `payment-service`.

- **Metrics:** `response_time_ms` measures request latency, while `cpu_percent`
	and `memory_percent` measure resource utilization. These numeric fields show
	the service's performance and resource health over time.
- **Log information:** `log_level` identifies the severity (`INFO` or `ERROR`),
	and `message` describes the event, such as a successful payment or a timeout.
	The `service` field identifies the source service.
- **Timestamps:** `timestamp` uses an ISO 8601-style date-time format and orders
	observations at one-minute intervals from 10:00 through 10:09 on 2026-09-20.
	This makes it possible to correlate metric changes with log events and see
	when the incident starts and ends.
- **Normal behaviour:** The records from 10:00-10:04 and 10:07-10:09 show
	successful `INFO` messages. Response time stays between 120 and 150 ms, CPU
	utilization between 42% and 50%, and memory utilization between 51% and 57%.
- **Unusual behaviour:** The records at 10:05 and 10:06 indicate a service
	incident. Response time jumps to 610 ms and 640 ms, and the log messages
	report a payment-service timeout followed by a database connection timeout.
	At 10:06, CPU reaches 94% and memory reaches 91%; at 10:05, they are also
	elevated at 75% and 70%. The metrics and `ERROR` logs together distinguish
	these observations from the normal baseline.

---

&copy; 2025 GitHub &bull; [Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/code_of_conduct.md) &bull; [MIT License](https://gh.io/mit)

## Task 2: Logs and Metrics Analysis

### Operational Data

The operational data is stored in `data/service_data.json` and contains
timestamped observations for the `payment-service`.

### Metrics

- `response_time_ms`: service response time in milliseconds
- `cpu_percent`: CPU utilization percentage
- `memory_percent`: memory utilization percentage

### Log Information

- `log_level`: severity/category of the log entry
- `message`: description of the service event

### Timestamp Analysis

The records are timestamped at one-minute intervals from 10:00 through
10:09 on 2026-09-20.

### Normal Behaviour

The observations from 10:00 through 10:04 and 10:07 through 10:09 show
relatively stable metrics and successful `INFO` messages indicating that
payment requests were processed successfully.

### Unusual Behaviour

At 10:05, the response time increased to 610 ms and the log level was
`ERROR` with a payment service timeout message.

At 10:06, the response time increased to 640 ms, CPU utilization reached
94%, memory utilization reached 91%, and the log reported a database
connection timeout.

These observations represent the unusual behaviour in the supplied
operational data.

## Task 3: Anomaly Detection

The provided `AnomalyDetector` was used to analyse the operational data
from `data/service_data.json`.

### Detection Result

The pipeline processed 10 records and detected 2 anomalies.

### Detected Anomalies

#### 10:05

- Response time: 610 ms
- CPU utilization: 75%
- Memory utilization: 70%
- Log level: ERROR
- Message: Payment service timeout
- Reasons: High response time, Error log detected

#### 10:06

- Response time: 640 ms
- CPU utilization: 94%
- Memory utilization: 91%
- Log level: ERROR
- Message: Database connection timeout
- Reasons: High response time, High CPU utilization,
  High memory utilization, Error log detected

### Normal Observations

The remaining records were not identified as anomalies by the
configured detection rules.

### Missed Anomalies

No expected anomaly was missed based on the supplied operational data
and the configured detection rules.

### Incorrectly Flagged Normal Events

No normal event was incorrectly flagged during the execution.

### Limitation / Possible Improvement

The detector uses fixed thresholds for response time, CPU utilization,
and memory utilization. An adaptive baseline based on historical
service behaviour could improve the detection approach.
## Task 4: AIOps Event Flow

The anomaly detection component generates an event when abnormal
behaviour is identified.

The event is passed to the EventProducer, which publishes it to the
shared in-memory anomaly-events topic.

The EventConsumer reads the events from the same topic and passes the
consumed events to the downstream AIOps processing.

The verified flow is:

Operational Data → Anomaly Detection → Event → Producer → Topic →
Consumer → AIOps Output

The final execution confirmed that the detected anomaly events were
published and subsequently consumed by the event-processing pipeline.
## Task 5: Troubleshooting and Corrections

### Issues Identified and Corrected

| Component | Problem | Cause | Correction | Verification |
|---|---|---|---|---|
| Anomaly Detector | Concerning error logs were not handled by the intended log rule | The detector checked the wrong log level | Corrected the log-level condition to match the supplied operational data | Pipeline detected the two anomalous records |
| Event Producer/Topic | Producer and consumer were connected to different topic objects | Separate in-memory `EventTopic` instances were created | Producer and consumer were connected to the same anomaly-event topic | Events were successfully consumed |
### Troubleshooting Verification

After applying the corrections, the workflow was executed again.

The operational dataset was processed successfully, anomalous records
were detected, anomaly events were published to the shared in-memory
topic, and the consumer successfully retrieved the generated events.

The provided test suite was also executed successfully.