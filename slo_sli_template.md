# API Service

| Category | SLI | SLO |
|---|---|---|
| Availability | Number of successful requests / Total number of requests | 99% |
| Latency | 90th percentile of the latency | 90% of requests below 100ms |
| Error Budget | 1 - Availability | Error budget is defined at 20%. This means that 20% of the requests can fail and still be within the budget |
| Throughput | Number of requests per second | 5 RPS indicates the application is functioning |