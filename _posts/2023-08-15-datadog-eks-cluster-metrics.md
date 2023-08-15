---
layout: post
title: "Querying Datadog metrics for EKS Monitoring"
excerpt: "Centralise data collected by Datadog"
tags: [datadog, governance, api]
comments: true
---

The idea was to collect the metrics across all the clusters running datadog agents. This enables us to track, visualize, and alert on key metrics, including CPU and memory usage, across EKS clusters over time.

## Setup

Assuming that the Datadog Agent is deployed as a DaemonSet within your cluster, you ensure that monitoring capabilities are automatically extended to every node, capturing vital metrics at both the pod and node level.

## Metric Collection and Analysis

Once integrated, Datadog begins aggregating data and toolset in the platform supports most of the use cases. In this case, I wanted to take a different direction and enhance the queryability against some of the metrics I had available directly from AWS + internal metrics.

## Snippet

```python
from datadog_api_client.v1 import ApiClient, ApiException
from datadog_api_client.v1.api import metrics_api
from datadog_api_client.v1.models import *
from datetime import datetime

configuration = datadog_api_client.v1.Configuration(
    host = "https://api.datadoghq.com"
)

configuration.api_key['apiKeyAuth'] = 'DATADOG_API_KEY'

with ApiClient(configuration) as api_client:
    api_instance = metrics_api.MetricsApi(api_client)
    now = datetime.now()
    metrics_payload = Series(
        series=[
            SeriesMetric(
                metric="eks.cluster.cpu.usage",
                type=MetricIntakeType("gauge"),
                interval=10,
                points=[
                    (now, 0.75)
                ],
                tags=[
                    "environment:production",
                    "cluster_name:foo_bar",
                ],
            ),
            SeriesMetric(
                metric="eks.cluster.memory.usage",
                type=MetricIntakeType("gauge"),
                interval=10,
                points=[
                    (now, 0.60)
                ],
                tags=[
                    "environment:production",
                    "cluster_name:foo_bar",
                ],
            ),
        ]
    )

    try:
        api_instance.submit_metrics(body=metrics_payload)
        print("💫 All good")
    except ApiException as e:
        print("Exception when calling MetricsApi->submit_metrics: %s\n" % e)

```

## Conclusion

While Datadog platform is a great place to run most the queries, there is no need to stop there. I used to picture Datadog as destination hub only where I would send all my metrics.
However, this experiments helped to picture a source of data as well. The agents work nicely in all clusters and collect valuable data that can be aggregated with other pieces of the system.

> **Punchline:** If you want to clean up data before sending to the store you may end up having specific implementation for each API call.
