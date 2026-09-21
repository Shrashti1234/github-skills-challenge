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

---

&copy; 2025 GitHub &bull; [Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/code_of_conduct.md) &bull; [MIT License](https://gh.io/mit)

