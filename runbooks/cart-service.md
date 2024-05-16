# Cart Service Runbook

## Symptom

Users unable to add items to the cart.

## Potential Causes

- Cart service is down.
- Database connection issues.

## Investigation Steps

1. Confirm the issue by reproducing it.
2. Check the status of the cart service deployment in Kubernetes:


    kubectl get pods -l app=cart-service

3. Review the logs of the cart service for any errors:

    
    kubectl logs <cart-service-pod-name>

4. Check the database connection status.

## Resolution Steps

- If the service is down, restart it.
- If there are database connection issues, check the database server status and network connectivity.