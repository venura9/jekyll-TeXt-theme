---
title: What's up Azure (July 2020)
key: 20200723
author: Venura Athukorala
tags: azure
---

With all the partners heading into Microsoft Inspire which is the main partner event, the last 30 days have been a bit quiet with the extravagant updates which is most likely to be released in the next 30 days. 

Key updates for this month covers into;

* Storage 
* Azure Arc


###[Customer-initiated Storage account failover is now generally available](https://azure.microsoft.com/en-us/updates/azure-storage-account-failover-ga/)

* For GRS and RA-GRS Storage you can now trigger the failover manually without Microsoft having to detect an issue with the primary region. 
At the point of writing this post the feature is broken and Australia doesn't yet support it. 

![Storage Account in Australia](https://whatcloud.xyz/assets/current-manual-storage-failover-status.png "Storage Account in Australia")

* [You can read about how the failover is triggered](https://docs.microsoft.com/en-au/azure/storage/common/storage-initiate-account-failover?tabs=azure-portal)

###[Public Preview for Azure Monitor for VMs on Arc Enabled Servers](https://azure.microsoft.com/en-us/updates/public-preview-for-azure-monitor-for-vms-on-arc-enabled-servers/)

This is effectively Azure Monitor for VMs extending to Azure Arc Instances.

![Azure Arc](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/6673cca7-4ea2-43be-af88-11ffec6a6659.png)

[Learn more about Azure Arc](https://azure.microsoft.com/en-au/services/azure-arc/#features)

###[Durable Functions support for Python](https://azure.microsoft.com/en-us/updates/durable-functions-now-supports-python/)

Might not make everyone excited. But,I am.
Durable functions allow you to extend the capabilities of Azure Functions to new heights. 

[Read More](https://docs.microsoft.com/en-au/azure/azure-functions/durable/durable-functions-overview?tabs=python)

- Function Chaining
- Fan out/fan in
- Async HTTP APIs
- Monitor
- Human interaction

E.g. Fan out/in
![Fan out/in](https://docs.microsoft.com/en-au/azure/azure-functions/durable/media/durable-functions-concepts/fan-out-fan-in.png)

```python
import azure.functions as func
import azure.durable_functions as df


def orchestrator_function(context: df.DurableOrchestrationContext):
    parallel_tasks = []

    # Get a list of N work items to process in parallel.
    work_batch = yield context.call_activity("F1", None)

    for i in range(0, len(work_batch)):
        parallel_tasks.append(context.call_activity("F2", work_batch[i]))
    
    outputs = yield context.task_all(parallel_tasks)

    # Aggregate all N outputs and send the result to F3.
    total = sum(outputs)
    yield context.call_activity("F3", total)


main = df.Orchestrator.create(orchestrator_function)
```