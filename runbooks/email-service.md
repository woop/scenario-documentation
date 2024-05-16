# Email Service Runbook

## Symptom

Emails not being sent or received.

## Potential Causes

- Email service is down.
- SMTP server issues.

## Investigation Steps

1. Confirm the issue by reproducing it.
2. Check the status of the email service deployment in Kubernetes:


    kubectl get pods -l app=email-service

3. Review the logs of the email service for any errors:


    kubectl logs <email-service-pod-name>

4. Check the SMTP server status.

## Resolution Steps

- If the service is down, restart it.
- If there are SMTP server issues, check the server status and network connectivity.
