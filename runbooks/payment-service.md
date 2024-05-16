# Payment Service Runbook

## Links

[Grafana dashboard](https://grafana.demo-04.demo.app.cleric.io/d/W2gX2zHVk/demo-dashboard?orgId=1&var-service=paymentservice)

## Symptom

Payment transactions failing.


## Potential Causes

- Payment service is down.
- API key for payment gateway is expired or invalid.

## Investigation Steps

1. Confirm the issue by reproducing it.
2. Check the status of the payment service deployment in Kubernetes:


    kubectl get pods -l app=payment-service

3. Review the logs of the payment service for any errors:

 
    kubectl logs <payment-service-pod-name>

4. Check the API key status for the payment gateway.


## Resolution Steps

- If the service is down, restart it.
- If the API key is expired or invalid, update it with a valid one.
