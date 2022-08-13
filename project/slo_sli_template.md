# API Service

| Category     | SLI                                                                                          | SLO                                                                                                          |
|--------------|----------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| Availability | Total number of successful requests / total number of requests                               | 99%                                                                                                          |
| Latency      | Bucket of requests below 100ms in a histogram showing the 90th percentile                    | 90% of requests below 100ms                                                                                  |
| Error Budget | The percentage of the remaining error budget. It shows how much is left of your error budget (Remaining error budget in percentage = 1-[(1-compliance)/(1-objective)]) | Error budget is defined at 20%. This means that 20% of the requests can fail and still be within the budget  |
| Throughput   | Total number of successful requests per second                                                          | 5 RPS indicates the application is functioning                                                               |
