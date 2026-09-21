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

# AIOps Assessment Report

## Task 1: Set Up and Understand the Environment

### Description

The repository was opened in GitHub Codespaces and the project structure was reviewed to understand the purpose of each component.

### Components Identified

| Component | File |
|------------|---------|
| Operational Data | `data/service_data.json` |
| Anomaly Detection | `src/anomaly_detector.py` |
| Event Producer | `src/event_producer.py` |
| Event Topic | `src/event_topic.py` |
| Event Consumer | `src/event_consumer.py` |
| Final AIOps Processing | `src/aiops_pipeline.py` |

### Service Being Monitored

The project monitors a payment-service application using operational metrics and logs.

### Operational Problem

The operations team needs to identify abnormal behaviour and process detected anomalies automatically.

### Purpose of AIOps

The purpose of this assessment is to analyse operational data, detect anomalies, generate events, and process them through an event-driven workflow.

---

## Task 2: Analyse Logs and Metrics

### Metrics Identified

- response_time_ms
- cpu_percent
- memory_percent

### Log Information Identified

- log_level
- message

### Timestamp Usage

The timestamp field records when each observation occurred and helps identify when abnormal behaviour happened.

### Normal Behaviour

Most records showed:

- Response time between 120–150 ms
- CPU usage between 42–50%
- Memory usage between 51–57%
- INFO log messages

### Unusual Behaviour

#### Record at 10:05:00

- Response time: 610 ms
- ERROR log: Payment service timeout

#### Record at 10:06:00

- Response time: 640 ms
- CPU usage: 94%
- Memory usage: 91%
- ERROR log: Database connection timeout

### Observation

Out of 10 records:

- 8 records were normal
- 2 records showed anomalous behaviour

---

## Task 3: Identify Anomalies

### Description

The anomaly detector was executed using the provided operational data.

### Result

The detector processed all 10 records and identified 2 anomalies.

### Detected Anomalies

#### Anomaly 1

Timestamp: 2026-09-20T10:05:00

Reason:
- High response time

Log:
- Payment service timeout

#### Anomaly 2

Timestamp: 2026-09-20T10:06:00

Reasons:
- High response time
- High CPU utilization
- High memory utilization

Log:
- Database connection timeout

### Findings

The detector successfully distinguished normal records from abnormal records and provided reasons for each anomaly.

### Limitation

The current implementation relies on fixed threshold values and may not detect more complex anomaly patterns.

---

## Task 4: Verify the AIOps Event Flow

### Workflow

Operational Data → Anomaly Detection → Event Generation → Producer → Topic → Consumer → AIOps Output

### Component Roles

#### Producer
Publishes anomaly events to the topic.

#### Topic
Stores anomaly events in memory.

#### Consumer
Reads anomaly events from the topic.

#### Event
Represents a detected anomaly.

### Verification

The workflow was executed and verified to ensure anomaly events moved successfully through the producer-topic-consumer pipeline.

### Result

Detected anomaly events were successfully generated, published, stored, consumed, and processed.

---

## Task 5: Investigate and Correct the Workflow

### Issue Identified

The producer and consumer were connected to different topic instances.

### Cause

Events were being published to one topic while the consumer was listening to another topic.

### Initial Result

```text
Records processed: 10
Anomalies detected: 2
Events consumed: 0
```

### Correction Applied

Both the producer and consumer were connected to the same shared topic instance.

### Verification

The workflow was executed again after applying the correction.

### Result After Fix

```text
Records processed: 10
Anomalies detected: 2
Events consumed: 2
```

The issue was successfully resolved.

---

## Task 6: Execute the End-to-End Pipeline

### Workflow Executed

Operational Data → Anomaly Detection → Event Generation → Producer → Topic → Consumer → AIOps Output

### Execution Result

- Records Processed: 10
- Anomalies Detected: 2
- Events Consumed: 2

### Outcome

Operational data was processed successfully.

Detected anomalies were converted into events, published to the topic, consumed successfully, and processed by the final AIOps component.

---

## Task 7: Documentation

### Documentation Added

The README was updated to include:

- AIOps scenario
- Operational data description
- Metrics and log analysis
- Anomaly detection findings
- Event processing flow
- Issues identified and corrected
- Final execution results
- Limitation
- Reproduction steps

### Reproduction Steps

1. Clone the repository.
2. Open the project in GitHub Codespaces or VS Code.
3. Install dependencies:

```bash
pip install -r requirements.txt
```

4. Run the AIOps pipeline:

```bash
python src/aiops_pipeline.py
```

5. Run validation tests:

```bash
pytest -v
```

---

## Task 8: Validation

### Validation Method

The provided test suite was executed.

### Command

```bash
pytest -v
```

### Result

```text
8 passed
```

### Verification

The successful test execution confirmed that:

- Operational data can be processed
- Anomaly detection works correctly
- Events are generated successfully
- Events move through the pipeline
- Consumers receive events
- The final workflow executes successfully

---

## Task 9: Commit, Push and Submission

### Git Operations Performed

- Reviewed changes
- Committed changes
- Pushed changes to GitHub

### Pull Request

A pull request was created from the forked repository to the original exercise repository.

### Pull Request Summary

#### What the workflow detects

- High response time
- High CPU utilization
- High memory utilization

#### Validation Method

- Pipeline execution
- Automated test execution

#### Final Result

- Records processed: 10
- Anomalies detected: 2
- Events consumed: 2

#### Issue Corrected

Producer and consumer were connected to different topic instances. Both components were connected to a shared topic to resolve the issue.

#### Limitation

The anomaly detector uses fixed threshold values and could be improved with more advanced anomaly-detection techniques.

---

# Final Outcome

The AIOps workflow successfully processed operational data, detected anomalies, generated events, passed them through the producer-topic-consumer pipeline, and produced the expected final output.
