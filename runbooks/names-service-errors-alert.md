# Names Service Errors Alert Runbook

## Overview

A high error rate can affect user experience and service reliability. Common causes of this alert include deployments or pod restarts, or unexpected spike in upstream request which causes Names service to be OOM killed.

## Steps to Triage

1. Analyze logs for errors:
   - Check the pod logs for the names service. see if there are indicative errors in the pod logs during the specified incident time, even if there are no errors at present, the incident might have happened in the past and the service may have subsequently recovered.
2. Check for any specific events if the pod is in Crashloop:
   - If the demo-names pod appears to be in crashloop, check the events of the pod to analyse the reason for the Crashloop
3. Check pod status
   - Look at the last known termination state for pods to confirm the reason for failure by describing pods using a Kubernetes client or CLI.
4. OOM killed status:
   - If the pod appears to be OOM killed, identify the reason for the crash. typically a high spike in traffic from the upstream service can cause the pod to crash. note the time when the OOM killed error started to happen.  When pods get OOM killed focus on the spike in incoming traffic as oppsed to other noise like Jaeger errors.
5. Correlate with other incidents:
   - Check if there are other related alerts or incidents that might be affecting this service. Sometimes a problem in one area can cascade and cause issues in another.

## Proposed next steps:
- If the issue appears to be related to resource constraints, consider scaling up resources or optimizing performance to alleviate the bottlenecks.
- If the incoming traffic violets agreed upon SLA reach out to the upstream team and investigate the sporadic spike in traffic. 

Monitoring and early detection are key. Always consider a broad timeframe when diagnosing issues to understand the full context and root cause of the problem.