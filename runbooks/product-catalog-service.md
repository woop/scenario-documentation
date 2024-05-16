# Product Catalog Service Runbook

## Symptom

Products not displaying or incorrect product information.

## Potential Causes

- Product catalog service is down.
- Database connection issues.

## Investigation Steps

1. Confirm the issue by reproducing it.
2. Check the status of the product catalog service deployment in Kubernetes:


    kubectl get pods -l app=product-catalog-service

3. Review the logs of the product catalog service for any errors:

   
    kubectl logs <product-catalog-service-pod-name>

4. Check the database connection status.

## Resolution Steps

- If the service is down, restart it.
- If there are database connection issues, check the database server status and network connectivity.