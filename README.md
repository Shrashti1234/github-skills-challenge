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

