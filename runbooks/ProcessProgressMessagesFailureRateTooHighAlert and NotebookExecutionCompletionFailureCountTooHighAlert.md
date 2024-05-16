# ProcessProgressMessagesFailureRateTooHighAlert and NotebookExecutionCompletionFailureCountTooHighAlert runbook

## Overview

Webapp monitors and processes the progress of notebook executions by periodically polling Chauffeur using the GetExecutionStatus RPC and updating the TreeStore with data from the response. If there are errors during this flow, the correct execution state and results may not be correctly persisted or shown to the user - thus, this alert monitors a core user journey for the notebook. These 2 alerts monitor the failure rate and count of this flow.

Specifically:

ProcessProgressMessagesFailureRateTooHighAlert tracks the failure rate of processing individual GetExecutionStatus responses (ProgressMessages) across a region - i.e. it will fire when the ratio of failed to successfully processed GetExecutionStatus polls is too high.

NotebookExecutionCompletionFailureCountTooHighAlert tracks the number of notebook executions which terminated with an internal system error across a region - i.e. it will fire when there is a large number of notebooks which had failures in processing GetExecutionStatus polls. This alert tracks the absolute number of ProgressMessage processing failures at a per-notebook granularity.

## Dashboards

go/nbf/stabilitydash (particularly Interactive execution row)

go/wpd (webapp performance dashboard)

## Steps to triage

1. Find the exception causing the errors:

ProcessProgressMessagesFailureRateTooHighAlert:

Narrow down the incident time period using this m3 query in the given region: sum(increase(notebook_progressMessageProcessed_count{status="failure"}[5m]))

Search ELK in the given region and time period for tags.subDir:"webapp" AND "Failed to process new ProgressMessage" to find exceptions which caused the errors.

NotebookExecutionCompletionFailureCountTooHighAlert:

Narrow down the incident time period using this m3 query in the given region: sum(increase(notebook_commandTerminationErrors_count[5m])) by (category, class)

Search ELK in the given region and time period for tags.subDir:"webapp" AND "Received error for notebook" to find exceptions which caused the errors. Note that this log only emits (and thus the metric for this alert only increments) at most once for a given execution.


2. Search ELK for occurrences of the given exception. Note: since GetExecutionStatus polling continues until webapp receives a terminal ProgressMessage (or a fatal error occurs), a single execution request may result in an unbounded number of ProgressMessages and thus a similarly unbounded number of ProgressMessage processing failures. ProgressMessage processing is done on a background thread pool so the best way to correlate them to 1 execution is via traceId

 

Most of the time, the exceptions are cluster connectivity errors from 1 notebook or cluster. If this is the case (caused by 1 workspace) and it is transient and self-mitigates, there is usually nothing actionable. Consider downgrading the incident severity.

If the exception is a cluster connectivity error that affects many workspaces, consider checking with #eng-cluster-team whether there was a widespread issue connecting to clusters.

If the exception is a client-side error (e.g. thrown somewhere in webapp):

- Check for webapp instability (e.g. CPU throttling) via go/nbf/stabilitydash → System row or go/wpd. If webapp system metrics indicate instability, consider checking with #eng-webapp.
- There may be a webapp regression which affects processing ProgressMessages. The stack trace should indicate the offending line, so debug from there. go/nbf/stabilitydash → Interactive execution row may help.